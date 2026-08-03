# QuizForge AI

QuizForge AI is a production-grade, modular monolithic SaaS application that allows users to upload PDF, DOCX, or TXT documents and automatically generate structured quizzes using generative AI models.

## 1. System Requirements

Ensure you have the following installed locally:
* **Docker & Docker Compose** (for running PostgreSQL)
* **Python 3.11+** (for FastAPI Backend)
* **Node.js 18+** & **npm** (for React Frontend)
* **Git** (for version control and pre-commit hooks)

---

## 2. Environment Variables & Secret Management Strategy

### Environment Separation
We separate configuration from code strictly adhering to **Factor III of the Twelve-Factor App methodology**.
* **Development**: Controlled via local `.env` files (git-ignored).
* **Production**: Set directly through the platform provider's environment panel (Railway for Backend, Vercel for Frontend).

### Secret Guidelines
1. **Never commit secrets**: All keys, passwords, and connection strings must live in `.env` files which are globally ignored.
2. **Pydantic Validation**: The backend validates environmental configurations using Pydantic Settings on startup. If a variable is missing or typed incorrectly, the server fails-fast immediately.
3. **Secret Rotation**: When rotating database credentials or API keys, update the config host dashboard and restart the instances without modifying the code.

---

## 3. Quick Start (Development Setup)

### A. Run Database
Spin up the local PostgreSQL container:
```bash
docker-compose up -d
```

### B. Backend Setup
1. Navigate to the backend folder:
   ```bash
   cd backend
   ```
2. Create virtual environment and activate it:
   ```bash
   python -m venv .venv
   # Windows:
   .venv\Scripts\activate
   # Unix/macOS:
   source .venv/bin/activate
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Copy environment config and update variables if necessary:
   ```bash
   cp .env.example .env
   ```
5. Install pre-commit hooks at root:
   ```bash
   cd ..
   pre-commit install
   ```

### C. Frontend Setup
1. Navigate to the frontend folder:
   ```bash
   cd frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Copy environment configuration:
   ```bash
   cp .env.example .env
   ```
4. Run the React dev server:
   ```bash
   npm run dev
   ```
