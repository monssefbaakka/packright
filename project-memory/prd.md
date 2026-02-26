# PackRight Product Requirements Document (PRD)

## 1. Overview & Objective
PackRight is a collaborative web application designed to help friend groups effortlessly coordinate packing for trips. It eliminates the standard chaos of WhatsApp coordination by providing a shared board where members can see the group's packing list, claim items, and track in real-time what has actually been packed. 

The primary goal is to shift the coordination burden off the "organizer", prevent duplicate packing, ensure no necessary items are forgotten, and foster trust within the group through transparent readiness tracking.

## 2. Target Audience & Personas

Based on early user interviews, our primary users fall into three main personas:

### 1. The Reluctant Organizer
*   **Background:** Often falls into the default planner role. Builds lists from scratch, herds people, and manually follows up.
*   **Pain Points:** Time spent building lists; friction in adding people to chats (duplicate names, missing numbers); unclaimed items; zero visibility into actual packing status.
*   **Goal:** Spin up a trip instantly, add friends via simple handles, and have the system automatically track who is bringing what without manual follow-up.

### 2. The Casual Participant
*   **Background:** Happy to contribute but prefers a minimal role. Often lacks visibility into group progress.
*   **Pain Points:** Duplicate packing (bringing what someone else already packed); missing essential shared items; feeling disconnected from the group's overall progress.
*   **Goal:** Easily see what is needed, claim it, and signal when it is packed without getting buried in a chat thread.

### 3. The Anxious Over-Packer
*   **Background:** Lacks trust in the group's verbal commitments and overpacks to compensate.
*   **Pain Points:** People saying they will bring an item and forgetting; stress from lack of reliable information; carrying unnecessary weight.
*   **Goal:** A source of truth that differentiates between "I will bring this" and "This is physically in my bag," and a group readiness dashboard to provide peace of mind.

## 3. User Stories

### Trip Creation & User Management
*   **As an Organizer**, I want to create a new trip by specifying the destination and duration, so that I have a dedicated space for coordination.
*   **As an Organizer**, I want to generate a starting packing list based on the trip details, so I don't have to think of everything from scratch.
*   **As a User**, I want to have a unique username/handle, so my friends can search and add me to trips without needing my phone number.
*   **As a User**, I want to see recognizable profiles (names/photos) of all trip members, so I know exactly who is responsible for what.

### The Packing Board
*   **As a User**, I want to view a centralized packing board, so I can see what items are needed, claimed, and completely packed.
*   **As a User**, I want to claim an item from the "needed" list, so others know I plan to bring it.
*   **As a User**, I want to explicitly mark an item as "Packed" (in my bag), so the group knows it's actually ready, not just claimed.
*   **As an Organizer**, I want to clearly see which items are unclaimed, so we can ensure there are no gaps.
*   **As a User**, I want to contribute to "shared items" (e.g., snacks) by claiming slots, so we know when we have enough contributors.

### Dashboard & Readiness
*   **As an Anxious Over-Packer**, I want to see a real-time "Group Readiness" percentage, so I can feel confident leaving duplicates at home.

## 4. Key Features & Functionality
1.  **User Authentication & Profiles:** Sign up via Google OAuth or Email/Password, create globally unique handles (for easy searching without phone numbers), add profile pictures.
2.  **Trip Dashboard:** Create trips (asking organizer for a trip description), invite members via global handle search.
3.  **Smart List Generation:** Call the GroqAPI with the trip description to interpret and automatically generate a tailored starting packing list.
4.  **The Packing Board:**
    *   **Needed:** Items required but not yet claimed. Displays a required count for multi-contributor items. When a user claims it, the count reduces by 1. Once the required count is met, it moves out of the unassigned/needed list.
    *   **Claimed:** Items a user has committed to bringing.
    *   **Packed:** Items physically packed in a bag.
5.  **Multi-Contributor Items:** Capability to set an item to require N people (e.g., 3 people to bring snacks).
6.  **Real-Time Sync:** Everything updating live so no one is looking at stale data.
7.  **Readiness Analytics:** Visual indicators of how ready the group is. The calculation is `Total items physically packed / Total required item counts * 100`. (Example: Item A needs 3 people, Item B needs 2 people -> Total required = 5. If 2 people packed Item A and 1 person packed Item B, Readiness = 3/5 = 60%).

## 5. Technology Stack & Engineering Standards
*   **Frontend & Backend:** Next.js (App Router). Online-only web app.
*   **Database & Auth:** Supabase (PostgreSQL), utilizing Email/Password and Google OAuth.
*   **Styling & UI/UX Standards:** Tailwind CSS combined with Shadcn UI. The project demands the **highest UI/UX standards**, drawing inspiration from the `project-memory/mockups`. This includes a premium, modern aesthetic with an earthy color palette (warm browns, forest greens, soft beiges), rounded containers, subtle soft shadows, and sophisticated typography (elegant Serif headers paired with clean Sans-serif body text).
*   **AI List Generation:** GroqAPI.
*   **Hosting/Deployment:** Vercel.

**Engineering Standards:**
*   **Code Quality:** ESLint and Prettier configured and enforced.
*   **Testing Philosophy:** Test-Driven Development (TDD). Target: Minimum **70% test coverage**.
    *   *Unit Tests:* Vitest or Jest.
    *   *End-to-End (E2E) Tests:* Playwright for critical user flows (login, creating a trip, claiming an item).
*   **CI/CD:** Automated checks on Pull Requests running linting, formatting, and unit/E2E tests before allowing merges. Continuous Deployment via Vercel.

## 6. Project Management & Workflow
*   **Methodology:** Scrum / Agile.
*   **Tracking:** GitHub Issues (organized into Milestones).
*   **Structure:** Development is divided into two primary Sprints/Milestones spanning evenly from today until March 10, 2026. Each sprint will deliver a functional, tested Minimum Viable Product (MVP). Every user story translates directly into one or more GitHub Issues detailed below.

### Sprint 1: Trip Organization & Smart List MVP
*   **Deadline:** March 3, 2026, 11:59 PM EST (Halfway Mark)
*   **Goal:** A functional web app where users can log in, create a trip, and automatically generate a packing list using AI.

#### Epic 1: Project Initialization & Infrastructure
1.  **Issue:** Initialize Next.js app with `create-next-app` (App Router, Tailwind CSS, TypeScript).
2.  **Issue:** Install and configure Shadcn UI components.
3.  **Issue:** Set up ESLint and Prettier for strict code quality formatting.
4.  **Issue:** Set up Vitest (Unit Tests) and Playwright (E2E Tests) to enable TDD.
5.  **Issue:** Configure GitHub Actions CI pipeline to run linting, Vitest, and Playwright automatically on Pull Requests.
6.  **Issue:** Configure continuous deployment pipeline to Vercel.

#### Epic 2: Database & Authentication (Supabase)
7.  **Issue:** Set up Supabase project (Database and Auth settings).
8.  **Issue:** Configure Email/Password and Google OAuth providers in Supabase.
9.  **Issue:** Implement User Auth UI (Sign Up / Login pages) with form validation. *(Requires E2E test coverage)*.
10. **Issue:** Implement Profile Creation flow requiring a globally unique `@handle` and display name.
11. **Issue:** Implement Next.js Middleware to protect authenticated routes. *(Requires Unit test coverage)*.

#### Epic 3: Trips & Member Management (MVP 1 Core)
12. **Issue:** Design and implement Database Schema for `Trips` table and `Items` table.
13. **Issue:** Create UI for "Create a Trip" (Prompting for trip description for GroqAPI). *(Requires E2E test coverage)*.
14. **Issue:** Build the main Trip Dashboard layout.

#### Epic 4: Smart List Generation (GroqAPI)
15. **Issue:** Integrate GroqAPI SDK into a Next.js API route.
16. **Issue:** Write robust prompt engineering to parse the trip description into a structured JSON packing list. *(Requires Unit test coverage mocking the API)*.
17. **Issue:** Save GroqAPI generated list items directly into the Supabase `Items` table connected to the active Trip.

*   **Sprint 1 Delivery & Verification:**
    *   Verify E2E auth and trip creation flows pass locally and in CI.
    *   Ensure >70% test coverage threshold is met via coverage reports.
    *   Confirm functional deployment on Vercel as **MVP 1**.

---

### Sprint 2: Real-Time Board & Readiness MVP
*   **Deadline:** March 10, 2026, 11:59 PM EST (Final Deadline)
*   **Goal:** Add real-time collaboration, allowing invited members to view the board, claim items, mark them as packed, and see the group readiness score.

#### Epic 5: Collaboration & Real-Time Sync
18. **Issue:** Design Database Schema for `TripMembers` joining table.
19. **Issue:** Implement Handle-based User Search UI to invite members to a trip.
20. **Issue:** Build the Real-Time Packing Board UI (Columns: Needed, Claimed, Packed).
21. **Issue:** Implement "Claim" functionality (reducing item required count). *(Requires E2E test coverage)*.
22. **Issue:** Implement "Pack" functionality (marking as physically packed). *(Requires E2E test coverage)*.
23. **Issue:** Integrate Supabase Realtime so state changes reflect instantly across all connected clients without refreshing.

#### Epic 6: Readiness Dashboard & Polish
24. **Issue:** Implement the Group Readiness Calculation logic (`Total physically packed / Total required counts * 100`). *(Requires Unit test coverage)*.
25. **Issue:** Build to a visual progress bar / widget for Group Readiness on the Trip Dashboard.
26. **Issue:** Conduct final UI/UX mobile-responsive polish across the app using Shadcn.

*   **Sprint 2 Delivery & Verification:**
    *   Verify real-time event updates across multiple browser sessions simultaneously.
    *   Verify all newly added issues meet the >70% test coverage minimum.
    *   Confirm final functionality of **MVP 2** deployed on Vercel.
