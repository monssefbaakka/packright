# PackRight GitHub Issues Breakdown

## Epic 1: Project Initialization & Infrastructure

**#1 Initialize Next.js App Repository**
*   **Title:** Initialize Next.js 14+ App Router with Tailwind and TypeScript
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `chore`
*   **Description:** Setup the foundational repository using the standard Next.js scaffold. This ensures we are on the current router standard (App Router), leveraging static typing (TS) and utility-first styling (Tailwind CSS) from day one.
*   **Acceptance Criteria:**
    *   [ ] Next.js project is fully initialized without errors.
    *   [ ] Tailwind CSS is configured and active.
    *   [ ] Default boilerplate is cleared.

**#2 Configure Shadcn UI Components**
*   **Title:** Setup Shadcn UI base theme and components
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `chore`
*   **Description:** Initialize the Shadcn UI library so we can rapidly build accessible, un-styled components that we will customize to match our required earthy, premium look and feel.
*   **Acceptance Criteria:**
    *   [ ] `components.json` is generated with earthy color theme settings.
    *   [ ] Base components (`Button`, `Card`, `Input`, `Dialog`) are installed and functional.
    *   [ ] Custom fonts (Serif header, Sans-serif body) are configured in Tailwind.

**#3 Configure ESLint and Prettier**
*   **Title:** Add strict ESLint and Prettier formatting rules
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `chore`
*   **Description:** Add formatting and linting rules to enforce a standardized code style before any specific feature development begins.
*   **Acceptance Criteria:**
    *   [ ] `.eslintrc.json` is configured strictly for Next.js.
    *   [ ] `.prettierrc` is configured.
    *   [ ] `npm run lint` and `npm run format` scripts are working without errors.

**#4 Set up Vitest and Playwright (TDD framework)**
*   **Title:** Setup Vitest for unit tests and Playwright for E2E flows
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `chore`
*   **Description:** Configure testing environments so we can practice Test-Driven Development (TDD) properly. This issue covers configuring both the unit test runner and browser-automation runner.
*   **Acceptance Criteria:**
    *   [ ] Vitest testing environment is functional.
    *   [ ] Playwright installed natively and base config (`playwright.config.ts`) is generated.
    *   [ ] A dummy test passes for both frameworks to verify setup.

**#5 Setup GitHub Actions CI Pipeline**
*   **Title:** Implement CI pipeline to block failing PRs
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `chore`
*   **Description:** Automate the execution of all tests (linting, formatting, unit, E2E) inside GitHub actions to prevent broken code from ever merging into the `main` branch.
*   **Acceptance Criteria:**
    *   [ ] `.github/workflows/ci.yml` is created.
    *   [ ] Workflow automatically runs ESLint, Vitest, and Playwright headless tests on PR to `main`.
    *   [ ] Failing tests successfully block merge.

**#6 Configure Vercel Deployment**
*   **Title:** Connect GitHub Repository to Vercel continuous deployment
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `chore`
*   **Description:** Setup automated continuous deployment utilizing Vercel's out-of-the-box GitHub integration so we always have a live link.
*   **Acceptance Criteria:**
    *   [ ] Main branch merges trigger Vercel deployment automatically.
    *   [ ] PRs generate a preview URL.

---

## Epic 2: Database & Authentication (Supabase)

**#7 Set up Supabase Project**
*   **Title:** Initialize Supabase database and obtain environment variables
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `chore`
*   **Description:** Create the core Supabase container that will host our PostgreSQL database and handle user authentication flows.
*   **Acceptance Criteria:**
    *   [ ] Supabase project created in dashboard.
    *   [ ] Local `.env.local` configured with `NEXT_PUBLIC_SUPABASE_URL` and keys.

**#8 Configure Supabase Auth Providers**
*   **Title:** Enable Email/Password and Google OAuth Auth in Supabase
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `chore`
*   **Description:** Turn on specific identity providers within the Supabase dashboard and generate the necessary OAuth client keys from Google.
*   **Acceptance Criteria:**
    *   [ ] Email confirmations enabled/disabled appropriately based on environment.
    *   [ ] Google OAuth credentials generated and securely added to Supabase.

**#9 Implement User Auth UI**
*   **Title:** Build Sign Up and Login pages
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Create custom frontend components matching the design specifications that communicate with Supabase to authenticate a user.
*   **Acceptance Criteria:**
    *   [ ] Clean authentication UI utilizing Shadcn.
    *   [ ] User can successfully sign up or log in via Email or Google.
    *   [ ] Written E2E test verifying successful login flow explicitly.

**#10 Implement Profile Creation Flow**
*   **Title:** Require a globally unique `@handle` and Display Name on first login
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Design the onboarding workflow to catch completely new users after login and ensure they establish a recognizable identity (handle and name).
*   **Acceptance Criteria:**
    *   [ ] New accounts are redirected to an onboarding screen.
    *   [ ] Users choose a handle and the system verifies uniqueness against DB before saving.
    *   [ ] Profile table updated with user info successfully.

**#11 Configure Auth Middleware Protection**
*   **Title:** Protect internal application routes using Next.js Middleware
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Ensure that unauthorized or unauthenticated users cannot directly navigate to internal pages (like trips or profiles) by putting up a middleware gate.
*   **Acceptance Criteria:**
    *   [ ] `middleware.ts` successfully inspects Supabase Auth JWT.
    *   [ ] Unauthenticated users attempting to access `/dashboard` get redirected to `/login`.
    *   [ ] Unit test written for middleware logic.

---

## Epic 3: Trips & Member Management (MVP 1 Core)

**#12 Design Trips and Items Database Schema**
*   **Title:** Create DB Tables and Row-Level Security (RLS) policies for Trips and Items
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Build out the SQL schemas mapping trips to their specific database entries, and configure RLS so data does not leak across organizational borders.
*   **Acceptance Criteria:**
    *   [ ] `trips` table created (id, title, destination, date_start, date_end).
    *   [ ] `items` table created (id, trip_id, name, required_count, category).
    *   [ ] RLS enforced strictly so users only see their own trips.

**#13 Create "New Trip" Modal and UI**
*   **Title:** Implement "Create a Trip" feature gathering description
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Create the interface where organizers define the parameters of a brand new trip.
*   **Acceptance Criteria:**
    *   [ ] Shadcn `Dialog` modal allows user to enter trip details (Text prompt, dates).
    *   [ ] Trip data saves successfully to database.
    *   [ ] E2E play-through test confirms successful trip creation.

**#14 Build Trip Dashboard**
*   **Title:** Develop primary Dashboard listing user's active/archived trips
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Build the user's primary landing hub displaying all trips they manage or participate in, styled dynamically as rich cards.
*   **Acceptance Criteria:**
    *   [ ] Dashboard accurately fetches all trips assigned to the logged-in user.
    *   [ ] Trips displayed as responsive card components.

---

## Epic 4: Smart List Generation (GroqAPI)

**#15 Integrate GroqAPI SDK into API Route**
*   **Title:** Setup API Route to communicate with GroqAPI securely
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Ensure the backend can safely speak to the Groq LLM without leaking API secrets to the frontend.
*   **Acceptance Criteria:**
    *   [ ] `GROQ_API_KEY` added to environment.
    *   [ ] App Router API Endpoint (`/api/generate-list`) constructed.

**#16 Write GroqAPI List Generation Logic**
*   **Title:** Implement structured Prompt Engineering to parse trip descriptions into JSON
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Provide logic that takes an end user's raw trip description string, feeds it into an LLM with specific instructions, and expects a reliable, strongly-typed JSON return object of packing list items.
*   **Acceptance Peak Criteria:**
    *   [ ] The prompt reliably accepts the "Trip Description" inputs.
    *   [ ] GroqAPI outputs a strict, pre-defined JSON schema of items and suggested item quantities.
    *   [ ] Unit Test logic by mocking API response and verifying data handling.

**#17 Persist Generated List to Database**
*   **Title:** Save the generated JSON list directly into Supabase Items table
*   **Milestone:** Sprint 1: Trip Organization & Smart List MVP
*   **Labels:** `feature`
*   **Description:** Build the functionality to permanently save the AI's suggestions directly into the user's trip records, associating them correctly via relational ID.
*   **Acceptance Criteria:**
    *   [ ] Once the user confirms the trip, the generated items perform a batch insert connecting to the `trip_id`.
    *   [ ] The UI redirects to the Trip view with items loaded.

==============================================================

## Epic 5: Collaboration & Real-Time Sync

**#18 Design Trip Members Database Schema**
*   **Title:** Create DB Joining Table `trip_members` and expand `items` table claims
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `feature`
*   **Description:** Adjust the schema strictly for multi-player mode. Adding tables to associate many users to one trip, and handling item claims from specific individuals tracking if it's "packed".
*   **Acceptance Criteria:**
    *   [ ] `trip_members` (trip_id, user_id, role) table created.
    *   [ ] RLS policies updated to allow access to users in `trip_members`.
    *   [ ] `item_claims` (item_id, user_id, is_packed) table created to handle multi-contributions.

**#19 Implement User Search and Invitation UI**
*   **Title:** Invite members by searching their global `@handle`
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `feature`
*   **Description:** Allow the organizer to invite additional members to the group by querying their globally unique username identifiers directly from the UI.
*   **Acceptance Criteria:**
    *   [ ] Input field queries Supabase profiles for handles matching text dynamically.
    *   [ ] Clicking user adds them safely to `trip_members`.

**#20 Build Real-Time Packing Board Layout**
*   **Title:** Implement Kanban Board UI (Needed, Claimed, Packed columns)
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `feature`
*   **Description:** Construct the core collaborative UI interface that maps packing list entries visually into distinct zones so users comprehend the current status broadly.
*   **Acceptance Criteria:**
    *   [ ] Items mapped correctly based on their claim/pack state into distinct visual regions.

**#21 Implement Item "Claim" Functionality**
*   **Title:** Allow users to claim items, dynamically decrementing required counts
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `feature`
*   **Description:** Introduce logic enabling a user to verify they intend to bring something, decrementing global requirements to prevent double-packing.
*   **Acceptance Criteria:**
    *   [ ] Clicking "Claim" injects a row into `item_claims`.
    *   [ ] UI moves logic block from "Needed" to "Claimed" based on remaining `required_count`.
    *   [ ] E2E Test covers claiming an item effectively.

**#22 Implement Item "Pack" Functionality**
*   **Title:** Allow users to toggle an item state from 'Claimed' to physically 'Packed'
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `feature`
*   **Description:** Separate the "intent" of claiming an item from the "reality" of packing it physically. This toggles state from promised to effectively handled.
*   **Acceptance Criteria:**
    *   [ ] Database column `is_packed` securely flips to true for the specific user's claim.
    *   [ ] UI updates element visually.
    *   [ ] E2E Testing for complete Packed flow.

**#23 Integrate Supabase Realtime Subscriptions**
*   **Title:** Make the entire board update live without page refreshes
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `feature`
*   **Description:** Utilize Websockets via Supabase Realtime to broadcast changes directly among clients viewing the same trip simultaneously, solving the coordination delay problem.
*   **Acceptance Criteria:**
    *   [ ] Subscribe to Supabase Realtime channels for the specific `trip_id`.
    *   [ ] When User A claims an item, User B immediately sees the change automatically propagated across the UI state.

---

## Epic 6: Readiness Dashboard & Polish

**#24 Implement Group Readiness Logic**
*   **Title:** Calculate readiness metrics dynamically across items
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `feature`
*   **Description:** Mathematically compile exactly how ready a group is based on `(Total Physically Packed) / (Total Required Counts) * 100`.
*   **Acceptance Criteria:**
    *   [ ] Backend or derived frontend state successfully processes all item requirements.
    *   [ ] Math resolves edge cases (0 items required = 100% or hidden entirely).
    *   [ ] Unit test specifically calculating the formula correctly.

**#25 Build Readiness Progress Visualizer UI**
*   **Title:** Display the Group Readiness score visually on Dashboard
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `feature`
*   **Description:** Expose the readiness formula calculation into an explicit, calming progress bar on the Trip dashboard surface to ease anxiety around group readiness.
*   **Acceptance Criteria:**
    *   [ ] Shadcn `Progress` component reflects the Readiness formula visually.

**#26 Final UI/UX Polish and Responsive Design**
*   **Title:** Standardize padding, shadows, and mobile responsiveness
*   **Milestone:** Sprint 2: Real-Time Board & Readiness MVP
*   **Labels:** `chore`
*   **Description:** Final pass over the product checking responsive states across multiple devices and ensuring design alignment with mockup concepts, colors, and shadows.
*   **Acceptance Criteria:**
    *   [ ] Mobile layouts render completely and legibly.
    *   [ ] Colors and fonts rigorously align with `project-memory/mockups` standards.
