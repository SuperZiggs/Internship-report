---
title: "Week 6 Worklog"
date: 2026-02-09
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* **Backend**: Build the `UserWorkoutSession` and `WorkoutLog` modules to record actual workout activity in real time.
* **Frontend**: Build `PlanDetailScreen` (active plan with day selector and session start) and `WorkoutSessionScreen` (fully interactive live workout experience).
* Complete the core workout tracking loop: plan → session → log sets → finish.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Build **UserWorkoutSession** entity <br>&emsp; + Fields: `user (@ManyToOne)`, `userWorkoutPlan (@ManyToOne)`, `workoutDate (LocalDate)`, `isActive (default true)`, `weekIndex`, `dayIndex` <br>&emsp; + Cascade to `WorkoutLog` via `@OneToMany(cascade = ALL)` | 02/10/2026 | 02/10/2026 | |
| 3   | - Build **WorkoutLog** entity <br>&emsp; + Fields: `userWorkoutSession (@ManyToOne)`, `exercise (@ManyToOne)`, `setNumber`, `reps`, `weight (Float)`, `durationSeconds` | 02/11/2026 | 02/11/2026 | |
| 3   | - Build **UserWorkoutSessionController** (`/api/sessions`) <br>&emsp; + `POST /` — create session <br>&emsp; + `GET /{id}`, `GET /user/{userId}`, `GET /user/{userId}/by-date?date=` <br>&emsp; + `GET /user/{userId}/active` — fetch currently active session <br>&emsp; + `PATCH /{sessionId}/deactivate` — mark session completed <br>&emsp; + `POST /{sessionId}/logs` — log an exercise set | 02/11/2026 | 02/11/2026 | |
| 4   | - Build **PlanDetailScreen** (Frontend) <br>&emsp; + Display active plan name and description <br>&emsp; + Day-of-week selector (Mon–Sun) — shows exercises for selected day <br>&emsp; + "Start Workout" button: create session via `POST /api/sessions`, navigate to `WorkoutSessionScreen` <br>&emsp; + Show active session banner if session already in progress <br>&emsp; + Navigate to `SessionCalendar` for history | 02/12/2026 | 02/12/2026 | |
| 5   | - Build **workoutSessionSlice** (Redux) <br>&emsp; + State: `sessionId`, `planId`, `exercises[]`, `currentExIndex`, `currentSet`, `completedSets[]`, `phase (workout/rest/done)`, `restSeconds`, `isSavingLog` <br>&emsp; + Actions: `initializeSession`, `restoreSessionState`, `logSetStart/Success/Failure`, `finishRest`, `skipExercise`, `skipSet`, `resetSession` <br>&emsp; + Persist session to `AsyncStorage` on every set — restore on app reload | 02/13/2026 | 02/13/2026 | |
| 6   | - Build **WorkoutSessionScreen** (Frontend) <br>&emsp; + `ActiveExerciseCard`: shows exercise name, set progress, reps target <br>&emsp; + `LogSetSheet`: input actual reps + weight → `addWorkoutLog` API call <br>&emsp; + `RestTimer`: countdown timer with auto-advance to next exercise/set after rest <br>&emsp; + Finish: `deactivateSession` → `resetSession` → navigate to `WorkoutSuccessScreen` <br> - Build **WorkoutSuccessScreen** <br>&emsp; + Trophy screen with exercise summary (exercises done, sets, total reps) <br>&emsp; + Blocks hardware back button → force navigate to Home | 02/14/2026 | 02/14/2026 | |

### Week 6 Achievements:

* **Backend — Session module**:
  * `UserWorkoutSession` + `WorkoutLog` entities persisted correctly with cascade DELETE.
  * `POST /api/sessions` creates a session linked to a plan and user; `PATCH /{id}/deactivate` marks it done.
  * `POST /api/sessions/{id}/logs` records individual set logs (exercise, set number, reps, weight).
  * `GET /user/{userId}/active` correctly returns the in-progress session or null.
  * Date-filtered query `GET /by-date?date=` working for session history retrieval.
* **Frontend — Live Workout**:
  * `PlanDetailScreen` loads active plan; day selector filters exercises by `dayOfWeek`.
  * Session state fully managed in Redux `workoutSessionSlice` — persisted to AsyncStorage for crash recovery.
  * `WorkoutSessionScreen` guides user through all exercises/sets with automatic rest timer.
  * Each set logged to backend via `addWorkoutLog` API call before advancing.
  * `WorkoutSuccessScreen` shows achieved stats and prevents accidental back navigation.
  * Complete workout loop tested end-to-end: plan → start session → log sets → rest timer → finish → success.

### AWS Knowledge Learned:

* Learned structured logging design for cloud operations, including fields like severity, requestId, userId, endpoint, and errorCode for fast filtering.
* Identified practical service indicators for the workout flow: session creation latency, log-write success rate, and active-session consistency.
* Studied CloudWatch alarm design around p95 latency, 5xx error thresholds, and database pressure indicators.
* Learned how to compose dashboards so they remain actionable for engineering review instead of becoming noisy metric walls.
* Practiced a basic incident-response workflow: detect, triage, mitigate, validate, and document follow-up improvements.
* Reinforced a rollback-first mindset for bad releases so user impact is minimized before root-cause analysis completes.
* Prepared for future alert routing through SNS or similar channels when critical production signals need escalation.

In summary, week 6 strengthened the operational and monitoring perspective required for reliable cloud-backed workout tracking.

### Next Week Plan:

* **Backend**: Build the Media module — `Image` entity, S3 integration, and `ImageController` for associating images with exercises, foods, and workout plans.
* **Frontend**: Build `SessionDetailScreen` (past workout recap) and `SessionCalendarScreen` (monthly calendar with session dots).
