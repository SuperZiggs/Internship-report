---
title: "Week 7 Worklog"
date: 2026-02-23
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* **Backend**: Architect and initialize the Media module — establishing a highly optimized polymorphic `Image` entity, integrating deeply with **Amazon S3**, and deploying an **Amazon CloudFront** distribution for accelerated asset delivery.
* **Frontend**: Construct the comprehensive `SessionDetailScreen` and `SessionCalendarScreen` to visualize workout history, and engineer a robust `mediaService` mapped to CloudFront URLs for lightning-fast, cached image rendering.
* Optimize global media delivery through a robust CDN architecture, completely superseding direct S3 bucket invocations to minimize latency and bandwidth costs.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Construct the **Image** entity (`module/media`) <br>&emsp; + Define schema fields: `url (≤500)`, `isThumbnail`, `food (@ManyToOne nullable)`, `workoutPlan (@ManyToOne nullable)`, `exercise (@ManyToOne nullable)` <br>&emsp; + Implement a rigorous DB check constraint `chk_image_exclusive`: mandating that exactly ONE of `food_id`, `workout_plan_id`, or `exercise_id` is populated (exclusive polymorphic foreign key) <br>&emsp; + Develop Flyway **V3** migration: scaffold the `image` table with the exclusive FK constraint | 02/24/2026 | 02/24/2026 | |
| 3   | - Develop the **ImageController** (`/api/images`) <br>&emsp; + `POST /` — securely register image records (URL persisted in DB; physical file resides in S3) <br>&emsp; + Expose specialized retrieval endpoints: `GET /{id}`, `GET /food/{foodId}`, `GET /workout-plan/{workoutPlanId}`, `GET /exercise/{exerciseId}` <br>&emsp; + Implement standard mutation endpoints: `PUT /{id}`, `DELETE /{id}` | 02/25/2026 | 02/25/2026 | |
| 3   | - Configure **AWS S3** integration (`config/AwsS3Config.java`) <br>&emsp; + AWS SDK v1: Instantiate the `AmazonS3` client bean, explicitly pinned to the `ap-southeast-1` region <br>&emsp; + Securely inject the target bucket name via the `S3_BUCKET_NAME` environment variable (default: `crawl.fitness`) <br>&emsp; + Develop comprehensive utility methods to handle robust media file uploads | 02/25/2026 | 02/25/2026 | <https://docs.aws.amazon.com/sdk-for-java/> |
| 4   | - Engineer the **mediaService** (Frontend) <br>&emsp; + `getImageUrl(owner, id)` — seamlessly fetch validated image URLs via `GET /api/images/{owner}/{id}` <br>&emsp; + Implement an advanced in-memory URL cache coupled with in-flight request deduplication (strictly eliminating redundant concurrent API calls for identical assets) <br>&emsp; + Build bulk retrieval helpers for optimized list rendering: `getFoodImageUrlMap`, `getWorkoutPlanImageUrlMap`, `getExerciseImageUrlMap` | 02/26/2026 | 02/26/2026 | |
| 5   | - Build the **SessionDetailScreen** (Frontend) <br>&emsp; + Render a comprehensive past workout recap: intelligently grouping executed sets by `exerciseId` <br>&emsp; + Visualize granular performance metrics: sets × reps × weight per exercise <br>&emsp; + Apply standardized date and time formatting leveraging the `utils/date.ts` utility module | 02/27/2026 | 02/27/2026 | |
| 6   | - Build the **SessionCalendarScreen** (Frontend) <br>&emsp; + Deploy a monthly calendar interface featuring visual dot markers to indicate days with successfully logged sessions <br>&emsp; + Integrate intuitive month-to-month pagination (prev/next chevrons) <br>&emsp; + Bind day-press events to dynamically render the corresponding granular session logs directly below the calendar <br>&emsp; + Fully localize temporal labels to standard Vietnamese (Thứ 2–CN, Tháng 1–12) | 02/28/2026 | 02/28/2026 | |

### Week 7 Achievements:

* **Backend & Cloud Architecture — Media module**:
  * Successfully executed the Flyway V3 migration, rigidly enforcing the exclusive polymorphic foreign key constraints at the database level to guarantee data integrity.
  * Configured the AWS S3 bucket with a maximum security posture (strictly private, explicitly blocking all public ACLs).
  * Successfully provisioned an **Amazon CloudFront** distribution leveraging **Origin Access Control (OAC)** to securely fetch and serve S3 media globally with ultra-low latency.
  * Refactored the `s3_images_upload.json` initialization script to map all default assets exclusively to CloudFront domains, drastically mitigating AWS Data Transfer Out (DTO) costs.
* **Frontend — Session History**:
  * The `SessionDetailScreen` accurately parses and groups workout telemetry, rendering complex performance metrics (sets/reps/weight) with high visual clarity.
  * The `SessionCalendarScreen` reliably tags active training days; tapping a marked date instantly unveils the associated session logs without noticeable delay.
  * Calendar pagination operates flawlessly, and the Vietnamese localization is perfectly applied across all temporal UI components.
* **Frontend — Media Service**:
  * The `mediaService` highly optimizes performance by caching retrieved image URLs in-memory — totally eradicating duplicated network requests for previously loaded assets.
  * In-flight request deduplication guarantees that concurrent UI components requesting the same image footprint resolve via a single, shared network Promise.
  * Complex UI elements (workout plan tiles, exercise catalogs) now load high-resolution S3 imagery rapidly via the CloudFront CDN, significantly elevating the perceived application speed.

### AWS Knowledge Learned:

* Mastered the mechanics of S3 Presigned URLs, empowering clients to perform secure, direct-to-bucket uploads and downloads without ever exposing long-lived AWS IAM credentials to the frontend.
* Grasped the strategic balance of defining short expiration windows for sensitive URLs—maximizing cryptographic security while preserving a seamless user experience.
* Internalized server-side upload validation best practices: strictly enforcing MIME types, imposing payload size limits, and sanitizing key paths to proactively mitigate storage abuse and malware vectors.
* Designed robust S3 Lifecycle Policies capable of automatically transitioning aging media to more cost-effective storage tiers (e.g., Standard-IA) or purging obsolete temporary files to optimize billing.
* Deepened understanding of controlled content distribution architectures via CloudFront to effectively thwart bandwidth theft (hotlinking) and unauthorized third-party media reuse.
* Analyzed the critical interplay between S3 Object Ownership and Bucket Policies, building the foundation necessary to swiftly diagnose and resolve complex Access Denied (403) errors.
* Synthesized client-side media caching strategies with macro-level cloud infrastructure goals: concurrently slashing cloud operational data transfer costs and accelerating mobile application load times.

In summary, Week 7 successfully translated theoretical S3 and CDN knowledge into a highly practical, secure, and cost-optimized media delivery model for the application.

### Next Week Plan:

* **Backend**: Architect and implement the comprehensive Food & Nutrition module — encompassing the `Food`, `Meal`, `MealFood`, and `DailyNutrition` entities, complete with a full CRUD API suite and an automated caloric/macro calculation engine.
* **Frontend**: Develop the `DietScreen` as the centralized nutritional hub, featuring intuitive daily meal management, a robust food search interface, and a seamless add-to-meal user flow.