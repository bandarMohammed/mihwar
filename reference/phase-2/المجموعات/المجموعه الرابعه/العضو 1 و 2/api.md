# API — مواصفات API (محور)

> **Status:** draft v0.8 · 2026-11-23. Done: screens ↔ operations + MVP (v0.2) · **fields, validation and errors for all 59 operations (section 5)** — Part 1 from `docs/database.md` part 1, Part 2 from part 2. Screen numbers: Group 1 `docs/screens.md` v3.
> **Owner:** Group 4 · Part 1 (Member 1: units 1, 2, 3, 6) + Part 2 (Member 2: units 4, 5).
> **Phase 2 = documents only.** No code, no SQL, no OpenAPI/YAML. This file becomes `docs/api/openapi.yaml` in phase 3.
> **Next:** when Group 3 changes a proposed table, update the fields in section 5. Figma links come with Group 2.
> **Date:** 2026-11-23 (v0.8: 2026-11-23 · v0.7 and v0.6: 2026-11-22 · v0.5, v0.4 and v0.3: 2026-11-12 · v0.2: 2026-11-10 · v0.1: 2026-11-04)
> **2026-11-24 (coordinator — decisions approved):** ✅ Q-05 · Q-10 → Q-13 (tables of `docs/database.md`) remove the ⚠️ from `listPermissions`, `updateRolePermissions`, `listKpis`, `listAskAiQuestions` and others · ✅ Q-4102 password rules (8 characters · 5 failed sign-ins → 15-minute pause) · Must 43 · Must ⚠️ 1.
> **v0.8 (2026-11-23, coordinator — ✅ ADR-1003):** 2 new operations (59 in total): `changeMyPassword` (every user, screen SCR-081 «حسابي») and `resetUserPassword` (System Administrator, SCR-160) · `login` returns `mustChangePassword` · Q-4103 closed.
> **v0.7 (2026-11-22, coordinator — decisions batch 2):** 3 new operations (57 in total): `closeRequest` (✅ Q-1201 · Q-4601) and `listSensitiveRequests` · `releaseSensitiveRequest` (✅ Q-3204 · Q-4210, permission `review_sensitive`) · new screen SCR-280 · Q-4401 / Q-4404 closed (company data = Excel/CSV uploaded by the System Administrator).
> **v0.6 (2026-11-22, coordinator):** temporary screen codes replaced by the real `SCR-…` numbers from Group 1 `docs/screens.md` v3 (old → new table in section 6) · ✅ Q-02 values applied · FR-05 now Must (✅ Q-08) · ✅ Q-4507 closed by ADR-3502 · new gap Q-4408. ⟵ عُدّل بقرار المنسق 2026-11-22.
> **v0.5:** applies ADR-4502 ✅ (Ask Mihwar for every role) and moves IDs that clashed with `docs/ai.md`: Q-4402 → Q-4405 · Q-4501 → Q-4504 · Q-4502 → Q-4505 · Q-4503 → Q-4406 · Q-4201 (edit or cancel a request) → Q-4213 · ADR-4401 → ADR-4403 · ADR-4501 → ADR-4503. Q-4401 is now one question shared with `docs/ai.md`.

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
**MVP** (from Group 1 draft `mvp-scope.md` v0.1, not approved yet — Q-03): `Must` · `Must ⚠️` (in MVP but blocked by an open question) · `Should` · `Later`.
**Screen:** the `SCR-…` number from `docs/screens.md` (Group 1, v3). The old temporary codes (A/E/M/T/D) are listed in section 6.
**🔒** = sensitive or repeated operation (see notes).

---

# Part 1 — الجزء الأول (Member 1)

## Unit 0/1 · تسجيل الدخول والمستخدمين والصلاحيات

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | MVP | Screen |
|---|---|---|---|---|---|---|---|
| 1 | `login` | تسجيل الدخول | POST /auth/login | All (before login) | NFR-01 | Must | SCR-080 تسجيل الدخول |
| 2 | `logout` | تسجيل الخروج | POST /auth/logout | All | NFR-01 | Must | SCR-001/020/040/060 الإطار العام (القائمة والهيدر) · SCR-081 حسابي |
| 3 | `getCurrentUser` | بيانات المستخدم الحالي ودوره | GET /me | All | FR-39, NFR-01, NFR-19 | Must | SCR-001/020/040/060 الإطار العام (القائمة والهيدر) · SCR-081 حسابي |
| 4 | `listUsers` | قائمة المستخدمين | GET /users | System Administrator | FR-33, FR-39 | Must | SCR-160 إدارة المستخدمين |
| 5 | `createUser` 🔒 | إضافة مستخدم | POST /users | System Administrator | FR-33, FR-39 | Must | SCR-160 إدارة المستخدمين |
| 6 | `getUser` | عرض مستخدم | GET /users/{userId} | System Administrator | FR-33 | Must | SCR-160 إدارة المستخدمين |
| 7 | `updateUser` 🔒 | تعديل مستخدم (الاسم، البريد، الدور، القسم) | PATCH /users/{userId} | System Administrator | FR-33, FR-39 | Must | SCR-160 إدارة المستخدمين |
| 8 | `deactivateUser` 🔒 | إيقاف مستخدم | POST /users/{userId}/deactivate | System Administrator | FR-33, NFR-07 | Must | SCR-160 إدارة المستخدمين |
| 9 | `listRoles` | قائمة الأدوار | GET /roles | System Administrator | FR-35 | Must | SCR-160 إدارة المستخدمين · SCR-161 الأدوار والصلاحيات |
| 10 | `listPermissions` | قائمة الصلاحيات | GET /permissions | System Administrator | FR-35 (✅ Q-11) | Must | SCR-161 الأدوار والصلاحيات |
| 11 | `updateRolePermissions` 🔒 | تعديل صلاحيات دور | PUT /roles/{roleId}/permissions | System Administrator | FR-35 (✅ Q-11) | Must | SCR-161 الأدوار والصلاحيات |
| 12 | `listSettings` | عرض إعدادات النظام | GET /settings | System Administrator | FR-36 | Should | SCR-162 إعدادات النظام |
| 13 | `updateSetting` 🔒 | تعديل إعداد | PATCH /settings/{settingKey} | System Administrator | FR-36, NFR-23 | Should | SCR-162 إعدادات النظام |
| 58 | `changeMyPassword` 🔒 | تغيير كلمة المرور | PATCH /me/password | All | NFR-01, NFR-05, FR-39 | Must | SCR-081 حسابي |
| 59 | `resetUserPassword` 🔒 | تعيين كلمة مرور مؤقتة لمستخدم | POST /users/{userId}/reset-password | System Administrator | FR-33, NFR-01, NFR-04, NFR-05 | Must | SCR-160 إدارة المستخدمين |

## Unit 2 · وصل وإدارة طلبات الموظفين

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | MVP | Screen |
|---|---|---|---|---|---|---|---|
| 14 | `createRequest` 🔒 | تقديم طلب أو شكوى | POST /requests | Employee | FR-01, FR-06, FR-07, FR-08, FR-09, FR-40 | Must | SCR-201 تقديم طلب أو شكوى |
| 15 | `uploadRequestDocument` 🔒 | إرفاق مستند بالطلب | POST /requests/{requestId}/documents | Employee (own request) | FR-02, FR-10 | Should | SCR-201 تقديم طلب أو شكوى |
| 16 | `downloadRequestDocument` | تحميل مستند مرفق | GET /requests/{requestId}/documents/{documentId} | Employee (own) · Department Manager (own department) | FR-10, NFR-05 | Should | SCR-203 تفاصيل طلبي · SCR-321 معالجة الطلب |
| 17 | `listMyRequests` | طلباتي | GET /requests/mine | Employee | FR-03 | Must | SCR-202 طلباتي |
| 18 | `getRequest` | تفاصيل الطلب وحالته | GET /requests/{requestId} | Employee (own) · Department Manager (own department) | FR-03, FR-12, NFR-20 | Must | SCR-203 تفاصيل طلبي · SCR-321 معالجة الطلب |
| 19 | `getRequestHistory` | سجل تغيّر حالة الطلب | GET /requests/{requestId}/history | Employee (own) · Department Manager (own department) | FR-03, NFR-08 (✅ Q-10) | Must | SCR-203 تفاصيل طلبي · SCR-321 معالجة الطلب |
| 20 | `getRequestStatusReport` | تقرير حالة الطلبات | GET /requests/status-report | Employee · Top Management (✅ Q-08) | FR-05 | Must | SCR-204 تقرير حالة طلباتي |
| 55 | `closeRequest` 🔒 | تأكيد الحل وإغلاق الطلب | POST /requests/{requestId}/close | Employee (own) | FR-15, FR-03, NFR-08 | Must | SCR-203 تفاصيل طلبي |
| 56 | `listSensitiveRequests` 🔒 | الشكاوى الحساسة بانتظار المراجعة | GET /sensitive-requests | صاحب صلاحية `review_sensitive` | FR-08, NFR-02, NFR-05 | Must | SCR-280 مراجعة الشكاوى الحساسة |
| 57 | `releaseSensitiveRequest` 🔒 | تحويل الشكوى الحساسة بعد المراجعة | POST /sensitive-requests/{requestId}/release | صاحب صلاحية `review_sensitive` | FR-08, FR-13, NFR-02 | Must | SCR-280 مراجعة الشكاوى الحساسة |

## Unit 3 · إدارة الأقسام ومعالجة الطلبات

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | MVP | Screen |
|---|---|---|---|---|---|---|---|
| 21 | `listDepartmentRequests` | طلبات قسمي | GET /department/requests | Department Manager | FR-12, NFR-03 | Must | SCR-320 طلبات القسم |
| 22 | `listDepartmentMembers` | موظفو قسمي (لاختيار المعالج) | GET /department/members | Department Manager | FR-13 | Must | SCR-321 معالجة الطلب |
| 23 | `assignRequest` 🔒 | إسناد الطلب لمعالج | PATCH /requests/{requestId}/assignee | Department Manager | FR-13 (Q-09 ✅) | Must | SCR-321 معالجة الطلب |
| 24 | `updateRequestStatus` 🔒 | تحديث حالة الطلب | PATCH /requests/{requestId}/status | Department Manager | FR-14, FR-11, FR-15 | Must | SCR-321 معالجة الطلب |
| 25 | `rerouteRequest` ⚠️ | إعادة توجيه الطلب لقسم آخر | PATCH /requests/{requestId}/department | Department Manager | ⚠️ لا يوجد متطلب مقابل (Q-4301) | Later | SCR-321 معالجة الطلب |
| 26 | `listDepartments` | قائمة الأقسام | GET /departments | System Administrator | FR-34 | Must | SCR-160 إدارة المستخدمين · SCR-360 إدارة الأقسام |
| 27 | `createDepartment` 🔒 | إضافة قسم | POST /departments | System Administrator | FR-34 | Must | SCR-360 إدارة الأقسام |
| 28 | `updateDepartment` 🔒 | تعديل قسم | PATCH /departments/{departmentId} | System Administrator | FR-34 | Must | SCR-360 إدارة الأقسام |
| 29 | `deactivateDepartment` 🔒 | إيقاف قسم | POST /departments/{departmentId}/deactivate | System Administrator | FR-34, NFR-07 | Must | SCR-360 إدارة الأقسام |

## Unit 6 · الإشعارات وسجلات العمليات

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | MVP | Screen |
|---|---|---|---|---|---|---|---|
| 30 | `listMyNotifications` | إشعاراتي | GET /notifications | All (own) | FR-04, FR-41 | Must | SCR-001/020/040/060 الإطار العام (القائمة والهيدر) · SCR-680 الإشعارات |
| 31 | `markNotificationRead` | تعليم إشعار كمقروء | PATCH /notifications/{notificationId}/read | All (own) | FR-41 | Must | SCR-680 الإشعارات |
| 32 | `markAllNotificationsRead` | تعليم الكل كمقروء | POST /notifications/read-all | All (own) | FR-41 | Must | SCR-680 الإشعارات |
| 33 | `listSystemLogs` | سجلات النظام | GET /system-logs | System Administrator | FR-37, NFR-22 | Should | SCR-660 سجلات النظام |
| 34 | `getSystemLog` | تفاصيل سجل | GET /system-logs/{logId} | System Administrator | FR-37 | Should | SCR-660 سجلات النظام |

### Notes — Part 1 (🔒 sensitive / repeated)

- `createRequest`: one submit = one request. Protect against double-click / resend (repeated call). Classification, priority and routing (FR-06 → FR-08) run inside this operation — the employee does **not** choose the department (Q-09 ✅).
- `uploadRequestDocument`: limit file type and size (values in week 2); only the request owner can upload (NFR-05).
- `createUser`, `updateUser`, `deactivateUser`, `updateRolePermissions`, `updateSetting`, department operations: System Administrator only (NFR-04), always logged (FR-38).
- `deactivateUser`, `deactivateDepartment`: no hard delete, because requests and logs point to them (NFR-07, NFR-08) — ADR-4001.
- `assignRequest`: the handler must be the manager or an employee **in the same department** (Q-09 ✅).
- `updateRequestStatus`: allowed status changes follow the workflow (FR-15); every change sends a notification to the employee (FR-11) and is recorded (NFR-08). Status names ✅ Q-02: `new` → `under_review` → `in_progress` → `resolved` → `closed`.
- `closeRequest`: only the employee who owns the request, and only from `resolved` (✅ Q-1201). A daily background job closes requests left `resolved` longer than the setting `auto_close_days` (proposed 3 working days). ⟵ أُضيف بقرار المنسق 2026-11-22.
- `changeMyPassword`, `resetUserPassword`: passwords are never returned or logged as text (NFR-05). Both end the user's other sessions. A user with `mustChangePassword = true` may call only `getCurrentUser`, `changeMyPassword` and `logout` until the password is changed (✅ ADR-1003). ⟵ أُضيف بقرار المنسق 2026-11-23
- `listSensitiveRequests`, `releaseSensitiveRequest`: only users holding the permission `review_sensitive` (✅ Q-3204 — a permission, not a fifth role). A complaint about a manager is never shown to that manager (ADR-4203 ✅). ⟵ أُضيف بقرار المنسق 2026-11-22.

---

# Part 2 — الجزء الثاني (Member 2)

## Unit 4 · العقل التشغيلي وتحليل بيانات الشركة

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | MVP | Screen |
|---|---|---|---|---|---|---|---|
| 35 | `listInsights` | رؤى الشركة | GET /insights | Top Management | FR-16, FR-28 (✅ Q-12) | Must | SCR-040 · SCR-440 → SCR-443 لوحة الإدارة العليا |
| 36 | `getInsight` | تفاصيل رؤية | GET /insights/{insightId} | Top Management | FR-16, NFR-27 | Must | SCR-440 · SCR-443 تفاصيل الرؤية أو المشكلة |
| 37 | `listKpis` | مؤشرات الأداء | GET /kpis | Top Management | FR-18 (✅ Q-05) | Must | SCR-040 · SCR-440 → SCR-443 لوحة الإدارة العليا |
| 38 | `getOperationalPerformance` | الأداء التشغيلي | GET /operational-performance | Top Management | FR-20 | Must | SCR-040 · SCR-440 → SCR-443 لوحة الإدارة العليا |
| 39 | `listDetectedIssues` | المشكلات المكتشفة | GET /issues | Top Management | FR-21, FR-25 | Must | SCR-040 · SCR-440 → SCR-443 لوحة الإدارة العليا |
| 40 | `getDetectedIssue` | تفاصيل مشكلة | GET /issues/{issueId} | Top Management | FR-21, NFR-27 | Must | SCR-440 · SCR-443 تفاصيل الرؤية أو المشكلة |
| 41 | `listOpportunities` | الفرص المحددة | GET /opportunities | Top Management | FR-22 (✅ Q-05) | Later | SCR-444 الفرص |
| 42 | `listAnomalies` ⚠️ | الحالات الشاذة | GET /anomalies | Top Management | FR-26 (✅ Q-13, Q-4405) | Should | SCR-442 · SCR-443 الحالات الشاذة والاتجاهات |
| 43 | `listTrends` ⚠️ | الاتجاهات | GET /trends | Top Management | FR-27 (Q-4405) | Should | SCR-442 · SCR-443 الحالات الشاذة والاتجاهات |
| 44 | `listAnalysisRuns` | عمليات التحليل | GET /analysis-runs | Top Management | FR-29, NFR-25 | Must | SCR-040 ⚠️ عمليات التحليل |
| 45 | `getAnalysisRun` | نتائج عملية تحليل | GET /analysis-runs/{analysisId} | Top Management | FR-29, FR-30, FR-43 | Must | SCR-440 · SCR-443 تفاصيل الرؤية أو المشكلة · SCR-040 ⚠️ عمليات التحليل |
| 46 | `importCompanyData` 🔒 ⚠️ | استيراد بيانات الشركة | POST /company-data/imports | ⚠️ System Administrator (Q-4401) | FR-42 | Must ⚠️ | SCR-060 ⚠️ استيراد بيانات الشركة |

## Unit 5 · المساعد الذكي والتقارير والتوصيات

| # | operationId | الاسم العربي | Method & path | Roles | Requirements | MVP | Screen |
|---|---|---|---|---|---|---|---|
| 47 | `runAnalysis` 🔒 | تشغيل تحليل عند الطلب | POST /analysis-runs | Top Management | FR-32, FR-29 | Must | SCR-040 ⚠️ عمليات التحليل |
| 48 | `askAi` 🔒 | اسأل محور | POST /ask-ai/questions | All (own chat · data by role — ADR-4502 ✅) | FR-17 (expanded), FR-32 | Must | SCR-580 اسأل محور |
| 49 | `listAskAiQuestions` | أسئلتي السابقة | GET /ask-ai/questions | All (own) | FR-17 (✅ Q-05, ✅ Q-4502) | Must | SCR-580 اسأل محور |
| 50 | `listRecommendations` | توصيات الذكاء الاصطناعي | GET /recommendations | Top Management | FR-19 (✅ Q-05) | Should | SCR-541 التوصيات |
| 51 | `getRecommendation` | تفاصيل توصية | GET /recommendations/{recommendationId} | Top Management | FR-19, NFR-27 | Should | SCR-541 التوصيات |
| 52 | `generateManagementReport` 🔒 | إنشاء تقرير إداري | POST /management-reports | Top Management | FR-23, FR-44 | Should | SCR-542 التقارير الإدارية |
| 53 | `listManagementReports` | التقارير الإدارية | GET /management-reports | Top Management | FR-44 | Should | SCR-542 التقارير الإدارية |
| 54 | `getManagementReport` | عرض تقرير إداري | GET /management-reports/{reportId} | Top Management | FR-23, FR-44 | Should | SCR-542 التقارير الإدارية |

### Notes — Part 2 (🔒 sensitive / repeated)

- Every read operation returns, for each result, its analysis run, data source and analysis date (NFR-25, NFR-27).
- `runAnalysis`, `askAi`, `generateManagementReport`: heavy AI work (NFR-10). Block a second run while one is still running, and limit how often a user can call them.
- `askAi`: one chat for every role (ADR-4502 ✅ — `docs/ai.md` §3). The server checks the role first and the model gets only results of metrics allowed for that role (`docs/ai.md` §3.5). Only Top Management analytic questions start an on-demand analysis run (FR-32). How the answer is built is in `docs/ai.md` (Member 3).
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
| FR-05 | getRequestStatusReport ✅ Q-08 |
| FR-06 → FR-11 | internal (section 3) |
| FR-12 | listDepartmentRequests, getRequest |
| FR-13 | listDepartmentMembers, assignRequest |
| FR-14 | updateRequestStatus |
| FR-15 | internal (section 3) · closeRequest |
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
| FR-33 | listUsers, createUser, getUser, updateUser, deactivateUser, resetUserPassword (✅ ADR-1003) |
| FR-34 | listDepartments, createDepartment, updateDepartment, deactivateDepartment |
| FR-35 | listRoles, listPermissions, updateRolePermissions |
| FR-36 | listSettings, updateSetting |
| FR-37 | listSystemLogs, getSystemLog |
| FR-38 → FR-44 | internal (section 3) · FR-39 → getCurrentUser, changeMyPassword (✅ ADR-1003) · FR-42 → importCompanyData |

**Totals:** 59 operations · Part 1: 34 (#1–34) · Part 2: 20 (#35–54) · v0.7 (coordinator): 3 (#55–57, unit 2) · v0.8 (coordinator): 2 (#58–59, unit 0/1) · no operation repeated between the two parts.
**MVP (v0.8, approved scope · 2026-11-24):** 43 Must · 1 Must ⚠️ (`importCompanyData`, Q-4408) · 13 Should · 2 Later = 59. ⟵ كان 39 Must · 5 Must ⚠️ قبل اعتماد Q-05 وQ-11. Nothing deleted; *Should* / *Later* operations stay documented for later releases.

---

## 5. Operation details — fields, validation, errors (week 2)

> **Sources:** `docs/database.md` part 1 (Group 3 · Member 3 · 2026-11-11) for Part 1, and part 2 (Group 3 · Member 4 · 2026-11-10) for Part 2. Most tables are `proposed` there, so every field here follows them and changes if they change.
> **✅ Values approved (Q-02 · 2026-11-22):** request `status` = `new` · `under_review` · `in_progress` · `resolved` · `closed` · `priority` = `urgent` · `high` · `medium` · `low` · `category` = the 13 keys in `docs/glossary.md` (from `docs/ai.md` 1.4) · `request_type` = `request` · `complaint`. ⟵ عُدّل بقرار المنسق 2026-11-22.
> **Screen** column uses the real `SCR-…` numbers (section 6).

### 5.0 Shared rules — both parts (proposed)

**Field names (ADR-4003):** API fields use camelCase English; each maps 1:1 to a `snake_case` column in `docs/database.md` (`request_number` → `requestNumber`, `analysis_id` → `analysisId`). Computed fields are marked.

**List operations (every `list…`):**

| Query field | Type | Required | Validation |
|---|---|---|---|
| `page` | integer | no | ≥ 1 · الافتراضي 1 |
| `pageSize` | integer | no | 1 → 100 · الافتراضي 20 (مقترح) |

- Response wrapper: `items` (list) · `total` (integer) · `page` · `pageSize`. An empty list is 200 with empty `items` (screen state `empty`), not an error.
- Date filters (`from`, `to`) are ISO 8601; `from` must not be after `to` (else 400). Times are stored with their time zone (`timestamptz`, PostgreSQL — ADR-3003) and shown in the user's time.

**Common errors:** 400 حقل ناقص أو غير صحيح · 401 غير مسجّل · 403 لا توجد صلاحية · 404 غير موجود · 409 تعارض مع حالة البيانات · 500 خطأ في الخادم. Every 4xx returns `code` (English) + `messageAr` (text for the screen).
**Permissions (NFR-02 → NFR-04):** checked on the server in every operation, as in `docs/database.md` (part 1 §1, part 2 §6). A Department Manager's department always comes from the session (ADR-4002).
**Logging (FR-38):** every operation that changes data writes one row in `system_logs` (`action` · `details` without passwords or secrets — NFR-05).

---

# Part 1 — Member 1 (#1–34)

### 5.1 Shared objects — Part 1 (from `docs/database.md` part 1)

**`user` object** — from `users` + `roles` + `departments`. `password_hash` is **never** returned (NFR-05).

| Field | Type | From (database) | Description |
|---|---|---|---|
| `userId` | uuid | `users.user_id` | معرّف المستخدم |
| `name` | text | `users.name` | الاسم |
| `email` | text | `users.email` | البريد |
| `role` | text | `roles.role_name` | الدور (القيم الأربع كما في `glossary.md`) |
| `departmentId` · `departmentName` | uuid · text | `users.department_id` → `departments.department_name` | القسم (فارغ لمن لا يتبع قسمًا) |
| `isActive` | boolean | `users.is_active` | الحساب فعّال أو موقوف |
| `mustChangePassword` | boolean | `users.must_change_password` | يلزمه تغيير كلمة مؤقتة قبل أي عملية ثانية (✅ ADR-1003) ⟵ أُضيف بقرار المنسق 2026-11-23 |

**`request` object** — from `requests`. What each role sees (database part 1 §3.6):

| Field | Type | From (database) | Employee | Department Manager |
|---|---|---|---|---|
| `requestId` | uuid | `request_id` | ✓ | ✓ |
| `requestNumber` | text | `request_number` (format ⚠️ Q-4206) | ✓ | ✓ |
| `requestType` · `category` | text | `request_type` · `category` (values ✅ Q-02 — glossary) | ✓ | ✓ |
| `priority` | text | `priority` (values ✅ Q-02 — glossary) | ✗ (Q-1203) | ✓ |
| `status` | text | `status` (values ✅ Q-02 — glossary) | ✓ | ✓ |
| `description` | text | `description` | ✓ | ✓ |
| `departmentName` | text | `department_id` → `departments` | ✓ | ✓ |
| `assignee` | `userId` · `name` | `assignee_id` → `users` | ✗ (Q-1203) | ✓ |
| `employee` | `userId` · `name` | `employee_id` → `users` | — | ✓ |
| `aiDecision` | `decision` · `summaryAr` · `reasonAr` · `flags` | latest row of `request_ai_decisions` | ✗ | ✓ (own department) |
| `createdAt` · `updatedAt` | timestamp | `created_at` · `updated_at` | ✓ | ✓ |

- System Administrator does **not** read requests (database part 1 §3.6).
- ⚠️ Requests flagged `sensitive` wait for Q-3204 / ADR-4203 before any extra rule.

---

## login · تسجيل الدخول
- Method & path: POST /auth/login
- Roles: All (before login) · Requirements: NFR-01, NFR-05 · MVP: Must · Screen: SCR-080
- Tables: `users` (session sign-in inside the server — ADR-3004)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `email` | text | yes | صيغة بريد صحيحة · بدون فرق بين الأحرف الكبيرة والصغيرة |
| `password` | text | yes | غير فارغ |

| Response 200 | `user` object (with `mustChangePassword`) · a session is opened (the form — cookie or token — follows ADR-3004). If `mustChangePassword = true` the screen goes to SCR-081 first (✅ ADR-1003) |
|---|---|
| Errors | 400 حقل ناقص · 401 البريد أو كلمة المرور غير صحيحة (نفس الرسالة للحالتين) · 403 الحساب موقوف (`is_active = false`) · 429 بعد 5 محاولات فاشلة يتوقف الدخول 15 دقيقة (✅ Q-4102) |

- Notes: 🔒 the password is compared with `password_hash` only, never stored or logged as text (NFR-05). Failed sign-ins are logged in `system_logs` without the password.

## logout · تسجيل الخروج
- Method & path: POST /auth/logout
- Roles: All · Requirements: NFR-01 · MVP: Must · Screen: SCR-001/020/040/060

| Request field | Type | Required | Validation |
|---|---|---|---|
| — | — | — | no body; ends the current session |

| Response 204 | no content |
|---|---|
| Errors | 401 غير مسجّل |

## getCurrentUser · بيانات المستخدم الحالي ودوره
- Method & path: GET /me
- Roles: All · Requirements: FR-39, NFR-01, NFR-19 · MVP: Must · Screen: SCR-001/020/040/060
- Tables: `users` · `roles` · `role_permissions` · `permissions`

| Request field | Type | Required | Validation |
|---|---|---|---|
| — | — | — | the user comes from the session |

| Response 200 | `user` object + `permissions[]` (`permission_key` = operationId, ADR-3101) — the screen shows only the menus the user may use |
|---|---|
| Errors | 401 غير مسجّل أو انتهت الجلسة |

- Notes: hiding menus is for comfort only; every operation checks again on the server (NFR-02).

## changeMyPassword · تغيير كلمة المرور
- Method & path: PATCH /me/password
- Roles: All · Requirements: NFR-01, NFR-05, FR-39 · MVP: Must · Screen: SCR-081 (✅ ADR-1003)
- Tables: `users` (`password_hash` · `must_change_password` · `password_changed_at`) · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `currentPassword` | text | yes | غير فارغ · يطابق `password_hash` الحالي |
| `newPassword` | text | yes | يطابق شروط كلمة المرور (✅ Q-4102: 8 أحرف على الأقل — الإعداد `password_min_length`) · لا يساوي الحالية |

| Response 204 | no content · `must_change_password = false` · `password_changed_at = now` · the current session stays |
|---|---|
| Errors | 400 حقل ناقص · 400 `WRONG_CURRENT_PASSWORD` كلمة المرور الحالية غير صحيحة · 400 `WEAK_PASSWORD` الجديدة لا تطابق الشروط أو تساوي الحالية · 401 غير مسجّل · 429 بعد 5 محاولات فاشلة يتوقف الدخول 15 دقيقة (✅ Q-4102) |

- Notes: 🔒 ends every other session of the user. Logged in `system_logs` as `password_changed` without any password (NFR-05). The «تأكيد كلمة المرور» field is checked in the screen only. ⟵ أُضيف بقرار المنسق 2026-11-23

## listUsers · قائمة المستخدمين
- Method & path: GET /users
- Roles: System Administrator · Requirements: FR-33, FR-39 · MVP: Must · Screen: SCR-160
- Tables: `users` · `roles` · `departments`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `role` | text | no | أحد الأدوار الأربعة |
| `departmentId` | uuid | no | قسم موجود |
| `isActive` | boolean | no | `true` · `false` |
| `search` | text | no | جزء من الاسم أو البريد · 2 أحرف على الأقل |

| Response 200 | `items[]`: `user` object — sorted by name |
|---|---|
| Errors | 400 · 401 · 403 |

## createUser · إضافة مستخدم
- Method & path: POST /users
- Roles: System Administrator · Requirements: FR-33, FR-39 · MVP: Must · Screen: SCR-160
- Tables: writes `users` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `name` | text | yes | غير فارغ |
| `email` | text | yes | صيغة صحيحة · غير مكرر (بدون فرق الأحرف) |
| `password` | text | yes | كلمة مرور أولية · 8 أحرف على الأقل (✅ Q-4102) |
| `roleId` | uuid | yes | دور موجود |
| `departmentId` | uuid | yes for Department Manager · else no | قسم موجود وفعّال (NFR-03) |
| `reviewSensitive` | boolean | no | صلاحية «مراجعة الحساس» لهذا الشخص (✅ Q-3204)، الافتراضي `false` · لمدير النظام فقط ⟵ أُضيف بقرار المنسق 2026-11-22 |

| Response 201 | `user` object |
|---|---|
| Errors | 400 حقل ناقص أو غير صحيح · مدير قسم بدون قسم · 401 · 403 · 404 الدور أو القسم غير موجود · 409 البريد مستخدم |

- Notes: 🔒 the server stores only `password_hash`; one row in `system_logs` without the password (FR-38). Repeated call with the same email → 409, so a double click never makes two accounts.

## getUser · عرض مستخدم
- Method & path: GET /users/{userId}
- Roles: System Administrator · Requirements: FR-33 · MVP: Must · Screen: SCR-160

| Request field | Type | Required | Validation |
|---|---|---|---|
| `userId` (path) | uuid | yes | صيغة uuid صحيحة |

| Response 200 | `user` object |
|---|---|
| Errors | 400 · 401 · 403 · 404 المستخدم غير موجود |

## updateUser · تعديل مستخدم
- Method & path: PATCH /users/{userId}
- Roles: System Administrator · Requirements: FR-33, FR-39 · MVP: Must · Screen: SCR-160
- Tables: writes `users` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `userId` (path) | uuid | yes | مستخدم موجود |
| `name` | text | no | غير فارغ إذا أُرسل |
| `email` | text | no | صيغة صحيحة · غير مكرر |
| `roleId` | uuid | no | دور موجود |
| `departmentId` | uuid أو فارغ | no | قسم فعّال · لا يُفرَّغ لمدير قسم |
| `reviewSensitive` | boolean | no | صلاحية «مراجعة الحساس» لهذا الشخص (✅ Q-3204)، الافتراضي `false` · لمدير النظام فقط ⟵ أُضيف بقرار المنسق 2026-11-22 |

| Response 200 | `user` object after the change |
|---|---|
| Errors | 400 لا يوجد حقل للتعديل أو قيمة غير صحيحة · 401 · 403 · 404 · 409 البريد مستخدم |

- Notes: 🔒 the password is not changed here (⚠️ Q-4103). Old and new values go to `system_logs` (FR-38). If a Department Manager changes department, their open assigned requests stay as they are (⚠️ Q-4104).

## deactivateUser · إيقاف مستخدم
- Method & path: POST /users/{userId}/deactivate
- Roles: System Administrator · Requirements: FR-33, NFR-07 · MVP: Must · Screen: SCR-160
- Tables: writes `users.is_active` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `userId` (path) | uuid | yes | مستخدم موجود |

| Response 200 | `user` object with `isActive = false` |
|---|---|
| Errors | 400 · 401 · 403 · 404 · 409 المستخدم موقوف أصلًا · إيقاف نفسك · آخر مدير نظام فعّال |

- Notes: 🔒 no hard delete (ADR-4001). Their sessions end, they cannot sign in, and their old requests stay readable.

## resetUserPassword · تعيين كلمة مرور مؤقتة لمستخدم
- Method & path: POST /users/{userId}/reset-password
- Roles: System Administrator · Requirements: FR-33, NFR-01, NFR-04, NFR-05 · MVP: Must · Screen: SCR-160 (✅ ADR-1003)
- Tables: `users` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `userId` (path) | uuid | yes | مستخدم موجود |
| `temporaryPassword` | text | yes | يطابق شروط كلمة المرور (✅ Q-4102: 8 أحرف على الأقل) |

| Response 204 | no content · `must_change_password = true` · every session of that user ends |
|---|---|
| Errors | 400 حقل ناقص أو كلمة لا تطابق الشروط · 401 غير مسجّل · 403 لغير مدير النظام · 404 المستخدم غير موجود · 409 الحساب موقوف |

- Notes: 🔒 for a user who forgot the password; self-service reset by email is Later (no email channel in the MVP — ADR-1601). The administrator gives the temporary password to the user directly. Logged as `password_reset` (who, for whom, when) without the password (NFR-05). ⟵ أُضيف بقرار المنسق 2026-11-23

## listRoles · قائمة الأدوار
- Method & path: GET /roles
- Roles: System Administrator · Requirements: FR-35 · MVP: Must · Screen: SCR-160, SCR-161
- Tables: `roles`

| Request field | Type | Required | Validation |
|---|---|---|---|
| — | — | — | — |

| Response 200 | `items[]`: `roleId` · `roleName` — the four fixed roles (Q-4101) |
|---|---|
| Errors | 401 · 403 |

## listPermissions · قائمة الصلاحيات
- Method & path: GET /permissions
- Roles: System Administrator · Requirements: FR-35 · MVP: Must (✅ Q-11 · ADR-3101) · Screen: SCR-161
- Tables: `permissions` · `role_permissions`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `roleId` | uuid | no | إذا أُرسل: يرجع لكل صلاحية هل هي ممنوحة لهذا الدور |

| Response 200 | `items[]`: `permissionId` · `permissionKey` (operationId) · `description` (Arabic) · `granted` (boolean, only with `roleId`) |
|---|---|
| Errors | 400 · 401 · 403 · 404 الدور غير موجود |

## updateRolePermissions · تعديل صلاحيات دور
- Method & path: PUT /roles/{roleId}/permissions
- Roles: System Administrator · Requirements: FR-35 · MVP: Must (✅ Q-11 · ADR-3101) · Screen: SCR-161
- Tables: writes `role_permissions` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `roleId` (path) | uuid | yes | دور موجود |
| `permissionIds` | list of uuid | yes | كل عنصر صلاحية موجودة · بدون تكرار · القائمة الكاملة الجديدة (تحل محل القديمة) |

| Response 200 | `roleId` · `permissions[]` after the change |
|---|---|
| Errors | 400 قائمة غير صحيحة · 401 · 403 · 404 · 409 صلاحية إدارة النظام لدور غير مدير النظام (NFR-04) · إزالة صلاحيات الإدارة من مدير النظام |

- Notes: 🔒 PUT = the full list each time, so sending it twice gives the same result. Added and removed keys go to `system_logs` (FR-38).

## listSettings · عرض إعدادات النظام
- Method & path: GET /settings
- Roles: System Administrator · Requirements: FR-36 · MVP: Should · Screen: SCR-162
- Tables: `system_settings`

| Request field | Type | Required | Validation |
|---|---|---|---|
| — | — | — | — |

| Response 200 | `items[]`: `settingKey` · `settingValue` · `updatedBy` (`userId` · `name`) · `updatedAt` (from `system_logs`) |
|---|---|
| Errors | 401 · 403 |

## updateSetting · تعديل إعداد
- Method & path: PATCH /settings/{settingKey}
- Roles: System Administrator · Requirements: FR-36, NFR-23 · MVP: Should · Screen: SCR-162
- Tables: writes `system_settings` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `settingKey` (path) | text | yes | مفتاح موجود (المفاتيح تُضاف مع التطوير لا من الواجهة) |
| `settingValue` | text | yes | غير فارغ · صيغة كل مفتاح حسب نوعه (⚠️ Q-4105) |

| Response 200 | `settingKey` · `settingValue` · `updatedBy` |
|---|---|
| Errors | 400 قيمة غير صحيحة · 401 · 403 · 404 مفتاح غير موجود |

- Notes: 🔒 no secrets or passwords of outside services here (NFR-05). Old and new values go to `system_logs`.

## createRequest · تقديم طلب أو شكوى
- Method & path: POST /requests
- Roles: Employee · Requirements: FR-01, FR-06, FR-07, FR-08, FR-09, FR-40 · MVP: Must · Screen: SCR-201
- Tables: writes `requests` · `request_ai_decisions` · `request_status_history` · `notifications` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `description` | text | yes | غير فارغ بعد حذف المسافات · الطول ⚠️ Q-4207 |
| `clarificationAnswer` | text | no | فقط عند الإرسال الثاني بعد سؤال توضيحي (ADR-4202) |

| Response 201 | `request` object (Employee view) — `requestNumber`, `requestType`, `category`, `status`, `departmentName` · **or 200** `needsClarification = true` · `clarifyQuestion` (no request is saved yet — ⚠️ Q-4208) |
|---|---|
| Errors | 400 وصف فارغ أو طويل · 401 · 403 · 422 الرسالة ليست طلبًا (`not_a_request` — Q-4203) · 429 |

- Notes: 🔒 the employee does **not** send type, category, priority, department or handler (Q-09 ✅, Q-1201). The AI decision is written in `request_ai_decisions`, the first history row in `request_status_history`, all in **one transaction** (NFR-06). Double click: the screen disables the button, and the server rejects the same text from the same employee within a short time (⚠️ Q-4207).

## uploadRequestDocument · إرفاق مستند بالطلب
- Method & path: POST /requests/{requestId}/documents
- Roles: Employee (own request) · Requirements: FR-02, FR-10 · MVP: Should · Screen: SCR-201
- Tables: writes `supporting_documents` (file in S3-compatible storage — ADR-3005)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` (path) | uuid | yes | طلب يملكه الموظف نفسه |
| `file` | file | yes | الأنواع والحجم ⚠️ Q-1202, Q-3006 |

| Response 201 | `documentId` · `fileName` · `uploadedAt` |
|---|---|
| Errors | 400 ملف مفقود أو نوع غير مسموح · 401 · 403 ليس طلبك · 404 · 409 الطلب مغلق · 413 الملف كبير |

- Notes: 🔒 only the path is stored (`file_path`); the AI does not read file content in the MVP (Q-4205).

## downloadRequestDocument · تحميل مستند مرفق
- Method & path: GET /requests/{requestId}/documents/{documentId}
- Roles: Employee (own) or Department Manager (own department) · Requirements: FR-10, NFR-05 · MVP: Should · Screen: SCR-203, SCR-321

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` · `documentId` (path) | uuid | yes | المستند يتبع هذا الطلب |

| Response 200 | a short-lived download link (`downloadUrl` · `expiresAt`) given only after the permission check (ADR-3005) |
|---|---|
| Errors | 400 · 401 · 403 · 404 المستند غير موجود أو لا يتبع الطلب |

## listMyRequests · طلباتي
- Method & path: GET /requests/mine
- Roles: Employee · Requirements: FR-03 · MVP: Must · Screen: SCR-202
- Tables: `requests` (index `(employee_id, created_at)`)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `status` | text | no | قيمة من `glossary.md` (Q-02) |

| Response 200 | `items[]`: `requestId` · `requestNumber` · `requestType` · `category` · `status` · `departmentName` · `createdAt` · `updatedAt` — newest first |
|---|---|
| Errors | 400 · 401 · 403 |

- Notes: the employee is taken from the session — no `employeeId` field.

## getRequest · تفاصيل الطلب وحالته
- Method & path: GET /requests/{requestId}
- Roles: Employee (own) or Department Manager (own department) · Requirements: FR-03, FR-12, NFR-20 · MVP: Must · Screen: SCR-203, SCR-321

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` (path) | uuid | yes | صيغة uuid صحيحة |

| Response 200 | `request` object (view by role, §5.1) + `documents[]` (`documentId` · `fileName` · `uploadedAt`) |
|---|---|
| Errors | 400 · 401 · 403 ليس طلبك أو ليس من قسمك · 404 |

- Notes: a request of another employee or another department → 403 or 404 (same answer, so ids cannot be guessed — ⚠️ Q-4209).

## getRequestHistory · سجل تغيّر حالة الطلب
- Method & path: GET /requests/{requestId}/history
- Roles: Employee (own) or Department Manager (own department) · Requirements: FR-03, NFR-08 · MVP: Must · Screen: SCR-203, SCR-321
- Tables: `request_status_history` (Q-10 → ADR-3201)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` (path) | uuid | yes | نفس صلاحية `getRequest` |

| Response 200 | `items[]`: `oldStatus` · `newStatus` · `changedAt` · `changedBy` (`name`, or «النظام» when empty) · `note` — oldest first |
|---|---|
| Errors | 400 · 401 · 403 · 404 |

## getRequestStatusReport · تقرير حالة الطلبات
- Method & path: GET /requests/status-report
- Roles: Employee (own) and Top Management (summary) — ✅ Q-08 · Requirements: FR-05 · MVP: Must (✅ Q-08 · ⟵ عُدّل بقرار المنسق 2026-11-22) · Screen: SCR-204 (Employee) · SCR-542 (Top Management summary — ADR-1204)
- Tables: `requests` (no extra column)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `from` · `to` | timestamp | no | على `createdAt` · `from` لا يتجاوز `to` |

| Response 200 | `byStatus[]` (`status` · `count`) · `total` — Employee: own requests only · Top Management: all departments, counts only, no request text |
|---|---|
| Errors | 400 · 401 · 403 |

- Notes: ✅ Q-08 approved by the coordinator (2026-11-22): for both roles (ADR-1204).

## listDepartmentRequests · طلبات قسمي
- Method & path: GET /department/requests
- Roles: Department Manager · Requirements: FR-12, NFR-03 · MVP: Must · Screen: SCR-320
- Tables: `requests` (index `(department_id, status)`)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `status` · `priority` · `category` | text | no | قيم من `glossary.md` (Q-02) |
| `assigneeId` | uuid | no | موظف في نفس القسم |
| `unassigned` | boolean | no | `true` = الطلبات بدون معالج |

| Response 200 | `items[]`: `request` object (Department Manager view, without `description` full text — first line only) — sorted by priority, then oldest first |
|---|---|
| Errors | 400 · 401 · 403 |

- Notes: the department comes from the session (ADR-4002, NFR-03); no `departmentId` field.

## listDepartmentMembers · موظفو قسمي
- Method & path: GET /department/members
- Roles: Department Manager · Requirements: FR-13 · MVP: Must · Screen: SCR-321
- Tables: `users` (same `department_id`, `is_active = true`)

| Request field | Type | Required | Validation |
|---|---|---|---|
| — | — | — | the department comes from the session |

| Response 200 | `items[]`: `userId` · `name` · `role` — no email or other data (database part 1 §3.5) |
|---|---|
| Errors | 401 · 403 |

## assignRequest · إسناد الطلب لمعالج
- Method & path: PATCH /requests/{requestId}/assignee
- Roles: Department Manager · Requirements: FR-13 · MVP: Must · Screen: SCR-321
- Tables: writes `requests.assigned_manager_id` + `assignee_id` · `notifications` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` (path) | uuid | yes | طلب في قسم المدير |
| `assigneeId` | uuid | yes | المدير نفسه أو موظف **فعّال** في نفس القسم (Q-09 ✅, ADR-3301) |

| Response 200 | `request` object after the change |
|---|---|
| Errors | 400 · 401 · 403 الطلب ليس من قسمك · 404 · 409 الطلب مغلق · 422 المعالج ليس من القسم أو موقوف |

- Notes: 🔒 the server fills `assigned_manager_id` = the logged-in manager. A re-assignment is logged (FR-38). Assigning does **not** change the status (Q-3301).

## updateRequestStatus · تحديث حالة الطلب
- Method & path: PATCH /requests/{requestId}/status
- Roles: Department Manager · Requirements: FR-14, FR-11, FR-15 · MVP: Must · Screen: SCR-321
- Tables: writes `requests.status` + `request_status_history` (one transaction) · `notifications`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` (path) | uuid | yes | طلب في قسم المدير |
| `status` | text | yes | قيمة من `glossary.md` · انتقال مسموح من الحالة الحالية (FR-15 · ✅ Q-02) |
| `note` | text | no | ملاحظة تُحفظ في `request_status_history.note` |

| Response 200 | `request` object + the new history row |
|---|---|
| Errors | 400 حالة غير معروفة · 401 · 403 · 404 · 409 الانتقال غير مسموح أو نفس الحالة الحالية |

- Notes: 🔒 status and history are saved together or not at all (NFR-06, NFR-12). One notification to the employee (FR-11). Only the manager updates, not the handler (Q-3301, Q-4302).

## closeRequest · تأكيد الحل وإغلاق الطلب
- Method & path: POST /requests/{requestId}/close
- Roles: Employee (own request) · Requirements: FR-15, FR-03, NFR-08 · MVP: Must · Screen: SCR-203
- Tables: writes `requests.status` (`resolved` → `closed`) + `request_status_history` (one transaction) · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` (path) | uuid | yes | طلب الموظف نفسه، وحالته `resolved` |
| `note` | text | no | ملاحظة اختيارية تُحفظ في `request_status_history.note` |

| Response 200 | `request` object (employee view) + the new history row |
|---|---|
| Errors | 401 · 403 طلب موظف آخر · 404 · 409 الطلب مو بحالة «مكتمل» |

- Notes: 🔒 ✅ Q-1201 (2026-11-22): الموظف يأكد إن مشكلته انحلت ويقفل الطلب. **الإغلاق التلقائي:** مهمة خلفية يومية تقفل كل طلب بقي `resolved` أكثر من الإعداد `auto_close_days` (مقترح 3 أيام عمل)، وتكتب سطر السجل باسم النظام. لا إعادة فتح في النسخة الأولى. ⟵ أُضيف بقرار المنسق 2026-11-22.

## listSensitiveRequests · الشكاوى الحساسة بانتظار المراجعة
- Method & path: GET /sensitive-requests
- Roles: any user holding the permission `review_sensitive` (✅ Q-3204) · Requirements: FR-08, NFR-02, NFR-05 · MVP: Must · Screen: SCR-280
- Tables: `requests` + `request_ai_decisions` (`decision = human_review`)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `state` | text | no | `pending` (لم تُراجع، الافتراضي) · `reviewed` |

| Response 200 | `items[]`: `requestId` · `requestNumber` · `descriptionExcerpt` · `category` · `flags` · `suggestedDepartment` · `createdAt` · `sensitiveReviewedAt` |
|---|---|
| Errors | 400 · 401 · 403 بدون الصلاحية |

- Notes: 🔒 الشكوى على مدير لا تظهر لذلك المدير حتى لو عنده الصلاحية (ADR-4203). كل فتح للقائمة أو للطلب يُسجَّل في `system_logs` (FR-38). المراجع يفتح الطلب كاملًا عبر `getRequest` (عرض المراجع). ⟵ أُضيف بقرار المنسق 2026-11-22.

## releaseSensitiveRequest · تحويل الشكوى الحساسة بعد المراجعة
- Method & path: POST /sensitive-requests/{requestId}/release
- Roles: any user holding the permission `review_sensitive` · Requirements: FR-08, FR-13, NFR-02 · MVP: Must · Screen: SCR-280
- Tables: `requests` (`department_id` · `sensitive_reviewed_by` · `sensitive_reviewed_at`) · `request_status_history` · `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` (path) | uuid | yes | طلب `human_review` لم يُراجع بعد |
| `departmentId` | uuid | yes | قسم فعّال. لو الشكوى على مدير قسم، ما تُحوَّل لقسمه (ADR-4203) |
| `note` | text | no | سبب القرار، يُحفظ في السجل |

| Response 200 | `request` object (reviewer view) |
|---|---|
| Errors | 400 · 401 · 403 · 404 · 409 تمت مراجعته من قبل |

- Notes: 🔒 بعد التحويل يظهر الطلب لمدير القسم (FR-12) ويُسند حسب ✅ Q-09. ⟵ أُضيف بقرار المنسق 2026-11-22.

## rerouteRequest · إعادة توجيه الطلب لقسم آخر
- Method & path: PATCH /requests/{requestId}/department
- Roles: Department Manager · Requirements: ⚠️ لا يوجد متطلب مقابل (Q-4301) · MVP: Later · Screen: SCR-321

| Request field | Type | Required | Validation |
|---|---|---|---|
| `requestId` (path) | uuid | yes | طلب في قسم المدير |
| `departmentId` | uuid | yes | قسم آخر **فعّال** |
| `reason` | text | yes | غير فارغ |

| Response 200 | `requestId` · `requestNumber` · `departmentName` (the manager no longer sees the request after this) |
|---|---|
| Errors | 400 · 401 · 403 · 404 · 409 نفس القسم الحالي · 422 القسم موقوف |

- Notes: 🔒 empties `assigned_manager_id` and `assignee_id` (they belong to the old department — database part 1 §3.6), logs the change and the reason (FR-38), and keeps the AI decision untouched for quality checks (ADR-3202).

## listDepartments · قائمة الأقسام
- Method & path: GET /departments
- Roles: System Administrator · Requirements: FR-34 · MVP: Must · Screen: SCR-160, SCR-360
- Tables: `departments`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `isActive` | boolean | no | `true` · `false` |

| Response 200 | `items[]`: `departmentId` · `departmentName` · `isActive` · `memberCount` (computed from `users`) |
|---|---|
| Errors | 400 · 401 · 403 |

## createDepartment · إضافة قسم
- Method & path: POST /departments
- Roles: System Administrator · Requirements: FR-34 · MVP: Must · Screen: SCR-360

| Request field | Type | Required | Validation |
|---|---|---|---|
| `departmentName` | text | yes | غير فارغ · غير مكرر |

| Response 201 | `departmentId` · `departmentName` · `isActive` (`true`) |
|---|---|
| Errors | 400 · 401 · 403 · 409 الاسم مستخدم |

- Notes: 🔒 logged (FR-38). Linking the department to categories for routing is a setting (ADR-4201 → `system_settings`), not a field here.

## updateDepartment · تعديل قسم
- Method & path: PATCH /departments/{departmentId}
- Roles: System Administrator · Requirements: FR-34 · MVP: Must · Screen: SCR-360

| Request field | Type | Required | Validation |
|---|---|---|---|
| `departmentId` (path) | uuid | yes | قسم موجود |
| `departmentName` | text | yes | غير فارغ · غير مكرر |

| Response 200 | `departmentId` · `departmentName` · `isActive` |
|---|---|
| Errors | 400 · 401 · 403 · 404 · 409 الاسم مستخدم |

## deactivateDepartment · إيقاف قسم
- Method & path: POST /departments/{departmentId}/deactivate
- Roles: System Administrator · Requirements: FR-34, NFR-07 · MVP: Must · Screen: SCR-360

| Request field | Type | Required | Validation |
|---|---|---|---|
| `departmentId` (path) | uuid | yes | قسم موجود وفعّال |

| Response 200 | `departmentId` · `isActive` (`false`) · `openRequests` (count still open in it) |
|---|---|
| Errors | 400 · 401 · 403 · 404 · 409 القسم موقوف أصلًا |

- Notes: 🔒 no delete (ADR-4001). New requests are not routed to it (database part 1 §3.4). Its open requests stay with it until handled (⚠️ Q-4304).

## listMyNotifications · إشعاراتي
- Method & path: GET /notifications
- Roles: All (own) · Requirements: FR-04, FR-41 · MVP: Must · Screen: SCR-001/020/040/060, SCR-680
- Tables: `notifications` (index `(user_id, is_read, sent_at)`)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `isRead` | boolean | no | `false` = غير المقروءة فقط |

| Response 200 | `items[]`: `notificationId` · `message` · `sentAt` · `isRead` · `requestId` · `requestNumber` — newest first · plus `unreadCount` for the header (Q-4602) |
|---|---|
| Errors | 400 · 401 |

## markNotificationRead · تعليم إشعار كمقروء
- Method & path: PATCH /notifications/{notificationId}/read
- Roles: All (own) · Requirements: FR-41 · MVP: Must · Screen: SCR-680

| Request field | Type | Required | Validation |
|---|---|---|---|
| `notificationId` (path) | uuid | yes | إشعار يخص المستخدم نفسه |

| Response 200 | `notificationId` · `isRead` (`true`) · `unreadCount` |
|---|---|
| Errors | 400 · 401 · 404 غير موجود أو ليس لك |

- Notes: changes only `is_read` (database part 1 §3.10). Calling it again gives the same result.

## markAllNotificationsRead · تعليم الكل كمقروء
- Method & path: POST /notifications/read-all
- Roles: All (own) · Requirements: FR-41 · MVP: Must · Screen: SCR-680

| Request field | Type | Required | Validation |
|---|---|---|---|
| — | — | — | the user comes from the session |

| Response 200 | `updated` (how many changed) · `unreadCount` (`0`) |
|---|---|
| Errors | 401 |

## listSystemLogs · سجلات النظام
- Method & path: GET /system-logs
- Roles: System Administrator · Requirements: FR-37, NFR-22 · MVP: Should · Screen: SCR-660
- Tables: `system_logs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `userId` | uuid | no | صيغة صحيحة |
| `action` | text | no | اسم إجراء من القائمة المتفق عليها (ADR-3404 · ⚠️ Q-4605) |
| `from` · `to` | timestamp | no | على `loggedAt` |

| Response 200 | `items[]`: `logId` · `action` · `user` (`userId` · `name`, or «النظام») · `loggedAt` — newest first, without `details` |
|---|---|
| Errors | 400 · 401 · 403 |

## getSystemLog · تفاصيل سجل
- Method & path: GET /system-logs/{logId}
- Roles: System Administrator · Requirements: FR-37 · MVP: Should · Screen: SCR-660

| Request field | Type | Required | Validation |
|---|---|---|---|
| `logId` (path) | uuid | yes | صيغة uuid صحيحة |

| Response 200 | list fields + `details` (old and new value — never passwords or secrets, NFR-05) |
|---|---|
| Errors | 400 · 401 · 403 · 404 |

- Notes: logs are read-only — no update or delete operation (database part 1 §3.11).

---

# Part 2 — Member 2 (#35–54)

### 5.2 Shared objects — Part 2 (from `docs/database.md` part 2)

**`analysis` object (ADR-4403)** — returned with every AI result (insight, issue, anomaly, trend, KPI, opportunity, recommendation, report, answer) so the screen can show data source and analysis date (NFR-25, NFR-27):

| Field | Type | From (database) | Description |
|---|---|---|---|
| `analysisId` | uuid | `analysis_runs.analysis_id` | التشغيل الذي أنتج النتيجة |
| `runAt` | timestamp | `analysis_runs.run_at` | تاريخ التحليل |
| `completedAt` | timestamp | `analysis_runs.completed_at` | وقت انتهاء التحليل |
| `dataSources` | list of text | `company_data.source` via `analysis_run_company_data` | مصادر البيانات المستخدمة (بدون تكرار) |

**Result lists (ADR-4402):** extra query field `analysisId` (uuid, optional). Without it: results of the **latest run with `status = completed`**. A run that is `running` or `failed` gives an empty list, not an error.
**Permission:** all Part 2 operations are **Top Management only** (`docs/database.md` part 2 §6) — except `importCompanyData` (⚠️ Q-4404) and `askAi` / `listAskAiQuestions` (every role — ADR-4502 ✅, ⚠️ Q-4507).

---

## listInsights · رؤى الشركة
- Method & path: GET /insights
- Roles: Top Management · Requirements: FR-16, FR-28 · MVP: Must · Screen: SCR-040 · SCR-440 → SCR-443
- Tables: `insights` + `analysis` object

| Request field | Type | Required | Validation |
|---|---|---|---|
| list fields (§5.0 + `analysisId` §5.2) | — | — | — |

| Response 200 | `items[]`: `insightId` (uuid) · `description` (text) · `analysis` (object) |
|---|---|
| Errors | 400 `analysisId` أو `page` غير صحيح · 401 · 403 · 404 `analysisId` غير موجود |

- Notes: one run gives many insights (1:M — ADR-3403 at Group 3, Q-12).

## getInsight · تفاصيل رؤية
- Method & path: GET /insights/{insightId}
- Roles: Top Management · Requirements: FR-16, NFR-27 · MVP: Must · Screen: SCR-440 · SCR-443

| Request field | Type | Required | Validation |
|---|---|---|---|
| `insightId` (path) | uuid | yes | صيغة uuid صحيحة |

| Response 200 | `insightId` · `description` · `analysis` |
|---|---|
| Errors | 400 صيغة غير صحيحة · 401 · 403 · 404 الرؤية غير موجودة |

## listKpis · مؤشرات الأداء
- Method & path: GET /kpis
- Roles: Top Management · Requirements: FR-18 · MVP: Must (✅ Q-05 · ✅ Q-1401) · Screen: SCR-040 · SCR-440 → SCR-443
- Tables: `kpi_results` (✅ ADR-3405)

| Request field | Type | Required | Validation |
|---|---|---|---|
| list fields (§5.0 + `analysisId` §5.2) | — | — | — |
| `kpiName` | text | no | اسم من قائمة المؤشرات المعتمدة (⚠️ Q-1401) |

| Response 200 | `items[]`: `kpiId` · `kpiName` · `value` (decimal) · `unit` (text, may be empty) · `period` (text) · `calculatedAt` · `analysis` |
|---|---|
| Errors | 400 · 401 · 403 · 404 `analysisId` غير موجود |

- Notes: returns the latest value of each KPI (one run). The change over time is `getOperationalPerformance`.

## getOperationalPerformance · الأداء التشغيلي
- Method & path: GET /operational-performance
- Roles: Top Management · Requirements: FR-20 · MVP: Must · Screen: SCR-040 · SCR-440 → SCR-443
- Tables: `kpi_results` (no own table — ⚠️ Q-4403)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `from` | timestamp | no | ISO 8601 · الافتراضي: آخر 6 أشهر (مقترح) |
| `to` | timestamp | no | ISO 8601 · لا يسبق `from` |
| `kpiName` | text | no | اسم مؤشر معتمد |

| Response 200 | `items[]`: `kpiName` · `unit` · `points[]` (`period` · `value` · `calculatedAt` · `analysisId`) |
|---|---|
| Errors | 400 `from` بعد `to` أو تاريخ غير صحيح · 401 · 403 |

- Notes: uses the index `(kpi_name, calculated_at)` from `docs/database.md`. Only completed runs.

## listDetectedIssues · المشكلات المكتشفة
- Method & path: GET /issues
- Roles: Top Management · Requirements: FR-21, FR-25 · MVP: Must · Screen: SCR-040 · SCR-440 → SCR-443
- Tables: `detected_issues`

| Request field | Type | Required | Validation |
|---|---|---|---|
| list fields (§5.0 + `analysisId` §5.2) | — | — | — |
| `severity` | text | no | `low` · `medium` · `high` · `critical` (⚠️ Q-3404) |
| `from` · `to` | timestamp | no | على `detectedAt` · `from` لا يتجاوز `to` |

| Response 200 | `items[]`: `issueId` · `description` · `severity` · `detectedAt` · `analysis` — sorted by severity (highest first), then newest |
|---|---|
| Errors | 400 قيمة `severity` غير مسموحة · 401 · 403 · 404 `analysisId` غير موجود |

## getDetectedIssue · تفاصيل مشكلة
- Method & path: GET /issues/{issueId}
- Roles: Top Management · Requirements: FR-21, NFR-27 · MVP: Must · Screen: SCR-440 · SCR-443

| Request field | Type | Required | Validation |
|---|---|---|---|
| `issueId` (path) | uuid | yes | صيغة uuid صحيحة |

| Response 200 | `issueId` · `description` · `severity` · `detectedAt` · `analysis` |
|---|---|
| Errors | 400 · 401 · 403 · 404 المشكلة غير موجودة |

- Notes: when `recommendations` is built (Should), add `recommendationIds[]` (recommendations linked by `issue_id`).

## listOpportunities · الفرص المحددة
- Method & path: GET /opportunities
- Roles: Top Management · Requirements: FR-22 · MVP: Later · Screen: SCR-444
- Tables: `opportunities` (✅ ADR-3406, phase 3 of table creation)

| Request field | Type | Required | Validation |
|---|---|---|---|
| list fields (§5.0 + `analysisId` §5.2) | — | — | — |
| `from` · `to` | timestamp | no | على `identifiedAt` |

| Response 200 | `items[]`: `opportunityId` · `description` · `identifiedAt` · `analysis` |
|---|---|
| Errors | 400 · 401 · 403 · 404 `analysisId` غير موجود |

## listAnomalies · الحالات الشاذة
- Method & path: GET /anomalies
- Roles: Top Management · Requirements: FR-26 · MVP: Should · Screen: SCR-442 · SCR-443 (⚠️ Q-4603)
- Tables: `anomalies` (the only source of anomaly details — ADR-3404)

| Request field | Type | Required | Validation |
|---|---|---|---|
| list fields (§5.0 + `analysisId` §5.2) | — | — | — |
| `from` · `to` | timestamp | no | على `detectedAt` |

| Response 200 | `items[]`: `anomalyId` · `description` · `detectedAt` · `analysis` |
|---|---|
| Errors | 400 · 401 · 403 · 404 `analysisId` غير موجود |

## listTrends · الاتجاهات
- Method & path: GET /trends
- Roles: Top Management · Requirements: FR-27 · MVP: Should · Screen: SCR-442 · SCR-443 (⚠️ Q-4603)
- Tables: `trends`

| Request field | Type | Required | Validation |
|---|---|---|---|
| list fields (§5.0 + `analysisId` §5.2) | — | — | — |

| Response 200 | `items[]`: `trendId` · `description` · `period` (text) · `analysis` |
|---|---|
| Errors | 400 · 401 · 403 · 404 `analysisId` غير موجود |

- Notes: the `period` format is agreed with Member 3 in `docs/ai.md` (NFR-24).

## listAnalysisRuns · عمليات التحليل
- Method & path: GET /analysis-runs
- Roles: Top Management · Requirements: FR-29, NFR-25 · MVP: Must · Screen: SCR-040 ⚠️
- Tables: `analysis_runs`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `triggerType` | text | no | `on_demand` · `request` · `periodic` (`periodic` Later — FR-31) |
| `status` | text | no | `running` · `completed` · `failed` |
| `from` · `to` | timestamp | no | على `runAt` |

| Response 200 | `items[]`: `analysisId` · `triggerType` · `status` · `runAt` · `completedAt` (empty while running) · `durationSeconds` (= `completedAt − runAt`, FR-29) · `triggeredByUserId` (only for `on_demand`) — newest first |
|---|---|
| Errors | 400 قيمة غير مسموحة · 401 · 403 |

- Notes: shows **all** statuses (unlike result lists) so the screen can show a running or failed run. `request_id` is **not** returned (⚠️ Q-4406). The name of the user who ran it waits for `users` (database part 1).

## getAnalysisRun · نتائج عملية تحليل
- Method & path: GET /analysis-runs/{analysisId}
- Roles: Top Management · Requirements: FR-29, FR-30, FR-43 · MVP: Must · Screen: SCR-440 · SCR-443, SCR-040 ⚠️
- Tables: `analysis_runs` · `analysis_run_company_data` · `company_data` · counts from all result tables

| Request field | Type | Required | Validation |
|---|---|---|---|
| `analysisId` (path) | uuid | yes | صيغة uuid صحيحة |

| Response 200 | run fields (as `listAnalysisRuns`) · `dataSources[]` (`source` · `recordCount` · `fromDate` · `toDate` from `recorded_at`) · `resultCounts` (`insights` · `issues` · `kpis` · `anomalies` · `trends` · `recommendations` · `opportunities`) |
|---|---|
| Errors | 400 · 401 · 403 · 404 التشغيل غير موجود |

- Notes: the screen opens each result list with `?analysisId=…`. Error details of a `failed` run stay in `system_logs` (ADR-3407) and are not shown here.

## importCompanyData · استيراد بيانات الشركة
- Method & path: POST /company-data/imports
- Roles: ⚠️ System Administrator (Q-4401, Q-4404) · Requirements: FR-42 · MVP: Must ⚠️ · Screen: SCR-060 ⚠️
- Tables: `company_data`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `file` | file | yes | جدول بيانات (spreadsheet أو CSV) · الحجم الأقصى ⚠️ Q-4505 |
| column `source` | text | yes (each row) | غير فارغ |
| column `metric_name` | text | yes (each row) | غير فارغ |
| column `value` | decimal | yes (each row) | رقم فقط (افتراض Q-3401) |
| column `recorded_at` | timestamp | yes (each row) | تاريخ صحيح · ليس في المستقبل |

| Response 201 | `receivedRows` · `insertedRows` · `duplicateRows` (same `source` + `metric_name` + `recorded_at` — skipped, not an error) · `rejectedRows[]` (`row` · `reason`) |
|---|---|
| Errors | 400 ملف مفقود · نوع غير مسموح · عمود ناقص · كل الصفوف مرفوضة · 401 · 403 · 413 الملف أكبر من المسموح |

- Notes: 🔒 changes the data the Brain reads → write one row in `system_logs` (FR-38). Duplicates follow the `UNIQUE (source, metric_name, recorded_at)` rule (NFR-06). Rows linked to a saved run are never overwritten.

## runAnalysis · تشغيل تحليل عند الطلب
- Method & path: POST /analysis-runs
- Roles: Top Management · Requirements: FR-32, FR-29 · MVP: Must · Screen: SCR-040 ⚠️
- Tables: writes `analysis_runs` (+ `analysis_run_company_data` and results, by the engine)

| Request field | Type | Required | Validation |
|---|---|---|---|
| — | — | — | no body; the server sets `triggerType = on_demand`, `triggeredByUserId` = logged-in user, `status = running`, `runAt` = now |

| Response 202 | `analysisId` · `status` (`running`) · `runAt` — the screen checks progress with `getAnalysisRun` (ADR-4503) |
|---|---|
| Errors | 401 · 403 · 409 يوجد تحليل قيد التشغيل · 429 طلبات كثيرة (⚠️ Q-4505) |

- Notes: 🔒 heavy AI work (NFR-10). The engine sets `completed` or `failed`; a failure is logged in `system_logs` (ADR-3404).

## askAi · اسأل محور
- Method & path: POST /ask-ai/questions
- Roles: All (own chat, data by role — ADR-4502 ✅) · Requirements: FR-17 (expanded), FR-32 · MVP: Must · Screen: SCR-580
- Tables: `ask_ai_questions` (✅ ADR-3502 · Q-05 · Q-4507) · `analysis_runs` (`on_demand` — Top Management analytic questions only)

| Request field | Type | Required | Validation |
|---|---|---|---|
| `questionText` | text | yes | غير فارغ بعد حذف المسافات · الطول الأقصى ⚠️ Q-4505 |

| Response 201 | `questionId` · `questionText` · `answerText` · `status` (`answered` · `failed`) · `askedAt` · `answeredAt` · `analysis` (Top Management only) · `draftRequest` (Employee only) |
|---|---|
| Errors | 400 سؤال فارغ أو طويل · 401 · 403 · 429 طلبات كثيرة · 504 انتهت مهلة الإجابة (Q-4504) |

- Notes: 🔒 one chat for every role (ADR-4502 ✅). The server takes the role from the session first and gives the model only the result of an allowed metric for that role (`docs/ai.md` §3.5 · ADR-4501, ADR-4504) — never tables (NFR-02, NFR-05). A question outside the role → 200 with a polite refusal in `answerText`, and nothing is revealed.
- Notes: only **Top Management analytic questions** start an `on_demand` run and fill `analysis` (FR-29, FR-32); for other roles `analysis` is empty and the answer shows its source and date (`docs/ai.md` §3.3). Employee: the answer can include `draftRequest` (`description`) — sent only after the employee confirms, through `createRequest`.
- Notes: if saving question text is not approved (ADR-3502 at Group 3): the answer is returned but not saved, `questionId` is empty, and `listAskAiQuestions` waits (Q-4502).

## listAskAiQuestions · أسئلتي السابقة
- Method & path: GET /ask-ai/questions
- Roles: All (own questions only) · Requirements: FR-17 · MVP: Must (✅ Q-05 · ✅ Q-4502) · Screen: SCR-580

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `status` | text | no | `pending` · `answered` · `failed` |

| Response 200 | `items[]`: `questionId` · `questionText` · `answerText` · `status` · `askedAt` · `answeredAt` · `analysisId` — newest first |
|---|---|
| Errors | 400 · 401 · 403 |

- Notes: the user is taken from the session — no `userId` field, so nobody can read another user's questions, even with the same role (`docs/database.md`).

## listRecommendations · توصيات الذكاء الاصطناعي
- Method & path: GET /recommendations
- Roles: Top Management · Requirements: FR-19 · MVP: Should · Screen: SCR-541
- Tables: `recommendations` (✅ ADR-3501)

| Request field | Type | Required | Validation |
|---|---|---|---|
| list fields (§5.0 + `analysisId` §5.2) | — | — | — |
| `issueId` | uuid | no | مشكلة موجودة |
| `opportunityId` | uuid | no | فرصة موجودة (بعد إنشاء `opportunities`) · لا يُرسل مع `issueId` |

| Response 200 | `items[]`: `recommendationId` · `description` · `explanation` · `issueId` · `opportunityId` · `createdAt` · `analysis` |
|---|---|
| Errors | 400 `issueId` و`opportunityId` معًا · 401 · 403 · 404 |

## getRecommendation · تفاصيل توصية
- Method & path: GET /recommendations/{recommendationId}
- Roles: Top Management · Requirements: FR-19, NFR-27 · MVP: Should · Screen: SCR-541

| Request field | Type | Required | Validation |
|---|---|---|---|
| `recommendationId` (path) | uuid | yes | صيغة uuid صحيحة |

| Response 200 | list fields + `issue` (`issueId` · `description` · `severity`) or `opportunity` (`opportunityId` · `description`) or neither (general recommendation) |
|---|---|
| Errors | 400 · 401 · 403 · 404 التوصية غير موجودة |

## generateManagementReport · إنشاء تقرير إداري
- Method & path: POST /management-reports
- Roles: Top Management · Requirements: FR-23, FR-44 · MVP: Should · Screen: SCR-542
- Tables: `management_reports`

| Request field | Type | Required | Validation |
|---|---|---|---|
| `analysisId` | uuid | yes | تشغيل موجود وحالته `completed` |

| Response 201 | `reportId` · `analysisId` · `generatedAt` · `summary` · `analysis` |
|---|---|
| Errors | 400 · 401 · 403 · 404 التشغيل غير موجود · 409 التشغيل لم يكتمل أو له تقرير سابق (علاقة 1:1) · 429 |

- Notes: 🔒 heavy AI work (NFR-10). Text only; a PDF file waits for Q-3501.

## listManagementReports · التقارير الإدارية
- Method & path: GET /management-reports
- Roles: Top Management · Requirements: FR-44 · MVP: Should · Screen: SCR-542

| Request field | Type | Required | Validation |
|---|---|---|---|
| `page` · `pageSize` | integer | no | as §5.0 |
| `from` · `to` | timestamp | no | على `generatedAt` |

| Response 200 | `items[]`: `reportId` · `analysisId` · `generatedAt` · `runAt` — newest first (no `summary`, to keep the list light) |
|---|---|
| Errors | 400 · 401 · 403 |

## getManagementReport · عرض تقرير إداري
- Method & path: GET /management-reports/{reportId}
- Roles: Top Management · Requirements: FR-23, FR-44 · MVP: Should · Screen: SCR-542

| Request field | Type | Required | Validation |
|---|---|---|---|
| `reportId` (path) | uuid | yes | صيغة uuid صحيحة |

| Response 200 | `reportId` · `analysisId` · `generatedAt` · `summary` · `analysis` |
|---|---|
| Errors | 400 · 401 · 403 · 404 التقرير غير موجود |

### 5.3 Check — every operation against `docs/database.md`
- **59 / 59** operations have fields, validation, response and errors ✅ (Part 1: 34 · Part 2: 20 · v0.7: 3 · v0.8: 2).
- Every field comes from a column in `docs/database.md`, or is marked as computed (`memberCount`, `unreadCount`, `durationSeconds`, `dataSources`, `resultCounts`).
- Every table in both parts is read or written by at least one operation ✅. `request_ai_decisions` has no operation of its own — written inside `createRequest`, read inside `getRequest` (manager view), as database part 1 §3.8 says.
- The operations each table lists under «API» in database part 1 match this file ✅.
- Permissions match both database parts — except `importCompanyData` ⚠️ Q-4404 and «اسأل محور» ⚠️ Q-4507 (database part 2 limits `ask_ai_questions` to Top Management; ADR-4502 ✅ opens it to every role).

---

## 6. Screens ↔ operations (week 2)

> ✅ Matched to Group 1 `docs/screens.md` v3 (2026-11-22). The «Old code» column keeps the temporary codes of v0.2 → v0.5. ⚠️ two operation groups have no screen of their own yet (Q-4408).

| SCR | Old code | Screen | Role | MVP | Operations |
|---|---|---|---|---|---|
| SCR-080 | A1 | تسجيل الدخول · Login | All | Must | `login` |
| SCR-001/020/040/060 | A2 | الإطار العام (القائمة والهيدر) · App shell · header & menu | All | Must | `getCurrentUser`, `logout`, `listMyNotifications` |
| SCR-081 | — | حسابي · My account (✅ ADR-1003) | All | Must | `getCurrentUser`, `changeMyPassword`, `logout` |
| SCR-680 | A3 | الإشعارات · Notifications | All | Must | `listMyNotifications`, `markNotificationRead`, `markAllNotificationsRead` |
| SCR-580 | A4 | اسأل محور (شات لكل الأدوار) · Ask Mihwar chat | All (ADR-4502 ✅) | Must | `askAi`, `listAskAiQuestions` |
| SCR-201 | E1 | تقديم طلب أو شكوى · Submit request | Employee | Must | `createRequest`, `uploadRequestDocument` |
| SCR-202 | E2 | طلباتي · My requests | Employee | Must | `listMyRequests` |
| SCR-203 | E3 | تفاصيل طلبي · My request details | Employee | Must | `getRequest`, `getRequestHistory`, `downloadRequestDocument`, `closeRequest` |
| SCR-204 | E4 | تقرير حالة طلباتي · Request status report | Employee (✅ Q-08) | Must | `getRequestStatusReport` |
| SCR-320 | M1 | طلبات القسم · Department requests | Department Manager | Must | `listDepartmentRequests` |
| SCR-321 | M2 | معالجة الطلب · Handle request | Department Manager | Must | `getRequest`, `getRequestHistory`, `listDepartmentMembers`, `assignRequest`, `updateRequestStatus`, `downloadRequestDocument`, `rerouteRequest` |
| SCR-280 | — | مراجعة الشكاوى الحساسة · Sensitive review | صاحب صلاحية `review_sensitive` | Must | `listSensitiveRequests`, `releaseSensitiveRequest`, `getRequest` |
| SCR-040 · SCR-440 → SCR-443 | T1 | لوحة الإدارة العليا · Top management dashboard | Top Management | Must | `listKpis`, `getOperationalPerformance`, `listInsights`, `listDetectedIssues` |
| SCR-440 · SCR-443 | T2 | تفاصيل الرؤية أو المشكلة · Insight / issue details | Top Management | Must | `getInsight`, `getDetectedIssue`, `getAnalysisRun` |
| SCR-040 ⚠️ | T3 | عمليات التحليل · Analysis runs | Top Management | Must | `listAnalysisRuns`, `getAnalysisRun`, `runAnalysis` |
| SCR-541 | T5 | التوصيات · Recommendations | Top Management | Should | `listRecommendations`, `getRecommendation` |
| SCR-442 · SCR-443 | T6 | الحالات الشاذة والاتجاهات · Anomalies & trends | Top Management | Should | `listAnomalies`, `listTrends` |
| SCR-444 | T7 | الفرص · Opportunities | Top Management | Later | `listOpportunities` |
| SCR-542 | T8 | التقارير الإدارية · Management reports | Top Management | Should | `generateManagementReport`, `listManagementReports`, `getManagementReport` |
| SCR-160 | D1 | إدارة المستخدمين · Users | System Administrator | Must | `listUsers`, `getUser`, `createUser`, `updateUser`, `deactivateUser`, `resetUserPassword`, `listRoles`, `listDepartments` |
| SCR-360 | D2 | إدارة الأقسام · Departments | System Administrator | Must | `listDepartments`, `createDepartment`, `updateDepartment`, `deactivateDepartment` |
| SCR-161 | D3 | الأدوار والصلاحيات · Roles & permissions | System Administrator | Must | `listRoles`, `listPermissions`, `updateRolePermissions` |
| SCR-162 | D4 | إعدادات النظام · System settings | System Administrator | Should | `listSettings`, `updateSetting` |
| SCR-660 | D5 | سجلات النظام · System logs | System Administrator | Should | `listSystemLogs`, `getSystemLog` |
| SCR-060 ⚠️ | D6 | استيراد بيانات الشركة · Company data import | System Administrator ⚠️ | Must ⚠️ | `importCompanyData` |

### 6.1 Check result
- Screens: 25 (17 Must · 1 Must ⚠️ · 5 Should · 2 Later) — v0.7 adds SCR-280 · v0.8 adds SCR-081. Operations: 59.
- Every screen has at least one operation ✅.
- Every operation (59) appears on at least one screen ✅.
- Every Must screen has only Must / Must ⚠️ operations, except optional parts marked Should (attachments on SCR-201/SCR-203/SCR-321) and `rerouteRequest` (Later) on SCR-321.

### 6.2 Screen states ↔ API responses (for Group 2)
| State | API response |
|---|---|
| `loading` | الطلب مرسل والرد ما وصل |
| `success` | الرد 200 / 201 ومعه بيانات |
| `empty` | الرد 200 لكن القائمة فاضية (مثلًا: ما عندك طلبات) |
| `error` | 400 حقل ناقص أو غلط · 404 غير موجود · 409 تعارض · 500 خطأ في الخادم |
| `no-permission` | 401 غير مسجّل ← نوديه لتسجيل الدخول · 403 ما عنده صلاحية |

### 6.3 Gaps found while matching
| ID | Question | Proposed answer | Affects | Status |
|---|---|---|---|---|
| Q-4601 | إغلاق الطلب: مسودة MVP تقترح حالة `resolved` ثم `closed`، و BPMN يقول الموظف هو اللي يغلق بعد الحل (Q-1201). لو اعتُمد ذلك، نحتاج عملية جديدة للموظف في شاشة «تفاصيل طلبي». | نضيف عملية مقترحة `closeRequest` · POST /requests/{requestId}/close · Employee (own) · FR-15 — بعد حسم Q-1201 فقط. | FR-14, FR-15 · SCR-203 · Q-1201 | ✅ closed 2026-11-22 — `closeRequest` added (Q-1201) |
| Q-4602 | رمز عدد الإشعارات غير المقروءة في الهيدر: نستخدم `listMyNotifications` ونحسب العدد، أو نضيف عملية خفيفة للعدد فقط؟ | MVP: نستخدم `listMyNotifications` (يرجع العدد مع القائمة). لا عملية جديدة. | FR-04, FR-41 · A2 | open |
| Q-4408 | شاشات بدون رقم SCR: «عمليات التحليل» (`listAnalysisRuns` · `runAnalysis`) و«استيراد بيانات الشركة» (`importCompanyData`). | قسمان داخل الشاشات الموجودة: زر «تشغيل التحليل» وآخر التشغيلات في SCR-040، والاستيراد في SCR-060 — بدون شاشات جديدة في MVP. | FR-29, FR-32, FR-42 · SCR-040, SCR-060 | open |
| Q-4603 | شاشة «الحالات الشاذة والاتجاهات»: مستقلة، أو جزء من لوحة الإدارة العليا؟ (امتداد لـ Q-4405) | تكون قسمًا داخل لوحة الإدارة العليا لاحقًا (Should)، بدون شاشة مستقلة في MVP. | FR-26, FR-27 · SCR-040 · SCR-440 → SCR-443, T6 | open |


---

## 7. قرارات وأسئلة مفتوحة

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

## ADR-4003 · API fields in camelCase, mapped 1:1 to database columns
- Status: proposed
- Context: `docs/database.md` uses `snake_case` columns; `operationId` is camelCase (AI_BRIEF §8.3).
- Decision: API field names are camelCase English, each mapped 1:1 to a column (`analysis_id` → `analysisId`).
- Reason: one naming style inside the API, and an easy rule to go back to the column.
- Consequences: computed fields (`durationSeconds`, `dataSources`, `resultCounts`) are marked as computed.
- Date: 2026-11-12 · Owner group: 4

## ADR-4204 · One `getRequest` with a view per role
- Status: proposed
- Context: `docs/database.md` part 1 §3.6 — the Employee does not see `priority`, `assignee_id` or the AI decision (Q-1203); the Department Manager sees them for their own department.
- Decision: one operation per action (`getRequest`, `listMyRequests`, `listDepartmentRequests`); the server removes the fields the role may not see. The `request` object (section 5.1) lists what each role gets.
- Reason: no duplicated operations, and hiding is done on the server, not in the screen (NFR-02).
- Consequences: Group 2 designs the Employee view without priority and handler.
- Date: 2026-11-12 · Owner group: 4

## ADR-4205 · Re-routing a request empties its assignment
- Status: proposed
- Context: database part 1 §3.6 — `assigned_manager_id` and `assignee_id` must belong to the request's department.
- Decision: `rerouteRequest` changes `department_id`, empties both fields, keeps the AI decision as it was, and logs the reason.
- Reason: the old handler is in another department; the new manager assigns again.
- Consequences: the request leaves the old manager's list right away.
- Date: 2026-11-12 · Owner group: 4

## ADR-4403 · One shared `analysis` object with every AI result
- Status: proposed
- Context: NFR-25 and NFR-27 — every AI result shows its analysis run, data source and analysis date.
- Decision: every result returns `analysis` = `analysisId` · `runAt` · `completedAt` · `dataSources[]`, built from `analysis_runs` and `analysis_run_company_data` (ADR-3401).
- Reason: the same shape on every screen; no repeated columns in the result tables (as ADR-3501 says).
- Consequences: Group 2 designs one shared "source and date" component for all AI results.
- Date: 2026-11-12 · Owner group: 4

## ADR-4402 · Result lists show the latest completed run by default
- Status: proposed
- Context: every result table is linked to a run; without a rule, a list mixes old and new runs.
- Decision: when `analysisId` is not sent, list operations return results of the latest run with `status = completed`. Running or failed runs give an empty list, not an error.
- Reason: the dashboard always shows one consistent set of numbers.
- Consequences: older runs are reached from `listAnalysisRuns` → `?analysisId=…`.
- Date: 2026-11-12 · Owner group: 4

## ADR-4503 · `runAnalysis` answers 202 and runs in the background
- Status: proposed
- Context: `analysis_runs.status` has `running` (ADR-3407); analysis can be long (NFR-10).
- Decision: `runAnalysis` returns 202 with `analysisId` and `status = running`; the screen follows it with `getAnalysisRun`. A second run while one is running → 409.
- Reason: the user is not left waiting on a long request, and two heavy runs never overlap.
- Consequences: `askAi` stays immediate with a time limit (Q-4504) — a different case because the user waits for an answer.
- Date: 2026-11-12 · Owner group: 4

### Open questions

| ID | Question | Proposed answer | Raised by | Affects | Status |
|---|---|---|---|---|---|
| Q-4101 | Are the four roles fixed, or can the admin create new roles? | Fixed four roles in the MVP; the admin edits permissions only (after Q-11). So no `createRole` / `deleteRole`. | Group 4 | FR-35 · listRoles | open |
| Q-4102 | Password rules and sign-in limits: minimum length, how many failed sign-ins before a pause? (not in the requirements) | Set them as settings (FR-36) with Group 3; suggest at least 8 characters and a short pause after 5 failed tries. | Group 4 | NFR-01, NFR-05 · login, createUser | ✅ closed 2026-11-24 — 8 أحرف على الأقل · بعد 5 محاولات دخول فاشلة يتوقف الدخول 15 دقيقة · القيم إعدادات يغيّرها مدير النظام (`password_min_length` · `max_failed_logins` · `login_lockout_minutes`) |
| Q-4103 | How does a user change or reset a password? There is no requirement and no operation for it. | MVP: the System Administrator sets a new initial password; a self-service reset is later. Needs a requirement from Group 1. | Group 4 | NFR-01 · updateUser | ✅ closed 2026-11-23 — ADR-1003: `changeMyPassword` + `resetUserPassword` |
| Q-4104 | If a Department Manager moves to another department, what happens to requests they assigned or handle? | They stay as they are; the new manager of the old department can re-assign them (`assignRequest`). | Group 4 | FR-13 · updateUser | open |
| Q-4105 | What is the value format of each setting key (number, time, list)? | Group 3 lists the keys and their formats in `system_settings`; `updateSetting` checks the format per key. | Group 4 | FR-36 · updateSetting | open |
| Q-4206 | Format of `request_number` (database part 1 says `docs/api.md` decides it). | `REQ-YYYY-NNNNN`, a sequence per year (e.g. `REQ-2026-00042`), made by the server on submit. | Group 4 | FR-01, FR-03 · createRequest | open |
| Q-4207 | Max length of a request description, and how to block the same request sent twice? | Max length decided with Member 3 (`docs/ai.md`); the server rejects the same text from the same employee within a short time. | Group 4 | FR-01, NFR-06 · createRequest | open |
| Q-4208 | Clarifying question (ADR-4202 «before sending»): is the request saved before the employee answers? | No. `createRequest` returns 200 with `clarifyQuestion` and saves nothing; the employee sends again with `clarificationAnswer`. One round only; if still unclear, it is saved and goes to human review. | Group 4 | FR-08 · createRequest · Q-07, Q-3202 | open |
| Q-4209 | A user opens a request that is not theirs (or not their department): 403 or 404? | 404 in both cases, so request ids cannot be guessed. | Group 4 | NFR-03, NFR-05 · getRequest | open |
| Q-4210 | No API operation or screen for the «specialist reviewer» of `human_review` requests (sensitive complaints, security incidents — `docs/ai.md` 1.6). | After Q-3204: a permission (not a new role) + operations to list and release reviewed requests. | Group 4 | FR-08 · ADR-4203 · screens.md | ✅ closed 2026-11-22 — permission `review_sensitive` + `listSensitiveRequests` · `releaseSensitiveRequest` + SCR-280 |
| Q-4507 | **Conflict:** database part 2 limits `ask_ai_questions` to Top Management and needs `analysis_id` for every answer; ADR-4502 ✅ opens «اسأل محور» to every role. | Group 3 extends the table to every role (`user_id` of any role), and `analysis_id` is needed only for Top Management analytic questions. | Group 4 | FR-17 · askAi · database part 2 | ✅ closed 2026-11-22 — ADR-3502 (Group 3) stores every role's questions |
| Q-4304 | When a department is stopped, what happens to its open requests? | They stay with it until handled; no new requests are routed to it. Moving them needs `rerouteRequest` (Later). | Group 4 | FR-34 · deactivateDepartment | open |
| Q-4605 | The list of `action` names in `system_logs` (for the log filter and for analysis events — ADR-3404). | Group 3 (Members 3 and 4) agree on one list in `glossary.md`. | Group 4 | FR-37, FR-38 · listSystemLogs | open |
| Q-4213 | Can the employee edit or cancel a request after sending it? | Not in the phase-1 requirements → not in the MVP; Group 1 decides. | Group 4 | FR-01 · createRequest | open |
| Q-4301 | If the AI routes a request to the wrong department, who fixes it and how? | The Department Manager re-routes it (`rerouteRequest`), and the change is logged. Needs a requirement; linked to Q-07 (`docs/ai.md`). | Group 4 | FR-08 · rerouteRequest | open |
| Q-4302 | Can the assigned employee (not the manager) update the request status? | MVP: manager only, as FR-14 says. | Group 4 | FR-14 · updateRequestStatus | open |
| Q-4401 | How does company operational data enter the system, which sources exist in the MVP, and who loads it? (one question shared with `docs/ai.md`) | MVP: file import (e.g. spreadsheet) by the System Administrator; integrations later. | Group 4 | FR-42 · importCompanyData | ✅ closed 2026-11-22 — Excel/CSV uploaded by the System Administrator (`importCompanyData`) |
| Q-4405 | Does Top Management need separate views for anomalies and trends? (no "view" requirement for them) | Keep read-only operations; show them inside insights and operational performance. | Group 4 | FR-26, FR-27 · listAnomalies, listTrends | open |
| Q-4504 | Does `askAi` answer immediately, or later (background) when analysis is long? | Decide with Member 3 and Group 3 (NFR-10). Suggest: immediate with a time limit in the MVP. | Group 4 | FR-17 · askAi | open |
| Q-4403 | `getOperationalPerformance` (FR-20) has no table of its own in `docs/database.md`. What does it show that `listKpis` doesn't? | Build it from `kpi_results` over time (each KPI with its values per period). No new table. | Group 4 | FR-20 · getOperationalPerformance · Group 3 | open |
| Q-4404 | **Conflict:** `docs/database.md` part 2 says no user writes `company_data` and who loads it is open (Q-3401); our Q-4401 proposes a file import by the System Administrator. | Keep `importCompanyData` for the System Administrator through the server (the server writes, not the user directly), and ask Group 3 to add this to the permissions table. Decide with Q-3401 / Q-1401. | Group 4 | FR-42 · importCompanyData · database §6 | ✅ closed 2026-11-22 — the System Administrator loads company data by file |
| Q-4505 | Limits not in the requirements: max question length for `askAi`, max import file size, how often a user can call `runAnalysis` / `askAi` / `generateManagementReport`. | Set them with Member 3 (`docs/ai.md`) and Group 3; write them as settings (FR-36) so they can change. | Group 4 | NFR-10 · askAi, importCompanyData, runAnalysis | open |
| Q-4406 | Should Top Management see which request started a run (`request_id`)? Top Management has no access to requests. | No — `listAnalysisRuns` returns only `triggerType = request`, without the request number. | Group 4 | NFR-02 · listAnalysisRuns | open |
| Q-4601 | إغلاق الطلب: مسودة MVP تقترح حالة `resolved` ثم `closed`، و BPMN يقول الموظف هو اللي يغلق بعد الحل (Q-1201). لو اعتُمد ذلك، نحتاج عملية جديدة للموظف في شاشة «تفاصيل طلبي». | نضيف عملية مقترحة `closeRequest` · POST /requests/{requestId}/close · Employee (own) · FR-15 — بعد حسم Q-1201 فقط. | Group 4 | FR-14, FR-15 · E3 · Q-1201 | ✅ closed 2026-11-22 — `closeRequest` added (Q-1201) |
| Q-4602 | رمز عدد الإشعارات غير المقروءة في الهيدر: نستخدم `listMyNotifications` ونحسب العدد، أو نضيف عملية خفيفة للعدد فقط؟ | MVP: نستخدم `listMyNotifications` (يرجع العدد مع القائمة). لا عملية جديدة. | Group 4 | FR-04, FR-41 · A2 | open |
| Q-4603 | شاشة «الحالات الشاذة والاتجاهات»: مستقلة، أو جزء من لوحة الإدارة العليا؟ (امتداد لـ Q-4405) | تكون قسمًا داخل لوحة الإدارة العليا لاحقًا (Should)، بدون شاشة مستقلة في MVP. | Group 4 | FR-26, FR-27 · T1, T6 | open |

**Existing questions this file depends on:** Group 3 part 1: Q-3201 → Q-3204, Q-3301, Q-3302, Q-3601, ADR-3101 → ADR-3301 · Group 4 `ai.md`: Q-4201, Q-4203, Q-4205, ADR-4201 → ADR-4203 · Group 3 part 2: Q-3401 (company data source) · Q-3402 (retention) · Q-3404 (severity values) · Q-3501 (report as text or file) · ADR-3401 → ADR-3408, ADR-3501, ADR-3502 (proposed tables) · Q-1201 (who closes a request) · Q-1401 (data sources & KPIs) · Q-02 (status names) · Q-03 (MVP) · Q-04 (technology) · Q-05 (KPIs, recommendations, opportunities, Ask-AI storage) · Q-07 (AI auto vs human) · Q-08 (FR-05 actor) · Q-10 (status history) · Q-11 (permissions) · Q-12 (insights 1:M) · Q-13 (anomalies storage).
