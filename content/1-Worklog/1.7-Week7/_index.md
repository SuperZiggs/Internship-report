---
title: "Week 7 Worklog"
date: 2026-02-23
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* **Backend**: Build the Media module — `Image` entity with polymorphic association (food / exercise / workout plan), AWS S3 integration.
* **Frontend**: Build `SessionDetailScreen` (past workout recap) and `SessionCalendarScreen` (monthly calendar view), plus the `mediaService` with image caching.
* Enable images to be displayed for exercises and workout plans throughout the app.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Build **Image** entity (`module/media`) <br>&emsp; + Fields: `url (≤500)`, `isThumbnail`, `food (@ManyToOne nullable)`, `workoutPlan (@ManyToOne nullable)`, `exercise (@ManyToOne nullable)` <br>&emsp; + DB check constraint `chk_image_exclusive`: exactly ONE of `food_id`, `workout_plan_id`, `exercise_id` is non-null (exclusive polymorphic FK) <br>&emsp; + Write Flyway **V3** migration: `image` table with exclusive FK constraint | 02/24/2026 | 02/24/2026 | |
| 3   | - Build **ImageController** (`/api/images`) <br>&emsp; + `POST /` — register image record (URL stored; file lives in S3) <br>&emsp; + `GET /{id}`, `GET /food/{foodId}`, `GET /workout-plan/{workoutPlanId}`, `GET /exercise/{exerciseId}` <br>&emsp; + `PUT /{id}`, `DELETE /{id}` | 02/25/2026 | 02/25/2026 | |
| 3   | - Configure **AWS S3** integration (`config/AwsS3Config.java`) <br>&emsp; + AWS SDK v1: `AmazonS3` bean configured with region `ap-southeast-1` <br>&emsp; + Bucket name from env `S3_BUCKET_NAME` (default `crawl.fitness`) <br>&emsp; + Upload utility method for media files | 02/25/2026 | 02/25/2026 | <https://docs.aws.amazon.com/sdk-for-java/> |
| 4   | - Build **mediaService** (Frontend) <br>&emsp; + `getImageUrl(owner, id)` — fetch image URL from `GET /api/images/{owner}/{id}` <br>&emsp; + In-memory URL cache + in-flight deduplication (prevents duplicate API calls for the same image) <br>&emsp; + Bulk helpers: `getFoodImageUrlMap(ids[])`, `getWorkoutPlanImageUrlMap(ids[])`, `getExerciseImageUrlMap(ids[])` | 02/26/2026 | 02/26/2026 | |
| 5   | - Build **SessionDetailScreen** (Frontend) <br>&emsp; + Past workout recap: exercises grouped by `exerciseId` <br>&emsp; + Shows sets × reps × weight per exercise <br>&emsp; + Formatted date/time display with `utils/date.ts` | 02/27/2026 | 02/27/2026 | |
| 6   | - Build **SessionCalendarScreen** (Frontend) <br>&emsp; + Monthly calendar view with dot indicators on days that have sessions <br>&emsp; + Month navigation (prev/next chevrons) <br>&emsp; + Click a date to show session logs below the calendar <br>&emsp; + Vietnamese day/month labels (Thứ 2–CN, Tháng 1–12) | 02/28/2026 | 02/28/2026 | |

### Week 7 Achievements:

* **Backend — Media module**:
  * Flyway V3 migration applied with exclusive FK constraint on `image` table — DB-level data integrity for polymorphic images.
  * `POST /api/images` registers image URL records linked to food/exercise/plan.
  * `GET /api/images/exercise/{id}` and similar endpoints return all images for a given entity.
  * AWS S3 `AmazonS3` bean configured locally (uses environment variables; real uploads tested with `generate_s3_json.py` tooling).
* **Frontend — Session History**:
  * `SessionDetailScreen` correctly groups workout logs by exercise and renders sets/reps/weight clearly.
  * `SessionCalendarScreen` marks training days with dots; tap to reveal session details inline.
  * Month navigation works; Vietnamese labels render correctly.
* **Frontend — Media Service**:
  * `mediaService` caches fetched image URLs in memory — no duplicate API calls for images already loaded.
  * In-flight deduplication ensures concurrent requests for the same image share a single promise.
  * Workout plan tiles and exercise list items now display associated images from S3.

### AWS Knowledge Learned:

* Learned how S3 presigned URLs support secure client upload and download without exposing long-lived AWS credentials.
* Understood how to choose short expiration windows for sensitive URLs while still preserving acceptable UX.
* Studied upload validation concerns such as MIME type restrictions, size limits, and constrained key paths to reduce abuse.
* Learned to design lifecycle policies that move stale media to cheaper storage classes and remove obsolete temporary assets.
* Understood controlled content distribution patterns to prevent uncontrolled reuse or hotlinking of application media.
* Learned how object ownership and bucket policy rules can create subtle access bugs if not aligned correctly.
* Connected media caching decisions with both performance improvements and cloud cost reduction.

In summary, week 7 turned S3 knowledge into a practical and secure media delivery model for the app.

### Next Week Plan:

* **Backend**: Build the Food & Nutrition module — `Food`, `Meal`, `MealFood`, `DailyNutrition` entities with full CRUD and nutrition calculation.
* **Frontend**: Build `DietScreen` with meal management, food search, and add-food-to-meal flow.
