# Decisions & open questions — Group 4 · API part

> Add these to `docs/decisions.md`. Raised in «مواصفات API» (docs/api.md). Status: proposed — the coordinator approves.

## Decisions (proposed)

## ADR-4001 · No hard delete for users and departments
- Status: proposed
- Context: requests, notifications and logs point to users and departments (NFR-07, NFR-08).
- Decision: `deactivateUser` and `deactivateDepartment` instead of DELETE.
- Reason: deleting breaks old requests and request history.
- Consequences: deactivated users cannot log in; their old requests stay readable.
- Date: 2026-11-04 · Owner group: 4

## ADR-4002 · The manager's department comes from the session, not the URL
- Status: proposed
- Context: NFR-03 — a Department Manager sees only their own department.
- Decision: department operations use `/department/...` (the department of the logged-in user), not `/departments/{id}/requests`.
- Reason: a manager cannot change a number in the URL to see another department.
- Consequences: System Administrator department management stays under `/departments`.
- Date: 2026-11-04 · Owner group: 4

## Open questions

| ID | Question | Proposed answer | Raised by | Affects | Status |
|---|---|---|---|---|---|
| Q-4101 | Are the four roles fixed, or can the admin create new roles? | Fixed four roles in the MVP; the admin edits permissions only (after Q-11). So no `createRole` / `deleteRole`. | Group 4 | FR-35 · listRoles | open |
| Q-4201 | Can the employee edit or cancel a request after sending it? | Not in the phase-1 requirements → not in the MVP; Group 1 decides. | Group 4 | FR-01 · createRequest | open |
| Q-4301 | If the AI routes a request to the wrong department, who fixes it and how? | The Department Manager re-routes it (`rerouteRequest`), and the change is logged. Needs a requirement; linked to Q-07 (`docs/ai.md`). | Group 4 | FR-08 · rerouteRequest | open |
| Q-4302 | Can the assigned employee (not the manager) update the request status? | MVP: manager only, as FR-14 says. | Group 4 | FR-14 · updateRequestStatus | open |
| Q-4401 | How does company operational data enter the system, and who does it? | MVP: file import (e.g. spreadsheet) by the System Administrator; integrations later. | Group 4 | FR-42 · importCompanyData | open |
| Q-4402 | Does Top Management need separate views for anomalies and trends? (no "view" requirement for them) | Keep read-only operations; show them inside insights and operational performance. | Group 4 | FR-26, FR-27 · listAnomalies, listTrends | open |
| Q-4501 | Does `askAi` answer immediately, or later (background) when analysis is long? | Decide with Member 3 and Group 3 (NFR-10). Suggest: immediate with a time limit in the MVP. | Group 4 | FR-17 · askAi | open |

**Existing questions this file depends on:** Q-02 (status names) · Q-03 (MVP) · Q-04 (technology) · Q-05 (KPIs, recommendations, opportunities, Ask-AI storage) · Q-07 (AI auto vs human) · Q-08 (FR-05 actor) · Q-10 (status history) · Q-11 (permissions) · Q-12 (insights 1:M) · Q-13 (anomalies storage).
