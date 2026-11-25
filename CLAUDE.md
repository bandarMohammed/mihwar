# Mihwar (محور)

> **Current phase: 3 — development (2026-11-24 → 2026-12-12). Plan, groups and hand-offs: `reference/phase-3/phase-3-plan.md`. Build only the tasks in `docs/tasks.md` — Must first, Should only after your Must tasks are merged, Later not in this phase. Don't change `docs/` without the coordinator's approval.**

<!--
  قالب CLAUDE.md لمشروع محور.
  يحطه المنسق في جذر الـ repo. Claude Code يقرأه تلقائيًا في بداية كل جلسة.
  كل سطر فيه TODO يعبّيه صاحبه، وبعدها احذفوا هالتعليقات.
  خلّوه قصير: الملخص والقواعد هنا، والتفاصيل في ملفات docs/.
-->

Full project context (phases, all FR/NFR, ERD, phase-2 plan, Markdown templates): @AI_BRIEF.md

## What this project is

Mihwar ("بياناتك.. قرارات أذكى") is an Arabic-first (RTL) platform for companies:

- **Wasl (وصل)**: employees submit requests and complaints. AI classifies them, sets a priority, and routes them to the right department. Users track status until the request is closed.
- **Operational Brain (العقل التشغيلي)**: analyzes company data and KPIs, detects problems and opportunities, and gives management evidence-based recommendations.
- **Ask AI (اسأل محور)**: a chat with Mihwar's one AI for every role. Each user has their own conversation and only sees what their role allows (FR-17, extended by Q-14).

There is one AI model, not one per module: it runs Wasl and the Operational Brain in the background and answers users in the Ask AI chat.
- Reports, notifications, and system logs.

MVP scope is defined in `docs/mvp-scope.md`. Build only what is in scope.

## Roles

| Role                 | Arabic        | Can access                                   |
| -------------------- | ------------- | -------------------------------------------- |
| Employee             | موظف          | Own requests, notifications                  |
| Department Manager   | مدير قسم      | Requests of own department                   |
| Top Management       | الإدارة العليا | KPIs, analytics, recommendations             |
| System Administrator | مدير النظام   | Users, roles, settings                       |

Every role also has the Ask AI chat (limited to what the role may see) and the «حسابي» account page (SCR-081). Reviewing sensitive complaints is a permission (`review_sensitive`), not a fifth role.

## Source of truth (read before coding)

| File                     | Contains                                                     |
| ------------------------ | ------------------------------------------------------------ |
| `docs/glossary.md`       | Names of entities, roles, request statuses. Use them exactly. |
| `docs/requirements.md`   | Functional and non-functional requirements (FR-xx, NFR-xx)   |
| `docs/mvp-scope.md`      | What's in the MVP and what's deferred                        |
| `docs/screens.md`        | Every screen (SCR-xx): role, content, actions, states, Figma link |
| `docs/user-flows.md`     | User flows per role (FLOW-xx, Mermaid)                       |
| `docs/design-system.md`  | Colors, fonts, spacing, components                           |
| `docs/architecture.md`   | Components and data flow                                     |
| `docs/tech-stack.md`     | Chosen technologies and why                                  |
| `docs/database.md`       | Database design as tables (phase 2 output, from the ERD)     |
| `docs/api.md`            | API operations as tables (phase 2 output, operationId per operation) |
| `db/schema.sql`          | Database schema — written in phase 3 from `docs/database.md` |
| `docs/api/openapi.yaml`  | API spec — written in phase 3 from `docs/api.md`             |
| `docs/ai.md`             | How AI works in Wasl and the Operational Brain               |
| `docs/tasks.md`          | Dev tasks (T-xx) with dependencies and acceptance criteria   |
| `docs/test-plan.md`      | Test types and coverage                                      |
| `docs/decisions.md`      | Decisions (ADR-xx) and open questions                        |
| `docs/acceptance-criteria.md` | Acceptance criteria (AC-xx) referenced by each task — appendix of `docs/tasks.md` |
| `reference/phase-3/phase-3-plan.md` | Phase-3 plan: which group owns which tasks, weekly plan, hand-offs between groups |

If two files disagree, stop and point it out. Don't pick one silently.
Decisions marked `proposed` in `docs/decisions.md` are not approved yet: ask before building on them.
The groups' original phase-2 submissions are archived in `reference/phase-2/المجموعات/` (history only — `docs/` wins).

## Tech stack

Approved 2026-11-22 (ADR-3001 → ADR-3007, details and reasons in `docs/tech-stack.md`):
- Frontend: Next.js (React) — ADR-3001
- Backend: NestJS (Node.js) — ADR-3002
- Database: PostgreSQL — ADR-3003
- Auth: inside the backend, secure server sessions + role permissions (`permissions` / `role_permissions`) — ADR-3004, ADR-3101
- File storage: S3-compatible object storage — ADR-3005
- AI services: one hosted LLM through an API (provider not chosen yet) — ADR-3006
- Hosting: Docker containers on a managed cloud service (provider not chosen yet) — ADR-3007

## Commands

<!-- TODO (phase 3, task T-080): fill once the project is scaffolded -->
- Install: `TODO`
- Run dev: `TODO`
- Test: `TODO`
- Lint / format: `TODO`
- DB migrations: `TODO`

## Conventions

- UI text is Arabic and the layout is RTL. Code, file names, DB tables/columns, and API fields are in English.
- Names of entities, roles, and statuses come from `docs/glossary.md` only.
- Follow `docs/design-system.md` for colors, fonts, and components. Don't invent new styles.
- Every screen handles these states: loading, success, error, empty, no-permission.
- Permissions are enforced on the server for every endpoint, not only hidden in the UI.
- Branches: each group has a protected branch — `group-1-wasl` · `group-2-shell` · `group-3-core` · `group-4-ai`. Start every task from your group's branch (update it first) as `feature/T-230-short-name`, and open the Pull Request **into your group's branch, not `main`**. The group leader reviews and merges; the coordinator merges group branches into `main`.
- Commits start with the task ID: `T-230: add request form`.
- One task per branch / PR.

<!-- TODO (Group 4): add any extra conventions from docs/test-plan.md -->

## How to work on a task

1. Read the task in `docs/tasks.md`, then its linked FR, SCR, and API operation.
2. Check that the tasks it depends on are done.
3. Implement only that task. Don't add features that aren't in the task.
4. Write or update tests for each acceptance criterion.
5. Run tests and lint. Fix failures before finishing.
6. In the PR description, list each acceptance criterion and how it was verified.

If something is unclear or missing in the docs, ask instead of guessing, and suggest adding it to `docs/decisions.md` as an open question.

## AI rules (Wasl & Operational Brain)

Approved rules (ADR-4202, ADR-4203 — full table in `docs/ai.md` 1.6):
- When the AI acts automatically and when a human reviews: confidence = the lower of category and department confidence. 0.85 or more → routed automatically (`auto`). 0.60 to under 0.85 → routed with a «راجع التوجيه» flag for the manager (`auto_review`). The thresholds are settings, not constants.
- Low-confidence or unclear requests: under 0.60, or `unclear` → one clarifying question to the employee (`clarify`), then one re-analysis. Still under 0.60 → `auto_review` to the closest department; no department at all → the default intake department setting. Never ask the employee twice.
- Sensitive complaints (`sensitive`, `security_incident`) always go to human review by a user with the `review_sensitive` permission; a complaint about a manager never reaches that manager (ADR-4203).
- Every AI decision is stored in `request_ai_decisions`; human changes go to `system_logs` (ADR-3202).
- Recommendations always show the data source and analysis date.
- Only send the model data the current user's role is allowed to see.
