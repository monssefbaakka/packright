# PackRight AI Coding Rules

## Project Context
- **Tech Stack**: Next.js 14+ (App Router), TypeScript, Tailwind CSS, Supabase (PostgreSQL, Auth), GroqAPI, Vercel.
- **Architecture**: Next.js App Router with secure backend API routes. Supabase for database, authentication, and Realtime websocket subscriptions to sync state across clients. 
- **Coding Standards**: Strict TypeScript. Formatting enforced by rigorous ESLint and Prettier configurations.
- **Testing Strategy**: Test-Driven Development (TDD) required. Minimum **70% test coverage**. Use Vitest for Unit tests and Playwright for End-to-End (E2E) tests covering critical user flows.

## PRD & Design References
- **PRD Document**: Reference `project-memory/prd.md` for full requirements.
- **Design/Mockups**: Reference `project-memory/mockups`. The UI demands a premium, modern aesthetic with an **earthy color palette** (warm browns, forest greens, soft beiges), rounded containers, and soft shadows. Elegant Serif headers with clean Sans-serif body text.
- **Key UI Components**: Shadcn UI base components customized to match the design palette. Key layouts include the Kanban Packing Board (Needed, Claimed, Packed columns) and the visual Group Readiness progress bar.
- **Expected User Flows**:
  1. Auth -> Required onboarding to claim a globally unique `@handle`.
  2. Create trip setting description -> GroqAPI processes text into structured packing list -> Saved to trip.
  3. Invite others via `@handle` search.
  4. Real-time collaboration: Users "Claim" items (reducing required count), then mark them "Packed" (physically in bag).

## Scrum & Workflow Instructions
- **Reference**: Follow the breakdown in `project-memory/github_issues.md`.
- **Branch Naming**: Use `feature/<issue-number>-<description>` or `bugfix/<issue-number>-<description>`.
- **Commit Format**: Start with the issue number, e.g., `[#42] feat: implement real-time board updates`.
- **PR Workflow**: Open PR against `main`. **Whenever there is any new PR, ensure that the test suite is systematically run and the code coverage is maintained at more than 70%**. Automated CI via GitHub Actions will execute the full suite (linting, Vitest, Playwright). PRs are strictly blocked and cannot be merged if any test fails or if coverage drops below the 70% threshold.
- **Testing Requirements for PRs**: **All PRs must ensure there are E2E and unit tests included**. Any new feature or bug fix submitted in a PR must be accompanied by appropriate End-to-End (E2E) tests to validate user flows and Unit tests to validate isolated logic.
- **Issue Referencing**: Always link the relevant GitHub Issue in your PRs and commits.

## Do's and Don'ts
- **DO** write robust Supabase Row-Level Security (RLS) policies to ensure users only see their own trips.
- **DO** use TDD. Write an E2E test for frontend flows (like login, claiming items) and a Unit test for complex backend logic (like Readiness calculations) alongside the implementation.
- **DO** prioritize Shadcn UI components for scaffolding before writing custom UI elements from scratch.
- **DON'T** expose sensitive keys (e.g., `GROQ_API_KEY`) to the client. Keep them securely in server-side API routes.
- **DON'T** merge code that lowers test coverage below the 70% threshold.
- **DON'T** deviate from the earthy, organic design palette specified in the mockups.
