---
title: "Week 3 Worklog"
date: 2026-01-19
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* **Backend**: Establish the common infrastructure layer — standardizing centralized exception handling, global CORS configuration, and initializing the `GoalType` module.
* **Frontend**: Architect a comprehensive navigation hierarchy and deploy a multi-step Onboarding wizard to optimize the initial user experience.
* Ensure a seamless core user flow from Authentication → Onboarding → Main Application, establishing a robust foundation prior to integrating deeper business logic.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Implement **com.example.fitme.common.exception.GlobalExceptionHandler** (`@RestControllerAdvice`) <br>&emsp; + Intercept and process `MethodArgumentNotValidException` → extract granular field-level validation errors <br>&emsp; + Handle `EntityNotFoundException` alongside custom business exceptions <br>&emsp; + Encapsulate all error payloads within a standardized `ApiResponse` envelope alongside appropriate HTTP status codes | 01/20/2026 | 01/20/2026 | |
| 2   | - Configure global **CorsConfig** <br>&emsp; + Grant access (allow origins) for local development and production frontend environments <br>&emsp; + Configure permitted headers: `Authorization`, `Content-Type` <br>&emsp; + Authorize all HTTP methods (GET, POST, PUT, DELETE, OPTIONS) | 01/20/2026 | 01/20/2026 | |
| 3   | - Construct the **GoalType** module <br>&emsp; + Design Entity: `name (UNIQUE)`, `description` <br>&emsp; + Initialize Repository, Service, and Controller layers <br>&emsp; + Expose **public** endpoints: `POST /api/goal-types`, `GET /api/goal-types`, `GET /api/goal-types/{id}`, `GET /api/goal-types/by-name/{name}` | 01/21/2026 | 01/21/2026 | |
| 4   | - Develop **Frontend** `RootNavigator` (within `src/navigation/`) <br>&emsp; + Listen to `isAuthenticated` and `hasCompletedOnboarding` states from the Redux `authSlice` <br>&emsp; + Dynamically route between `AuthStack` / `OnboardingStack` / `MainTabs` based on state <br> - Build **AuthStack**: Integrate `WelcomeScreen` and `LoginScreen` <br> - Build **MainTabs** featuring a `CustomTabBar` <br>&emsp; + Allocate 4 core tabs: Home, Workout, Diet, Health <br>&emsp; + Design a prominent, centered Floating Action Button (Chat) symmetrically splitting the tabs | 01/22/2026 | 01/22/2026 | <https://reactnavigation.org/> |
| 5   | - Implement **OnboardingStack** (5-step data collection wizard) <br>&emsp; + Step 1: Gender, Date of Birth (slider inputs), Height, Weight <br>&emsp; + Step 2: Physical activity frequency and occupational nature <br>&emsp; + Step 3: Primary fitness goals, Target weight, Priority body areas for improvement <br>&emsp; + Step 4: Dietary preferences, Food allergies, Daily meal frequency, Hydration targets <br>&emsp; + Step 5: Training experience, Workout environments, Frequency, Session duration | 01/23/2026 | 01/23/2026 | |
| 6   | - Integrate **CaloriesScreen** and **TrainingScreen** into the onboarding summary preview <br>&emsp; + `CaloriesScreen`: Animated UI demonstrating calorie management <br>&emsp; + `TrainingScreen`: Preview of a day-by-day weekly workout schedule <br> - Handle onboarding completion event: trigger `dispatch(completeOnboarding())` and seamlessly synchronize the user profile via the `POST /user/sync` API | 01/24/2026 | 01/24/2026 | |

### Week 3 Achievements:

* **Backend — Common Layer**:
  * `GlobalExceptionHandler` successfully intercepts all standard and custom exceptions, guaranteeing that client payloads strictly adhere to the unified `ApiResponse` envelope.
  * Global CORS policies are effectively enforced, allowing the frontend operating on `localhost:8081` to seamlessly communicate with the API server at `localhost:8080`.
* **Backend — GoalType module**:
  * The `GoalType` entity is successfully persisted within the PostgreSQL database, supporting fundamental classifications such as **Weight Loss**, **Muscle Gain**, and **Maintenance**.
  * Successfully deployed 4 public endpoints facilitating creation, complete list retrieval, and targeted queries by ID or name.
  * Finalized this foundational module, establishing the mapping layer required to align workout plans with personalized user fitness objectives.
* **Frontend — Navigation**:
  * The `RootNavigator` system operates flawlessly, driving precise absolute navigation strictly derived from authentication and onboarding states.
  * The `MainTabs` interface renders an intuitive `CustomTabBar`, highlighted by a centrally positioned Floating Action Button (Chat) that symmetrically splits the layout.
  * Fully initialized and registered all core navigation stacks: `AuthStack`, `OnboardingStack`, `WorkoutStack`, `DietStack`, and `HealthStack`.
* **Frontend — Onboarding**:
  * The 5-step Wizard process is fully functional, enhanced with smooth slide transitions and animations.
  * Successfully captured and structured comprehensive user demographic and preference data (biometrics, goals, nutritional habits) ready for profile storage.
  * The onboarding resolution precisely triggers the `completeOnboarding()` action, seamlessly facilitating bidirectional data synchronization with the Backend.

### AWS Knowledge Learned:

* Mastered the underlying mechanics and implications of CORS when frontend and backend architectures are distributed across independent domains or subdomains within a Cloud ecosystem.
* Gained a clear understanding of preflight request (`OPTIONS`) behavior and troubleshooting strategies for access blocks caused by missing HTTP methods or headers in production environments.
* Cultivated an API security zoning mindset, strictly delineating public, authenticated, and admin-only endpoints to establish a solid foundation for future **Amazon API Gateway** integration.
* Deeply recognized the value of standardizing response envelopes for monitoring, drastically optimizing log tracing and error analysis within **Amazon CloudWatch**.
* Applied request correlation concepts by attaching `requestId` or `traceId` metadata, providing crucial support for debugging workflows in distributed systems.
* Understood the strategic benefit of centralized exception handling in harmonizing error patterns, thereby enabling more accurate and automated alarm configurations.
* Refactored the codebase toward an observability-driven design by comprehensively standardizing log formats and response payloads.

In summary, Week 3 emphasized the application of Cloud architecture principles and standardized API design to enhance system stability and future observability.

### Next Week Plan:

* **Backend**: Initialize and architect the System Workout subsystem — encompassing `MuscleGroup`, `Exercise`, `WorkoutPlan`, and `WorkoutPlanExercise` entities, alongside administrative CRUD APIs and public read endpoints for clients.
* **Frontend**: Implement the `SuggestedPlanScreen` interface (a 3-step personalized workout plan proposal wizard) and the `PlanExercisePicker` component.