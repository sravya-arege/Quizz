# Quizz – AI-Powered Quiz Generation Platform

Quizz is a full-stack AI-powered SaaS application that transforms documents into high-quality, editable quizzes using Google Gemini AI. Users can upload PDFs, DOCX, or TXT files, automatically generate quizzes, edit questions, manage versions, and analyze AI usage through an interactive dashboard.

Built with a production-ready architecture, Quizz demonstrates modern software engineering practices including modular backend design, JWT authentication, AI orchestration, optimistic locking, transactional outbox patterns, version management, and responsive React frontend development.

## ✨ Features

- 🔐 JWT Authentication with Refresh Tokens
- 📄 PDF, DOCX & TXT Document Upload
- 🧠 AI Quiz Generation using Google Gemini
- ✍️ Interactive Quiz Editor
- 🔄 Single Question AI Regeneration
- 📚 Quiz Version History & Rollback
- 📊 Analytics Dashboard
- 🔍 Search, Filter & Pagination
- 📦 RESTful API with FastAPI
- 🎨 Modern React + TailwindCSS UI
- 🌙 Dark/Light Theme Support
- 📱 Fully Responsive Design

## 🛠 Tech Stack

### Frontend
- React 18
- TypeScript
- Vite
- Tailwind CSS
- React Router
- Axios
- TanStack React Query
- Framer Motion
- Recharts
- React Hook Form
- Zod

### Backend
- FastAPI
- SQLAlchemy 2.0
- Pydantic
- Alembic
- JWT Authentication
- Google Gemini API
- Python

### Database
- PostgreSQL / SQLite (Development)

### Dev Tools
- Docker
- Pytest
- Ruff
- ESLint
- GitHub

## 🚀 Core Workflow

1. Register/Login
2. Upload documents
3. Process document into chunks
4. Generate AI quizzes
5. Review & edit questions
6. Publish or archive quizzes
7. Track AI usage and analytics

## 📂 Project Structure

```
Quizz/
├── backend/
│   ├── app/
│   ├── tests/
│   ├── migrations/
│   └── docs/
│
├── frontend/
│   ├── src/
│   ├── public/
│   └── components/
│
└── README.md
```

## ⚡ Installation

### Clone Repository

```bash
git clone https://github.com/sravya-arege/Quizz.git
cd Quizz
```

### Backend

```bash
cd backend
python -m venv .venv

# Windows
.venv\Scripts\activate

pip install -r requirements.txt

uvicorn app.main:app --reload
```

Backend runs on:

```
http://127.0.0.1:8000
```

Swagger:

```
http://127.0.0.1:8000/docs
```

### Frontend

```bash
cd frontend

npm install
npm run dev
```

Frontend:

```
http://localhost:5173
```

## 🧪 Testing

Backend

```bash
pytest
```

Lint

```bash
ruff check .
```

Frontend

```bash
npm run lint
npm run build
```

## 📸 Screenshots

Add screenshots here after deployment.

## 🔮 Future Improvements

- AI-generated explanations
- Multi-language quiz generation
- Team collaboration
- Real-time notifications
- Export to PDF/CSV
- Public quiz sharing
- Online quiz taking
- Leaderboards
- Email reports
- Stripe subscription support

## 📄 License

This project is licensed under the MIT License.

## 👨‍💻 Author

Developed by **Sravya Arege**

GitHub: https://github.com/sravya-arege
