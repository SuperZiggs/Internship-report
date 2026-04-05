---
title: "Week 1 Worklog"
date: 2026-01-05
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives

* Kick off the deployment of the **myFit** project — a comprehensive health and fitness management platform.
* Initialize and configure project repositories for both sides: Backend (using Spring Boot) and Frontend (using React Native integrated with Expo).
* Set up the local development environment, including: Java 17, Node.js, Expo CLI, and a PostgreSQL database running via Docker.
* Finalize the overall system architecture, technology stack, and module breakdown structure with the entire team.

### Tasks to be carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Project kickoff: finalize scope, features, and MVP requirements <br>&emsp; + Core features: workout roadmaps, nutrition management, body metrics tracking, session logging <br>&emsp; + Authentication/Login via AWS Cognito <br>&emsp; + Build an Admin panel to manage exercise data | 01/06/2026 | 01/06/2026 | |
| 3   | - Set up the **Backend** project using Spring Boot 3 and Maven <br>&emsp; + Add essential dependencies: `spring-boot-starter-web`, `spring-data-jpa`, `spring-security` <br>&emsp; + Customize `pom.xml` to integrate Flyway, Lombok, jjwt 0.11.5, and AWS SDK v1 <br>&emsp; + Structure directories under `com.example.fitme`: `common/`, `config/`, `module/` | 01/07/2026 | 01/07/2026 | <https://start.spring.io/> |
| 4   | - Deploy local **PostgreSQL** via Docker Compose <br>&emsp; + Write `docker-compose.yml` to run the `postgres:15` service <br>&emsp; + Configure `application.properties`: database info, JPA `ddl-auto=create-drop`, Flyway settings <br>&emsp; + Establish `.env` and `.env.example` files to secure sensitive information <br> - Build the `EntityBase` class with `@MappedSuperclass` containing: `id (UUID)`, `createdAt`, `updatedAt` | 01/08/2026 | 01/08/2026 | <https://docs.docker.com/compose/> |
| 4   | - Set up the **Frontend** project: React Native + Expo ~54 (with TypeScript ~5.9) <br>&emsp; + Execute `npx create-expo-app --template` <br>&emsp; + Customize `tsconfig.json`, `babel.config.js`, and `metro.config.js` files <br>&emsp; + Integrate NativeWind v4 and update `tailwind.config.js` | 01/08/2026 | 01/08/2026 | <https://docs.expo.dev/> |
| 5   | - Design the system **entity-relationship diagram** (ERD) <br>&emsp; + Define tables: UserProfile, Food, Meal, Exercise, WorkoutPlan, BodyMetric, Session, Image… <br>&emsp; + Clarify relationships and cardinalities between entities <br> - Create a standardized **ApiResponse\<T\>** wrapper class consisting of: `code`, `message`, `result`, `timestamp`, `path` | 01/09/2026 | 01/09/2026 | |
| 6   | - Configure **Redux Toolkit** for Frontend state management <br>&emsp; + Install `@reduxjs/toolkit` and `react-redux` packages <br>&emsp; + Wrap `store/index.ts` and `app/providers.tsx` into the root `App.tsx` file <br> - Set up **Axios** client to fetch data using the base URL from the env file (`EXPO_PUBLIC_BACKEND_API_URL`) <br> - Initialize **TanStack React Query** v5 with `QueryClient` | 01/10/2026 | 01/10/2026 | <https://redux-toolkit.js.org/> |

### Week 1 Achievements

* Successfully built the basic skeleton for both the backend and frontend, ensuring smooth operation in the local environment.
* **Backend (Spring Boot)**:
  * The system boots successfully and maintains a stable connection with PostgreSQL running in the background via Docker Compose (`docker-compose up -d`).
  * The `EntityBase` class, integrating a UUID primary key and audit timestamps, is fully complete and ready for inheritance.
  * Standardized the API response format across all future endpoints through the `ApiResponse<T>` wrapper class.
  * The Maven build process runs smoothly without any compilation errors.
* **Frontend (React Native + Expo)**:
  * The app interface renders successfully on the Android emulator using the `npx expo start` command.
  * NativeWind v4 operates stably, allowing the direct use of TailwindCSS utility classes within React Native components.
  * State management (Redux store) and data fetching (React Query `QueryClient`) have been fully integrated into the system via `providers.tsx`.
  * Axios successfully reads the base URL configuration from the `.env` file (`EXPO_PUBLIC_BACKEND_API_URL=http://localhost:8080`).
* Ensured the consistency of the Docker Compose environment, allowing team members to easily reproduce it without configuration discrepancies.
* Applied the standard practice of managing environment variables via `.env`, completely eliminating the hardcoding of sensitive credentials and paving the way for future Backend API and CloudFront CDN integration.

### AWS Knowledge Accumulated

* Gained a clear understanding of the process for setting up a production-standard AWS account: enforcing MFA, clearly separating admin privileges, and applying the least-privilege principle for both developer and CI roles.
* Grasped the method of separating identities based on purpose (manual user access, deployment role, runtime application role) to strictly limit the blast radius in case of risks.
* Proficiently used AWS CLI profile strategies for `dev` and `staging` environments, combined with fixed region selection to prevent accidental cross-environment operations.
* Deeply understood the AWS credential provider chain and recognized the critical importance of never committing access keys or secret keys to the source code repository.
* Practiced tagging resources according to standards (`Project`, `Environment`, `Owner`, `CostCenter`) to optimize cost tracking and future system management.
* Mastered the AWS Shared Responsibility Model with concrete project mapping: AWS manages the core infrastructure, while the team remains responsible for the application logic, IAM policies, and sensitive data security.
* Built a cloud-ready system configuration mindset early on: all sensitive information is decoupled from the code base, ready to be migrated to AWS Secrets Manager or Systems Manager (SSM).

In summary, the first week helped shape a professional AWS infrastructure management mindset before diving into specific service implementations.

### Next Week's Plan

* **Backend**: Begin AWS Cognito integration — set up `SecurityConfig` to support a JWT resource server, develop a custom `OAuth2TokenValidator`, and finalize the `UserProfile` entity alongside the `UserProfileController` with sync/CRUD endpoints.
* **Frontend**: Develop the full Authentication flow — implement `LoginScreen` applying the PKCE OAuth standard via the `expo-auth-session` library, securely store tokens using `expo-secure-store`, and control the login state via `authSlice`.