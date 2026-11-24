docs/requirements.md

# مراجعة المتطلبات

> **Source:** Phase 1 — System Requirements Report (`reference/phase-1-analysis/Mehwar_System_Requirements_formatted.pdf`). Every *Source text* line is copied word for word from that report — do not edit it.
> **Status:** ✅ merged and approved by the coordinator (2026-11-22) — all 71 priorities are set (Group 1: Member 1 for FR, Member 2 for NFR). FR / NFR numbers never change and new FR / NFR numbers are not created here; a missing requirement becomes an open question in `docs/decisions.md`.
> **Unit** = the functional unit used for numbering screens, flows and tasks (AI_BRIEF.md section 8.4).

| | Count |
|---|---|
| Functional requirements | 44 (FR-01 → FR-44) |
| Non-functional requirements | 27 (NFR-01 → NFR-27) |
| Items with an open question ⚠️ | 8 (+3 decided ✅) |
| Priority set | 71 / 71 — FR: Must 33 · Should 9 · Later 2 — NFR: Must 25 · Should 2 · Later 0 |

> **النسخة المدموجة · 2026-11-22:** أولويات المتطلبات الوظيفية من العضو 1، وغير الوظيفية من العضو 2 (القائد)، ودمجها المنسق. النص الأصلي (*Source text*) والأرقام بدون أي تغيير.
> **قرارات المنسق المطبّقة:** ✅ Q-05 · Q-10 → Q-13 (2026-11-24) · ✅ Q-14 (FR-17) · ✅ Q-08 (FR-05) · ✅ ADR-1003 «حسابي» وكلمة المرور والخروج (FR-39 · NFR-01 · NFR-05، 2026-11-23) · تعديل أولويات FR-17 وFR-26 وFR-31 وFR-32 — كل تعديل معلّم داخل المتطلب.

---

# Functional requirements — المتطلبات الوظيفية

**A. Employee — الموظف**

## FR-01 · تقديم طلب أو شكوى
- Actor: Employee
- Description: يتيح النظام للموظف تقديم طلب أو شكوى.
- Source text: "The system shall allow the Employee to submit a request or complaint." — *Submit Request / Complaint*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must
- Priority reason: نقطة البداية الأساسية لاستخدام وصل.
- Source: Phase 1 — System Requirements Report

## FR-02 · إرفاق مستندات داعمة
- Actor: Employee
- Description: يتيح النظام للموظف إرفاق مستندات داعمة بالطلب أو الشكوى.
- Source text: "The system shall allow the Employee to attach supporting documents to a request or complaint." — *Add Supporting Documents*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Should
- Priority reason: المرفقات توضّح الطلب، لكنها ليست شرطًا لتقديمه.
- Source: Phase 1 — System Requirements Report

## FR-03 · متابعة حالة الطلب
- Actor: Employee
- Description: يتيح النظام للموظف متابعة حالة طلباته المقدَّمة.
- Source text: "The system shall allow the Employee to track the status of submitted requests." — *Track Request Status*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must
- Priority reason: يعرّف الموظف بما تم على طلبه حتى إغلاقه.
- Source: Phase 1 — System Requirements Report

## FR-04 · استلام الإشعارات
- Actor: Employee
- Description: يتيح النظام للموظف استلام الإشعارات المتعلقة بطلباته.
- Source text: "The system shall allow the Employee to receive notifications related to their requests." — *Receive Notifications*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Must
- Priority reason: يُبقي الموظف على اطلاع بتحديثات طلبه.
- Source: Phase 1 — System Requirements Report

## FR-05 · إنشاء تقرير حالة الطلبات
- Actor: Employee (+ ملخص للإدارة العليا — ✅ Q-08)
- Description: يتيح النظام للموظف إنشاء تقرير يوضح حالة طلباته.
- Source text: "The system shall allow the Employee to generate a report showing the status of their requests." — *Generate Request Status Report*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must
- Priority reason: مقترح إتاحته للموظف والإدارة العليا ضمن MVP، وصيغة التقرير تحتاج تأكيد (ADR-1204 · Q-1206).
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-08, 2026-11-22): التقرير للاثنين — الموظف يولّد تقرير حالة طلباته (SCR-204)، والإدارة العليا تشوف ملخص حالة الطلبات داخل التقرير الإداري (SCR-542). نص المتطلب يبقى كما هو.
- ✅ ADR-1204 (Status: approved · 2026-11-22) — تقرير SCR-204 للموظف عن طلباته، وملخص حالة الطلبات داخل التقرير الإداري SCR-542 للإدارة العليا، وضمن MVP. صيغة التقرير: Q-1206. ⟵ رقمه كان ADR-1201 (مستخدم عند القائد لحالات الطلب).

**B. Request management — إدارة الطلبات**

## FR-06 · تصنيف الطلب
- Actor: System
- Description: يصنّف النظام الطلبات المقدَّمة تلقائيًا حسب فئتها.
- Source text: "The system shall automatically classify submitted requests based on their category." — *Classify Request*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must
- Priority reason: التصنيف التلقائي من الوظائف الأساسية لوصل.
- Source: Phase 1 — System Requirements Report

## FR-07 · تحديد الأولوية
- Actor: System
- Description: يحدد النظام أولوية كل طلب مقدَّم.
- Source text: "The system shall determine the priority of each submitted request." — *Determine Priority*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must
- Priority reason: يساعد في ترتيب الطلبات حسب أهميتها.
- Source: Phase 1 — System Requirements Report

## FR-08 · توجيه الطلب إلى القسم
- Actor: System
- Description: يوجّه النظام كل طلب إلى القسم المناسب.
- Source text: "The system shall route each request to the appropriate department." — *Route Request to Department*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must
- Priority reason: جوهر التوجيه التلقائي للطلبات.
- Source: Phase 1 — System Requirements Report

## FR-09 · حفظ الطلب
- Actor: System
- Description: يحفظ النظام بيانات الطلب: الموظف، والقسم، والفئة، والأولوية، والحالة، والوصف، والأوقات.
- Source text: "The system shall store request information including employee, department, category, priority, status, description, and timestamps." — *Store Request*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must
- Priority reason: ضروري لحفظ الطلب ومتابعة معالجته.
- Source: Phase 1 — System Requirements Report

## FR-10 · حفظ المستندات الداعمة
- Actor: System
- Description: يحفظ النظام بيانات المستندات الداعمة المرتبطة بالطلبات.
- Source text: "The system shall store supporting document information associated with requests." — *Store Supporting Documents*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Should
- Priority reason: مرتبط بميزة المرفقات الاختيارية في FR-02.
- Source: Phase 1 — System Requirements Report

## FR-11 · إرسال إشعار بتغيّر الحالة
- Actor: System
- Description: يرسل النظام إشعارًا عند تحديث حالة الطلب.
- Source text: "The system shall send a notification when the status of a request is updated." — *Send Status Notification*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Must
- Priority reason: يدعم متابعة الموظف للتحديثات المهمة على طلبه.
- Source: Phase 1 — System Requirements Report

**C. Department Manager — مدير القسم**

## FR-12 · عرض طلبات القسم
- Actor: Department Manager
- Description: يتيح النظام لمدير القسم عرض الطلبات المرتبطة بقسمه.
- Source text: "The system shall allow the Department Manager to view requests associated with their department." — *View Department Requests*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must
- Priority reason: يمكّن مدير القسم من متابعة الطلبات المحالة لقسمه.
- Source: Phase 1 — System Requirements Report

## FR-13 · إسناد الطلب
- Actor: Department Manager
- Description: يتيح النظام لمدير القسم إسناد الطلبات للمعالجة.
- Source text: "The system shall allow the Department Manager to assign requests for handling." — *Assign Request*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must
- Priority reason: ضروري لتحديد من ينفّذ المعالجة داخل القسم (تطبيقًا لقرار Q-09).
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-09): الموظف ما يختار. النظام يوجّه الطلب للقسم تلقائيًا (FR-08)، ومدير القسم يقرر مين يعالجه: هو بنفسه أو موظف في قسمه. المجموعة 3 تضيف حقل للشخص المسؤول عن المعالجة.

## FR-14 · تحديث حالة الطلب
- Actor: Department Manager
- Description: يتيح النظام لمدير القسم تحديث حالة الطلبات.
- Source text: "The system shall allow the Department Manager to update the status of requests." — *Update Request Status*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must
- Priority reason: ضروري لإظهار تقدّم المعالجة للموظف والإدارة.
- Source: Phase 1 — System Requirements Report

## FR-15 · استمرار سير عمل الطلب
- Actor: System
- Description: يحافظ النظام على سير عمل الطلب حتى يُحل أو يحتاج إلى إجراء إضافي.
- Source text: "The system shall maintain the request workflow until the request is resolved or requires further action." — *Maintain Request Workflow*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must
- Priority reason: يربط مراحل الطلب من الاستلام حتى الإغلاق.
- Source: Phase 1 — System Requirements Report

**D. Top Management — الإدارة العليا**

## FR-16 · عرض رؤى الشركة
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض رؤى الشركة التي يولّدها النظام.
- Source text: "The system shall allow Top Management to view company insights generated by the system." — *View Company Insights*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: من المخرجات الرئيسية للعقل التشغيلي.
- Source: Phase 1 — System Requirements Report

## FR-17 · سؤال الذكاء الاصطناعي عن بيانات الشركة
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا طرح أسئلة عن بيانات الشركة واستلام تحليل يولّده الذكاء الاصطناعي.
- Source text: "The system shall allow Top Management to submit questions about company data and receive AI-generated analysis." — *Ask AI About Company Data*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Must  ⟵ عُدّلت بقرار المنسق 2026-11-22 (كانت Should)
- Priority reason: «اسأل محور» شات لكل الأدوار (✅ Q-14)، وهو جزء أساسي من محور.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-05): جدول `ask_ai_questions` لكل مستخدم (ADR-3502).
- ✅ Decided (Q-14): «اسأل محور» شات لكل الأدوار، مو للإدارة العليا بس. كل مستخدم له محادثته ويشوف حسب صلاحيات دوره: الموظف طلباته، ومدير القسم قسمه، والإدارة العليا الشركة كلها، ومدير النظام بيانات الإدارة. وفي محور ذكاء اصطناعي واحد فقط. نص المتطلب أعلاه يبقى كما هو من المرحلة الأولى.
- 💡 Proposed: ADR-1502 (Status: proposed) — تُحفظ محادثة كل مستخدم في «اسأل محور» (السؤال والجواب والتاريخ)، ويقرأها صاحبها فقط. ⟵ عُدّل بسبب Q-14 (كان: لا يُحفظ نص السؤال والجواب).

## FR-18 · متابعة مؤشرات الأداء
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا متابعة مؤشرات أداء الشركة (KPIs).
- Source text: "The system shall allow Top Management to monitor company KPIs." — *Monitor Company KPIs*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: يساعد الإدارة على متابعة الأداء التشغيلي.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-05): جدول `kpi_results` (ADR-3405).
- 💡 Proposed: ADR-1404 (Status: proposed) — المؤشرات تُحسب من بيانات الشركة ونتائج التحليل، بدون كيان مستقل في MVP.

## FR-19 · عرض توصيات الذكاء الاصطناعي
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض التوصيات التي يولّدها الذكاء الاصطناعي.
- Source text: "The system shall allow Top Management to view AI-generated recommendations." — *View AI Recommendations*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Should
- Priority reason: تضيف قيمة لاتخاذ القرار بعد توفّر التحليلات الأساسية.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-05, 2026-11-24): جدول `recommendations` (ADR-3501).
- ✅ Decided: ADR-1503 (approved 2026-11-24) — التوصيات تُمثَّل كبيانات منظمة مرتبطة بعملية التحليل عند تنفيذها.

## FR-20 · عرض الأداء التشغيلي
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض معلومات الأداء التشغيلي.
- Source text: "The system shall allow Top Management to view operational performance information." — *View Operational Performance*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: يعطي الإدارة صورة عن أداء العمليات.
- Source: Phase 1 — System Requirements Report

## FR-21 · عرض المشكلات المكتشفة
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض المشكلات التشغيلية التي اكتشفها محرك التحليل.
- Source text: "The system shall allow Top Management to view operational issues detected by the AI Analytics Engine." — *View Detected Issues*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: يُبرز المشكلات التي تحتاج انتباهًا.
- Source: Phase 1 — System Requirements Report

## FR-22 · عرض الفرص المحددة
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض الفرص المستخلصة من تحليل بيانات الشركة.
- Source text: "The system shall allow Top Management to view opportunities identified through company data analysis." — *View Identified Opportunities*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Later
- Priority reason: يمكن إضافته توسعةً بعد استقرار التحليل الأساسي.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-05, 2026-11-24): جدول `opportunities` (ADR-3406).
- ✅ Decided: ADR-1503 (approved 2026-11-24) — الفرص تُمثَّل كبيانات منظمة عند تنفيذها (الأولوية Later).

## FR-23 · إنشاء تقرير إداري
- Actor: System (for Top Management)
- Description: ينشئ النظام تقارير إدارية تتضمن تحليل الشركة والرؤى والمؤشرات والمعلومات التشغيلية.
- Source text: "The system shall generate management reports containing relevant company analysis, insights, KPIs, and operational information." — *Generate Management Report*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Should
- Priority reason: مفيد للتوثيق والمشاركة، ويمكن عرض المعلومات داخل المنصة مبدئيًا.
- Source: Phase 1 — System Requirements Report

**E. AI analytics — التحليل بالذكاء الاصطناعي**

## FR-24 · تحليل بيانات الشركة
- Actor: AI Analytics Engine
- Description: يحلل النظام بيانات الشركة التشغيلية المتاحة عبر محرك التحليل بالذكاء الاصطناعي.
- Source text: "The system shall analyze available company operational data through the AI Analytics Engine." — *Analyze Company Data*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: الأساس الذي تعتمد عليه وظائف العقل التشغيلي.
- Source: Phase 1 — System Requirements Report

## FR-25 · اكتشاف المشكلات التشغيلية
- Actor: AI Analytics Engine
- Description: يكتشف النظام المشكلات التشغيلية المحتملة من بيانات الشركة المحللة.
- Source text: "The system shall detect potential operational issues from analyzed company data." — *Detect Operational Issues*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: ضروري لاكتشاف الخلل في العمليات.
- Source: Phase 1 — System Requirements Report

## FR-26 · اكتشاف الحالات الشاذة
- Actor: AI Analytics Engine
- Description: يكتشف النظام الحالات الشاذة في بيانات الشركة المتاحة.
- Source text: "The system shall detect anomalies within the available company data." — *Detect Anomalies*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Should  ⟵ عُدّلت بقرار المنسق 2026-11-22 (كانت Later)
- Priority reason: يُضاف بعد التحليل الأساسي، والجدول والعمليات مصممة له (Should في قاعدة البيانات والـ API).
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-13, 2026-11-24): التفاصيل في جدول `anomalies`، وإشارة مختصرة في سجلات النظام (ADR-3404).
- ✅ Decided: ADR-1403 (approved 2026-11-24) — تفاصيل الحالة الشاذة في كيان Anomaly، وحدث اكتشافها أو تحديثها يُسجَّل في SystemLog للتتبّع.

## FR-27 · تحديد الاتجاهات
- Actor: AI Analytics Engine
- Description: يحدد النظام الاتجاهات من بيانات الشركة المحللة.
- Source text: "The system shall identify trends from analyzed company data." — *Identify Trends*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Should
- Priority reason: يعزّز فهم التغيّرات، لكنه ليس شرطًا للوظائف الأساسية.
- Source: Phase 1 — System Requirements Report

## FR-28 · توليد الرؤى
- Actor: AI Analytics Engine
- Description: يولّد النظام رؤى مبنية على نتائج تحليل بيانات الشركة.
- Source text: "The system shall generate insights based on the results of company data analysis." — *Generate Insights*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: يحوّل نتائج التحليل إلى معلومات قابلة للاستخدام.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-12, 2026-11-24): العلاقة 1:M (ADR-3403).
- ✅ Decided: ADR-1402 (approved 2026-11-24) — العلاقة بين AnalysisRun وInsight تكون 1:M.

## FR-29 · حفظ عمليات التحليل
- Actor: System
- Description: يسجّل النظام كل عملية تحليل مع ما أطلقها ووقت تنفيذها.
- Source text: "The system shall record each analysis run, including its trigger and execution time." — *Store Analysis Runs*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: يوثّق عمليات التحليل ويسمح بالرجوع إليها.
- Source: Phase 1 — System Requirements Report

## FR-30 · حفظ نتائج التحليل
- Actor: System
- Description: يحفظ النظام المشكلات والحالات الشاذة والاتجاهات والرؤى المرتبطة بعملية التحليل.
- Source text: "The system shall store detected issues, anomalies, trends, and insights associated with an analysis run." — *Store Analysis Results*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: ضروري لحفظ مخرجات التحليل وعرضها.
- Source: Phase 1 — System Requirements Report

## FR-31 · التحليل الدوري
- Actor: System
- Description: يدعم النظام التحليل الدوري لبيانات الشركة.
- Source text: "The system shall support periodic analysis of company data." — *Periodic Analysis*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Later  ⟵ عُدّلت بقرار المنسق 2026-11-22 (كانت Must)
- Priority reason: نبدأ بالتحليل عند الطلب (FR-32) في النسخة الأولى، والتشغيل المجدول بعدها.
- Source: Phase 1 — System Requirements Report

## FR-32 · التحليل عند الطلب
- Actor: Top Management
- Description: يدعم النظام تحليلًا تطلقه الإدارة العليا عند الطلب.
- Source text: "The system shall support analysis triggered by a request from Top Management." — *On-Demand Analysis*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Must  ⟵ عُدّلت بقرار المنسق 2026-11-22 (كانت Should)
- Priority reason: التحليل عند الطلب هو اللي يشغّل «اسأل محور» وزر «تشغيل التحليل» للإدارة العليا في النسخة الأولى.
- Source: Phase 1 — System Requirements Report

**F. System Administrator — مدير النظام**

## FR-33 · إدارة المستخدمين
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام إدارة مستخدمي النظام.
- Source text: "The system shall allow the System Administrator to manage system users." — *Manage Users*
- Module: Users & permissions · Unit 1
- Priority (MVP): Must
- Priority reason: من الوظائف الإدارية الأساسية للمنصة.
- Source: Phase 1 — System Requirements Report

## FR-34 · إدارة الأقسام
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام إدارة الأقسام.
- Source text: "The system shall allow the System Administrator to manage departments." — *Manage Departments*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must
- Priority reason: ضروري لتنظيم الأقسام وتوجيه الطلبات إليها.
- Source: Phase 1 — System Requirements Report

## FR-35 · إدارة الأدوار والصلاحيات
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام إدارة الأدوار والصلاحيات.
- Source text: "The system shall allow the System Administrator to manage roles and permissions." — *Manage Roles and Permissions*
- Module: Users & permissions · Unit 1
- Priority (MVP): Must
- Priority reason: أساسي للتحكم في الوصول إلى وظائف المنصة وبياناتها.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-11, 2026-11-24): جدولا `permissions` و`role_permissions` (ADR-3101).
- ✅ Decided: ADR-1102 (approved 2026-11-24) — إضافة كيان Permission وجدول ربط RolePermission، بمراجعة المجموعة 3.

## FR-36 · إدارة إعدادات النظام
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام إدارة إعدادات النظام.
- Source text: "The system shall allow the System Administrator to manage system settings." — *Manage System Settings*
- Module: Users & permissions · Unit 1
- Priority (MVP): Should
- Priority reason: مهم للإدارة، ويمكن البدء بإعدادات افتراضية محددة.
- Source: Phase 1 — System Requirements Report

## FR-37 · عرض سجلات النظام
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام عرض سجلات النظام.
- Source text: "The system shall allow the System Administrator to view system logs." — *View System Logs*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Should
- Priority reason: مهم للمراجعة والدعم، لكنه ليس وظيفة استخدام يومية أساسية.
- Source: Phase 1 — System Requirements Report

## FR-38 · تسجيل سجلات النظام
- Actor: System
- Description: يسجّل النظام الإجراءات المهمة في سجلات النظام.
- Source text: "The system shall record relevant system actions in the System Logs." — *Record System Logs*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Must
- Priority reason: يدعم التتبّع والتدقيق ومعرفة الأحداث داخل النظام.
- Source: Phase 1 — System Requirements Report

**G. Data management — إدارة البيانات**

## FR-39 · إدارة بيانات المستخدمين
- Actor: System
- Description: يحفظ النظام بيانات المستخدمين وأدوارهم وارتباطهم بالأقسام.
- Source text: "The system shall store user information, roles, and department associations." — *User Data Management*
- Module: Users & permissions · Unit 1
- Priority (MVP): Must
- Priority reason: ضروري لحفظ بيانات حسابات المستخدمين.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (ADR-1003 · Q-1003, 2026-11-23): كل مستخدم يشوف بيانات حسابه (الاسم، البريد، الدور، القسم) في صفحة «حسابي» SCR-081 للقراءة فقط، والتعديل يبقى لمدير النظام (FR-33). نص المتطلب أعلاه يبقى كما هو من المرحلة الأولى. ⟵ أُضيف بقرار المنسق 2026-11-23

## FR-40 · إدارة بيانات الطلبات
- Actor: System
- Description: يحتفظ النظام بسجلات الطلبات وعلاقاتها بالموظفين والأقسام والمدراء.
- Source text: "The system shall maintain request records and their relationships with employees, departments, and managers." — *Request Data Management*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must
- Priority reason: ضروري لاستمرار عمل وصل.
- Source: Phase 1 — System Requirements Report

## FR-41 · إدارة الإشعارات
- Actor: System
- Description: يحفظ النظام الإشعارات المرتبطة بالمستخدمين والطلبات، مع حالة كل إشعار.
- Source text: "The system shall store notifications associated with users and requests, including notification status." — *Notification Management*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Must
- Priority reason: ضروري لإرسال الإشعارات ومتابعتها.
- Source: Phase 1 — System Requirements Report

## FR-42 · بيانات الشركة التشغيلية
- Actor: System
- Description: يحفظ النظام بيانات الشركة التشغيلية التي يستخدمها محرك التحليل.
- Source text: "The system shall store company operational data used by the AI Analytics Engine." — *Company Operational Data*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: يوفّر البيانات التي يعتمد عليها التحليل التشغيلي.
- Source: Phase 1 — System Requirements Report

## FR-43 · ربط بيانات التحليل
- Actor: System
- Description: يربط النظام نتائج التحليل بعملية التحليل الخاصة بها.
- Source text: "The system shall associate analysis results with their corresponding analysis run." — *Analysis Data Association*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must
- Priority reason: يربط نتائج التحليل بعملية التحليل الخاصة بها.
- Source: Phase 1 — System Requirements Report

## FR-44 · حفظ التقارير الإدارية
- Actor: System
- Description: يحفظ النظام التقارير الإدارية المُنشأة والتحليل المرتبط بها.
- Source text: "The system shall store generated management reports and their associated analysis." — *Management Report Storage*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Should
- Priority reason: يمكن تأجيل الحفظ المستقل إذا عُرضت التقارير داخل المنصة مبدئيًا.
- Source: Phase 1 — System Requirements Report

---

# Non-functional requirements — المتطلبات غير الوظيفية

**A. Security — الأمان**

## NFR-01 · المصادقة
- Category: Security
- Description: يطلب النظام من المستخدمين تسجيل الدخول قبل الوصول إلى الوظائف المحمية.
- Source text: "The system shall require users to authenticate before accessing protected system functions." — *Authentication*
- Applies to: Unit 1 · Users & permissions
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): المصادقة شرط لحماية جميع وظائف المنصة ومنع الدخول غير المصرّح.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (ADR-1003 · Q-1003 · Q-4103, 2026-11-23): المصادقة تشمل زر «تسجيل الخروج» في كل الشاشات، وتغيير كلمة المرور من «حسابي» SCR-081، وكلمة مؤقتة يعيّنها مدير النظام عند النسيان ويلزم تغييرها أول دخول. «نسيت كلمة المرور» الذاتية بالبريد Later. نص المتطلب أعلاه يبقى كما هو. ⟵ أُضيف بقرار المنسق 2026-11-23

## NFR-02 · التحكم بالوصول حسب الدور
- Category: Security
- Description: يطبّق النظام صلاحيات الوصول بناءً على دور المستخدم.
- Source text: "The system shall enforce access permissions based on the user's assigned role." — *Role-Based Access Control*
- Applies to: Unit 1 · Users & permissions
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): الصلاحيات حسب الدور ضرورة لأن المنصة تخدم أربعة أدوار ببيانات ووظائف مختلفة.
- Source: Phase 1 — System Requirements Report

## NFR-03 · التحكم بالوصول حسب القسم
- Category: Security
- Description: يقصر النظام وصول مدير القسم على طلبات القسم المصرَّح له.
- Source text: "The system shall restrict Department Manager access to requests associated with their authorized department." — *Department Access Control*
- Applies to: Unit 1 · Users & permissions
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): منع مدير القسم من الاطلاع على طلبات الأقسام الأخرى شرط خصوصية أساسي.
- Source: Phase 1 — System Requirements Report

## NFR-04 · التحكم بالوصول الإداري
- Category: Security
- Description: وظائف إدارة النظام متاحة لمدير النظام فقط.
- Source text: "System administration functions shall only be accessible to the System Administrator." — *Administrative Access Control*
- Applies to: Unit 1 · Users & permissions
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): حصر أدوات الإدارة بمدير النظام يمنع التعديلات والإجراءات غير المصرّح بها.
- Source: Phase 1 — System Requirements Report

## NFR-05 · حماية البيانات
- Category: Security
- Description: يحمي النظام بيانات المستخدمين والطلبات والبيانات التشغيلية وبيانات التحليل من الوصول غير المصرَّح به.
- Source text: "The system shall protect user, request, company operational, and analysis data from unauthorized access." — *Data Protection*
- Applies to: Unit 1 · Users & permissions
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): حماية بيانات الموظفين والشركة والتحليل شرط قبل أي تشغيل فعلي.
- Source: Phase 1 — System Requirements Report

**B. Data integrity — سلامة البيانات**

## NFR-06 · اتساق البيانات
- Category: Data integrity
- Description: يحافظ النظام على الاتساق بين المستخدمين والأقسام والطلبات والإشعارات وسجلات التحليل والتقارير.
- Source text: "The system shall maintain consistency between users, departments, requests, notifications, analysis records, and reports." — *Data Consistency*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): اتساق بيانات المستخدمين والطلبات والإشعارات والتحليل يمنع ظهور معلومات متناقضة.
- Source: Phase 1 — System Requirements Report

## NFR-07 · سلامة العلاقات المرجعية
- Category: Data integrity
- Description: يحافظ النظام على صحة العلاقات بين السجلات المرتبطة في قاعدة البيانات.
- Source text: "The system shall maintain valid relationships between related records in the database." — *Referential Integrity*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): صحة الروابط بين الطلبات والمستخدمين والأقسام ونتائج التحليل ضرورية لعمل المسارات.
- Source: Phase 1 — System Requirements Report

## NFR-08 · تتبّع الطلب
- Category: Data integrity
- Description: يحتفظ النظام بالسجل التاريخي والأوقات اللازمة لتتبّع دورة حياة الطلب.
- Source text: "The system shall maintain the history and timestamps required to track the request lifecycle." — *Request Traceability*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): تاريخ تغيّر حالة الطلب وأوقاته مطلوبان للمتابعة والمساءلة؛ آلية التخزين تُحسم في Q-10.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-10, 2026-11-24): جدول `request_status_history` (ADR-3201).

**C. Performance — الأداء**

## NFR-09 · سرعة الاستجابة
- Category: Performance
- Description: يقدّم النظام أداءً سريع الاستجابة أثناء الاستخدام المعتاد.
- Source text: "The system shall provide responsive performance during normal user operations." — *System Responsiveness*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): يجب أن تعمل عمليات الاستخدام اليومية دون بطء يعيق إرسال الطلبات ومتابعتها؛ معيار الزمن يحتاج تحديدًا.
- Source: Phase 1 — System Requirements Report

## NFR-10 · أداء معالجة الذكاء الاصطناعي
- Category: Performance
- Description: يعالج النظام طلبات التحليل بالذكاء الاصطناعي خلال وقت مقبول لحجم العمل المتوقع.
- Source text: "The system shall process AI analysis requests within an acceptable time for the expected system workload." — *AI Processing Performance*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): معالجة التصنيف والتحليل ضمن زمن مقبول لازمة لأن الذكاء الاصطناعي جزء من المسار الأساسي؛ الزمن المستهدف مفتوح.
- Source: Phase 1 — System Requirements Report

## NFR-11 · الوصول المتزامن
- Category: Performance
- Description: يدعم النظام دخول عدة مستخدمين مصرَّح لهم في الوقت نفسه.
- Source text: "The system shall support concurrent access by multiple authorized users." — *Concurrent Access*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): تشغيل عدة موظفين ومديري أقسام في الوقت نفسه شرط واقعي للنسخة الأولى، ويحتاج حجم حمل معتمدًا.
- Source: Phase 1 — System Requirements Report

**D. Reliability — الموثوقية**

## NFR-12 · معالجة الأخطاء
- Category: Reliability
- Description: يتعامل النظام مع أخطاء النظام والمعالجة دون إتلاف البيانات الموجودة.
- Source text: "The system shall handle system and processing errors without corrupting existing data." — *Error Handling*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): أخطاء النظام أو خدمة الذكاء الاصطناعي يجب ألا تتلف طلبات أو تحليلات محفوظة.
- Source: Phase 1 — System Requirements Report

## NFR-13 · موثوقية معالجة الطلبات
- Category: Reliability
- Description: يحافظ النظام على بيانات الطلب طوال دورة إدارة الطلب.
- Source text: "The system shall preserve request information throughout the request management lifecycle." — *Reliable Request Processing*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): المحافظة على الطلب حتى المعالجة والإغلاق جوهر تجربة «وصل».
- Source: Phase 1 — System Requirements Report

## NFR-14 · موثوقية التحليل
- Category: Reliability
- Description: يحتفظ النظام بسجلات عمليات التحليل المكتملة ونتائجها.
- Source text: "The system shall maintain records of completed AI analysis runs and their results." — *Analysis Reliability*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): التحليل المختصر داخل MVP يحتاج حفظ عمليات التحليل المكتملة ونتائجها بلا فقد.
- Source: Phase 1 — System Requirements Report

**E. Availability & scalability — الإتاحة وقابلية التوسع**

## NFR-15 · الإتاحة
- Category: Availability & scalability
- Description: يكون النظام متاحًا للمستخدمين المصرَّح لهم خلال فترات التشغيل المعتادة.
- Source text: "The system shall be available to authorized users during normal operating periods." — *Availability*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): الإتاحة خلال أوقات التشغيل المعتادة شرط لاستخدام المنصة، وتُحدد نافذة التشغيل مع الفريق.
- Source: Phase 1 — System Requirements Report

## NFR-16 · قابلية التوسع
- Category: Availability & scalability
- Description: يدعم النظام الزيادة المستقبلية في المستخدمين والأقسام والطلبات وبيانات الشركة التشغيلية.
- Source text: "The system shall support future increases in users, departments, requests, and company operational data." — *Scalability*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Should
- Priority rationale (proposed; Member 2): الاستعداد لزيادة كبيرة مستقبلًا مهم، لكن حدود التوسع والأحمال الكبيرة يمكن تحسينها بعد تثبيت النسخة الأولى.
- Source: Phase 1 — System Requirements Report

## NFR-17 · قابلية التوسع المعيارية
- Category: Availability & scalability
- Description: تسمح معمارية النظام بتوسيع مكوّنات إدارة الطلبات وتحليل الذكاء الاصطناعي وإدارة النظام كلٌّ على حدة.
- Source text: "The system architecture shall allow the request management, AI analytics, and system administration components to be extended independently." — *Modular Scalability*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): الفصل المعماري بين «وصل» والتحليل والإدارة قرار تأسيسي يمنع إعادة البناء لاحقًا؛ لا يفرض خدمات منفصلة.
- Source: Phase 1 — System Requirements Report

**F. Usability — سهولة الاستخدام**

## NFR-18 · سهولة الاستخدام
- Category: Usability
- Description: يوفّر النظام واجهة سهلة وواضحة لجميع الأدوار المحددة.
- Source text: "The system shall provide an intuitive interface for all defined Actors." — *Usability*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): سهولة الاستخدام بالعربية واتجاه RTL أساسية لتعامل الموظفين والأدوار الأربعة.
- Source: Phase 1 — System Requirements Report

## NFR-19 · واجهة حسب الدور
- Category: Usability
- Description: يعرض النظام الوظائف والمعلومات بحسب دور المستخدم.
- Source text: "The system shall display functions and information according to the user's role." — *Role-Specific Interface*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): إظهار الوظائف والمعلومات المناسبة لكل دور ضروري للوضوح وداعم للأمان، مع فحص الصلاحيات على الخادم.
- Source: Phase 1 — System Requirements Report

## NFR-20 · وضوح معلومات الحالة
- Category: Usability
- Description: يعرض النظام بوضوح حالات الطلبات والإشعارات والمؤشرات والرؤى ونتائج التحليل للمستخدمين المصرَّح لهم.
- Source text: "The system shall clearly display request statuses, notifications, KPIs, insights, and analysis results to authorized users." — *Clear Status Information*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): وضوح حالة الطلب والإشعارات ومخرجات التحليل شرط للمتابعة الصحيحة واتخاذ القرار.
- Source: Phase 1 — System Requirements Report

**G. Maintainability — قابلية الصيانة**

## NFR-21 · هيكل برمجي معياري
- Category: Maintainability
- Description: يستخدم النظام هيكلًا معياريًا يسهّل الصيانة والتطوير المستقبلي.
- Source text: "The system shall use a modular structure that facilitates maintenance and future development." — *Modular Codebase*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): تنظيم المكونات برمجيًا من البداية يقلل التداخل بين الوحدات الأساسية في MVP.
- Source: Phase 1 — System Requirements Report

## NFR-22 · السجلات والمراقبة
- Category: Maintainability
- Description: يحتفظ النظام بسجلات النظام لدعم المراقبة وحل المشكلات.
- Source text: "The system shall maintain system logs to support monitoring and troubleshooting." — *Logging and Monitoring*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): السجلات اللازمة للأخطاء والتغييرات الحساسة مطلوبة للتشخيص والتدقيق منذ الإصدار الأول.
- Source: Phase 1 — System Requirements Report

## NFR-23 · إدارة الإعدادات
- Category: Maintainability
- Description: يتيح النظام إعدادات قابلة للتهيئة عبر مدير النظام.
- Source text: "The system shall allow configurable system settings through the System Administrator." — *Configuration Management*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Should
- Priority rationale (proposed; Member 2): لوحة إدارة الإعدادات القابلة للتغيير مهمة، لكن يمكن تأجيل واجهتها المتقدمة مع استمرار ضبط الإعدادات الأساسية بأمان.
- Source: Phase 1 — System Requirements Report

**H. AI analysis quality — جودة تحليل الذكاء الاصطناعي**

## NFR-24 · مخرجات ذكاء اصطناعي منظمة
- Category: AI analysis quality
- Description: ينتج محرك التحليل نتائج تحليل منظمة يمكن حفظها واستخدامها في النظام.
- Source text: "The AI Analytics Engine shall produce structured analysis results that can be stored and used by the system." — *Structured AI Output*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): توحيد بنية نتائج الذكاء الاصطناعي ضروري لحفظ الرؤى وعرضها وربطها بالطلبات والبيانات.
- Source: Phase 1 — System Requirements Report

## NFR-25 · تتبّع نتائج التحليل
- Category: AI analysis quality
- Description: يمكن تتبّع كل نتيجة تحليل بالذكاء الاصطناعي إلى عملية التحليل الخاصة بها.
- Source text: "Each AI analysis result shall be traceable to its corresponding analysis run." — *Analysis Traceability*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): ربط كل نتيجة بعملية تحليلها ضروري للتدقيق والموثوقية وفق FR-29 وFR-30 وFR-43.
- Source: Phase 1 — System Requirements Report

## NFR-26 · معالجة متسقة
- Category: AI analysis quality
- Description: يتبع محرك التحليل مسارات تحليل محددة عند معالجة بيانات الشركة.
- Source text: "The AI Analytics Engine shall follow defined analysis workflows when processing company data." — *Consistent Processing*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): مسار معالجة محدد يساعد في ثبات نتائج التحليل والتعامل مع الحالات غير الواضحة.
- Source: Phase 1 — System Requirements Report

## NFR-27 · سياق النتيجة
- Category: AI analysis quality
- Description: تتضمن الرؤى والمشكلات والحالات الشاذة والاتجاهات معلومات كافية ليفهم المستخدمون المصرَّح لهم النتيجة.
- Source text: "AI-generated insights, detected issues, anomalies, and trends shall contain sufficient information for authorized users to understand the generated result." — *Result Context*
- Applies to: the whole system (Unit 0)
- Priority (MVP): Must
- Priority rationale (proposed; Member 2): كل نتيجة معروضة تحتاج سياقًا مفهومًا ومصدر بيانات وتاريخ التحليل حتى لا تُفهم خارج سياقها.
- Source: Phase 1 — System Requirements Report
---

# قرارات وأسئلة مفتوحة

> مقترحات المجموعة 1 (العضو 1 للمتطلبات الوظيفية). كلها `Status: proposed` لين يعتمدها المنسق، إلا المعلّم ✅. تنتقل إلى `docs/decisions.md`.
> أولويات المتطلبات غير الوظيفية (العضو 2) بدون قرارات جديدة، وسؤالها Q-10 جوابه المعتمد (2026-11-24) في «تصميم قاعدة البيانات» (ADR-3201: جدول `request_status_history`).

## ADR-1003 · صفحة «حسابي» وكلمة المرور وتسجيل الخروج لكل الأدوار
- Status: approved (المنسق · 2026-11-23)
- Answers: Q-1003 · Q-4103
- Affects: FR-39 · NFR-01 · NFR-05
- Decision: صفحة «حسابي» SCR-081 لكل الأدوار: بيانات الحساب للقراءة، وتغيير كلمة المرور (الحالية + الجديدة + التأكيد)، وتسجيل الخروج. بطاقة المستخدم وزر «تسجيل الخروج» أسفل القائمة الجانبية. عند النسيان يعيّن مدير النظام كلمة مؤقتة (SCR-160) ويلزم تغييرها أول دخول. الاستعادة الذاتية بالبريد Later.
- Why: بدونها ما يقدر المستخدم يغيّر كلمة مروره ولا يخرج بأمان، وهذا جزء من المصادقة وحماية البيانات (NFR-01 · NFR-05). لا رقم FR جديد (AI_BRIEF 8.4)، والنص الأصلي ما يتغير، مثل Q-14 مع FR-17.

## ADR-1204 · تقرير حالة الطلبات للاثنين: SCR-204 للموظف عن طلباته، وملخص حالة الطلبات داخل التقرير الإداري SCR-542 للإدارة العليا، وضمن MVP.
- Status: approved (المنسق · 2026-11-22) ⟵ رقمه كان ADR-1201
- Answers: Q-08
- Affects: FR-05
- Decision: SCR-204 للموظف عن طلباته فقط، وملخص حالة الطلبات داخل التقرير الإداري SCR-542 للإدارة العليا، وضمن MVP.
- Why: يحل التعارض بين تقرير المتطلبات (الموظف) وجدول حالات الاستخدام (الإدارة العليا) بدون حذف أي منهما.
## ADR-1502 · تُحفظ محادثة كل مستخدم في «اسأل محور» (السؤال والجواب والتاريخ)، ويقرأها صاحبها فقط.
- Status: approved (المنسق · 2026-11-24) — نفس ✅ ADR-3502 ⟵ عُدّل بسبب ✅ Q-14 (كان: لا يُحفظ نص السؤال والجواب في MVP)
- Answers: Q-05
- Affects: FR-17
- Decision: تُحفظ أسئلة وأجوبة كل مستخدم من كل الأدوار، مرتبطة بصاحبها، ولا يقرأها غيره.
- Why: ✅ Q-14 يجعل «اسأل محور» محادثة خاصة لكل مستخدم، والمحادثة تحتاج حفظ. يتفق مع ADR-3502 في «تصميم قاعدة البيانات».
## ADR-1404 · المؤشرات (KPIs) تُحسب من بيانات الشركة ونتائج التحليل، بدون كيان مستقل في MVP.
- Status: proposed
- Answers: Q-05
- Affects: FR-18
- Decision: المؤشرات (KPIs) تُحسب من بيانات الشركة ونتائج التحليل، بدون كيان مستقل في MVP.
- Why: المؤشرات مشتقة من بيانات موجودة أصلًا في الـ ERD.

## ADR-1503 · التوصيات والفرص تُمثَّل كبيانات منظمة مرتبطة بعملية التحليل عند تنفيذها.
- Status: approved (المنسق · 2026-11-24)
- Answers: Q-05
- Affects: FR-19 · FR-22
- Decision: التوصيات والفرص تُمثَّل كبيانات منظمة مرتبطة بعملية التحليل عند تنفيذها.
- Why: يتوافق مع NFR-25 (تتبّع النتيجة لعملية التحليل).

## ADR-1102 · إضافة كيان Permission وجدول ربط RolePermission بين الأدوار والصلاحيات.
- Status: approved (المنسق · 2026-11-24)
- Answers: Q-11
- Affects: FR-35
- Decision: إضافة كيان Permission وجدول ربط RolePermission بين الأدوار والصلاحيات.
- Why: FR-35 يطلب إدارة الأدوار والصلاحيات، والـ ERD فيه Role فقط. يحتاج مراجعة المجموعة 3.

## ADR-1402 · العلاقة بين AnalysisRun وInsight تكون 1:M.
- Status: approved (المنسق · 2026-11-24)
- Answers: Q-12
- Affects: FR-28
- Decision: العلاقة بين AnalysisRun وInsight تكون 1:M.
- Why: عملية التحليل الواحدة قد تنتج أكثر من رؤية.

## ADR-1403 · تفاصيل الحالة الشاذة تُحفظ في كيان Anomaly، وحدث اكتشافها أو تحديثها يُسجَّل في SystemLog.
- Status: approved (المنسق · 2026-11-24)
- Answers: Q-13
- Affects: FR-26
- Decision: تفاصيل الحالة الشاذة تُحفظ في كيان Anomaly، وحدث اكتشافها أو تحديثها يُسجَّل في SystemLog.
- Why: يجمع بين الـ ERD (جدول مستقل) والـ DFD (التسجيل في السجلات) مع غرض واضح لكل منهما.

## Applied decisions
- Q-09 ✅ (FR-08 · FR-13): النظام يوجّه الطلب للقسم، ومدير القسم يحدد المعالج.
- Q-14 ✅ (FR-17): ذكاء اصطناعي واحد، و«اسأل محور» شات لكل الأدوار حسب صلاحيات كل دور.
- Q-08 ✅ (FR-05, 2026-11-22): التقرير للاثنين (ADR-1204).
- Q-05 · Q-10 → Q-13 ✅ (2026-11-24): ADR-1102 · ADR-1402 · ADR-1403 · ADR-1502 · ADR-1503 معتمدة، مع جداول «تصميم قاعدة البيانات».
- Q-1003 ✅ (FR-39 · NFR-01 · NFR-05, 2026-11-23): صفحة «حسابي» وكلمة المرور والخروج (ADR-1003).
- أولويات المنسق (2026-11-22): FR-05 Must · FR-17 Must · FR-26 Should · FR-31 Later · FR-32 Must — كل تعديل معلّم داخل المتطلب بـ «⟵ عُدّلت بقرار المنسق».

## Open questions (referenced, not new)
| ID | Question | Raised by | Affects | Status |
|---|---|---|---|---|
| Q-1206 | صيغة تقرير حالة الطلبات (FR-05): عرض داخل المنصة، أو ملف، أو الاثنان؟ — سؤال قائم في docs/screens.md (رقمه Q-1204 في إصدار 12 نوفمبر، وQ-1206 في إصدار 17 نوفمبر)، نعتمد عليه ولا نكرره. | Group 1 · member 3 | FR-05 · docs/screens.md | open |

> Q-10 (NFR-08) خارج نطاق العضو 1 — يتناوله مراجع المتطلبات غير الوظيفية (العضو 2).
