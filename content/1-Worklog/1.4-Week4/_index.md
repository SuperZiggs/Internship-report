---
title: "Week 4 Worklog"
date: 2026-01-26
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* **Backend**: Design and implement the System Workout module — focusing on the centralized management of master exercise repositories and standardized workout plans (System Plans) governed by administrators.
* **Frontend**: Develop core workout browsing interfaces — specifically, a personalized suggested plan wizard and a highly interactive exercise picker component.
* Establish and solidify the relational data model between `GoalType` ↔ `WorkoutPlan` ↔ `Exercise`, forming the logical foundation for the workout plan recommendation engine.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Construct the **MuscleGroup** entity and administrative CRUD APIs <br>&emsp; + Define Entity schema: `name (UNIQUE, ≤100 chars)`, `description (≤500 chars)` <br>&emsp; + Implement `AdminMuscleGroupController` (`/api/admin/muscle-groups`) strictly secured with `@PreAuthorize("hasRole('ADMIN')")` <br>&emsp; + Deliver full CRUD operations: `POST`, `GET /`, `GET /{id}`, `PUT /{id}`, `DELETE /{id}` | 01/27/2026 | 01/27/2026 | |
| 3   | - Develop the **Exercise** entity and corresponding admin APIs <br>&emsp; + Define Entity: `name (≤150)`, `description (≤500)`, `equipment`, `muscleGroup (@ManyToOne)` <br>&emsp; + Build `AdminExerciseController` (`/api/admin/exercises`): featuring comprehensive CRUD capabilities paired with muscle group filtering <br>&emsp; + Expose Public Endpoints: `GET /api/workouts/exercises`, `GET /api/workouts/exercises/{id}`, `GET /api/workouts/exercises/by-muscle-group/{id}`, `POST /api/workouts/exercises/custom` | 01/28/2026 | 01/28/2026 | |
| 4   | - Architect the **WorkoutPlan** and **WorkoutPlanExercise** entities <br>&emsp; + `WorkoutPlan` schema: `name`, `description`, `goalType (@ManyToOne)`, `difficultyLevel`, `estimatedDurationMinutes`, `isSystemPlan` <br>&emsp; + `WorkoutPlanExercise` mapping: granular scheduling via `dayOfWeek (1-7)`, `sets`, `reps`, `restSeconds`, `dayIndex`, `weekIndex`, `orderIndex` <br>&emsp; + Implement `AdminWorkoutPlanController` supporting full CRUD and filtering by goal type | 01/29/2026 | 01/29/2026 | |
| 4   | - Deploy **public workout endpoints** via the `WorkoutController` (`/api/workouts`) <br>&emsp; + Muscle Groups: `GET /muscle-groups`, `GET /muscle-groups/{id}` <br>&emsp; + Exercises: `GET /exercises`, `GET /exercises/{id}`, `GET /exercises/by-muscle-group/{id}` <br>&emsp; + Workout Plans: `GET /plans`, `GET /plans/{id}`, `GET /plans/by-goal-type/{id}` | 01/29/2026 | 01/29/2026 | |
| 5   | - Construct the **SuggestedPlanScreen** (3-step wizard frontend) <br>&emsp; + Step 1: Survey fitness goal types (invoking `GET /api/goal-types`) <br>&emsp; + Step 2: Render system workout plan tiles showcasing visual assets and difficulty levels <br>&emsp; + Step 3: Preview granular daily exercise routines → enable template cloning via the `cloneFromSystemPlan` mechanism | 01/30/2026 | 01/30/2026 | |
| 6   | - Develop the **PlanExercisePicker** screen <br>&emsp; + Render a comprehensive list of system exercises with visual context and muscle group categorizations <br>&emsp; + Integrate dynamic search and filtering by exercise name <br>&emsp; + Handle selection events: trigger `addExerciseToPlan(planId, dayOfWeek, exerciseId)` <br> - Engineer a **DatabaseSeeder** to auto-populate initial exercise and muscle group master data into the local database upon application startup | 01/31/2026 | 01/31/2026 | |

### Week 4 Achievements:

* **Backend — System Workout**:
  * The core `MuscleGroup` entity and admin CRUD suite are fully operational; successfully seeded with standard anatomical groups (Chest, Back, Legs, Shoulders, Arms, Core).
  * The `Exercise` module seamlessly integrates both administrative management and public read access, reliably serving client requests.
  * The complex `WorkoutPlan` and `WorkoutPlanExercise` relationship is complete, accommodating daily scheduling and flexible multi-week progressions via the `weekIndex` field.
  * Maintained strict authorization boundaries: all admin endpoints are rigorously protected by `ROLE_ADMIN`, while public read endpoints operate independently without authentication requirements.
  * The `GET /api/workouts/plans/by-goal-type/{id}` endpoint accurately filters and retrieves system-curated plans aligned with specific user fitness goals.
* **Frontend — Workout Browsing**:
  * The `SuggestedPlanScreen` fluidly navigates users through a logical 3-step pipeline, from goal definition to customized plan selection.
  * Plan tiles are visually optimized, displaying essential metadata including name, difficulty level, estimated duration, and associated goal type.
  * The `PlanExercisePicker` intuitively catalogs exercises with muscle group context, providing a frictionless experience for users adding custom exercises to their routines.
* The `DatabaseSeeder` robustly parses the `s3_images_upload.json` configuration to automatically provision foundational muscle group and exercise data. It thoroughly resolves data mapping anomalies caused by special characters and whitespace, guaranteeing 100% alignment between the local database and S3/CloudFront image URLs.

### AWS Knowledge Learned:

* Internalized the design principles of a scalable media architecture: utilizing Amazon S3 for binary object storage and PostgreSQL for metadata, ensuring each system operates within its optimal domain.
* Established standardized object key naming conventions (e.g., `workouts/`, `exercises/`, `foods/`) to streamline storage organization, simplify cleanup processes, and facilitate targeted lifecycle policies.
* Solidified the "private-by-default" access paradigm for S3 buckets, deeply understanding the security risks of applying broad Public ACLs to internal application assets.
* Analyzed server-side encryption methodologies (SSE-S3 and SSE-KMS), clearly distinguishing scenarios where the enhanced access control of KMS justifies the additional architectural complexity.
* Recognized the strategic value of S3 Versioning as an automated defense mechanism against the accidental overwriting or deletion of critical media assets.
* Evaluated the trade-offs between S3 storage costs and data transfer bandwidth for image-intensive applications, underscoring the necessity of pre-upload file size and format optimization.
* Architected a "cloud-ready" storage model strategically positioned to seamlessly integrate Amazon CloudFront as a CDN ahead of S3, requiring zero modifications to the existing application business logic.

In summary, Week 4 successfully translated theoretical AWS storage concepts into a highly maintainable, production-ready media architecture for the workout and exercise modules.

### Next Week Plan:

* **Backend**: Implement the advanced `UserWorkoutPlan` module, featuring capabilities to clone system templates, enforce soft-delete mechanisms, and execute dynamic plan activation/deactivation logic.
* **Frontend**: Construct the personal plan management interface suite, including `MyPlansScreen`, `CreatePlanScreen`, and the day-tabbed `PlanEditScreen` for granular routine customization.