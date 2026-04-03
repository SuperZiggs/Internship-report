---
title: "Week 11 Worklog"
date: 2026-03-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* **Backend**: Final API review, Docker Compose production-readiness, and complete environment variable documentation.
* **Frontend**: Build `HomeScreen` as the central dashboard, resolve all outstanding bugs, and polish the overall UX across the app.
* Achieve a fully functional, integrated application ready for final testing.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Build **HomeScreen** (Frontend) — 5 parallel data fetches on mount <br>&emsp; + Today’s meals: sum MealFood calories → daily calorie progress bar vs. 2500 kcal target <br>&emsp; + Latest `HealthCalculation` → show BMI <br>&emsp; + Latest `BodyMetric` → show current height/weight <br>&emsp; + Active workout plan name → quick link to PlanDetail <br>&emsp; + This-week session count → weekly progress vs. 4 sessions target | 03/24/2026 | 03/24/2026 | |
| 3   | - Backend **Docker Compose** final refinements <br>&emsp; + Add `healthcheck` to `postgres` service: `pg_isready` with `interval`, `timeout`, `retries` <br>&emsp; + Add `depends_on.db.condition: service_healthy` to API service <br>&emsp; + Verify `Spring Actuator` `/actuator/health` liveness + readiness probes <br>&emsp; + Complete `.env.example` with all required variables documented | 03/25/2026 | 03/25/2026 | |
| 4   | - Comprehensive **bug fix** session across all Frontend screens <br>&emsp; + Fix `WorkoutSessionScreen`: edge case when all exercises completed before timer finishes <br>&emsp; + Fix `DietScreen`: ensure `ensureDailyMeals` not called on every render — move to `useEffect` with empty deps <br>&emsp; + Fix `HealthDashboardScreen`: loading state shown during `calculateMetrics` call <br>&emsp; + Fix `BMITrendChart`: empty state when user has fewer than 2 health calculations | 03/26/2026 | 03/26/2026 | |
| 5   | - **Response interceptor** improvements (Axios `client.ts`) <br>&emsp; + Implement token refresh with request queue: concurrent `401` responses queued, one refresh attempted, all retried <br>&emsp; + On refresh failure: `forceLogout` clears tokens + dispatches Redux `logout` + navigates to Login <br>&emsp; + Handle `code === 4040` "user not found for cognitoId" → auto `forceLogout` | 03/27/2026 | 03/27/2026 | |
| 6   | - **UX polish** pass across all screens <br>&emsp; + Add `NotificationBox` global alert component (replaces native `Alert.alert` via `installAlertProxy`) <br>&emsp; + Add pull-to-refresh on `HomeScreen` <br>&emsp; + Ensure consistent loading spinners and error states across all screens <br>&emsp; + Add `ActivityLevelLabels` Vietnamese display names for all enum values <br>&emsp; + Vietnamese label consistency check across navigation tabs and headings | 03/28/2026 | 03/28/2026 | |

### Week 11 Achievements:

* **Backend — Docker Compose**:
  * Health check on PostgreSQL service ensures the API container waits for DB to be fully ready.
  * `GET /actuator/health` returns `{status: UP}` with liveness/readiness sub-probes.
  * `.env.example` fully documents all 10+ required environment variables with descriptions.
* **Frontend — HomeScreen**:
  * 5 parallel `Promise.all` API calls complete in under 1.5s on localhost.
  * Daily calorie + weekly session progress indicators give users clear at-a-glance goals.
  * BMI and body metric snapshot visible on home without navigating away.
* **Frontend — Bug Fixes**:
  * Workout session completion edge case resolved — app no longer freezes on final set.
  * `ensureDailyMeals` called only once on first mount — eliminated 4 redundant API calls per render.
  * Token refresh queue prevents logged-out flash when multiple concurrent requests get 401.
* **Frontend — UX Polish**:
  * `NotificationBox` replaces native alerts — consistent in-app notification style.
  * All loading states and error messages standardized across the application.
  * Vietnamese labels complete and consistent throughout the UI.

### AWS Knowledge Learned (Assumed Application):

* Learned frontend delivery architecture using S3 static hosting together with CloudFront for global caching and lower latency.
* Studied HTTPS setup from end to end: ACM certificate issuance, validation workflow, and Route 53 DNS routing.
* Understood the Origin Access Control model so S3 can remain private while CloudFront serves user traffic securely.
* Learned cache-key and invalidation strategy to balance fresh UI delivery with strong CDN efficiency.
* Studied AWS WAF as a baseline web-protection layer for common attacks and noisy traffic patterns.
* Understood edge error handling and fallback strategies required to support SPA routing without broken deep links.
* Practiced frontend release thinking with cache busting, controlled propagation, and rollback-friendly asset versioning.

In summary, week 11 focused on the AWS delivery layer that would make the frontend production-ready and globally accessible.

### Next Week Plan:

* Run full end-to-end integration test of the complete application (all features, all screens).
* Write project documentation (README, API reference, architecture diagram).
* Prepare final demo and code cleanup for project handoff.
