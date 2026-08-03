# Comprehensive Technical Debt Review Report - Sprints 1 to 7

This report provides a Principal-level review of the architectural, security, database, performance, and scaling technical debts across the QuizForge AI backend codebase (Sprints 1 through 7).

---

## 1. Summary of Findings

| ID | Finding Description | Area | Severity | Estimated Effort | v1.0 Fix? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **TD-01** | In-Memory BackgroundTasks CPU-Bound Thread Saturation | Performance / Concurrency | **Critical** | 2-3 Days | Yes |
| **TD-02** | Missing Outbox Event Background Processing Worker | Architecture | **Critical** | 2 Days | Yes |
| **TD-03** | Synchronous File reads for prompt loading templates | Performance | **High** | 1 Day | Yes |
| **TD-04** | Mock Tokenizer stubs in Ingestion and Generation pipelines | AI Engineering | **High** | 1 Day | Yes |
| **TD-05** | Refresh Token Rotation (RTR) Race Grace Windows | Security / Resiliency | **High** | 2 Days | Yes |
| **TD-06** | Manual Soft Delete filter checks (prone to leakage) | Database / Code Quality | **Medium** | 1 Day | No |
| **TD-07** | Missing Compound Indexes on Auditing & Versions | Database | **Medium** | 0.5 Day | Yes |
| **TD-08** | Inconsistent Pydantic Configurations (v1 vs v2 definitions) | Code Quality | **Low** | 0.5 Day | No |
| **TD-09** | google-generativeai SDK Deprecation Warning | Dependencies | **Low** | 1 Day | No |

---

## 2. Detailed Findings

### TD-01: In-Memory BackgroundTasks CPU-Bound Thread Saturation
* **Sprints Affected**: Sprint 5 (Document Ingestion), Sprint 6 (Quiz Generation).
* **Impact**: Document parsing (`PyMuPDF`, `python-docx`) is highly CPU-bound. Offloading these tasks directly to FastAPI's in-memory `BackgroundTasks` runs them on the web server's internal threadpool. Under heavy load, concurrent parses will saturate server CPU, delaying simple incoming HTTP requests.
* **Risk**: **Critical**. Severe latency regressions on main API routes.
* **Resolution**: Replace FastAPI `BackgroundTasks` with a dedicated asynchronous task worker queue (e.g. Celery + Redis).

### TD-02: Missing Outbox Event Background Processing Worker
* **Sprints Affected**: Sprint 7 (Quiz Management).
* **Impact**: Domain events (`QuizPublished`, `QuizArchived`, etc.) are written successfully to the `outbox_events` table within local SQL transactions, ensuring transaction safety. However, there is no background worker polling and dispatching these events. They will accumulate in the database indefinitely.
* **Risk**: **Critical**. Indefinite database growth and complete failure of downstream event delivery.
* **Resolution**: Write a polling daemon inside Celery Beat or set upPgOutput logical replication to read from the WAL log.

### TD-03: Synchronous Prompt File Reads
* **Sprints Affected**: Sprints 5.5, 6, 7.
* **Impact**: `PromptBuilder` reads text template files (`system_prompt.txt`, etc.) from physical disk synchronously using Python's blocking `open` call on every request. Disk I/O blocks the asynchronous event loop.
* **Risk**: **High**. Retards event-loop response throughput under high load.
* **Resolution**: Cache prompt templates in memory on application startup.

### TD-04: Mock Tokenizer Stubs
* **Sprints Affected**: Sprints 5.5, 6, 7.
* **Impact**: Input and output token calculations are computed using approximate character divisions (`len(text) // 4`). This leads to inaccurate cost and token tracking, rendering spending guards ineffective.
* **Risk**: **High**. Financial risk of context overrun and billing spikes.
* **Resolution**: Integrate proper tokenizers (e.g. tiktoken for OpenAI or Gemini's count_tokens endpoint).

### TD-05: Refresh Token Rotation Race Conditions
* **Sprints Affected**: Sprint 4 (Authentication).
* **Impact**: If a client app triggers two concurrent token refreshes (e.g., during page load), both will submit the same old refresh token. The first request succeeds, but the second will fail and trigger a breach warning, terminating the user's active session.
* **Risk**: **High**. Poor user experience due to false-positive security lockouts.
* **Resolution**: Implement a brief grace window (e.g., 10 seconds) during which recently rotated refresh tokens are still accepted.

### TD-06: Manual Soft Delete Filter Checks
* **Sprints Affected**: Sprint 7.
* **Impact**: Service queries filter out soft-deleted quizzes using `.where(Quiz.is_deleted.is_(False))` manually. Developers writing new modules might forget this condition, causing deleted quizzes to leak into API responses.
* **Risk**: **Medium**. Data leakage and query inconsistency.
* **Resolution**: Implement global database filters in SQLAlchemy (e.g. `with_loader_criteria`).

---

## 3. Version 1.0 Roadmap Recommendations

To ensure enterprise-grade stability, cost containment, and security before launch, we recommend addressing the following items in a prep sprint before version 1.0:

1. **Celery Offloading (TD-01 & TD-02)**: Set up a Celery worker to handle both CPU-bound document parsing and outbox event publishing.
2. **True Tokenizer Integration (TD-04)**: Replace the dummy division calculations with Gemini's token API to make token budget guards robust.
3. **In-Memory Prompt Caching (TD-03)**: Load prompts on startup.
4. **RTR Grace Window (TD-05)**: Prevent session drops.
5. **Database Indexing (TD-07)**: Add compound indexes on `(user_id, is_deleted, status)` to ensure fast queries as database records grow.
