---
title: "Week 9 Worklog"
date: 2026-03-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* **Backend**: Architect and initialize the User Metric module — encompassing the `BodyMetric` entity for capturing anthropometric data and the `HealthCalculation` engine for processing core physiological metrics (BMI, BMR, TDEE).
* **Frontend**: Construct a comprehensive health management UI ecosystem, including the `HealthDashboardScreen` (integrating goal input and dynamic calculations), `BodyMetricListScreen`, `BodyMetricFormScreen`, alongside advanced data visualization chart components.
* Transform raw telemetry into clear, actionable insights, empowering users to seamlessly monitor their physical well-being and establish precise caloric targets.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Construct the **BodyMetric** entity (table `body_metric`) <br>&emsp; + Define schema fields: `user (@ManyToOne)`, `heightCm (Float, min=50)`, `weightKg (Float, min=20)`, `age (Integer, min=10)`, `gender (Gender enum)`, `activityLevel (ActivityLevel enum)` <br>&emsp; + Implement `BodyMetricController` (`/api/body-metrics`): providing creation APIs, user-specific retrieval (prioritizing the latest records), and ID-based get/update/delete operations | 03/09/2026 | 03/09/2026 | |
| 3   | - Design the **HealthCalculation** entity (table `health_calculation`) <br>&emsp; + Establish mapping: `user`, `bodyMetric (optional snapshot FK)`, `bmi`, `bmr`, `tdee`, `goalType (GoalTypes enum)` <br>&emsp; + Apply standard formulas: <br>&emsp;&emsp; BMI = weight / (height in m)² <br>&emsp;&emsp; BMR (Mifflin-St Jeor): Men = 10×w + 6.25×h - 5×age + 5; Women = −5 <br>&emsp;&emsp; TDEE = BMR × activity multiplier | 03/10/2026 | 03/10/2026 | |
| 3   | - Deploy the **HealthMetricsController** (`/api/metrics`) <br>&emsp; + `POST /calculate` — execute calculations and persist BMI/BMR/TDEE derived from the `CalculateMetricsRequest` payload <br>&emsp; + Expose endpoints: `GET /user/{userId}` (complete historical data), `GET /user/{userId}/latest`, `GET /{id}` | 03/10/2026 | 03/10/2026 | |
| 4   | - Develop the **HealthDashboardScreen** (Frontend) <br>&emsp; + Integrate `WheelPicker` for an optimized height (cm) and weight (kg) input UX <br>&emsp; + Deploy `ActivityLevel` dropdown selector <br>&emsp; + Goal classification: Cutting / Bulking / Maintain / UP_Power <br>&emsp; + On submission workflow: trigger `createBodyMetric` + `calculateMetrics` → render `HealthResultCard` (displaying BMI, BMR, TDEE, Macros) <br>&emsp; + Implement fallback redirection to Profile if gender/birthdate data is missing | 03/11/2026 | 03/11/2026 | |
| 5   | - Architect health **chart components** (`src/components/health/`) <br>&emsp; + `BMITrendChart`: line chart featuring time-range filters (7d/30d/90d/all) + dynamic color-coding for BMI thresholds <br>&emsp; + `WeightChart`: line chart + integrated 7-day moving average trend line + goal weight proximity calculation <br>&emsp; + `CalorieEnergyCharts`: dual-line BMR/TDEE visualization with mode toggle capabilities <br>&emsp; + `MacrosDisplay`: 3 progress bars (protein/carbs/fat) annotated with quantitative (grams) and percentage labels <br>&emsp; + `HealthResultCard`: comprehensive summary card aggregating all computed physiological metrics | 03/12/2026 | 03/12/2026 | |
| 6   | - Build the **BodyMetricListScreen** (Frontend) <br>&emsp; + Render complete measurement history with standardized temporal formatting <br>&emsp; + Pin `WeightChart` prominently at the top of the list view <br>&emsp; + Integrate inline edit (routing to `BodyMetricFormScreen`) and secure deletion via `ConfirmModal` <br> - Build the **BodyMetricFormScreen** <br>&emsp; + Facilitate creation/update flows: height, weight, activity level, goal type <br>&emsp; + Automatically extract and synchronize gender + age from the `authSlice` user state | 03/13/2026 | 03/13/2026 | |
| 7   | - Execute **Backend endpoint refinement** Phase 1 <br>&emsp; + Rigorously apply `@Valid` annotations and custom constraint validators across all request DTOs <br>&emsp; + Standardize pagination payloads: wrapping data in `PageResponse<T>` coupled with `page`, `size`, `totalPages`, `totalElements` <br>&emsp; + Inject time-range filtering (`GET /api/sessions/user/{userId}?startDate=&endDate=`) into the session history endpoint | 03/14/2026 | 03/14/2026 | |
| 8   | - Engineer comprehensive **Unit tests** (Spring Boot Test + JUnit 5 + Mockito) <br>&emsp; + `UserWorkoutPlanServiceTest`: validate cloning logic, activation mechanisms, and IDOR prevention <br>&emsp; + `HealthCalculationServiceTest`: test edge cases for BMI/BMR/TDEE mathematical models <br>&emsp; + `UserWorkoutSessionServiceTest`: verify active session queries and deactivation behaviors | 03/15/2026 | 03/15/2026 | |

### Week 9 Achievements:

* **Backend — User Metric module**:
  * `BodyMetric` records are securely persisted, passing stringent validation layers (height ≥ 50 cm, weight ≥ 20 kg, age ≥ 10).
  * Successfully integrated **AWS Secrets Manager** to dynamically retrieve PostgreSQL credentials and JWT secrets at runtime, strictly encrypted via **AWS KMS**.
  * The BMR calculation engine operates flawlessly per the Mifflin-St Jeor equation; TDEE accurately maps physical activity multipliers (1.2 – 1.9).
  * The `POST /api/metrics/calculate` endpoint reliably returns a standardized `HealthCalculationResponse`, delivering BMI, BMR, TDEE, and goal-specific macronutrient allocations (protein/carbs/fat grams).
  * Historical data queries are optimized, ensuring records strictly adhere to a `createdAt DESC` sort order.
  * Concluded Phase 1 of API refinement: all DTOs are governed by `@Valid`, pagination utilizes the `PageResponse<T>` envelope, and session time-range filters are fully operational.
  * Finalized a high-coverage Unit test suite: `UserWorkoutPlanServiceTest`, `HealthCalculationServiceTest`, and `UserWorkoutSessionServiceTest` pass all edge-case scenarios utilizing Mockito-based mocking.
* **Frontend — Health Dashboard**:
  * The `HealthDashboardScreen`'s wheel pickers deliver a remarkably smooth user experience (UX) for height and weight data entry.
  * Optimized render lifecycle: the `HealthResultCard` immediately reflects API responses, aggressively mitigating redundant BMI/BMR recalculations on the main thread.
  * Successfully integrated the `react-native-gifted-charts` library, architecting a high-performance and deeply customizable charting system.
  * The `BMITrendChart` dynamically color-codes data points based on health thresholds: underweight (blue), normal (green), overweight (yellow), and obese (red).
  * The `WeightChart` intelligently activates a moving average trend line once the user accumulates ≥ 7 data points.
  * The `BodyMetricListScreen` operates seamlessly, displaying complete historical measurements alongside fluid inline CRUD capabilities.

### AWS Knowledge Learned:

* Internalized cloud secret management paradigms: transitioning from long-lived plaintext distribution to leveraging **AWS Secrets Manager** or **SSM Parameter Store**.
* Grasped core **AWS KMS** architectures, including at-rest data encryption, key policy boundaries, and the intricate relationship between IAM and key execution rights.
* Understood secret rotation lifecycles and the application-level reload strategies required when configuration values change.
* Strictly enforced environment isolation principles, guaranteeing that dev, staging, and production credentials remain entirely siloed.
* Leveraged **AWS CloudTrail** to establish robust audit trails, continuously monitoring modifications to sensitive configurations and permission boundaries.
* Applied the security principle of minimizing long-lived credentials, prioritizing short-lived, role-based access tokens across all system touchpoints.
* Directly mapped these infrastructure security controls to the practical requirement of safeguarding sensitive user identities and physiological health data.

In summary, Week 9 successfully shifted the focus from feature implementation to enforcing robust AWS security controls that protect both system access and sensitive application data.

### Next Week Plan:

* **Backend**: Initiate the AWS deployment pipeline (Dockerize → push to ECR → operate on ECS Fargate behind an ALB), provision AWS WAF to enforce rate limiting, and conduct comprehensive configuration reviews.
* **Frontend**: Develop the `ChatScreen` featuring an intelligent virtual assistant powered by AWS Bedrock AI, and finalize the `ProfileScreen` for personal data and account management.