---
title: "Week 10 Worklog"
date: 2026-03-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* **Frontend**:
  * Develop the `ChatScreen` integrating the **AWS Bedrock (Claude 3.5 Haiku)** virtual assistant featuring a sliding-window context memory mechanism. Finalize the `ProfileScreen` for comprehensive user identity management.
  * Implement robust **token refresh logic** utilizing an Axios interceptor queue, guaranteeing a seamless and uninterrupted user experience (UX) during session expirations.
  * Construct the `HomeScreen` to serve as the central unified dashboard, comprehensively resolve all outstanding bugs, and heavily polish the global UX across the application.
* **Backend**:
  * Prepare the production deployment infrastructure: configure Docker Compose health checks, push immutable images to **Amazon ECR**, provision task definitions for **ECS Fargate**, and activate the **AWS WAF** security shield.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Optimize backend endpoints <br>&emsp; + Rigorously apply `@Valid` annotations and custom constraint validators across all request DTOs <br>&emsp; + Standardize paginated responses utilizing the `PageResponse<T>` envelope alongside metadata: `page`, `size`, `totalPages`, `totalElements` <br>&emsp; + Inject the date-range filter `GET /api/sessions/user/{userId}?startDate=&endDate=` to facilitate session history queries | 03/16/2026 | 03/16/2026 | |
| 3   | - Engineer comprehensive **Unit tests** (Spring Boot Test + JUnit 5 + Mockito) <br>&emsp; + `UserWorkoutPlanServiceTest`: validate cloning logic, activation mechanisms, and IDOR vulnerability prevention <br>&emsp; + `HealthCalculationServiceTest`: test edge cases for BMI/BMR/TDEE mathematical formulas <br>&emsp; + `UserWorkoutSessionServiceTest`: verify active session queries and deactivation behaviors | 03/17/2026 | 03/17/2026 | |
| 4   | - Architect **chatService** (Frontend) <br>&emsp; + Establish direct communication with the AWS Bedrock Runtime API (bypassing Lambda proxies) <br>&emsp; + Integrate the `anthropic.claude-3-5-haiku-20241022-v1:0` model <br>&emsp; + Design a Vietnamese system prompt: shaping a professional fitness coach persona <br>&emsp; + Transmit the last 12 conversation turns as context (applying the sliding window memory algorithm) | 03/18/2026 | 03/18/2026 | <https://docs.aws.amazon.com/bedrock/> |
| 5   | - Construct the **ChatScreen** interface (Frontend) <br>&emsp; + Design a full-screen chat UI featuring an intuitive user/bot message bubble layout <br>&emsp; + Integrate a typing indicator animation (3 bouncing dots) during AI inference wait times <br>&emsp; + Deploy 4 quick-access action chips: "Suggest exercises", "Today's menu", "Calorie goal", "Weight loss advice" <br>&emsp; + Initialize a default Vietnamese greeting from the AI coach <br>&emsp; + Optimize UX with seamless Keyboard Avoiding mechanics <br>&emsp; + Intercept and process errors via a centralized `notifyAlert` proxy | 03/19/2026 | 03/19/2026 | |
| 6   | - Develop the **ProfileScreen** (Frontend) <br>&emsp; + Render personal data: avatar (initial-letter fallback), name, email, username, birthdate, gender <br>&emsp; + Edit modal: update birthdate (YYYY-MM-DD) and gender via the `updateUserProfile` API combined with `dispatch(updateUserProfile)` <br>&emsp; + Logout flow: execute `signOut` (revoke Cognito session) + `dispatch(logout)` + purge secure storage <br>&emsp; + Account deletion flow: `deleteUserProfile` + `signOut` — strictly safeguarded by a `ConfirmModal` | 03/20/2026 | 03/20/2026 | |
| 7   | - Implement the **HomeScreen** (Frontend) — synchronize 5 parallel data fetching streams on mount <br>&emsp; + Nutrition: aggregate calories from `MealFood` → render daily progress bar against the 2500 kcal target <br>&emsp; + Health: extract the latest `HealthCalculation` → display BMI index <br>&emsp; + Metrics: extract the latest `BodyMetric` → reflect current height/weight <br>&emsp; + Plan: display the active Workout Plan name → provide quick navigation to `PlanDetail` <br>&emsp; + Weekly progress: compute this-week session count → compare against the 4-session target | 03/21/2026 | 03/21/2026 | |
| 7   | - Prepare **Backend deployment environment** <br>&emsp; + Refine Docker Compose configurations integrating `postgres` health checks and `Spring Actuator` liveness/readiness probes <br>&emsp; + CI/CD pipeline: Build and push immutable images to **Amazon ECR** <br>&emsp; + Establish task definitions for **ECS Fargate** coupled with an **ALB** for serverless runtime architecture <br>&emsp; + Deploy **AWS WAF** to protect public-facing endpoints | 03/21/2026 | 03/21/2026 | |
| 8   | - Execute a comprehensive **bug fix** and UX optimization campaign across core Frontend screens <br>&emsp; + `WorkoutSessionScreen`: definitively resolve the edge case where a user completes all exercises before the rest timer concludes <br>&emsp; + `DietScreen`: restructure `ensureDailyMeals` logic into a `useEffect` with empty dependencies, eliminating redundant re-renders <br>&emsp; + `HealthDashboardScreen`: inject loading skeleton states while awaiting the `calculateMetrics` API resolution | 03/22/2026 | 03/22/2026 | |


### Week 10 Achievements:

* **Backend**:
  * All request DTOs are successfully encapsulated with `@Valid` constraints, finalizing the rigorous validation pipeline inherited from Week 9.
  * The API system has achieved production-ready status: standardizing `PageResponse<T>` for all paginated endpoints, ensuring flawless date-range filtering, and passing 100% of test cases.
  * Production images were successfully pushed to **Amazon ECR**; task definitions are fully initialized for **ECS Fargate** operating behind an **ALB**.
  * **AWS WAF** is fully configured, providing a robust defensive shield for public endpoints against common web attack vectors.
  * Integrated health checks for PostgreSQL and Spring Actuator guarantee resilient container startups and stable operations.
  * Comprehensively documented `.env.example`, detailing over 10 essential environment variables with precise technical descriptions.
* **Frontend**:
  * The `chatService.sendChatToBedrock` module establishes direct communication with AWS Bedrock Runtime, securely interpolating credentials from the `.env` file.
  * The sliding window memory architecture (12 conversational turns) excellently maintains AI context while heavily optimizing token expenditure.
  * The action chips system provides an instantaneous interactive experience requiring zero typing from the user.
  * Typing indicator effects deliver a natural conversational feel; keyboard avoidance mechanisms operate flawlessly across iOS and Android ecosystems.
  * Profile data is hydrated directly from the Redux `authSlice` — achieving zero API calls upon screen mount.
  * The edit modal employs an optimistic UI update philosophy (updating local state prior to API confirmation), maximizing interface responsiveness.
  * Sensitive actions (logout, account deletion) are routed through multi-layered confirmation flows — completely neutralizing the risk of accidental data loss.
  * Parallel API invocation architecture via `Promise.all` on the `HomeScreen` achieved impressive performance, resolving in under 1.5s in local environments.
  * The data visualization dashboard (daily calories, workout progress, BMI) provides a comprehensive health snapshot directly from the main screen.
  * Completely eradicated application freeze (crash) bugs occurring during the final set within the `WorkoutSessionScreen`.
  * Eliminated 4 redundant API calls per re-render on the `DietScreen` by strictly controlling the `ensureDailyMeals` lifecycle.
  * The Axios interceptor queue mechanism perfectly resolves race conditions when multiple concurrent requests yield 401 errors, preventing logout screen flickering.
  * Consolidated all alert prompts into the internal `NotificationBox` component, unifying the application's design language.
  * Comprehensively standardized loading states and error handling mechanisms throughout the entire system.
  * Finalized localization efforts, ensuring all Vietnamese labels are accurate, consistent, and professional.

### AWS Knowledge Learned:

* **Backend**:
  * Mastered the container artifact packaging workflow: optimizing Docker builds, applying immutable tags, and publishing images to **Amazon ECR**.
  * Deeply understood how **ECS Fargate** abstracts infrastructure via task definitions, services, health checks, and auto-scaling mechanisms based on desired counts.
  * Architected AWS container networking: establishing private subnets, routing egress traffic through NAT Gateways, and configuring integrated ALB target groups.
  * Strategized zero-downtime deployment rollouts leveraging rolling updates and fine-tuning minimum healthy percent parameters.
  * Practiced the principle of strictly decoupling runtime configurations from container images via task-level environment variables and secure secret injection.
  * Established quality gates within CI/CD GitHub Actions: mandating the successful passage of all linting, testing, and building phases prior to triggering AWS deployment pipelines.
  * Correlated release observability with rollback decision-making, ensuring deployment safety is grounded in measurable, real-world runtime behavior.
  * In summary, successfully translated theoretical AWS container architecture into a production-grade deployment pipeline for the Backend.
* **Frontend**:
  * Internalized static SPA delivery architecture: synergizing **Amazon S3** static hosting with **Amazon CloudFront** to establish global caching and minimize page load latency.
  * Mastered end-to-end TLS/HTTPS setup processes: from certificate issuance via **ACM** and domain validation to configuring DNS routing on **Route 53**.
  * Deployed the **Origin Access Control (OAC)** model, securing the S3 bucket in an absolute private state while enabling CloudFront to safely distribute content to end-users.
  * Designed intelligent cache-key and invalidation strategies, successfully balancing the trade-off between real-time UI updates and maximizing CDN caching efficiency.
  * Deployed **AWS WAF** at the edge layer as a proactive defense shield against scraper bots and common web application attack vectors.
  * Resolved SPA-specific client-side routing challenges by configuring custom error responses on CloudFront, ensuring deep links remain consistently functional.
  * Cultivated modern frontend release mentalities: applying cache busting techniques, resource versioning, and guaranteeing instant rollback capabilities.
  * In summary, finalized the delivery layer architecture on AWS, elevating the Frontend to the standards of a globally accessible, production-ready application.

### Next Week Plan:

* Execute a comprehensive **End-to-End (E2E) Integration Testing** campaign encompassing all screens and features — guaranteeing flawless execution across 6 core user journeys.
* Draft in-depth **project documentation** (including the Backend README, Frontend Development Guide, and an overarching Architecture Diagram).
* **Eradicate Technical Debt (Code Cleanup)**: purge console/debug statements, resolve all linting warnings, and secure a consistent CI/CD build pass status.
* Perform final refinements: optimize the Response Interceptor, apply final UX polishing, and squash remaining residual bugs (e.g., handling the empty state of `BMITrendChart`).
* Conduct a comprehensive project Retrospective meeting and initiate formal handoff procedures.