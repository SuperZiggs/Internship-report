---
title: "Week 5 Worklog"
date: 2026-02-02
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* **Backend**: Architect the `UserWorkoutPlan` module — facilitating personalized workout regimens via system template cloning, implementing soft-delete mechanisms, and enforcing a strict single-active-plan policy.
* **Frontend**: Construct a comprehensive suite of plan management interfaces — encompassing `MyPlansScreen`, `CreatePlanScreen`, and a `PlanEditScreen` featuring day-tabbed granular exercise modifications.
* Empower users with full autonomy over their fitness schedules, providing robust flexibility to seamlessly create, clone, and deeply customize personal workout plans.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Construct the **UserWorkoutPlan** entity <br>&emsp; + Define schema fields: `user (@ManyToOne)`, `name (≤150 chars)`, `description`, `goalTypeId (UUID)`, `isActive (default true)`, `isDeleted (default false)` <br>&emsp; + Integrate soft-delete functionality via `@SQLRestriction("is_deleted = false")` — ensuring logically deleted records are completely isolated from Hibernate queries <br>&emsp; + Develop Flyway **V1** migration scripts to initialize the `user_workout_plan` and `user_workout_plan_exercises` tables | 02/03/2026 | 02/03/2026 | |
| 2   | - Execute Flyway **V2** migration: append the `is_deleted BOOLEAN DEFAULT FALSE` column to the `user_workout_plans` table to support soft-deletion operations | 02/03/2026 | 02/03/2026 | |
| 3   | - Architect the **UserWorkoutPlanExercise** entity <br>&emsp; + Replicate core scheduling attributes: `dayOfWeek`, `sets`, `reps`, `restSeconds`, `dayIndex`, `weekIndex`, `orderIndex` <br>&emsp; + Establish foreign key references mapping to the system `Exercise` catalog and the parent `UserWorkoutPlan` | 02/04/2026 | 02/04/2026 | |
| 3   | - Implement critical business logic within **UserWorkoutPlanService** <br>&emsp; + Mandate `userId` extraction exclusively from the `Jwt.getSubject()` claim — strictly bypassing request payloads to eliminate IDOR vulnerabilities <br>&emsp; + **Clone Strategy**: Execute a deep-copy of all associated `WorkoutPlanExercise` entries into new `UserWorkoutPlanExercise` records — intentionally severing all foreign key ties to the source template <br>&emsp; + **Activation Engine**: Perform transactional state updates to assert `isActive=true` for the target plan while overriding all prior plans to `isActive=false` (enforcing the one-active rule) | 02/04/2026 | 02/04/2026 | |
| 4   | - Develop the **UserWorkoutPlanController** (`/api/user-workout-plans`) <br>&emsp; + Initialization endpoints: `POST /me` (instantiate standard plan), `POST /me/clone/{systemPlanId}` (clone template) <br>&emsp; + Retrieval endpoints: `GET /me` (aggregation list), `GET /me/active`, `GET /{id}` <br>&emsp; + Mutation/Deletion endpoints: `PUT /{id}` (update metadata), `PUT /{id}/activate`, `DELETE /{id}` (executes soft-delete) <br>&emsp; + Granular exercise management: `POST /{planId}/exercises`, `GET /{planId}/exercises`, `PUT /{planId}/exercises/{id}`, `DELETE /{planId}/exercises/{id}` | 02/05/2026 | 02/05/2026 | |
| 5   | - Construct the **MyPlansScreen** interface (Frontend) <br>&emsp; + Render an aggregated list of user plans accented with dynamic active/inactive indicator badges <br>&emsp; + Implement state-toggling mechanisms and guarded soft-delete actions utilizing a `ConfirmModal` <br>&emsp; + Facilitate seamless navigation routing to `CreatePlanScreen` and `PlanEditScreen` | 02/06/2026 | 02/06/2026 | |
| 5   | - Build the **CreatePlanScreen** interface (Frontend) <br>&emsp; + Design a comprehensive input form: plan name, description, and a dynamic goal type dropdown populated via `GET /api/goal-types` <br>&emsp; + Handle submission lifecycle: dispatch `createMyPlan` API call followed by an automatic route transition to the `PlanEditScreen` | 02/06/2026 | 02/06/2026 | |
| 6   | - Develop the **PlanEditScreen** interface (Frontend) <br>&emsp; + Integrate a chronological day-of-week tab bar (Mon-Sun) to partition and render daily exercise schedules <br>&emsp; + Render contextual exercise lists supporting intuitive reordering (up/down sorting arrows) and deletion capabilities <br>&emsp; + Bridge navigation to the `PlanExercisePicker` for precise daily exercise additions <br>&emsp; + Bind UI interactions to backend state via `addExerciseToPlan`, `updatePlanExercise`, and `removePlanExercise` API integrations | 02/07/2026 | 02/07/2026 | |

### Week 5 Achievements:

* **Backend — UserWorkoutPlan module**:
  * Successfully applied Flyway V1 and V2 migrations in tandem with Hibernate's `ddl-auto=create-drop`, ensuring strict schema consistency.
  * The soft-delete paradigm employing `@SQLRestriction` operates flawlessly — logically removed plans are securely filtered from all application queries.
  * The template cloning endpoint generates entirely autonomous deep copies of system plans — rigorously verified by the absence of foreign key pointers, thereby safeguarding core system data from user mutations.
  * The single-active-plan constraint is robustly enforced at the service level — triggering the activation of a new plan programmatically deactivates all competing user plans via a single database transaction.
  * User identity (`userId`) is strictly derived from the validated JWT `sub` claim — definitively neutralizing any potential IDOR attack vectors.
* **Frontend — Plan Management**:
  * The `MyPlansScreen` precisely reflects active/inactive application states and enforces defensive UX patterns via deletion confirmation modals.
  * The `CreatePlanScreen` rigorously validates input payload structures (name/description) prior to initiating network requests.
  * The `PlanEditScreen`'s day-tabbed architecture provides an exceptionally intuitive UX for highly granular, daily workout customization.
  * Array mutations (reordering and removal) immediately synchronize with the UI upon API resolution, ensuring perceived performance.
  * Redux `uiSlice` state management (`planEditorDay`, `plansReloadKey`) maintains absolute consistency of the plan editor context across the navigation stack.

### AWS Knowledge Learned:

* Mastered fundamental Amazon RDS architectures tailored for PostgreSQL production deployments: configuring managed automated backups, defining non-disruptive maintenance windows, tuning custom parameter groups, and monitoring service quotas.
* Architected robust database isolation by deploying instances within private subnets, utilizing strict Security Group configurations that exclusively whitelist backend service ingress.
* Analyzed data protection mechanisms, specifically snapshot lifecycle management and Point-in-Time Recovery (PITR), to safeguard immutable user workout history against operational errors or botched deployments.
* Solidified database migration discipline by treating Flyway execution scripts as deterministic deployment artifacts, guaranteeing idempotent schema evolutions across environments.
* Deepened understanding of the finite nature of cloud database connections, highlighting the critical necessity of meticulous application-side connection pool tuning.
* Explored advanced read-scaling strategies leveraging RDS Read Replicas, preparing the architecture to handle future read-heavy analytics and reporting workloads.
* Correlated technical data retention and disaster recovery strategies directly with overarching business requirements, such as auditability, operational reversibility, and long-term user trust.

In summary, week 5 linked application data design to AWS-managed relational database practices and recovery planning.

### Next Week Plan:

* **Backend**: Architect the `UserWorkoutSession` and `WorkoutLog` modules to facilitate real-time tracking and persistence of live workout activities.
* **Frontend**: Construct the `PlanDetailScreen` (a comprehensive dashboard for the active plan featuring a day selector) and the `WorkoutSessionScreen` (an interactive live-workout interface tightly coupled with Redux session state management).