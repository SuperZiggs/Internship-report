---
title: "Week 2 Worklog"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* **Backend**: Integrate Enterprise-grade authentication flows using **Amazon Cognito User Pools** combined with Spring Security. Initialize and build the `UserProfile` module.
* **Frontend**: Implement a comprehensive authentication flow via the **PKCE (Proof Key for Code Exchange)** protocol — covering everything from the login screen rendering and secure token storage to robust user state management.
* Establish an optimal token lifecycle management pattern, including automatic background refresh capabilities and strict handling of Token Revocation scenarios.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Conduct in-depth research on AWS Cognito User Pools <br>&emsp; + User Pool setup: password policy configuration, app clients, and PKCE standards <br>&emsp; + Analyze and clarify the distinctions between ID Tokens and Access Tokens <br>&emsp; + Investigate crucial JWT claims: `sub`, `cognito:groups`, `token_use` | 01/13/2026 | 01/13/2026 | <https://docs.aws.amazon.com/cognito/> |
| 3   | - Integrate and configure JWT for **Spring Security** <br>&emsp; + Add the `spring-security-oauth2-resource-server` dependency <br>&emsp; + Configure `SecurityConfig`: apply stateless mechanism, disable CSRF, and enable CORS <br>&emsp; + Declare the Cognito issuer-uri in `application.properties` <br>&emsp; + Customize `OAuth2TokenValidator` to strictly reject tokens where `token_use != "access"` | 01/14/2026 | 01/14/2026 | <https://docs.spring.io/spring-security/> |
| 3   | - Develop role extraction mechanism from JWT <br>&emsp; + Read `cognito:groups` claims and map them to Spring's `ROLE_<GROUP>` format <br>&emsp; + Apply `@PreAuthorize("hasRole('ADMIN')")` to secure admin endpoints <br>&emsp; + Standardize authorization rules: public areas, authenticated zones, and admin-only routes | 01/14/2026 | 01/14/2026 | |
| 4   | - Design the **UserProfile** entity and repository <br>&emsp; + Define data fields: `cognitoId (UNIQUE)`, `email (UNIQUE)`, `username`, `name`, `gender`, `birthdate`, `phoneNumber`, `picture`, `emailVerified` <br>&emsp; + Initialize `UserProfileRepository` extending the `JpaRepository` interface | 01/15/2026 | 01/15/2026 | |
| 4   | - Develop **UserProfileService** and **UserProfileController** <br>&emsp; + `POST /user/sync` API — execute profile upserts based on Cognito claims (ensuring IDOR safety by utilizing the `cognitoSub` mapped from the JWT `sub`) <br>&emsp; + Provide standard CRUD endpoints: `GET /user/{id}`, `PUT /user/{id}`, `DELETE /user/{id}` | 01/15/2026 | 01/15/2026 | |
| 5   | - Design the **Frontend** `LoginScreen` interface <br>&emsp; + Position the "Sign in with AWS Cognito" button as the exclusive authentication method <br>&emsp; + Initiate the PKCE flow through the integration of `expo-auth-session` and `expo-web-browser` <br>&emsp; + Handle redirect callbacks: safely exchange authorization codes for tokens <br>&emsp; + Utilize the `jwt-decode` library to decode ID Tokens for user information extraction | 01/16/2026 | 01/16/2026 | <https://docs.expo.dev/guides/authentication/> |
| 6   | - Implement **authSlice** (Redux Toolkit) and token storage mechanisms <br>&emsp; + State structure: `isAuthenticated`, `user`, `token`, `refreshToken`, `hasCompletedOnboarding` <br>&emsp; + Declare Actions: `login`, `logout`, `completeOnboarding`, `updateUserProfile` <br>&emsp; + Manage flexible token storage: utilize `expo-secure-store` for mobile and `localStorage` for web via the `utils/storage.ts` module <br> - Set up Axios **request interceptor**: automatically attach the `Authorization: Bearer <token>` header to all outgoing requests | 01/17/2026 | 01/17/2026 | |

### Week 2 Achievements:

* **Backend — Security**:
  * Successfully finalized Spring Security configuration integrating **Amazon Cognito** as the JWT issuer, smoothly operating Enterprise-standard RBAC (Role-Based Access Control).
  * Successfully deployed a customized `OAuth2TokenValidator` to prevent the misuse of ID Tokens — the API system now strictly accepts only Access Tokens.
  * Role-based access control functions accurately: `cognito:groups` claims are seamlessly mapped to `ROLE_ADMIN`, ensuring administrative privileges are granted to the right entities.
  * Established strict security boundaries: clearly delineated public zones for health checks, general authenticated zones, and `/admin/**` areas restricted to administrators.
* **Backend — UserProfile module**:
  * The `POST /user/sync` endpoint securely executes user data upserts from Cognito JWT claims, comprehensively mitigating IDOR vulnerability risks.
  * Delivered a complete suite of CRUD operations (`GET`, `PUT`, `DELETE`) via the `/user/{id}` path, accompanied by rigorous authorization checks.
  * The `UserProfile` entity was successfully mapped and persisted into the PostgreSQL database via JPA.
* **Frontend — Authentication**:
  * The `LoginScreen` UI renders accurately; button interactions successfully trigger the secure Cognito Hosted UI opening process within the system's default browser.
  * The **PKCE code exchange** mechanism operates flawlessly end-to-end via `expo-auth-session`, completely eliminating the security risks associated with storing Secret Keys on the client side.
  * The `authSlice` module dynamically updates the `isAuthenticated` state, supporting `RootNavigator` in precise navigation between interface flows (stacks).
  * The Axios interceptor automatically attaches Bearer tokens to headers and smoothly handles **Token Lifecycle management** (supporting automatic background refreshes and forced logouts upon token invalidation).

### AWS Knowledge Learned:

* Delved deeply into the architectural setup of Cognito User Pools, including: app client configuration, callback and logout URLs, OAuth scope optimization, and strategic token lifetime settings tailored for mobile application environments.
* Mastered the complete operational cycle of the PKCE protocol (from `code_verifier` and `code_challenge` to the `S256` hashing algorithm), while clearly understanding its absolute necessity for public client deployments.
* Strictly differentiated the roles of ID Tokens and Access Tokens in real-world API applications, internalizing the core principle: the Backend must exclusively use Access Tokens for authorization.
* Fully grasped the JWT validation pipeline, from verifying standard claims such as `iss`, `aud`, `exp`, `nbf`, and `token_use`, to validating the JWK signature provided by Cognito.
* Gained proficiency in mapping `cognito:groups` claims to internal application roles (e.g., `ROLE_ADMIN`, `ROLE_USER`), laying a solid foundation for robust RBAC security model implementation.
* Acquired deep insights into the art of refresh token management on mobile platforms: covering secure storage, token rotation mechanisms, and forced logout strategies when the refresh process fails.
* Affirmed and reinforced the immutable design principle: Utilizing the `sub` claim as the unique, unchangeable user identifier across all Backend modules.

In summary, Week 2 tightly coupled foundational AWS Identity concepts with a practical engineering mindset for building real-world authentication and authorization systems.

### Next Week Plan:

* **Backend**: Focus on building robust foundational system layers, including `GlobalExceptionHandler` for centralized error management and `CorsConfig`. Initiate the design and development of the first business module, `GoalType`.
* **Frontend**: Construct a comprehensive navigation framework for the application, including `RootNavigator`, `AuthStack`, a `MainTabs` system integrated with a custom tab bar, and an `OnboardingStack` introduction flow.