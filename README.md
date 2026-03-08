# PackRight

**PackRight** is a real-time collaborative packing coordination app for friend groups. It eliminates the chaos of WhatsApp group coordination by providing a shared packing board where members can claim items, mark them as packed, and see the group's readiness in real time.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend & Backend | Next.js 14 (App Router), TypeScript (Strict) |
| Styling & UI | Tailwind CSS v3, Shadcn UI, Framer Motion |
| Database & Auth | Supabase (PostgreSQL, Realtime, Email/OAuth) |
| AI List Generation | GroqAPI (`llama-3` family) |
| State Management | Zustand |
| Drag & Drop | dnd-kit |
| Forms & Validation | react-hook-form + zod |
| Testing | Jest (unit), Playwright (E2E) |
| Hosting | Vercel |

---

## Getting Started

### Prerequisites

- Node.js 18+
- A Supabase project (with Auth and Realtime enabled)
- A GroqAPI key

### Environment Variables

Create a `.env.local` file in the project root:

```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
GROQ_API_KEY=your_groq_api_key
```

> ⚠️ **Never expose `GROQ_API_KEY` to the client.** It must only be used in server-side API routes.

### Running Locally

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to see the app.

### Running Tests

```bash
# Unit tests (Jest)
npm run test

# Unit tests with coverage report
npm run test:coverage

# E2E tests (Playwright)
npx playwright test
```

A minimum of **70% test coverage** is enforced in CI.

---

## Project Structure

```
src/
  app/          # Next.js App Router pages, layouts, and API routes
  components/
    ui/         # Base Shadcn UI components
    layout/     # Navbar, Sidebar
    features/   # Domain-specific components (Packing Board, Trip Modal, etc.)
  lib/
    supabase/   # Supabase client and DB query helpers
    utils.ts    # Tailwind cn() merger and general utilities
  store/        # Zustand client-side state stores
  types/        # Global TypeScript types and Zod schemas
tests/
  e2e/          # Playwright end-to-end test specs
```

---

## Project Memory

The `project-memory/` directory contains living documentation for the project:

| File | Purpose |
|------|---------|
| [`prd.md`](./project-memory/prd.md) | Full Product Requirements Document |
| [`ai-mastery.md`](./project-memory/ai-mastery.md) | **AI Mastery documentation** — modalities used, prompt engineering strategies, and concrete examples |
| [`mom_test_summary.md`](./project-memory/mom_test_summary.md) | Comprehensive summary of *The Mom Test* mapped to software engineering |

---

## AI Usage

PackRight used AI assistance throughout development. For a full breakdown of **which AI tools were used, when, why, and with what prompt engineering strategies**, see:

📄 **[`project-memory/ai-mastery.md`](./project-memory/ai-mastery.md)**

This document covers:
- **Claude Web vs. IDE-Centric AI (Antigravity)** — the decision framework for when to use each
- **GroqAPI prompt engineering** for the Smart List Generator feature (the in-product runtime AI)
- **Specific prompt templates** used at each phase and why they work
- **Prompt engineering strategies**: Role Assignment, Context Injection, Few-Shot, TDD enforcement, Negative Constraints, and more
- **Lessons learned** from AI-assisted development across a full sprint cycle

---

## Deployment

PackRight deploys automatically to **Vercel** from the `main` branch via CI/CD. Every Pull Request runs linting, Jest unit tests, and Playwright E2E tests before merging is allowed.

---

## Contributing

See [`RULES.md`](./RULES.md) for the full coding standards, branching conventions, TDD requirements, and Scrum workflow. All contributions must adhere to those rules.
