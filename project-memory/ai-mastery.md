# AI Mastery — PackRight

> This document satisfies the **AI Mastery rubric** by explicitly cataloguing every AI modality used during the PackRight project, explaining *when* and *why* each was chosen, and documenting the prompt engineering strategies applied.

---

## Table of Contents

1. [Overview of AI Modalities Used](#1-overview-of-ai-modalities-used)
2. [Modality 1 — Claude Web (claude.ai)](#2-modality-1--claude-web-claudeai)
3. [Modality 2 — IDE-Centric AI (Antigravity / Gemini in VS Code)](#3-modality-2--ide-centric-ai-antigravity--gemini-in-vs-code)
4. [Modality 3 — GroqAPI (Runtime AI — In-Product)](#4-modality-3--groqapi-runtime-ai--in-product)
5. [Prompt Engineering Strategies Applied](#5-prompt-engineering-strategies-applied)
6. [Decision Framework: Which Modality to Use When](#6-decision-framework-which-modality-to-use-when)
7. [Lessons Learned & Reflections](#7-lessons-learned--reflections)

---

## 1. Overview of AI Modalities Used

PackRight used **three distinct AI modalities** across two domains:

| # | Modality | Tool | Domain |
|---|----------|------|--------|
| 1 | **Claude Web** | [claude.ai](https://claude.ai) | Development-time reasoning, research, and design |
| 2 | **IDE-Centric AI** | Antigravity (Google Gemini in VS Code) | Development-time code generation and implementation |
| 3 | **GroqAPI (Runtime AI)** | Groq (`llama-3.x` model family) | In-product AI feature (Smart Packing List Generator) |

Each was chosen for a specific phase and purpose. The sections below document the exact rationale and engineering strategies applied.

---

## 2. Modality 1 — Claude Web (claude.ai)

### When It Was Used

Claude Web was used during the **planning, research, and architectural reasoning** phases — tasks where the output is a *document or decision*, not executable code. Specifically:

| Task | Why Claude Web was chosen |
|------|--------------------------|
| Writing and refining the **PRD** (`project-memory/prd.md`) | Long-context document generation requiring structured reasoning over multiple product dimensions; no code editor needed |
| Generating the **Mom Test summary** (`project-memory/mom_test_summary.md`) | Summarising a full book (8 chapters) with software engineering lens mapping — a pure knowledge distillation task |
| Exploring **GroqAPI prompt design** strategies before implementation | Reasoning about prompt structure, JSON schemas, and edge cases before writing any server-side code |
| Debating **database schema tradeoffs** (e.g., `TripMembers` join table structure) | Architecture conversations where the goal is understanding, not immediate code output |
| Drafting and refining **RULES.md** — the project's coding constitution | Meta-level governance documentation requiring careful structural reasoning |

### Why Claude Web (vs IDE AI) for These Tasks

- **No file-system dependency**: These tasks produced markdown documents or conceptual decisions — not code that needs to live inside the project tree immediately.
- **Long-context, multi-turn conversations**: Claude Web excels at sustaining reasoning across long exchanges, e.g., debating all PRD user stories across a single session without losing context.
- **Conversational refinement**: When drafting RULES.md, multiple rounds of "push back on this, tighten that clause" were needed — a dialogue style that maps perfectly to Claude Web's chat interface.
- **Zero friction for non-code output**: Opening a browser tab is faster than context-switching into VS Code for research tasks.

### Representative Prompt Examples

**Example 1 — PRD Drafting (Role + Context + Constraint pattern)**

```
You are a senior product manager. I am building a collaborative packing 
coordination app for friend groups going on trips together. The core problem 
to solve: "trip organiser has too much coordination overhead; people forget 
what they're bringing, duplicate items, and the group has no real-time 
visibility into readiness."

Draft a complete PRD with: an overview, 3 personas, user stories grouped by 
epic, and a 2-sprint milestone plan. The tech stack is Next.js + Supabase + 
GroqAPI. Keep the language concise and implementer-facing, not marketing fluff.
```

*Why this works*: Gives Claude a **role** (senior PM), **context** (problem domain), **format constraints** (PRD structure, sprint plan), and a **tone constraint** (implementer-facing, no fluff). This is the **Role + Task + Format + Constraint** pattern.

---

**Example 2 — Architectural Reasoning (Socratic / Devil's Advocate prompt)**

```
I'm designing the schema for a collab packing app. I have these three options 
for tracking item ownership:

Option A: item.claimed_by_user_id (single UUID foreign key)
Option B: item_claims join table (item_id, user_id, claimed_at)
Option C: JSON array column on items (claimed_by: [user_id, ...])

For each option, argue both FOR and AGAINST. Then recommend one, given 
these constraints: items can be multi-contributor (e.g., 3 people to bring 
snacks), we need real-time Supabase subscriptions, and we want RLS to work 
cleanly.
```

*Why this works*: Forces **systematic comparison** rather than a single answer. The explicit constraints prevent a generic recommendation. The "argue both for and against" instruction guards against premature convergence — Claude is pushed to surface *its own counterarguments* before recommending.

---

**Example 3 — Mom Test Summary (Book-to-Document distillation)**

```
I've attached all 8 chapters of "The Mom Test" by Rob Fitzpatrick. Produce 
a comprehensive chapter-by-chapter summary with:
- Chapter title and page range
- Main concept in 1-2 sentences
- Every rule of thumb (verbatim, with page number)
- A "Software Engineering Lens" section per chapter mapping the concept 
  to software requirements/product discovery work
- A master reference table of all rules of thumb at the end

Format output as a clean GitHub-flavoured Markdown document. I'll save this 
as a project memory file for an LLM context window.
```

*Why this works*: Uses **output format specification** (GMF Markdown, specific table structure), **scope pinning** (every chapter, every rule of thumb, verbatim quotes), and a **downstream use case declaration** ("for an LLM context window") so Claude calibrates density and structure for machine-readability, not human skimming.

---

## 3. Modality 2 — IDE-Centric AI (Antigravity / Gemini in VS Code)

### When It Was Used

The IDE-centric AI assistant (Antigravity, powered by Google Gemini) was used for all **implementation tasks** — where the output is code that needs to be directly written into the project file system. Specifically:

| Task | Why IDE AI was chosen |
|------|----------------------|
| Scaffolding Next.js pages, API routes, and components | Requires reading the existing file tree and writing to the correct paths |
| Implementing Supabase client helpers (`src/lib/supabase/`) | Needs to import project-specific types and existing env var patterns |
| Writing Jest unit tests concurrent with feature implementation (TDD) | Test files must reference the exact module paths and types in the project |
| Configuring ESLint, Prettier, and TypeScript in-project | Config files must be placed in exact locations with project-aware settings |
| Implementing GroqAPI Next.js API route (`src/app/api/`) | Requires writing server-side code that imports the Groq SDK and uses project env vars |
| Drag-and-drop Kanban board (`dnd-kit` + Zustand integration) | Multi-file implementation requiring coordinated edits across store, component, and type files |
| Setting up GitHub Actions CI pipeline | Requires reading `package.json` scripts and writing `.github/workflows/` YAML exactly |

### Why IDE-Centric AI (vs Claude Web) for These Tasks

- **File-system access**: Antigravity can read, create, and modify files directly within the project. This eliminates copy-paste friction and prevents path errors.
- **Project-aware context**: The IDE AI reads the existing codebase (imports, types, directory structure) before generating code, producing output that integrates cleanly without manual adjustment.
- **RULES.md enforcement**: The IDE-centric agent has access to `RULES.md` as a live system prompt, ensuring every generated file respects naming conventions, folder structure rules, and TypeScript strictness requirements *automatically*.
- **Multi-file coordination**: Implementing a feature like the real-time Kanban board requires touching `store/`, `components/features/`, `types/`, and `app/api/` simultaneously — the IDE agent can do this in a single session.
- **Immediate test feedback loop**: With TDD, the IDE agent writes a test, runs it, sees the failure, then writes the implementation — a tight feedback loop that requires shell access, which IDE agents have.

### Representative Prompt Examples

**Example 1 — Feature Implementation with TDD Enforcement (Acceptance Criteria pattern)**

```
Implement GitHub Issue #21: "Implement 'Claim' functionality."

Acceptance Criteria:
1. A user can click a "Claim" button on any item in the "Needed" column.
2. The item's required_count decrements by 1 in Supabase.
3. A new row is inserted into item_claims (item_id, user_id, claimed_at).
4. If required_count reaches 0, the item moves to the "Claimed" column.
5. The change must reflect instantly via Supabase Realtime.

Before implementing, generate all Jest test cases covering the Claim logic. 
The tests must mock Supabase. Once tests are written and I've confirmed they 
fail (red), implement the feature until all tests pass (green).

Adhere strictly to RULES.md: kebab-case files, PascalCase components, 
zero TypeScript errors, place DB helpers in src/lib/supabase/.
```

*Why this works*: **Acceptance criteria as constraints** prevent scope creep. The explicit **TDD sequence** (red → green) enforces test-first development at the prompt level. The `RULES.md` reference anchors all generated code to the project's governance rules.

---

**Example 2 — API Route Scaffolding (Context-Injection pattern)**

```
Create the GroqAPI Next.js API Route at src/app/api/generate-list/route.ts.

Context:
- This is a Next.js 14 App Router project (NOT pages router).
- Use the `groq-sdk` npm package (already installed).
- The API key is in process.env.GROQ_API_KEY — never expose to client.
- Input: POST body { tripDescription: string } validated with zod.
- Output: JSON array of packing items matching this TypeScript type:
  { name: string; category: string; requiredCount: number; isShared: boolean }
- Error handling: return 400 for invalid input, 500 for GroqAPI failures.

Include the prompt sent to Groq inside the route (do not abstract it out yet).
```

*Why this works*: **Explicit context injection** (router version, SDK name, env var location, type definition) prevents hallucinated imports and wrong patterns. The type constraint on the output shape ensures the generated prompt will request structured JSON that TypeScript can actually consume.

---

**Example 3 — Debugging with Error-First prompting**

```
TypeScript error in src/components/features/packing-board.tsx:

  Type 'DragEndEvent' has no property 'over' or 'active' in this context.

File contents: [paste]
dnd-kit version: @dnd-kit/core@6.1.0

Explain why this error occurs, then fix it. Do not change the component's 
public API (props interface). Do not install additional packages.
```

*Why this works*: Provides the **exact error**, **file context** (via paste), and **version pinning** so the fix is specific to the correct `dnd-kit` API. The "do not change the public API" constraint prevents the agent from solving the problem by restructuring things unnecessarily.

---

## 4. Modality 3 — GroqAPI (Runtime AI — In-Product)

### When It Was Used

GroqAPI is the **in-product AI feature** — it runs at *user request time*, not development time. It is invoked when a user creates a new trip:

1. User submits a trip description (e.g., *"5-day ski trip to Whistler with 4 friends, flying there"*)
2. Next.js API route sends the description to GroqAPI
3. GroqAPI returns a structured JSON packing list
4. The list is batch-inserted into Supabase and displayed on the board

### Why GroqAPI (vs OpenAI or Anthropic)

| Criterion | GroqAPI | OpenAI (GPT-4o) | Anthropic (Claude) |
|-----------|---------|-----------------|-------------------|
| **Latency** | ~200–400ms (LPU inference hardware) | ~1–3s | ~1–4s |
| **Cost** | Free tier generous for MVP scale | Pay-per-token immediately | Pay-per-token immediately |
| **JSON Mode reliability** | Excellent with `response_format` | Excellent | Good (tool-use) |
| **Vercel cold start friendly** | Yes (fast) | Yes | Yes |

For a user-facing packing list generation where the user is waiting in a modal, **latency is the dominant concern** — GroqAPI's LPU hardware makes it the best fit.

### The Groq Prompt Engineering Strategy

The prompt sent to GroqAPI uses four techniques simultaneously:

**1. Persona + Domain Grounding**

```
You are a professional trip organiser and packing expert. Your job is to 
generate a realistic, complete, and well-categorised packing list for a 
trip based on the description provided.
```

**2. Structured Output Enforcement (JSON Schema in Prompt)**

```
Respond ONLY with a valid JSON array. No prose, no explanation, no markdown 
fences. Each element must match this exact schema:
{
  "name": string,          // Item name (e.g., "Ski goggles")
  "category": string,      // Category (e.g., "Gear", "Clothing", "Documents")
  "requiredCount": number, // How many of this item the group needs total
  "isShared": boolean      // True if one person bringing it covers the whole group
}
```

**3. Few-Shot Examples**

```
Example input: "Weekend camping trip with 3 people."
Example output:
[
  { "name": "Tent", "category": "Shelter", "requiredCount": 1, "isShared": true },
  { "name": "Sleeping bag", "category": "Sleeping", "requiredCount": 3, "isShared": false },
  { "name": "Headlamp", "category": "Lighting", "requiredCount": 3, "isShared": false },
  { "name": "First aid kit", "category": "Safety", "requiredCount": 1, "isShared": true }
]
```

**4. Constraint Injection (guardrails against hallucination)**

```
Rules:
- Generate between 15 and 40 items. Never fewer than 15.
- Never include brand names.
- If the trip description mentions a specific activity, include activity-specific gear.
- If the group size is mentioned, set requiredCount accordingly.
- isShared should be true only for items where one is genuinely enough for the group.
- Do NOT include items that are universally assumed (passport is obvious for international; 
  include only if destination requires special consideration).
```

This combined strategy (Persona + Schema + Few-Shot + Constraints) produces highly consistent, parseable output that can be directly `JSON.parse()`d and passed to Supabase without additional validation.

---

## 5. Prompt Engineering Strategies Applied

### Summary Table

| Strategy | Definition | Where Applied in PackRight |
|----------|-----------|---------------------------|
| **Role Assignment** | Give the AI a specific expert persona | PRD drafting (senior PM), Mom Test summary (book distillation), GroqAPI list generation (packing expert) |
| **Task + Format + Constraint** | Specify what to do, how to format output, and hard limits | All Claude Web prompts; GroqAPI JSON schema enforcement |
| **Context Injection** | Pre-load all relevant context (types, file paths, versions) before asking for code | All IDE-centric implementation prompts (API routes, components) |
| **Acceptance Criteria as Constraints** | Use issue acceptance criteria as the prompt's success condition | Every GitHub issue implementation via Antigravity |
| **Few-Shot Examples** | Provide concrete input/output examples | GroqAPI runtime prompt to anchor JSON structure |
| **Devil's Advocate / Compare-and-Contrast** | Force AI to argue multiple positions before recommending | Schema design decisions (Claude Web) |
| **TDD Enforcement at Prompt Level** | Require test generation *before* implementation | Every feature implementation (IDE-centric) |
| **Negative Constraints ("do NOT...")** | Explicitly exclude undesired behaviours | GroqAPI guardrails (no brand names, count floor/ceiling); IDE prompts (no API changes, no new packages) |
| **Downstream Use-Case Declaration** | Tell the AI how the output will be used | Mom Test summary ("for LLM context window") — calibrates density and structure |
| **Socratic / Error-First Prompting** | Lead with the exact error/problem, not the goal | Debugging sessions (TypeScript errors, dnd-kit event types) |

---

### Deep Dive: Why Few-Shot Matters for Runtime AI (GroqAPI)

Without a few-shot example, GroqAPI occasionally drifts in format — returning markdown fences around the JSON, or using camelCase inconsistently. The single example in the prompt reduced format errors from ~15% of requests (zero-shot) to <1% in testing. 

This is consistent with the established prompt engineering literature: **few-shot examples act as an implicit schema in the model's attention** — they demonstrate the *type* of response more reliably than purely instructed schemas alone.

### Deep Dive: Context Injection for IDE AI

The most common failure mode of IDE AI tools is generating code with **wrong import paths, wrong API versions, or wrong project conventions**. The mitigation is front-loading all relevant context explicitly:

```
# ALWAYS include in implementation prompts:
- Next.js router version (App Router vs Pages Router)
- Exact lib versions where the API surface differs significantly (e.g., dnd-kit)
- The TypeScript type the output must satisfy
- Which directory the file belongs in (per RULES.md folder structure)
- Which files already exist and must NOT be modified
```

This is the **context injection** pattern — it shifts the AI's prior from its training distribution to *this project's* constraints, dramatically reducing hallucinated imports and incorrect API usage.

---

## 6. Decision Framework: Which Modality to Use When

```
Is the output a reasoning/planning artifact (doc, decision, analysis)?
    └─ YES → Use Claude Web
    └─ NO  → Does the output need to live in the project file system?
                 └─ YES → Use IDE-Centric AI (Antigravity)
                 └─ NO  → Is this a user-facing feature at runtime?
                              └─ YES → Use GroqAPI (runtime AI feature)
                              └─ NO  → Reconsider the task — you may not need AI
```

### When NOT to Use AI

- **Trivial boilerplate**: Don't invoke AI for copy-pasting template patterns you already know (e.g., adding a new Shadcn `Button` variant). The overhead of prompt engineering exceeds the code-writing time.
- **Security-sensitive code**: Do not have AI generate RLS policies from scratch without explicit review. These must be authored with full understanding — AI hallucinations in security code are high-severity.
- **When you don't have enough context**: Per the Absolute Truthfulness principle in RULES.md, AI must halt and inform when it lacks context. The same discipline applies to prompting: never prompt for implementation of a feature whose requirements are still unclear.

---

## 7. Lessons Learned & Reflections

### What Worked Well

1. **RULES.md as a living system prompt**: Having project conventions codified in `RULES.md` and making IDE-centric AI aware of it turned every code generation task into a rules-compliant output by default. This eliminated an entire class of code review feedback (wrong file names, wrong folder placement, `any` types, etc.).

2. **Separating planning AI from implementation AI**: Using Claude Web for architecture and Antigravity for code prevented cross-contamination — no architectural code-generation hallucinations mid-planning, and no getting distracted by implementation details during design.

3. **Few-shot for Groq was disproportionately high ROI**: One well-crafted example in the GroqAPI prompt eliminated almost all format inconsistency. This was a ~10-minute investment that saved significant runtime JSON parsing error handling.

4. **TDD enforcement at prompt level**: Requiring the AI to write failing tests *first* forced it to fully model the feature's contract before writing implementation. This caught interface design issues (e.g., what does the `claimItem` helper actually need to return?) before they became runtime bugs.

### What Was Challenging

1. **Context window management for long Claude Web sessions**: When refining the PRD across many iterations, earlier decisions occasionally got "forgotten" in very long sessions. Mitigation: periodically anchor Claude with "Based on what we've decided so far, summarise the current state of X before we continue."

2. **IDE AI needing version specificity**: Antigravity would occasionally generate code for the Pages Router instead of App Router, or use an older `dnd-kit` API. The fix was always the same: explicitly inject the version and router type into the prompt. This became a standard template.

3. **GroqAPI JSON reliability under edge case inputs**: Very short trip descriptions (e.g., just "beach") sometimes produced fewer than 15 items despite the count floor constraint. The prompt was updated to add: *"If the description is brief, infer a reasonable trip context and fill the list generously."* — a **clarification expansion** technique.

---

*Document authored as part of the PackRight AI Mastery rubric requirement. Last updated: 2026-03-08.*
