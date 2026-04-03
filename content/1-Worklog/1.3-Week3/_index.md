---
title: "Week 3 Worklog"
date: 2026-01-19
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* **Backend**: Build the application-wide common layer — exception handling, CORS, and the `GoalType` module.
* **Frontend**: Scaffold the complete navigation hierarchy and implement the multi-step Onboarding flow.
* Ensure end-to-end flow from login → onboarding → main app works before adding deeper features.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Implement **GlobalExceptionHandler** (`@RestControllerAdvice`) <br>&emsp; + Handle `MethodArgumentNotValidException` → field-level validation errors <br>&emsp; + Handle `EntityNotFoundException`, custom business exceptions <br>&emsp; + All errors returned in `ApiResponse` envelope with appropriate HTTP status | 01/20/2026 | 01/20/2026 | |
| 2   | - Configure **CorsConfig** globally <br>&emsp; + Allow origins for local dev and production frontend URLs <br>&emsp; + Allow headers: `Authorization`, `Content-Type` <br>&emsp; + Allow all HTTP methods | 01/20/2026 | 01/20/2026 | |
| 3   | - Build **GoalType** module <br>&emsp; + Entity: `name (UNIQUE)`, `description` <br>&emsp; + Repository, Service, Controller <br>&emsp; + Endpoints (all **public**): `POST /api/goal-types`, `GET /api/goal-types`, `GET /api/goal-types/{id}`, `GET /api/goal-types/by-name/{name}` | 01/21/2026 | 01/21/2026 | |
| 4   | - Build **Frontend** `RootNavigator` <br>&emsp; + Reads Redux `isAuthenticated` + `hasCompletedOnboarding` from `authSlice` <br>&emsp; + Routes to `AuthStack` (unauthenticated) / `OnboardingStack` (pending) / `MainTabs` (ready) <br> - Build **AuthStack**: `WelcomeScreen` + `LoginScreen` <br> - Build **MainTabs** with `CustomTabBar` <br>&emsp; + 4 bottom tabs: Home, Workout, Diet, Health <br>&emsp; + Center floating Chat button splitting left/right tabs | 01/22/2026 | 01/22/2026 | <https://reactnavigation.org/> |
| 5   | - Build **OnboardingStack** (5-step wizard) <br>&emsp; + Step 1: Gender + Date-of-birth (day/month/year sliders) + Height + Weight <br>&emsp; + Step 2: Activity level + Job type <br>&emsp; + Step 3: Main fitness goal + Target weight + Target body areas <br>&emsp; + Step 4: Diet preferences + Food allergies + Meals per day + Water intake <br>&emsp; + Step 5: Experience level + Workout location + Frequency + Session duration | 01/23/2026 | 01/23/2026 | |
| 6   | - Add **CaloriesScreen** and **TrainingScreen** to Onboarding preview <br>&emsp; + CaloriesScreen: animated calorie management demo UI <br>&emsp; + TrainingScreen: weekly workout plan by day preview <br> - Wire onboarding completion: `dispatch(completeOnboarding())` → sync user profile to backend via `POST /user/sync` | 01/24/2026 | 01/24/2026 | |

### Week 3 Achievements:

* **Backend — Common Layer**:
  * `GlobalExceptionHandler` catches all standard and custom exceptions; returns consistent `ApiResponse` to clients.
  * CORS configured globally — frontend running on `localhost:8081` can communicate with the API on `localhost:8080`.
* **Backend — GoalType module**:
  * `GoalType` entity persisted to PostgreSQL; supports types like **Weight Loss**, **Muscle Gain**, **Maintenance**.
  * All 4 public endpoints operational: create, list all, get by ID, get by name.
  * GoalTypes serve as the backbone for linking workout plans to user fitness goals.
* **Frontend — Navigation**:
  * `RootNavigator` correctly redirects based on auth + onboarding state — no logic in individual screens.
  * `MainTabs` renders with `CustomTabBar`; tab bar splits left (Home, Workout) and right (Diet, Health) around a center Chat button.
  * All stacks registered: `AuthStack`, `OnboardingStack`, `WorkoutStack`, `DietStack`, `HealthStack`.
* **Frontend — Onboarding**:
  * Full 5-step wizard implemented with slide animations between steps.
  * User data collected (gender, DOB, height, weight, goals, diet prefs, workout prefs) ready to be stored in profile.
  * Onboarding completion dispatches `completeOnboarding()` and calls the backend sync endpoint.

### AWS Knowledge Learned:

* Learned the cloud deployment implications of CORS when frontend and backend live on different domains or subdomains.
* Understood preflight `OPTIONS` behavior and how misconfigured headers or methods can silently break authenticated production traffic.
* Mapped the current API structure to a future API Gateway style boundary with clear separation of public, authenticated, and admin routes.
* Recognized why standardized error envelopes make CloudWatch troubleshooting faster and client-side debugging more predictable.
* Practiced the idea of request correlation using request IDs or trace IDs so multi-service debugging becomes easier later.
* Understood how centralized exception handling supports cleaner alarm rules because error shapes become consistent.
* Prepared the code structure for future tracing and observability integration by keeping responses and logs structured.

In summary, week 3 focused on AWS-relevant API governance and observability foundations rather than infrastructure provisioning alone.

### Next Week Plan:

* **Backend**: Build the System Workout module — `MuscleGroup`, `Exercise`, `WorkoutPlan`, `WorkoutPlanExercise` entities with admin CRUD and public read endpoints.
* **Frontend**: Implement the `SuggestedPlanScreen` (3-step plan selection wizard) and `PlanExercisePicker`.
