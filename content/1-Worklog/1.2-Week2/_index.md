---
title: "Week 2 Worklog"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* **Backend**: Integrate AWS Cognito authentication into Spring Security. Build the `UserProfile` module.
* **Frontend**: Implement the complete authentication flow — from the Login screen through token storage and state management.
* Establish secure token handling patterns that will be reused across the entire application.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Study AWS Cognito User Pools concepts <br>&emsp; + User Pool configuration: password policies, app clients, PKCE <br>&emsp; + ID Token vs Access Token — difference and proper usage <br>&emsp; + Cognito JWT claims: `sub`, `cognito:groups`, `token_use` | 01/13/2026 | 01/13/2026 | <https://docs.aws.amazon.com/cognito/> |
| 3   | - Implement **Spring Security** JWT configuration <br>&emsp; + Add `spring-security-oauth2-resource-server` dependency <br>&emsp; + Configure `SecurityConfig`: stateless sessions, CSRF disabled, CORS enabled <br>&emsp; + Set Cognito issuer-uri in `application.properties` <br>&emsp; + Write custom `OAuth2TokenValidator` — reject tokens where `token_use != "access"` | 01/14/2026 | 01/14/2026 | <https://docs.spring.io/spring-security/> |
| 3   | - Implement role extraction from JWT <br>&emsp; + Read `cognito:groups` claim → convert to Spring `ROLE_<GROUP>` authorities <br>&emsp; + Configure `@PreAuthorize("hasRole('ADMIN')")` for admin endpoints <br>&emsp; + Define authorization rules: public endpoints, authenticated, admin-only | 01/14/2026 | 01/14/2026 | |
| 4   | - Build **UserProfile** entity & repository <br>&emsp; + Fields: `cognitoId (UNIQUE)`, `email (UNIQUE)`, `username`, `name`, `gender`, `birthdate`, `phoneNumber`, `picture`, `emailVerified` <br>&emsp; + `UserProfileRepository` extends `JpaRepository` | 01/15/2026 | 01/15/2026 | |
| 4   | - Build **UserProfileService** & **UserProfileController** <br>&emsp; + `POST /user/sync` — upsert profile from Cognito claims (IDOR-safe: `cognitoSub` from JWT `sub`) <br>&emsp; + `GET /user/{id}`, `PUT /user/{id}`, `DELETE /user/{id}` | 01/15/2026 | 01/15/2026 | |
| 5   | - Build **Frontend** `LoginScreen` <br>&emsp; + Single "Sign in with AWS Cognito" button <br>&emsp; + Initiate PKCE flow via `expo-auth-session` + `expo-web-browser` <br>&emsp; + Handle redirect callback: exchange code → tokens <br>&emsp; + Decode ID Token with `jwt-decode` to extract user claims | 01/16/2026 | 01/16/2026 | <https://docs.expo.dev/guides/authentication/> |
| 6   | - Build **authSlice** (Redux) and token storage <br>&emsp; + State: `isAuthenticated`, `user`, `token`, `refreshToken`, `hasCompletedOnboarding` <br>&emsp; + Actions: `login`, `logout`, `completeOnboarding`, `updateUserProfile` <br>&emsp; + Persist tokens to `expo-secure-store` (mobile) / `localStorage` (web) via `utils/storage.ts` <br> - Wire Axios **request interceptor**: auto-attach `Authorization: Bearer <token>` | 01/17/2026 | 01/17/2026 | |

### Week 2 Achievements:

* **Backend — Security**:
  * Spring Security fully configured with AWS Cognito as JWT issuer.
  * Custom `OAuth2TokenValidator` blocks ID tokens — only Access Tokens accepted at the API layer.
  * Role-based access control works: `ROLE_ADMIN` group from Cognito grants admin privileges.
  * All security rules defined: public health check, authenticated user routes, admin-only `/admin/**` routes.
* **Backend — UserProfile module**:
  * `POST /user/sync` correctly upserts user from Cognito JWT claims without IDOR vulnerability.
  * Full CRUD (`GET`, `PUT`, `DELETE`) on `/user/{id}` with proper authorization checks.
  * `UserProfile` entity persisted to PostgreSQL via JPA.
* **Frontend — Authentication**:
  * `LoginScreen` renders correctly; tapping the button opens Cognito Hosted UI in browser.
  * PKCE code exchange works end-to-end — tokens returned and stored securely.
  * `authSlice` correctly toggles `isAuthenticated`; `RootNavigator` redirects to the right stack.
  * Axios interceptor auto-attaches Bearer token — subsequent API calls authenticated.

### AWS Knowledge Learned:

* Studied Cognito User Pools in detail: app clients, callback URLs, logout URLs, OAuth scopes, and token lifetime tradeoffs for a mobile application.
* Understood the PKCE flow end to end, including `code_verifier`, `code_challenge`, and `S256`, and why it is essential for public clients.
* Distinguished ID token and access token in a real API context, reinforcing that backend authorization must rely on access token claims rather than ID token data.
* Learned a robust JWT validation path: `iss`, `aud`, `exp`, `nbf`, `token_use`, and signature validation through Cognito JWKs.
* Practiced mapping `cognito:groups` into internal app roles like `ROLE_ADMIN` and `ROLE_USER` to support secure RBAC.
* Understood refresh-token storage boundaries for mobile clients, especially secure storage, rotation handling, and forced logout on invalid refresh.
* Reinforced the importance of using the immutable `sub` claim as the stable user identity across all backend modules.

In summary, week 2 connected AWS identity concepts directly to the authentication and authorization model of the project.

### Next Week Plan:

* **Backend**: Build the common infrastructure layer — `GlobalExceptionHandler`, `CorsConfig`. Implement the `GoalType` module (first business module).
* **Frontend**: Build navigation foundation — `RootNavigator`, `AuthStack`, `MainTabs` with custom tab bar, `OnboardingStack`.
