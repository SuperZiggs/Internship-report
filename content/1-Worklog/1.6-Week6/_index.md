---
title: "Week 6 Worklog"
date: 2026-02-09
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* **Backend**: Architect the `UserWorkoutSession` and `WorkoutLog` modules to facilitate real-time telemetry and precise persistence of live workout activities.
* **Frontend**: Construct the `PlanDetailScreen` (an interactive dashboard for the active plan featuring a chronological day selector) and the `WorkoutSessionScreen` (optimizing the live-workout interactive experience).
* Comprehensively finalize the core user workout lifecycle: from plan initialization → session execution → granular set logging → to completion and statistical review.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Construct the **UserWorkoutSession** entity <br>&emsp; + Define schema fields: `user (@ManyToOne)`, `userWorkoutPlan (@ManyToOne)`, `workoutDate (LocalDate)`, `isActive (default true)`, `weekIndex`, `dayIndex` <br>&emsp; + Establish a Cascade DELETE mechanism to `WorkoutLog` via `@OneToMany(cascade = ALL)` to ensure synchronized data lifecycle management | 02/10/2026 | 02/10/2026 | |
| 3   | - Architect the **WorkoutLog** entity <br>&emsp; + Detailed schema: `userWorkoutSession (@ManyToOne)`, `exercise (@ManyToOne)`, `setNumber`, `reps`, `weight (Float)`, `durationSeconds` | 02/11/2026 | 02/11/2026 | |
| 3   | - Implement the **UserWorkoutSessionController** (`/api/sessions`) <br>&emsp; + `POST /` — instantiate a new workout session <br>&emsp; + `GET /{id}`, `GET /user/{userId}`, `GET /user/{userId}/by-date?date=` — comprehensive retrieval endpoints <br>&emsp; + `GET /user/{userId}/active` — retrieve the currently active, in-progress session <br>&emsp; + `PATCH /{sessionId}/deactivate` — mark session as gracefully completed <br>&emsp; + `POST /{sessionId}/logs` — securely log an individual exercise set | 02/11/2026 | 02/11/2026 | |
| 4   | - Develop the **PlanDetailScreen** frontend interface <br>&emsp; + Visually render the active plan's name and description <br>&emsp; + Integrate a day-of-week selector (Mon–Sun) to dynamically filter daily exercise rosters <br>&emsp; + "Start Workout" action: triggers `POST /api/sessions` API and bridges seamless navigation to the `WorkoutSessionScreen` <br>&emsp; + Display an alert banner if a session is currently in progress <br>&emsp; + Enable intuitive navigation to `SessionCalendar` for historical tracking | 02/12/2026 | 02/12/2026 | |
| 5   | - Deploy the **workoutSessionSlice** (Redux Toolkit) <br>&emsp; + State architecture: `sessionId`, `planId`, `exercises[]`, `currentExIndex`, `currentSet`, `completedSets[]`, `phase (workout/rest/done)`, `restSeconds`, `isSavingLog` <br>&emsp; + Define Actions: `initializeSession`, `restoreSessionState`, `logSetStart/Success/Failure`, `finishRest`, `skipExercise`, `skipSet`, `resetSession` <br>&emsp; + Integrate `AsyncStorage` to reliably persist session state per set — enabling robust automatic hydration and crash recovery upon app reload | 02/13/2026 | 02/13/2026 | |
| 6   | - Construct the highly interactive **WorkoutSessionScreen** <br>&emsp; + `ActiveExerciseCard`: visually project the active exercise name, current set progress, and target reps <br>&emsp; + `LogSetSheet`: capture actual reps & weight input → dispatch `addWorkoutLog` API calls <br>&emsp; + `RestTimer`: intelligent countdown timer with auto-advancement to the subsequent exercise/set post-rest <br>&emsp; + Finish execution: invoke `deactivateSession` → `resetSession` → route to `WorkoutSuccessScreen` <br> - Develop the **WorkoutSuccessScreen** <br>&emsp; + Trophy dashboard visualizing workout summary statistics (completed exercises, total sets, aggregate reps) <br>&emsp; + Deeply integrate hardware back button interception to prevent accidental flow exits, forcing a safe route to the Home screen | 02/14/2026 | 02/14/2026 | |

### Week 6 Achievements:

* **Backend — Session module**:
  * The `UserWorkoutSession` and `WorkoutLog` entities are successfully persisted with exact ORM mapping; cascade DELETE operations securely prevent orphan data leaks.
  * The `POST /api/sessions` endpoint robustly instantiates sessions tied to user plans, while `PATCH /{id}/deactivate` reliably processes session finalization.
  * The `POST /api/sessions/{id}/logs` API accurately captures high-fidelity telemetry for individual exercise sets (exercise references, set index, reps, and weight).
  * The `GET /user/{userId}/active` endpoint successfully detects and retrieves any lingering in-progress sessions.
  * Date-based parameter filtering via `GET /by-date?date=` performs flawlessly for historical session lookups.
* **Frontend — Live Workout**:
  * The `PlanDetailScreen` fluidly loads the active plan context; the day-of-week selector accurately filters and maps the corresponding daily exercises.
  * Redux `workoutSessionSlice` acts as the single source of truth, managing the entire session lifecycle and flawlessly persisting state to AsyncStorage for bulletproof crash recovery.
  * The `WorkoutSessionScreen` intuitively navigates users through complex multi-exercise routines, seamlessly synchronized with the automated rest timer.
  * Each set is securely logged to the backend via the `addWorkoutLog` API prior to allowing user advancement, ensuring zero data loss.
  * The `WorkoutSuccessScreen` elegantly displays performance metrics while aggressively safeguarding the session conclusion by disabling native back navigation.
  * **Frontend UI Optimization**: Completely eradicated critical stale closure bugs within the timer execution loop and systematically consolidated redundant `useEffect` dependencies. Leveraged `useRef` to securely anchor active session states, guaranteeing silky-smooth 60fps timer renders without congesting the main UI thread.
  * The end-to-end live workout feature loop (plan definition → session initiation → granular set logging → rest intervals → completion → success summary) passed all integration scenarios with excellence.

### AWS Knowledge Learned:

* Mastered the art of structured logging design for Cloud environments: standardizing core payload fields (severity, requestId, userId, endpoint, errorCode) to drastically accelerate search and filtering across **Amazon CloudWatch**.
* Identified and monitored critical Service Level Indicators (SLIs) for the real-time workout flow: session initialization latency, log-write success rates, and active-session state consistency.
* Internalized proactive CloudWatch Alarm design philosophies, establishing alerting thresholds around p95 latency percentiles, 5xx HTTP error rates, and relational database resource pressure.
* Engineered actionable operational dashboards, prioritizing decision-driving metrics over cluttered, non-contextual data visualization walls.
* Practiced a disciplined incident response framework: encompassing detection, rigorous triage, rapid mitigation, validation, and post-mortem documentation for continuous improvement.
* Reinforced a strict "Rollback-First" deployment philosophy when encountering faulty releases, ruthlessly prioritizing the mitigation of user impact over immediate root-cause investigation.
* Architected a forward-looking alert routing strategy, preparing the infrastructure for **Amazon SNS** (or equivalent notification channels) integration to handle critical production escalations.

In summary, Week 6 successfully elevated the team's perspective from basic feature development to a proactive, cloud-native operational monitoring and incident response mindset.

### Next Week Plan:

* **Backend**: Initialize the core Media management subsystem — designing the `Image` entity, deeply integrating with **Amazon S3** via the AWS SDK, and building the `ImageController` to route static assets for exercises, nutritional items, and workout plans.
* **Frontend**: Develop the `SessionDetailScreen` (a comprehensive performance recap dashboard) and the `SessionCalendarScreen` (a monthly calendar view visualizing workout density via session markers).