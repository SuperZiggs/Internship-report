---
title: "Week 8 Worklog"
date: 2026-03-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* **Backend**: Architect and implement the comprehensive Food & Nutrition module (`Food`, `Meal`, `MealFood`), simultaneously preparing the underlying data layer to serve as a dynamic context engine for the forthcoming **Amazon Bedrock AI Coach** integration.
* **Frontend**: Construct the `DietScreen` leveraging the robust `ensureDailyMeals` logic to automate and enforce a consistent 4-meal daily layout, alongside the development of the `DietHistoryScreen`.
* Empower users to meticulously track daily macronutrient intake while initiating advanced AI Prompt Engineering experiments tailored for fitness and nutritional analysis contexts.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Construct the **Food** entity (mapping the `food` table) <br>&emsp; + Define schema fields: `name`, `caloriesPer100g`, `proteinPer100g`, `carbsPer100g`, `fatsPer100g`, `unit` <br>&emsp; + Implement `FoodController` (`/api/foods`): supporting full CRUD operations, pagination (`GET /`), and dynamic keyword search (`GET /search?keyword=`) | 03/03/2026 | 03/03/2026 | |
| 3   | - Architect the **Meal** entity (mapping the `meal` table) <br>&emsp; + Define schema: `userProfile (@ManyToOne)`, `date (LocalDateTime)`, `mealType (enum: BREAKFAST/LUNCH/SNACK/DINNER)`, `note` <br>&emsp; + Develop `MealController` (`/api/meals`): facilitating creation, ID-based retrieval, paginated listing, and flexible filtering by date or meal type | 03/04/2026 | 03/04/2026 | |
| 4   | - Deploy the **MealFood** intermediate entity (table `meal_food`) — acting as a join table with computed macros <br>&emsp; + Map fields: `meal (@ManyToOne)`, `food (@ManyToOne)`, `quantity (float grams)` <br>&emsp; + Engineer auto-calculation logic for `calories`, `protein`, `carbs`, and `fats` triggered upon insertion (formula: Food’s per-100g values × (quantity/100)) <br>&emsp; + Develop `MealFoodController` (`/api/meal-foods`): handling workflows to add, list, and remove foods from meals | 03/05/2026 | 03/05/2026 | |
| 4   | - Establish the **DailyNutrition** entity (table `daily_nutrition`) <br>&emsp; + Schema mapping: `nutritionDate (LocalDate)`, `totalCalories`, `totalProtein`, `totalCarbs`, `totalFats` <br>&emsp; + Implement `DailyNutritionController` (`/api/daily-nutrition`): `POST /calculate?date=` to dynamically recompute and persist daily aggregates; `GET /?date=` for data retrieval | 03/05/2026 | 03/05/2026 | |
| 5   | - Construct the **DietScreen** interface (Frontend) <br>&emsp; + Render today's 4 standard meals (Breakfast/Lunch/Snack/Dinner) enforced via the `ensureDailyMeals` mechanism <br>&emsp; + Meal detailing: display food item catalogs, localized calorie counts, and progress bars against targets <br>&emsp; + "Add food" Modal: integrate dynamic search (`searchFoods`), gram-based quantity inputs, and dispatch `addFoodToMeal` submissions <br>&emsp; + Implement a master daily calorie progress bar at the header for optimized UX <br>&emsp; + Integrate Pull-to-refresh capabilities | 03/06/2026 | 03/06/2026 | |
| 6   | - Develop the **DietHistoryScreen** interface (Frontend) <br>&emsp; + Deploy a monthly calendar UI — enabling users to tap specific dates to unveil granular daily meal breakdowns <br>&emsp; + Render per-meal food itemizations alongside their respective calorie aggregates <br> - Integrate frontend `foodService`: connecting `getMealsByUser`, `getMealFoodsByMealId`, `addFoodToMeal`, and `deleteMealFood` into the `DietScreen` ecosystem <br> - Execute end-to-end testing of the full nutrition tracking loop: food addition → nutrient breakdown visualization → daily aggregate tracking | 03/07/2026 | 03/07/2026 | |

### Week 8 Achievements:

* **Backend — Food & Nutrition module**:
  * The `Food` entity was successfully seeded with authentic real-world data via the `fitness_crawler` tool (populating over 100 standardized food items into the local database).
  * `MealFood` seamlessly and accurately auto-calculates macronutrients (`calories`, `protein`, `carbs`, `fats`) upon insertion using the predefined `quantity / 100 * per100gValue` formula.
  * The `POST /api/daily-nutrition/calculate?date=` endpoint flawlessly aggregates all isolated `MealFood` entries for a specific date into a unified `DailyNutrition` record.
  * Pagination implementation on `GET /api/foods` operates perfectly, leveraging the `PageResponse<T>` wrapper to neatly handle `page`, `size`, `totalPages`, and `totalElements` metadata.
  * Keyword searching via `GET /api/foods/search?keyword=` delivers rapid, case-insensitive LIKE query resolutions.
* **Frontend — Diet tracking**:
  * The `DietScreen` strictly adheres to the `ensureDailyMeals` logic, guaranteeing the existence of exactly 4 designated meal slots for the current day.
  * The food search modal integrates paginated results with smooth lazy loading performance.
  * Food addition workflow operates with zero latency: as quantity is inputted, the backend processes macro calculations, and the UI instantly refreshes to reflect updated totals.
  * The `DietHistoryScreen` calendar accurately fetches and renders historical meal data correlated to user-selected dates.
  * The master daily calorie progress bar dynamically and precisely reflects progress against the standard `totalCalories / 2500` target.

### AWS Knowledge Learned:

* Gained profound insights into the **Amazon Bedrock Runtime** invocation lifecycle: from constructing precise request payloads and selecting optimal model IDs to parsing responses securely and stably.
* Analyzed and weighed the critical tradeoffs of AI model selection across three primary axes: response latency, output quality, and token cost-efficiency within the specific domain of a fitness coach.
* Elevated Prompt Engineering methodologies by establishing strict architectural constraints: explicitly defining role instructions, enforcing scope boundaries, and dictating expected output structures to guarantee predictable AI behavior.
* Cultivated a defensive programming mindset for AI integration: maintaining rigorous context-window control, identifying fundamental prompt-injection vulnerabilities, and engineering safe fallback mechanisms for when the model falters.
* Implemented standard resiliency patterns such as retry logic, exponential backoff, and strict timeout budgeting to ensure the chat UX remains fluid and responsive despite transient cloud service disruptions.
* Recognized the imperative of stringent quota and AI usage governance — treating it as a core operational responsibility rather than merely a billing afterthought.
* Identified and established crucial operational metrics tailored for AI features, including response success rates, invocation latency, and categorized failure tracking.

In summary, Week 8 successfully expanded AWS architectural learning from core infrastructure into the integration and production constraints of Managed AI Services.

### Next Week Plan:

* **Backend**: Architect and initialize the User Metric module — focusing on the `BodyMetric` entity and developing the `HealthCalculation` engine to process core physiological metrics including BMI, BMR, and TDEE.
* **Frontend**: Deploy the health management UI ecosystem, encompassing the `HealthDashboardScreen` (featuring intuitive wheel pickers), `BodyMetricListScreen`, `BodyMetricFormScreen`, and the integration of comprehensive health data visualization charts.