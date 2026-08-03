# Contributing Guidelines

Welcome to **QuizForge AI**! To maintain production-grade software standards, all contributors must adhere to these guidelines for code standards, branching, and commits.

## 1. Branching Strategy

We use a modified Git Flow model optimized for continuous integration and delivery.

* **`main`**: Production-ready branch. Must always be stable. Direct commits are blocked.
* **Feature Branches (`feat/...`)**: Used for developing new features. Example: `feat/quiz-generation`.
* **Bugfix Branches (`fix/...`)**: Used for resolving defects. Example: `fix/jwt-expiration`.
* **Chore Branches (`chore/...`)**: Used for repository maintenance, configs, or CI updates. Example: `chore/add-pre-commit`.

### Workflow
1. Branch off `main`.
2. Implement and verify code locally (linters, formatters, and unit tests must pass).
3. Submit a Pull Request (PR) to `main`.
4. Ensure CI pipeline checks pass successfully.

---

## 2. Commit Message Standards (Conventional Commits)

Commit messages must follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```text
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types
* **`feat`**: A new feature for the user.
* **`fix`**: A bug fix.
* **`docs`**: Documentation changes only.
* **`style`**: Changes that do not affect the meaning of the code (formatting, missing semi-colons).
* **`refactor`**: A code change that neither fixes a bug nor adds a feature.
* **`test`**: Adding missing tests or correcting existing tests.
* **`chore`**: Changes to the build process or auxiliary tools and libraries.

### Examples
* `feat(auth): add JWT cookie configuration`
* `fix(quiz): handle empty document edge case in parser`
* `chore(deps): upgrade fastapi and uvicorn`

---

## 3. Pull Request Requirements

Before submitting a Pull Request, ensure that:
* Code formatting is verified using Prettier (frontend) and Ruff (backend).
* Pre-commit hooks run and pass without errors.
* Unit tests pass (`pytest` for backend, `npm run test` for frontend once added).
* No secrets or raw API keys are committed. All configurations are read from environment variables.
