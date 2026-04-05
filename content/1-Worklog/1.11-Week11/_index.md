---
title: "Week 11 Worklog"
date: 2026-03-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Execute a comprehensive **End-to-End (E2E) Integration Testing** campaign encompassing the entire myFit application ecosystem (all features, all screens) to guarantee release quality.
* Author **comprehensive technical documentation** — including the Backend README, detailed API references, and overarching architecture blueprints.
* Perform rigorous **Technical Debt Eradication (Code Cleanup)**, systematically purging debug artifacts, optimizing the codebase, and preparing the project for formal handoff.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - **End-to-End Integration Testing** — Backend <br>&emsp; + Rigorously verify all API endpoints against valid (happy path) and invalid (edge case) payloads <br>&emsp; + Validate Flyway V1/V2/V3 migrations executing flawlessly on a pristine database <br>&emsp; + Assess Docker Compose cold start performance: ensuring the `db` and `api` cluster achieves a healthy state in under 30s <br>&emsp; + Audit `application.properties` — strictly preventing dev-environment configurations from leaking into production | 03/23/2026 | 03/23/2026 | |
| 3   | - **End-to-End Integration Testing** — Frontend (6 core user journeys) <br>&emsp; + Journey 1: Authentication → Onboarding data collection → Home Dashboard <br>&emsp; + Journey 2: Personal Plan Creation → System template cloning → Plan activation <br>&emsp; + Journey 3: Live Session Initiation → Granular set telemetry logging → Rest timer management → Workout completion → Success summary <br>&emsp; + Journey 4: Distribute foods across 4 standard meals → Reconcile daily caloric aggregates <br>&emsp; + Journey 5: Anthropometric data entry → BMI/BMR/TDEE algorithmic processing → Chart visualization <br>&emsp; + Journey 6: AI Assistant interaction → Evaluate Bedrock's natural language responses in Vietnamese | 03/24/2026 | 03/24/2026 | |
| 4   | - Author **Backend README** (`myFit-api/README.md`) <br>&emsp; + Project overview & architectural data flow (Spring Boot API ↔ PostgreSQL ↔ AWS Cognito ↔ AWS S3) <br>&emsp; + Module-specific documentation: Auth, Food, SystemWorkout, UserWorkoutPlan, Session, UserMetric, Media, GoalType <br>&emsp; + Setup guide: prerequisites, `.env` configurations, Docker Compose orchestration commands <br>&emsp; + API Reference catalog: comprehensive routing, HTTP methods, payload schemas, and authorization requirements | 03/25/2026 | 03/25/2026 | |
| 4   | - Finalize **Frontend Development Guide** (`guide.md`) <br>&emsp; + Tech stack ecosystem summary table <br>&emsp; + Architectural Navigation Tree diagram (AuthStack / OnboardingStack / MainTabs) <br>&emsp; + Local setup workflow: `npm install`, `.env` management, Expo initialization commands <br>&emsp; + Screen dictionary detailing specific feature allocations <br>&emsp; + Technical documentation for AWS Cognito PKCE and AWS Bedrock integration | 03/25/2026 | 03/25/2026 | |
| 5   | - **Code Cleanup** — Backend <br>&emsp; + Systematically sweep and purge all `TODO`, `FIXME`, and debug `System.out.println` artifacts <br>&emsp; + Enforce Javadoc documentation for all non-trivial public business logic methods <br>&emsp; + Audit Spring Security configurations to prevent inadvertent exposure of protected routes <br>&emsp; + Final artifact build validation: execute `mvn clean package -DskipTests` to confirm seamless JAR compilation | 03/26/2026 | 03/26/2026 | |
| 5   | - Optimize **Response Interceptor** + **UX Polish** + **Final Bug Fix** (deferred from Week 10) <br>&emsp; + Finalize the multi-threaded Token Refresh mechanism: enqueue concurrent `401` unauthorized requests, execute a singular refresh cycle, and seamlessly retry the queue <br>&emsp; + Deploy `NotificationBox` as a global alert proxy, entirely supplanting the native `Alert.alert` <br>&emsp; + Integrate Pull-to-refresh on `HomeScreen` + refine skeleton loading logic on `HealthDashboardScreen` <br>&emsp; + Standardize Vietnamese localization for all enum values (`ActivityLevelLabels`) <br>&emsp; + Harmonize loading spinners and fallback error states across the global UI | 03/26/2026 | 03/26/2026 | |
| 6   | - **Code Cleanup** — Frontend + **Final Bug Fix** <br>&emsp; + Eradicate all `console.log` debug statements <br>&emsp; + Execute `eslint` and definitively resolve all lingering warnings <br>&emsp; + Tree-shake and remove orphaned/unused imports <br>&emsp; + Patch the `BMITrendChart` empty state anomaly (deferred from Week 10) <br>&emsp; + Production export validation: run `npx expo export` to confirm zero TypeScript compilation errors <br> - Conduct **Project Retrospective**: synthesize technical lessons learned, evaluate architectural decisions, and outline a future upgrade roadmap | 03/27/2026 | 03/27/2026 | |

### Week 11 Achievements:

* **Integration Testing**:
  * 100% of backend API endpoints successfully passed manual testing against both valid (happy path) and invalid (edge case) input scenarios.
  * Docker Compose cold starts demonstrated high reliability, achieving API readiness within 25 seconds of initialization.
  * Flyway DB migrations (V1-V3) executed with deterministic stability on a pristine PostgreSQL instance.
  * All 6 core user journeys were navigated end-to-end flawlessly without encountering any critical blockers.
* **Documentation**:
  * The Backend README now provides an exhaustive setup guide, environment variable blueprints, and granular API specifications.
  * The Frontend Development Guide was finalized with navigation trees, screen inventories, and AWS service mappings.
  * The holistic data flow architecture was clearly documented: Spring Boot API ↔ PostgreSQL ↔ AWS Cognito ↔ AWS S3 ↔ React Native App ↔ AWS Bedrock.
* **CI/CD & Code Quality**:
  * Successfully integrated GitHub Actions workflows to automate the entire CI/CD pipeline: Linting → Testing → Building → Rolling Deployment updates directly to **Amazon ECS Fargate** upon code merge.
  * Source code achieved a clean state with zero `console.log` or print statements persisting in production execution paths.
  * Automated TypeScript compilation (`npx expo export`) passed CI quality gates with zero errors.
  * The Maven pipeline automatically compiled and packaged the final JAR artifact seamlessly within the CI environment.
* **Project Retrospective — Key Lessons Learned**:
  * **IDOR Prevention**: Mandating JWT `sub` extraction for identity resolution is an uncompromising security pattern across user-scoped REST APIs.
  * **Soft Deletion**: Leveraging `@SQLRestriction` offers a vastly superior user experience compared to hard deletes, enabling data recovery and preserving historical integrity.
  * **Stateless JWT Architecture**: This pattern eliminates backend session management overhead, but necessitates highly robust frontend handling for token revocation (forced logouts) and renewal (refresh queues) to preserve UX.
  * **React Native + NativeWind**: This synergy dramatically accelerates mobile UI development velocity by bringing TailwindCSS paradigms directly into native environments.
  * **State Management Separation**: The deliberate architecture where **Redux** governs static auth/session states and **React Query** manages server caching and background synchronization proved highly scalable and efficient.

### AWS Knowledge Learned:

* Conducted a comprehensive **AWS Well-Architected Framework** review, evaluating the infrastructure against the five core pillars: Security, Reliability, Performance Efficiency, Cost Optimization, and Operational Excellence.
* Formulated a robust **Reliability** posture through rigorous dependency health checks, strategic backup/restore planning, and defining pragmatic RPO/RTO baselines.
* Embraced a **FinOps** methodology by identifying right-sizing opportunities, enforcing S3 lifecycle policies, configuring budget anomaly alerts, and purging orphaned resources.
* Finalized a **Defense in Depth** security hardening checklist encompassing IAM boundary audits, token policy enforcement, KMS encryption scoping, secret handling, and CloudTrail audit logging.
* Authored comprehensive **Cloud Operations Runbooks** detailing procedures for deployments, emergency rollbacks, incident triage, and routine operational health validations.
* Established strict **Handover Readiness** criteria, guaranteeing environment reproducibility, transparent documentation, and explicit shared responsibility boundaries.
* Architected a **Future Cloud Roadmap**, proposing strategies for ECS Auto-scaling optimization, ALB WAF rule hardening, advanced CloudFront CDN tuning, improved observability, and staged production rollouts.

In summary, Week 11 successfully translated theoretical AWS knowledge into an actionable operational review and a robust system handover plan.

### Next Week Plan:

* Finalize self-assessment documentation and synthesize constructive feedback.
* Complete and submit the official internship report.