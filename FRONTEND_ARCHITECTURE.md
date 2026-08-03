# QuizForge AI — Frontend SaaS Application Architecture

## 1. Executive Summary

The QuizForge AI frontend is an enterprise-grade React 18 single-page application built with TypeScript, Vite, TailwindCSS, Framer Motion, and TanStack React Query. It is designed to match the high UX standards of modern developer platforms like Vercel, Linear, and Clerk.

---

## 2. Directory & Feature Architecture

```
frontend/src/
├── types/
│   └── index.ts               # Domain TypeScript interfaces (User, Document, Quiz, Question, Analytics)
├── lib/
│   ├── api.ts                 # Axios client with JWT injection & 401 refresh token interceptors
│   └── utils.ts               # Tailwind class merge utility (clsx + tailwind-merge)
├── context/
│   ├── auth-context.ts        # React AuthContext definition
│   └── AuthContext.tsx        # AuthProvider session state manager
├── hooks/
│   └── useAuth.ts             # Custom hook accessing AuthContext
├── components/
│   ├── ui/                    # Reusable UI Primitives (Button, Card, Badge, Input, Skeleton, EmptyState)
│   ├── auth/                  # ProtectedRoute guard component
│   ├── layout/                # AppLayout, Sidebar, TopNavbar, CommandPalette (Ctrl+K)
│   ├── documents/             # DocumentUploadModal (Drag-and-drop, magic byte check)
│   └── quizzes/               # GenerateQuizModal wizard
├── pages/
│   ├── Login.tsx              # Auth sign in page
│   ├── Register.tsx           # Account registration page
│   ├── Dashboard.tsx          # Analytics KPI cards & Recharts token/cost trend chart
│   ├── DocumentsPage.tsx      # Knowledge file repository & chunk management
│   ├── QuizzesPage.tsx        # Quiz collection, filtering, publishing, archiving & deleting
│   ├── QuizEditorPage.tsx     # Question & option editor with snapshot version rollback drawer
│   ├── ProfilePage.tsx        # User profile & credentials manager
│   ├── SettingsPage.tsx       # AI configuration & LLM parameter tuning
│   └── errors/                # Error fallback pages (403 Forbidden, 404 Not Found, 500 Server Error)
└── App.tsx                    # React Router configuration & query client provider
```

---

## 3. Core Enterprise UX Features

1. **Global Command Palette (`Ctrl+K` / `Cmd+K`)**:
   - Instant keyboard navigation to Dashboard, Documents, Quizzes, Profile, Settings, and AI Generation modal.
2. **Toast Notification System (`sonner`)**:
   - Non-blocking, contextual feedback for uploads, generation steps, and session events.
3. **Optimistic Loading & Skeleton Loaders**:
   - Zero abrupt layout jumps during API data fetching.
4. **Interactive AI Quiz Generation Wizard**:
   - Step-by-step modal showing real-time pipeline states (`QUEUED` $\rightarrow$ `BUILDING_PROMPT` $\rightarrow$ `GENERATING` $\rightarrow$ `VALIDATING` $\rightarrow$ `READY`).
5. **Snapshot Version History & Rollback**:
   - Drawer interface allowing O(1) rollbacks to historical quiz versions.
6. **Dark / Light Glassmorphism Design System**:
   - Theme toggle with smooth ambient background glows and responsive mobile sidebar drawer.

---

## 4. Verification & Build Quality

- **TypeScript Compilation**: `npx tsc --noEmit` passed with 0 errors.
- **ESLint Standard**: `npm run lint` passed with 0 errors and 0 warnings.
- **Production Bundle**: `npm run build` compiled successfully via Vite.
