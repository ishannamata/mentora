# Mentora — AI-Powered Career Coaching Platform

> Personalized career guidance powered by LLMs and Machine Learning. Built for students and professionals who want data-driven clarity on their career path.

🔗 **Live Demo:** [mentora-aicareercoach.vercel.app](https://mentora-aicareercoach.vercel.app)
📦 **ML Service:** [github.com/ishannamata/ml-service](https://github.com/ishannamata/ml-service)

---

## What is Mentora?

Mentora is a full-stack AI career coaching platform that helps users identify skill gaps, practice interviews, build resumes, and get personalized career guidance — all in one place.

It combines a custom Machine Learning pipeline (TF-IDF + logistic regression) with LLM integration (Claude) to deliver recommendations that are both data-driven and conversationally intelligent.

---

## Features

- **Skill Gap Analysis** — Users complete assessments; a TF-IDF vectorization + logistic regression model evaluates performance against target roles and surfaces specific skill gaps with actionable recommendations.
- **AI Career Chatbot** — Conversational LLM-powered assistant that answers career questions, suggests learning paths, and guides users through decisions.
- **Resume Builder** — Dynamic resume generation tool tailored to the user's skills, experience, and target roles.
- **Mock Interview Simulator** — AI-generated interview questions based on the user's target role; includes response evaluation and feedback.
- **Dynamic Quizzes** — Adaptive quizzes generated per-topic to continuously assess user knowledge and track improvement over time.
- **Personalized Dashboard** — Progress tracking across all modules, skill improvement trends, and recommended next actions.

---

## Tech Stack

### Frontend
| Tech | Purpose |
|---|---|
| Next.js 14 (App Router) | Full-stack React framework with SSR |
| React.js | UI library |
| Tailwind CSS | Utility-first styling |
| Shadcn/UI | Component library |

### Backend & Infrastructure
| Tech | Purpose |
|---|---|
| Next.js API Routes | REST API endpoints |
| PostgreSQL | Primary relational database |
| Prisma ORM | Type-safe database client & migrations |
| Inngest | Background job processing & event-driven workflows |
| Gemini | LLM for chatbot, interview, resume, and quiz generation |

### Machine Learning Service (separate repo)
| Tech | Purpose |
|---|---|
| Python | ML service runtime |
| TF-IDF Vectorization | Text feature extraction for skill gap analysis |
| Logistic Regression | Role-fit classification model |
| REST API | Exposes ML predictions to the Next.js backend |

---

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Next.js Frontend                     │
│         (App Router · Tailwind CSS · Shadcn/UI)         │
└────────────────────────┬────────────────────────────────┘
                         │ API Routes
┌────────────────────────▼────────────────────────────────┐
│                  Next.js API Layer                      │
│            (Auth · REST Endpoints · Inngest)            │
└──────┬────────────────┬────────────────┬────────────────┘
       │                │                │
┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐
│ PostgreSQL  │  │  Gemini LLM │  │  ML Service │
│ + Prisma    │  │             │  │  (Python)   │
└─────────────┘  └─────────────┘  └─────────────┘
```

---

## Getting Started

### Prerequisites

- Node.js ≥ 18.x
- PostgreSQL (local or hosted e.g. Railway, Neon)
- Python ≥ 3.9 (for ML service)
- Anthropic API Key

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/ishannamata/mentora.git
cd mentora
```

2. **Install dependencies**
```bash
npm install
```

3. **Set up environment variables**

Create a `.env.local` file in the root directory:
```env
DATABASE_URL=postgresql://user:password@localhost:5432/mentora
ANTHROPIC_API_KEY=your_anthropic_api_key
INNGEST_EVENT_KEY=your_inngest_event_key
INNGEST_SIGNING_KEY=your_inngest_signing_key
ML_SERVICE_URL=http://localhost:8000
NEXTAUTH_SECRET=your_nextauth_secret
NEXTAUTH_URL=http://localhost:3000
```

4. **Run database migrations**
```bash
npx prisma migrate dev
npx prisma generate
```

5. **Start the ML service** (see [ml-service repo](https://github.com/ishannamata/ml-service))

6. **Run the development server**
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## Project Structure

```
mentora/
├── app/                    # Next.js App Router
│   ├── (auth)/             # Auth pages (login, signup)
│   ├── dashboard/          # User dashboard
│   ├── interview/          # Mock interview module
│   ├── quiz/               # Adaptive quiz module
│   ├── resume/             # Resume builder
│   ├── chat/               # AI career chatbot
│   └── api/                # API route handlers
│       ├── auth/
│       ├── inngest/        # Inngest event handlers
│       └── ml/             # ML service proxy
├── components/             # Reusable React components
├── lib/                    # Utility functions & config
│   ├── prisma.js           # Prisma client instance
│   ├── anthropic.js        # Claude LLM client
│   └── inngest.js          # Inngest client
├── hooks/                  # Custom React hooks
├── actions/                # Next.js server actions
├── data/                   # Static data & constants
├── prisma/
│   └── schema.prisma       # Database schema
└── public/                 # Static assets
```

---

## Key Technical Decisions

**Why TF-IDF + Logistic Regression for skill gap analysis?**
The ML model compares a user's quiz/assessment responses against a corpus of role-specific competency descriptions. TF-IDF surfaces the most relevant skill signals from free-text inputs, while logistic regression provides probabilistic role-fit scores — giving users clear, explainable feedback rather than a black-box output.

**Why Inngest for background jobs?**
Resume generation and detailed skill analysis can be slow LLM calls. Inngest decouples these from the request lifecycle, enabling real-time UI updates via events without blocking API responses or hitting timeout limits on serverless functions.

**Why Prisma + PostgreSQL?**
Relational data (users, assessments, quiz results, resume drafts) benefits from structured schemas and foreign key integrity. Prisma adds type safety across the entire data layer, reducing runtime errors significantly.

---

## Deployment

The frontend is deployed on **Vercel** with automatic CI/CD on push to `main`.

The ML service is designed to be deployed on **Railway** as a standalone Python service.

Database is hosted on **Neon** (serverless PostgreSQL).

---

## Related Repositories

| Repo | Description |
|---|---|
| [mentora](https://github.com/ishannamata/mentora) | Main Next.js application (this repo) |
| [ml-service](https://github.com/ishannamata/ml-service) | Python ML service — TF-IDF skill gap analysis |

---

## Author

**Ishan Namata**
- GitHub: [@ishannamata](https://github.com/ishannamata)
- Email: ishannamata65@gmail.com
- LinkedIn: [ishannamata](https://linkedin.com/in/ishannamata)

---

## License

This project is for portfolio and academic purposes.
