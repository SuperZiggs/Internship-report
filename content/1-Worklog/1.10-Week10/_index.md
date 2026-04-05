---
title: "Week 10 Worklog"
date: 2026-03-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* **Frontend**:
  * Build the `ChatScreen` integrating **AWS Bedrock (Claude 3.5 Haiku)** with a sliding-window memory, and `ProfileScreen` with user account management.
  * Implement robust **token refresh logic** via Axios interceptor queues for seamless UX during session expiration.
  * Build `HomeScreen` as the central dashboard, resolve all outstanding bugs, and polish the overall UX across the app.
* **Backend**:
  * Prepare for deployment: Docker Compose health checks, push image to **Amazon ECR**, define **ECS Fargate** task, enable **AWS WAF**.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Backend endpoint refinements <br>&emsp; + Add `@Valid` annotations and custom constraint validators to all request DTOs <br>&emsp; + Standardize paginated responses: `PageResponse<T>` with `page`, `size`, `totalPages`, `totalElements` <br>&emsp; + Add `GET /api/sessions/user/{userId}?startDate=&endDate=` date-range filter for session history | 03/16/2026 | 03/16/2026 | |
| 3   | - Write **unit tests** (Spring Boot Test + JUnit 5 + Mockito) <br>&emsp; + `UserWorkoutPlanServiceTest`: clone method, activate logic, IDOR prevention <br>&emsp; + `HealthCalculationServiceTest`: BMI/BMR/TDEE formulas for various inputs <br>&emsp; + `UserWorkoutSessionServiceTest`: active session query, deactivate behavior | 03/17/2026 | 03/17/2026 | |
| 4   | - Build **chatService** (Frontend) <br>&emsp; + Direct AWS Bedrock Runtime API call (no Lambda proxy) <br>&emsp; + Model: `anthropic.claude-3-5-haiku-20241022-v1:0` <br>&emsp; + Vietnamese system prompt: fitness coach persona <br>&emsp; + Send last 12 conversation turns as context (sliding window memory) | 03/18/2026 | 03/18/2026 | <https://docs.aws.amazon.com/bedrock/> |
| 5   | - Build **ChatScreen** (Frontend) <br>&emsp; + Full-screen chat UI with message bubble list (user / bot) <br>&emsp; + Animated "typing" indicator (3 bouncing dots) while waiting for response <br>&emsp; + 4 quick-option chips: "Suggest exercises", "Today’s menu", "Calorie goal", "Weight loss advice" <br>&emsp; + Vietnamese initial greeting from the fitness bot <br>&emsp; + Animated keyboard avoidance <br>&emsp; + `notifyAlert` error handling via global alert proxy | 03/19/2026 | 03/19/2026 | |
| 6   | - Build **ProfileScreen** (Frontend) <br>&emsp; + Display avatar (initial-letter fallback), name, email, username, birthdate, gender <br>&emsp; + Edit modal: update birthdate (YYYY-MM-DD) and gender via `updateUserProfile` API + `dispatch(updateUserProfile)` <br>&emsp; + Logout: `signOut` (Cognito revoke) + `dispatch(logout)` + clear secure storage <br>&emsp; + Delete account: `deleteUserProfile` + `signOut` — both gated by `ConfirmModal` | 03/20/2026 | 03/20/2026 | |
| 7   | - Build **HomeScreen** (Frontend) — 5 parallel data fetches on mount <br>&emsp; + Today’s meals: sum MealFood calories → daily calorie progress bar vs. 2500 kcal target <br>&emsp; + Latest `HealthCalculation` → show BMI <br>&emsp; + Latest `BodyMetric` → show current height/weight <br>&emsp; + Active workout plan name → quick link to PlanDetail <br>&emsp; + This-week session count → weekly progress vs. 4 sessions target | 03/21/2026 | 03/21/2026 | |
| 7   | - Backend deployment preparation <br>&emsp; + Refine Docker Compose with `postgres` health checks and `Spring Actuator` liveness/readiness probes <br>&emsp; + CI/CD: Push immutable backend image to **Amazon ECR** <br>&emsp; + Define **ECS Fargate** task definition mapped to **ALB** for serverless runtime <br>&emsp; + Protect public endpoints with **AWS WAF** | 03/21/2026 | 03/21/2026 | |
| 8   | - Comprehensive **bug fix** session across key Frontend screens <br>&emsp; + Fix `WorkoutSessionScreen`: edge case when all exercises completed before timer finishes <br>&emsp; + Fix `DietScreen`: ensure `ensureDailyMeals` not called on every render — move to `useEffect` with empty deps <br>&emsp; + Fix `HealthDashboardScreen`: loading state shown during `calculateMetrics` call | 03/22/2026 | 03/22/2026 | |


### Week 10 Achievements:

* **Backend**:
  * All request DTOs have `@Valid` constraints and validation was completed in Week 9.
  * API tested and ready: `PageResponse<T>` standardizes all paginated endpoints, date-range filtering works, unit tests all pass.
  * Production image pushed successfully to **Amazon ECR**, task definition created for **ECS Fargate** behind **ALB**.
  * **AWS WAF** configured and protecting public endpoints from common attacks.
  * Health checks on PostgreSQL and Spring Actuator ensure reliable container startup.
  * `.env.example` fully documents all 10+ required environment variables with descriptions.
* **Frontend**:
  * `chatService.sendChatToBedrock` calls AWS Bedrock Runtime directly using credentials from `.env`.
  * Sliding window of 12 turns maintains conversation context economically.
  * Quick-option chips provide instant first interaction without typing.
  * Typing indicator creates natural chat feel; keyboard avoidance works on both iOS and Android.
  * Profile data loaded from Redux `authSlice` — no extra API call needed on screen mount.
  * Edit modal updates local state optimistically and confirms via API.
  * Logout and delete account flows both require confirmation — no accidental data loss.
  * 5 parallel `Promise.all` API calls complete in under 1.5s on localhost.
  * Daily calorie + weekly session progress indicators give users clear at-a-glance goals.
  * BMI and body metric snapshot visible on home without navigating away.
  * Workout session completion edge case resolved — app no longer freezes on final set.
  * `ensureDailyMeals` called only once on first mount — eliminated 4 redundant API calls per render.
  * Token refresh queue prevents logged-out flash when multiple concurrent requests get 401.
  * `NotificationBox` replaces native alerts — consistent in-app notification style.
  * All loading states and error messages standardized across the application.
  * Vietnamese labels complete and consistent throughout the UI.

### AWS Knowledge Learned:

* **Backend**:
  * Learned the full container artifact flow: optimized Docker builds, immutable image tagging, and publishing backend images to Amazon ECR.
  * Understood how ECS Fargate models deployment through task definitions, services, health checks, and desired task count.
  * Studied container networking on AWS, including private subnets, outbound access through NAT, and ALB target group integration.
  * Learned safe rollout patterns such as rolling updates and minimum healthy percent to reduce release risk.
  * Practiced separating runtime configuration from the image itself through task-level environment variables and secret injection.
  * Understood how GitHub Actions quality gates should protect AWS deployment stages by requiring lint, test, and build success first.
  * Connected release observability with rollback decisions so deployment safety is based on measurable runtime behavior.
  * In summary, connected AWS container platform knowledge directly to a realistic deployment path for the backend.
* **Frontend**:
  * Learned frontend delivery architecture using S3 static hosting together with CloudFront for global caching and lower latency.
  * Studied HTTPS setup from end to end: ACM certificate issuance, validation workflow, and Route 53 DNS routing.
  * Understood the Origin Access Control model so S3 can remain private while CloudFront serves user traffic securely.
  * Learned cache-key and invalidation strategy to balance fresh UI delivery with strong CDN efficiency.
  * Studied AWS WAF as a baseline web-protection layer for common attacks and noisy traffic patterns.
  * Understood edge error handling and fallback strategies required to support SPA routing without broken deep links.
  * Practiced frontend release thinking with cache busting, controlled propagation, and rollback-friendly asset versioning.
  * In summary, focused on the AWS delivery layer that would make the frontend production-ready and globally accessible.

### Next Week Plan:

* Run full end-to-end integration testing of the entire application (all features, all screens) — 6 user journeys.
* Write comprehensive project documentation (Backend README, Frontend guide, architecture overview).
* Code cleanup: remove debug statements, fix linting, ensure CI/CD builds pass.
* Response interceptor refinement + UX polish + final bug fixes (BMITrendChart empty state handling).
* Project retrospective and preparation for handoff.
