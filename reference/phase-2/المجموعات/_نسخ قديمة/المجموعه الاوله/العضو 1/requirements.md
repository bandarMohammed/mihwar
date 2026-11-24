docs/requirements.md

# مراجعة المتطلبات (جزء: الوظيفية)

> **المجموعة 1 · العضو 1** · المتطلبات الوظيفية FR-01 → FR-44 · 2026-11-04
> يراجع هذا الجزء المتطلبات الوظيفية ويقترح أولوية كل متطلب في النسخة الأولى (MVP) مع سبب مختصر. الأولويات **مقترحات** تنتظر اعتماد المنسق. نص المرحلة الأولى (*Source text*) منقول كما هو دون تعديل، والأرقام لم تتغيّر.
> المتطلبات غير الوظيفية NFR-01 → NFR-27 خارج هذا الجزء (العضو 2).

| الأولوية | المعنى | العدد |
|---|---|---|
| **Must** | ضروري لإطلاق النسخة الأولى | 33 |
| **Should** | مهم ويُفضَّل إدخاله إذا سمح الوقت | 9 |
| **Later** | يمكن تأجيله لمرحلة لاحقة | 2 |
| **الإجمالي** | | 44 / 44 |

---

# Functional requirements — المتطلبات الوظيفية

**A. Employee — الموظف**

## FR-01 · تقديم طلب أو شكوى
- Actor: Employee
- Description: يتيح النظام للموظف تقديم طلب أو شكوى.
- Source text: "The system shall allow the Employee to submit a request or complaint." — *Submit Request / Complaint*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must — نقطة البداية الأساسية لاستخدام وصل.
- Source: Phase 1 — System Requirements Report

## FR-02 · إرفاق مستندات داعمة
- Actor: Employee
- Description: يتيح النظام للموظف إرفاق مستندات داعمة بالطلب أو الشكوى.
- Source text: "The system shall allow the Employee to attach supporting documents to a request or complaint." — *Add Supporting Documents*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Should — المرفقات توضّح الطلب، لكنها ليست شرطًا لتقديمه.
- Source: Phase 1 — System Requirements Report

## FR-03 · متابعة حالة الطلب
- Actor: Employee
- Description: يتيح النظام للموظف متابعة حالة طلباته المقدَّمة.
- Source text: "The system shall allow the Employee to track the status of submitted requests." — *Track Request Status*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must — يعرف الموظف ما تمّ في طلبه حتى يُغلق.
- Source: Phase 1 — System Requirements Report

## FR-04 · استلام الإشعارات
- Actor: Employee
- Description: يتيح النظام للموظف استلام الإشعارات المتعلقة بطلباته.
- Source text: "The system shall allow the Employee to receive notifications related to their requests." — *Receive Notifications*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Must — يبقى الموظف على اطلاع بتحديثات طلبه.
- Source: Phase 1 — System Requirements Report

## FR-05 · إنشاء تقرير حالة الطلبات
- Actor: Employee ⚠️ (see Q-08)
- Description: يتيح النظام للموظف إنشاء تقرير يوضح حالة طلباته.
- Source text: "The system shall allow the Employee to generate a report showing the status of their requests." — *Generate Request Status Report*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must — اتُّفق مبدئيًا على إدخاله للموظف والإدارة العليا، وشكل التقرير يحتاج تأكيدًا (Q-08).
- Source: Phase 1 — System Requirements Report
- ⚠️ Open: Q-08 — تقرير المتطلبات يجعله للموظف، وجدول حالات الاستخدام يجعله للإدارة العليا. لم يُحسم بعد.
- 💡 Proposal: مقترح المجموعة 1 (Q-08): التقرير للموظف والإدارة العليا معًا، ويبقى شكله مفتوحًا (ملف للتنزيل، عرض داخل المنصة، أو الاثنان). يحتاج اعتماد المنسق.

**B. Request management — إدارة الطلبات**

## FR-06 · تصنيف الطلب
- Actor: System
- Description: يصنّف النظام الطلبات المقدَّمة تلقائيًا حسب فئتها.
- Source text: "The system shall automatically classify submitted requests based on their category." — *Classify Request*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must — التصنيف الآلي من وظائف وصل الأساسية.
- Source: Phase 1 — System Requirements Report

## FR-07 · تحديد الأولوية
- Actor: System
- Description: يحدد النظام أولوية كل طلب مقدَّم.
- Source text: "The system shall determine the priority of each submitted request." — *Determine Priority*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must — يساعد على ترتيب الطلبات حسب أهميتها.
- Source: Phase 1 — System Requirements Report

## FR-08 · توجيه الطلب إلى القسم
- Actor: System
- Description: يوجّه النظام كل طلب إلى القسم المناسب.
- Source text: "The system shall route each request to the appropriate department." — *Route Request to Department*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must — أساس التوجيه الآلي للطلبات.
- Source: Phase 1 — System Requirements Report

## FR-09 · حفظ الطلب
- Actor: System
- Description: يحفظ النظام بيانات الطلب: الموظف، والقسم، والفئة، والأولوية، والحالة، والوصف، والأوقات.
- Source text: "The system shall store request information including employee, department, category, priority, status, description, and timestamps." — *Store Request*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must — ضروري لحفظ الطلب ومتابعة معالجته.
- Source: Phase 1 — System Requirements Report

## FR-10 · حفظ المستندات الداعمة
- Actor: System
- Description: يحفظ النظام بيانات المستندات الداعمة المرتبطة بالطلبات.
- Source text: "The system shall store supporting document information associated with requests." — *Store Supporting Documents*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Should — مرتبط بميزة المرفقات الاختيارية في FR-02.
- Source: Phase 1 — System Requirements Report

## FR-11 · إرسال إشعار بتغيّر الحالة
- Actor: System
- Description: يرسل النظام إشعارًا عند تحديث حالة الطلب.
- Source text: "The system shall send a notification when the status of a request is updated." — *Send Status Notification*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Must — يدعم متابعة الموظف للتحديثات المهمة.
- Source: Phase 1 — System Requirements Report

**C. Department Manager — مدير القسم**

## FR-12 · عرض طلبات القسم
- Actor: Department Manager
- Description: يتيح النظام لمدير القسم عرض الطلبات المرتبطة بقسمه.
- Source text: "The system shall allow the Department Manager to view requests associated with their department." — *View Department Requests*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must — يمكّن مدير القسم من متابعة الطلبات المحالة لقسمه.
- Source: Phase 1 — System Requirements Report

## FR-13 · إسناد الطلب
- Actor: Department Manager
- Description: يتيح النظام لمدير القسم إسناد الطلبات للمعالجة.
- Source text: "The system shall allow the Department Manager to assign requests for handling." — *Assign Request*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must — ضروري لتحديد من يعالج الطلب داخل القسم.
- Source: Phase 1 — System Requirements Report
- ✅ Decided (Q-09): الموظف ما يختار. النظام يوجّه الطلب للقسم تلقائيًا (FR-08)، ومدير القسم يقرر مين يعالجه: هو بنفسه أو موظف في قسمه. المجموعة 3 تضيف حقل للشخص المسؤول عن المعالجة.

## FR-14 · تحديث حالة الطلب
- Actor: Department Manager
- Description: يتيح النظام لمدير القسم تحديث حالة الطلبات.
- Source text: "The system shall allow the Department Manager to update the status of requests." — *Update Request Status*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must — ضروري لإظهار تقدّم المعالجة للموظف والإدارة.
- Source: Phase 1 — System Requirements Report

## FR-15 · استمرار سير عمل الطلب
- Actor: System
- Description: يحافظ النظام على سير عمل الطلب حتى يُحل أو يحتاج إلى إجراء إضافي.
- Source text: "The system shall maintain the request workflow until the request is resolved or requires further action." — *Maintain Request Workflow*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must — يربط مراحل الطلب من الاستلام حتى الإغلاق.
- Source: Phase 1 — System Requirements Report

**D. Top Management — الإدارة العليا**

## FR-16 · عرض رؤى الشركة
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض رؤى الشركة التي يولّدها النظام.
- Source text: "The system shall allow Top Management to view company insights generated by the system." — *View Company Insights*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — من المخرجات الرئيسية للعقل التشغيلي.
- Source: Phase 1 — System Requirements Report

## FR-17 · سؤال الذكاء الاصطناعي عن بيانات الشركة
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا طرح أسئلة عن بيانات الشركة واستلام تحليل يولّده الذكاء الاصطناعي.
- Source text: "The system shall allow Top Management to submit questions about company data and receive AI-generated analysis." — *Ask AI About Company Data*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Must — «اسأل محور» صار شاتًا لكل الأدوار وجزءًا أساسيًا من محور، ويدخل نطاق MVP (✅ Q-14). ⟵ عُدّلت من Should: سبب العضو 1 كان «ميزة تفاعلية مفيدة، ويمكن تقديم التحليلات الأساسية قبلها»، وكُتب قبل القرار Q-14.
- Source: Phase 1 — System Requirements Report
- ⚠️ Open: Q-05 — نص السؤال والإجابة غير محفوظ في الـ ERD.
- ✅ Decided (Q-14): «اسأل محور» شات لكل الأدوار، مو للإدارة العليا بس. كل مستخدم له محادثته ويشوف حسب صلاحيات دوره: الموظف طلباته، ومدير القسم قسمه، والإدارة العليا الشركة كلها، ومدير النظام بيانات الإدارة. وفي محور ذكاء اصطناعي واحد فقط. نص المتطلب أعلاه يبقى كما هو من المرحلة الأولى.
- 💡 Proposal: مقترح المجموعة 1 (Q-05): يُحفظ نص السؤال والإجابة، لأن لكل مستخدم محادثته (✅ Q-14). مكان الحفظ تحدده المجموعة 3. ⟵ عُدّل: مقترح العضو 1 كان «لا يُحفظ في MVP إلا إذا احتجنا سجل محادثات»، وهذا يخالف Q-14.

## FR-18 · متابعة مؤشرات الأداء
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا متابعة مؤشرات أداء الشركة (KPIs).
- Source text: "The system shall allow Top Management to monitor company KPIs." — *Monitor Company KPIs*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — يساعد الإدارة على متابعة الأداء التشغيلي.
- Source: Phase 1 — System Requirements Report
- ⚠️ Open: Q-05 — لا يوجد كيان للمؤشرات (KPIs) في الـ ERD.
- 💡 Proposal: مقترح المجموعة 1 (Q-05): تُحسب المؤشرات من بيانات الشركة ونتائج التحليل.

## FR-19 · عرض توصيات الذكاء الاصطناعي
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض التوصيات التي يولّدها الذكاء الاصطناعي.
- Source text: "The system shall allow Top Management to view AI-generated recommendations." — *View AI Recommendations*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Should — يضيف قيمة لاتخاذ القرار بعد توفّر التحليلات الأساسية.
- Source: Phase 1 — System Requirements Report
- ⚠️ Open: Q-05 — لا يوجد كيان للتوصيات في الـ ERD.
- 💡 Proposal: مقترح المجموعة 1 (Q-05): تُمثَّل التوصيات كبيانات منظّمة عند تنفيذها.

## FR-20 · عرض الأداء التشغيلي
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض معلومات الأداء التشغيلي.
- Source text: "The system shall allow Top Management to view operational performance information." — *View Operational Performance*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — يعطي الإدارة صورة عن أداء العمليات.
- Source: Phase 1 — System Requirements Report

## FR-21 · عرض المشكلات المكتشفة
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض المشكلات التشغيلية التي اكتشفها محرك التحليل.
- Source text: "The system shall allow Top Management to view operational issues detected by the AI Analytics Engine." — *View Detected Issues*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — يساعد على إظهار المشكلات التي تحتاج انتباهًا.
- Source: Phase 1 — System Requirements Report

## FR-22 · عرض الفرص المحددة
- Actor: Top Management
- Description: يتيح النظام للإدارة العليا عرض الفرص المستخلصة من تحليل بيانات الشركة.
- Source text: "The system shall allow Top Management to view opportunities identified through company data analysis." — *View Identified Opportunities*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Later — يمكن إضافته توسعةً بعد استقرار التحليل الأساسي.
- Source: Phase 1 — System Requirements Report
- ⚠️ Open: Q-05 — لا يوجد كيان للفرص في الـ ERD.
- 💡 Proposal: مقترح المجموعة 1 (Q-05): تُمثَّل الفرص كبيانات منظّمة عند تنفيذها.

## FR-23 · إنشاء تقرير إداري
- Actor: System (for Top Management)
- Description: ينشئ النظام تقارير إدارية تتضمن تحليل الشركة والرؤى والمؤشرات والمعلومات التشغيلية.
- Source text: "The system shall generate management reports containing relevant company analysis, insights, KPIs, and operational information." — *Generate Management Report*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Should — مفيد للتوثيق والمشاركة، ويمكن عرض المعلومات داخل المنصة في البداية.
- Source: Phase 1 — System Requirements Report

**E. AI analytics — التحليل بالذكاء الاصطناعي**

## FR-24 · تحليل بيانات الشركة
- Actor: AI Analytics Engine
- Description: يحلل النظام بيانات الشركة التشغيلية المتاحة عبر محرك التحليل بالذكاء الاصطناعي.
- Source text: "The system shall analyze available company operational data through the AI Analytics Engine." — *Analyze Company Data*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — الأساس الذي تعتمد عليه وظائف العقل التشغيلي.
- Source: Phase 1 — System Requirements Report

## FR-25 · اكتشاف المشكلات التشغيلية
- Actor: AI Analytics Engine
- Description: يكتشف النظام المشكلات التشغيلية المحتملة من بيانات الشركة المحللة.
- Source text: "The system shall detect potential operational issues from analyzed company data." — *Detect Operational Issues*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — ضروري لاكتشاف الخلل في العمليات.
- Source: Phase 1 — System Requirements Report

## FR-26 · اكتشاف الحالات الشاذة
- Actor: AI Analytics Engine
- Description: يكتشف النظام الحالات الشاذة في بيانات الشركة المتاحة.
- Source text: "The system shall detect anomalies within the available company data." — *Detect Anomalies*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Later — وظيفة تحليلية متقدمة يمكن تأجيلها حتى يُبنى التحليل الأساسي.
- Source: Phase 1 — System Requirements Report
- ⚠️ Open: Q-13 — الـ DFD يحفظ الحالات الشاذة في سجلات النظام، والـ ERD له جدول Anomaly مستقل.
- 💡 Proposal: مقترح المجموعة 1 (Q-13): تُحفظ تفاصيل الحالة الشاذة في كيان Anomaly، ويُسجَّل حدث اكتشافها أو تحديثها في SystemLog للتتبّع.

## FR-27 · تحديد الاتجاهات
- Actor: AI Analytics Engine
- Description: يحدد النظام الاتجاهات من بيانات الشركة المحللة.
- Source text: "The system shall identify trends from analyzed company data." — *Identify Trends*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Should — يعزّز فهم التغيّرات، لكنه ليس شرطًا للوظائف الأساسية.
- Source: Phase 1 — System Requirements Report

## FR-28 · توليد الرؤى
- Actor: AI Analytics Engine
- Description: يولّد النظام رؤى مبنية على نتائج تحليل بيانات الشركة.
- Source text: "The system shall generate insights based on the results of company data analysis." — *Generate Insights*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — يحوّل نتائج التحليل إلى معلومات قابلة للاستخدام.
- Source: Phase 1 — System Requirements Report
- ⚠️ Open: Q-12 — Insight علاقته 1:1 مع AnalysisRun، والأقرب 1:M.
- 💡 Proposal: مقترح المجموعة 1 (Q-12): تكون العلاقة بين AnalysisRun وInsight واحد إلى متعدد (1:M)، لأن عملية التحليل الواحدة قد تُنتج أكثر من رؤية.

## FR-29 · حفظ عمليات التحليل
- Actor: System
- Description: يسجّل النظام كل عملية تحليل مع ما أطلقها ووقت تنفيذها.
- Source text: "The system shall record each analysis run, including its trigger and execution time." — *Store Analysis Runs*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — يوثّق عمليات التحليل ويسمح بالرجوع إليها.
- Source: Phase 1 — System Requirements Report

## FR-30 · حفظ نتائج التحليل
- Actor: System
- Description: يحفظ النظام المشكلات والحالات الشاذة والاتجاهات والرؤى المرتبطة بعملية التحليل.
- Source text: "The system shall store detected issues, anomalies, trends, and insights associated with an analysis run." — *Store Analysis Results*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — ضروري لحفظ مخرجات التحليل وعرضها.
- Source: Phase 1 — System Requirements Report

## FR-31 · التحليل الدوري
- Actor: System
- Description: يدعم النظام التحليل الدوري لبيانات الشركة.
- Source text: "The system shall support periodic analysis of company data." — *Periodic Analysis*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — يوفّر تحديثًا منتظمًا للرؤى التشغيلية.
- Source: Phase 1 — System Requirements Report

## FR-32 · التحليل عند الطلب
- Actor: Top Management
- Description: يدعم النظام تحليلًا تطلقه الإدارة العليا عند الطلب.
- Source text: "The system shall support analysis triggered by a request from Top Management." — *On-Demand Analysis*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Should — مفيد عند الحاجة لتحليل فوري، ويمكن أن يأتي بعد التحليل الدوري.
- Source: Phase 1 — System Requirements Report

**F. System Administrator — مدير النظام**

## FR-33 · إدارة المستخدمين
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام إدارة مستخدمي النظام.
- Source text: "The system shall allow the System Administrator to manage system users." — *Manage Users*
- Module: Users & permissions · Unit 1
- Priority (MVP): Must — من الوظائف الإدارية الأساسية للمنصة.
- Source: Phase 1 — System Requirements Report

## FR-34 · إدارة الأقسام
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام إدارة الأقسام.
- Source text: "The system shall allow the System Administrator to manage departments." — *Manage Departments*
- Module: Departments & request handling · Unit 3
- Priority (MVP): Must — ضروري لتنظيم الأقسام وتوجيه الطلبات إليها.
- Source: Phase 1 — System Requirements Report

## FR-35 · إدارة الأدوار والصلاحيات
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام إدارة الأدوار والصلاحيات.
- Source text: "The system shall allow the System Administrator to manage roles and permissions." — *Manage Roles and Permissions*
- Module: Users & permissions · Unit 1
- Priority (MVP): Must — أساسي للتحكم في الوصول لوظائف المنصة وبياناتها.
- Source: Phase 1 — System Requirements Report
- ⚠️ Open: Q-11 — الـ ERD فيه Role فقط بدون Permission.
- 💡 Proposal: مقترح المجموعة 1 (Q-11): إضافة كيان Permission وجدول ربط RolePermission بين الأدوار والصلاحيات، بعد مراجعة فريق قاعدة البيانات.

## FR-36 · إدارة إعدادات النظام
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام إدارة إعدادات النظام.
- Source text: "The system shall allow the System Administrator to manage system settings." — *Manage System Settings*
- Module: Users & permissions · Unit 1
- Priority (MVP): Should — مهم للإدارة، لكن يمكن البدء بإعدادات افتراضية محددة.
- Source: Phase 1 — System Requirements Report

## FR-37 · عرض سجلات النظام
- Actor: System Administrator
- Description: يتيح النظام لمدير النظام عرض سجلات النظام.
- Source text: "The system shall allow the System Administrator to view system logs." — *View System Logs*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Should — مهم للمراجعة والدعم، لكنه ليس وظيفة استخدام يومية أساسية.
- Source: Phase 1 — System Requirements Report

## FR-38 · تسجيل سجلات النظام
- Actor: System
- Description: يسجّل النظام الإجراءات المهمة في سجلات النظام.
- Source text: "The system shall record relevant system actions in the System Logs." — *Record System Logs*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Must — يدعم التتبّع والتدقيق ومعرفة ما يحدث داخل النظام.
- Source: Phase 1 — System Requirements Report

**G. Data management — إدارة البيانات**

## FR-39 · إدارة بيانات المستخدمين
- Actor: System
- Description: يحفظ النظام بيانات المستخدمين وأدوارهم وارتباطهم بالأقسام.
- Source text: "The system shall store user information, roles, and department associations." — *User Data Management*
- Module: Users & permissions · Unit 1
- Priority (MVP): Must — ضروري لحفظ بيانات حسابات المستخدمين.
- Source: Phase 1 — System Requirements Report

## FR-40 · إدارة بيانات الطلبات
- Actor: System
- Description: يحتفظ النظام بسجلات الطلبات وعلاقاتها بالموظفين والأقسام والمدراء.
- Source text: "The system shall maintain request records and their relationships with employees, departments, and managers." — *Request Data Management*
- Module: Wasl (وصل) · Unit 2
- Priority (MVP): Must — ضروري لاستمرار عمل وصل.
- Source: Phase 1 — System Requirements Report

## FR-41 · إدارة الإشعارات
- Actor: System
- Description: يحفظ النظام الإشعارات المرتبطة بالمستخدمين والطلبات، مع حالة كل إشعار.
- Source text: "The system shall store notifications associated with users and requests, including notification status." — *Notification Management*
- Module: Notifications & system logs · Unit 6
- Priority (MVP): Must — ضروري لإرسال الإشعارات ومتابعتها.
- Source: Phase 1 — System Requirements Report

## FR-42 · بيانات الشركة التشغيلية
- Actor: System
- Description: يحفظ النظام بيانات الشركة التشغيلية التي يستخدمها محرك التحليل.
- Source text: "The system shall store company operational data used by the AI Analytics Engine." — *Company Operational Data*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — يوفّر البيانات التي يعتمد عليها التحليل التشغيلي.
- Source: Phase 1 — System Requirements Report

## FR-43 · ربط بيانات التحليل
- Actor: System
- Description: يربط النظام نتائج التحليل بعملية التحليل الخاصة بها.
- Source text: "The system shall associate analysis results with their corresponding analysis run." — *Analysis Data Association*
- Module: Operational Brain (العقل التشغيلي) · Unit 4
- Priority (MVP): Must — يربط نتائج التحليل بعملية التحليل الخاصة بها.
- Source: Phase 1 — System Requirements Report

## FR-44 · حفظ التقارير الإدارية
- Actor: System
- Description: يحفظ النظام التقارير الإدارية المُنشأة والتحليل المرتبط بها.
- Source text: "The system shall store generated management reports and their associated analysis." — *Management Report Storage*
- Module: Ask AI, reports & recommendations · Unit 5
- Priority (MVP): Should — يمكن تأجيل الحفظ المستقل إذا عُرضت التقارير داخل المنصة في البداية.
- Source: Phase 1 — System Requirements Report

---

## قرارات وأسئلة مفتوحة

| البند | المتطلب | مقترح المجموعة 1 | الحالة |
|---|---|---|---|
| Q-08 | FR-05 | التقرير للموظف والإدارة العليا معًا، وشكله مفتوح | proposed |
| ✅ Q-09 | FR-06 → FR-08، FR-13 | مطبّق: النظام يحدد القسم، ومدير القسم يختار من يعالج الطلب (هو أو موظف من قسمه)، والموظف لا يختار شيئًا | approved |
| Q-05 | FR-17، FR-18، FR-19، FR-22 | يُحفظ نص السؤال والإجابة (✅ Q-14)، والمؤشرات تُحسب من البيانات ونتائج التحليل، والتوصيات والفرص بيانات منظّمة | proposed |
| Q-11 | FR-35 | كيان Permission وجدول RolePermission | proposed |
| Q-12 | FR-28 | العلاقة AnalysisRun ← Insight تكون 1:M | proposed |
| Q-13 | FR-26 | التفاصيل في Anomaly، والحدث في SystemLog | proposed |
| ✅ Q-14 | FR-17 | مطبّق: «اسأل محور» شات لكل الأدوار، فصارت أولوية FR-17 = Must | approved |
| Q-10 | NFR-08 | خارج هذا الجزء، يتركه العضو 1 للعضو 2 (المتطلبات غير الوظيفية) | open |

**حدود هذه المراجعة:** تقتصر على FR-01 → FR-44. المقترحات المتعلقة بنموذج البيانات تحتاج تأكيد المجموعة 3 والمنسق.
