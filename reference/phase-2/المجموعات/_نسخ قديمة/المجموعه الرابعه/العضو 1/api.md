# API — مواصفات API (محور)

> **Status:** draft · Week 1 — operations list only (no field details yet).
> **Owner:** Group 4 · Part 1 (Member 1: units 1, 2, 3, 6) + Part 2 (Member 2: units 4, 5).
> **Phase 2 = documents only.** No code, no SQL, no OpenAPI/YAML. This file becomes `docs/api/openapi.yaml` in phase 3.
> **Next (week 2):** after Group 3 delivers `docs/database.md`, each row below becomes its own section (fields, validation, response, errors) using the template in AI_BRIEF.md §8.3. After Group 2 delivers the screens, fill the *Screen* column.
> **Date:** 2026-11-04

---

## 0. General rules for every operation

- **Authentication (NFR-01):** every operation needs a logged-in user, except `login`.
- **Permissions on the server (NFR-02, NFR-03, NFR-04):** every operation checks the role on the server, not only by hiding buttons in the UI.
- **Department Manager sees only their department (NFR-03):** the department is taken from the logged-in user, never from the URL (see ADR-4002).
- **System Administrator only (NFR-04):** user, department, role, setting and log operations.
- **Logging (FR-38, NFR-22):** every operation that changes data writes a row in the system logs.
- **AI results (NFR-25, NFR-27):** every insight, issue, recommendation and answer returns its analysis run, data source and analysis date.
- **Common errors (filled per operation in week 2):** 400 missing/invalid field · 401 not logged in · 403 no permission · 404 not found.
- **Names:** `operationId` in camelCase English; paths in English, plural nouns; Arabic names for people.
- **Technology:** not assumed — Group 3 decides (Q-04).

**Legend:** Roles → `Employee` موظف · `Department Manager` مدير قسم · `Top Management` الإدارة العليا · `System Administrator` مدير النظام · `All` كل المستخدمين المسجّلين.
**Screen:** ⏳ = waiting for Group 1/2 screens list (week 2).
**🔒** = sensitive or repeated operation (see notes).

---

# Part 1 — الجزء الأول (Member 1)

## Unit 0/1 · تسجيل الدخول والمستخدمين والصلاحيات

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | Screen |
|---|---|---|---|---|---|---|
| 1 | `login` | تسجيل الدخول | POST /auth/login | All (before login) | NFR-01 | ⏳ |
| 2 | `logout` | تسجيل الخروج | POST /auth/logout | All | NFR-01 | ⏳ |
| 3 | `getCurrentUser` | بيانات المستخدم الحالي ودوره | GET /me | All | FR-39, NFR-01, NFR-19 | ⏳ |
| 4 | `listUsers` | قائمة المستخدمين | GET /users | System Administrator | FR-33, FR-39 | ⏳ |
| 5 | `createUser` 🔒 | إضافة مستخدم | POST /users | System Administrator | FR-33, FR-39 | ⏳ |
| 6 | `getUser` | عرض مستخدم | GET /users/{userId} | System Administrator | FR-33 | ⏳ |
| 7 | `updateUser` 🔒 | تعديل مستخدم (الاسم، البريد، الدور، القسم) | PATCH /users/{userId} | System Administrator | FR-33, FR-39 | ⏳ |
| 8 | `deactivateUser` 🔒 | إيقاف مستخدم | POST /users/{userId}/deactivate | System Administrator | FR-33, NFR-07 | ⏳ |
| 9 | `listRoles` | قائمة الأدوار | GET /roles | System Administrator | FR-35 | ⏳ |
| 10 | `listPermissions` ⚠️ | قائمة الصلاحيات | GET /permissions | System Administrator | FR-35 (Q-11) | ⏳ |
| 11 | `updateRolePermissions` 🔒 ⚠️ | تعديل صلاحيات دور | PUT /roles/{roleId}/permissions | System Administrator | FR-35 (Q-11) | ⏳ |
| 12 | `listSettings` | عرض إعدادات النظام | GET /settings | System Administrator | FR-36 | ⏳ |
| 13 | `updateSetting` 🔒 | تعديل إعداد | PATCH /settings/{settingKey} | System Administrator | FR-36, NFR-23 | ⏳ |

## Unit 2 · وصل وإدارة طلبات الموظفين

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | Screen |
|---|---|---|---|---|---|---|
| 14 | `createRequest` 🔒 | تقديم طلب أو شكوى | POST /requests | Employee | FR-01, FR-06, FR-07, FR-08, FR-09, FR-40 | ⏳ |
| 15 | `uploadRequestDocument` 🔒 | إرفاق مستند بالطلب | POST /requests/{requestId}/documents | Employee (own request) | FR-02, FR-10 | ⏳ |
| 16 | `downloadRequestDocument` | تحميل مستند مرفق | GET /requests/{requestId}/documents/{documentId} | Employee (own) · Department Manager (own department) | FR-10, NFR-05 | ⏳ |
| 17 | `listMyRequests` | طلباتي | GET /requests/mine | Employee | FR-03 | ⏳ |
| 18 | `getRequest` | تفاصيل الطلب وحالته | GET /requests/{requestId} | Employee (own) · Department Manager (own department) | FR-03, FR-12, NFR-20 | ⏳ |
| 19 | `getRequestHistory` ⚠️ | سجل تغيّر حالة الطلب | GET /requests/{requestId}/history | Employee (own) · Department Manager (own department) | FR-03, NFR-08 (Q-10) | ⏳ |
| 20 | `getRequestStatusReport` ⚠️ | تقرير حالة الطلبات | GET /requests/status-report | ⚠️ Employee و/أو Top Management (Q-08) | FR-05 | ⏳ |

## Unit 3 · إدارة الأقسام ومعالجة الطلبات

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | Screen |
|---|---|---|---|---|---|---|
| 21 | `listDepartmentRequests` | طلبات قسمي | GET /department/requests | Department Manager | FR-12, NFR-03 | ⏳ |
| 22 | `listDepartmentMembers` | موظفو قسمي (لاختيار المعالج) | GET /department/members | Department Manager | FR-13 | ⏳ |
| 23 | `assignRequest` 🔒 | إسناد الطلب لمعالج | PATCH /requests/{requestId}/assignee | Department Manager | FR-13 (Q-09 ✅) | ⏳ |
| 24 | `updateRequestStatus` 🔒 | تحديث حالة الطلب | PATCH /requests/{requestId}/status | Department Manager | FR-14, FR-11, FR-15 | ⏳ |
| 25 | `rerouteRequest` ⚠️ | إعادة توجيه الطلب لقسم آخر | PATCH /requests/{requestId}/department | Department Manager | ⚠️ لا يوجد متطلب مقابل (Q-4301) | ⏳ |
| 26 | `listDepartments` | قائمة الأقسام | GET /departments | System Administrator | FR-34 | ⏳ |
| 27 | `createDepartment` 🔒 | إضافة قسم | POST /departments | System Administrator | FR-34 | ⏳ |
| 28 | `updateDepartment` 🔒 | تعديل قسم | PATCH /departments/{departmentId} | System Administrator | FR-34 | ⏳ |
| 29 | `deactivateDepartment` 🔒 | إيقاف قسم | POST /departments/{departmentId}/deactivate | System Administrator | FR-34, NFR-07 | ⏳ |

## Unit 6 · الإشعارات وسجلات العمليات

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | Screen |
|---|---|---|---|---|---|---|
| 30 | `listMyNotifications` | إشعاراتي | GET /notifications | All (own) | FR-04, FR-41 | ⏳ |
| 31 | `markNotificationRead` | تعليم إشعار كمقروء | PATCH /notifications/{notificationId}/read | All (own) | FR-41 | ⏳ |
| 32 | `markAllNotificationsRead` | تعليم الكل كمقروء | POST /notifications/read-all | All (own) | FR-41 | ⏳ |
| 33 | `listSystemLogs` | سجلات النظام | GET /system-logs | System Administrator | FR-37, NFR-22 | ⏳ |
| 34 | `getSystemLog` | تفاصيل سجل | GET /system-logs/{logId} | System Administrator | FR-37 | ⏳ |

### Notes — Part 1 (🔒 sensitive / repeated)

- `createRequest`: one submit = one request. Protect against double-click / resend (repeated call). Classification, priority and routing (FR-06 → FR-08) run inside this operation — the employee does **not** choose the department (Q-09 ✅).
- `uploadRequestDocument`: limit file type and size (values in week 2); only the request owner can upload (NFR-05).
- `createUser`, `updateUser`, `deactivateUser`, `updateRolePermissions`, `updateSetting`, department operations: System Administrator only (NFR-04), always logged (FR-38).
- `deactivateUser`, `deactivateDepartment`: no hard delete, because requests and logs point to them (NFR-07, NFR-08) — ADR-4001.
- `assignRequest`: the handler must be the manager or an employee **in the same department** (Q-09 ✅).
- `updateRequestStatus`: allowed status changes follow the workflow (FR-15); every change sends a notification to the employee (FR-11) and is recorded (NFR-08). Status names come from Group 1 (Q-02).

---

# Part 2 — الجزء الثاني (Member 2)

## Unit 4 · العقل التشغيلي وتحليل بيانات الشركة

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | Screen |
|---|---|---|---|---|---|---|
| 35 | `listInsights` ⚠️ | رؤى الشركة | GET /insights | Top Management | FR-16, FR-28 (Q-12) | ⏳ |
| 36 | `getInsight` | تفاصيل رؤية | GET /insights/{insightId} | Top Management | FR-16, NFR-27 | ⏳ |
| 37 | `listKpis` ⚠️ | مؤشرات الأداء | GET /kpis | Top Management | FR-18 (Q-05) | ⏳ |
| 38 | `getOperationalPerformance` | الأداء التشغيلي | GET /operational-performance | Top Management | FR-20 | ⏳ |
| 39 | `listDetectedIssues` | المشكلات المكتشفة | GET /issues | Top Management | FR-21, FR-25 | ⏳ |
| 40 | `getDetectedIssue` | تفاصيل مشكلة | GET /issues/{issueId} | Top Management | FR-21, NFR-27 | ⏳ |
| 41 | `listOpportunities` ⚠️ | الفرص المحددة | GET /opportunities | Top Management | FR-22 (Q-05) | ⏳ |
| 42 | `listAnomalies` ⚠️ | الحالات الشاذة | GET /anomalies | Top Management | FR-26 (Q-13, Q-4402) | ⏳ |
| 43 | `listTrends` ⚠️ | الاتجاهات | GET /trends | Top Management | FR-27 (Q-4402) | ⏳ |
| 44 | `listAnalysisRuns` | عمليات التحليل | GET /analysis-runs | Top Management | FR-29, NFR-25 | ⏳ |
| 45 | `getAnalysisRun` | نتائج عملية تحليل | GET /analysis-runs/{analysisId} | Top Management | FR-29, FR-30, FR-43 | ⏳ |
| 46 | `importCompanyData` 🔒 ⚠️ | استيراد بيانات الشركة | POST /company-data/imports | ⚠️ System Administrator (Q-4401) | FR-42 | ⏳ |

## Unit 5 · المساعد الذكي والتقارير والتوصيات

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | Screen |
|---|---|---|---|---|---|---|
| 47 | `runAnalysis` 🔒 | تشغيل تحليل عند الطلب | POST /analysis-runs | Top Management | FR-32, FR-29 | ⏳ |
| 48 | `askAi` 🔒 | اسأل محور | POST /ask-ai/questions | Top Management | FR-17, FR-32 | ⏳ |
| 49 | `listAskAiQuestions` ⚠️ | أسئلتي السابقة | GET /ask-ai/questions | Top Management (own) | FR-17 (Q-05) | ⏳ |
| 50 | `listRecommendations` ⚠️ | توصيات الذكاء الاصطناعي | GET /recommendations | Top Management | FR-19 (Q-05) | ⏳ |
| 51 | `getRecommendation` | تفاصيل توصية | GET /recommendations/{recommendationId} | Top Management | FR-19, NFR-27 | ⏳ |
| 52 | `generateManagementReport` 🔒 | إنشاء تقرير إداري | POST /management-reports | Top Management | FR-23, FR-44 | ⏳ |
| 53 | `listManagementReports` | التقارير الإدارية | GET /management-reports | Top Management | FR-44 | ⏳ |
| 54 | `getManagementReport` | عرض تقرير إداري | GET /management-reports/{reportId} | Top Management | FR-23, FR-44 | ⏳ |

### Notes — Part 2 (🔒 sensitive / repeated)

- Every read operation returns, for each result, its analysis run, data source and analysis date (NFR-25, NFR-27).
- `runAnalysis`, `askAi`, `generateManagementReport`: heavy AI work (NFR-10). Block a second run while one is still running, and limit how often a user can call them.
- `askAi`: starts an on-demand analysis run (FR-32, BPMN). The AI model receives only data the Top Management role may see (AI_BRIEF §8.2). How the answer is built is in `docs/ai.md` (Member 3).
- `importCompanyData`: changes the data the Brain analyses — log it (FR-38) and check data quality before use (`docs/ai.md`).

---

## 3. Requirements with no own operation (internal system work)

These are done by the system inside other operations or as background jobs — no endpoint is called for them.

| Requirement | Where it happens |
|---|---|
| FR-06 تصنيف · FR-07 الأولوية · FR-08 التوجيه | inside `createRequest` |
| FR-09 حفظ الطلب · FR-40 بيانات الطلبات | inside `createRequest`, `assignRequest`, `updateRequestStatus` |
| FR-10 حفظ المستندات | inside `uploadRequestDocument` |
| FR-11 إشعار بتغيّر الحالة | inside `updateRequestStatus` (and `assignRequest`) |
| FR-15 سير عمل الطلب | status rules inside `updateRequestStatus` |
| FR-24 → FR-28, FR-30 التحليل ونتائجه | inside `runAnalysis`, `askAi` and the periodic job |
| FR-31 التحليل الدوري | background scheduled job (schedule can be a setting → `updateSetting`) |
| FR-38 تسجيل السجلات | inside every operation that changes data |
| FR-39 بيانات المستخدمين | inside the user operations |
| FR-41 إدارة الإشعارات | inside the notification operations |
| FR-42 بيانات الشركة | `importCompanyData` (⚠️ Q-4401) |
| FR-43 ربط نتائج التحليل | inside `runAnalysis`, read by `getAnalysisRun` |
| FR-44 حفظ التقارير | inside `generateManagementReport` |

## 4. Coverage — every FR has an operation

| FR | Operations |
|---|---|
| FR-01 | createRequest |
| FR-02 | uploadRequestDocument |
| FR-03 | listMyRequests, getRequest, getRequestHistory |
| FR-04 | listMyNotifications |
| FR-05 | getRequestStatusReport ⚠️ Q-08 |
| FR-06 → FR-11 | internal (section 3) |
| FR-12 | listDepartmentRequests, getRequest |
| FR-13 | listDepartmentMembers, assignRequest |
| FR-14 | updateRequestStatus |
| FR-15 | internal (section 3) |
| FR-16 | listInsights, getInsight |
| FR-17 | askAi, listAskAiQuestions |
| FR-18 | listKpis |
| FR-19 | listRecommendations, getRecommendation |
| FR-20 | getOperationalPerformance |
| FR-21 | listDetectedIssues, getDetectedIssue |
| FR-22 | listOpportunities |
| FR-23 | generateManagementReport, getManagementReport |
| FR-24 → FR-28 | internal (section 3) · read by listDetectedIssues, listAnomalies, listTrends, listInsights |
| FR-29 | listAnalysisRuns, getAnalysisRun, runAnalysis |
| FR-30, FR-31 | internal (section 3) |
| FR-32 | runAnalysis, askAi |
| FR-33 | listUsers, createUser, getUser, updateUser, deactivateUser |
| FR-34 | listDepartments, createDepartment, updateDepartment, deactivateDepartment |
| FR-35 | listRoles, listPermissions, updateRolePermissions |
| FR-36 | listSettings, updateSetting |
| FR-37 | listSystemLogs, getSystemLog |
| FR-38 → FR-44 | internal (section 3) · FR-42 → importCompanyData |

**Totals:** 54 operations · Part 1: 34 (#1–34) · Part 2: 20 (#35–54) · no operation repeated between the two parts.
MVP: Group 1 has not delivered the MVP scope yet (Q-03). When it arrives, mark operations outside the MVP as *Later* — do not delete them.

---

## 5. قرارات وأسئلة مفتوحة

### Decisions (proposed)

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

### Open questions

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
