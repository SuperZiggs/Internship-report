---
title: "Worklog Tuần 9"
date: 2026-03-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu Tuần 9:

* **Backend**: Xây dựng module User Metric — `BodyMetric` để ghi lại các chỉ số cơ thể và `HealthCalculation` để tính BMI, BMR và TDEE.
* **Frontend**: Xây dựng `HealthDashboardScreen` với nhập mục tiêu + tính toán, `BodyMetricListScreen`, `BodyMetricFormScreen`, và toàn bộ các chart component hiển thị sức khỏe.
* Cung cấp cho người dùng thông tin rõ ràng và có thể hành động về sức khỏe thể chất và mục tiêu calo.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Xây dựng entity **BodyMetric** (bảng `body_metric`) <br>&emsp; + Các trường: `user (@ManyToOne)`, `heightCm (Float, min=50)`, `weightKg (Float, min=20)`, `age (Integer, min=10)`, `gender (Gender enum)`, `activityLevel (ActivityLevel enum)` <br>&emsp; + `BodyMetricController` (`/api/body-metrics`): tạo mới, lấy theo user (mới nhất trước), lấy mới nhất, get/update/delete theo ID | 03/09/2026 | 03/09/2026 | |
| 3   | - Xây dựng entity **HealthCalculation** (bảng `health_calculation`) <br>&emsp; + Các trường: `user`, `bodyMetric (snapshot FK tùy chọn)`, `bmi`, `bmr`, `tdee`, `goalType (GoalTypes enum)` <br>&emsp; + Công thức tính toán: <br>&emsp;&emsp; BMI = cân nặng / (chiều cao tính bằng m)² <br>&emsp;&emsp; BMR (Mifflin-St Jeor): Nam = 10×w + 6.25×h - 5×age + 5; Nữ = −5 <br>&emsp;&emsp; TDEE = BMR × hệ số hoạt động | 03/10/2026 | 03/10/2026 | |
| 3   | - Xây dựng **HealthMetricsController** (`/api/metrics`) <br>&emsp; + `POST /calculate` — tính toán & lưu BMI/BMR/TDEE từ `CalculateMetricsRequest` <br>&emsp; + `GET /user/{userId}` (toàn bộ lịch sử), `GET /user/{userId}/latest`, `GET /{id}` | 03/10/2026 | 03/10/2026 | |
| 4   | - Xây dựng **HealthDashboardScreen** (Frontend) <br>&emsp; + `WheelPicker` chọn chiều cao (cm) và cân nặng (kg) <br>&emsp; + Dropdown `ActivityLevel` <br>&emsp; + Chọn loại mục tiêu: Cutting / Bulking / Maintain / UP_Power <br>&emsp; + Khi submit: `createBodyMetric` + `calculateMetrics` → hiển thị `HealthResultCard` (BMI, BMR, TDEE, Macros) <br>&emsp; + Chuyển hướng đến Profile nếu thiếu giới tính/ngày sinh | 03/11/2026 | 03/11/2026 | |
| 5   | - Xây dựng các **health chart components** (`src/components/health/`) <br>&emsp; + `BMITrendChart`: biểu đồ đường với bộ lọc khoảng thời gian (7d/30d/90d/all) + màu sắc theo vùng BMI <br>&emsp; + `WeightChart`: biểu đồ đường + moving average 7 ngày tùy chọn + khoảng cách đến cân nặng mục tiêu <br>&emsp; + `CalorieEnergyCharts`: BMR + TDEE dual-line với chế độ chuyển đổi <br>&emsp; + `MacrosDisplay`: 3 progress bar (protein/carbs/fat) với nhãn gam + % <br>&emsp; + `HealthResultCard`: thẻ tóm tắt tổng hợp tất cả các giá trị đã tính | 03/12/2026 | 03/12/2026 | |
| 6   | - Xây dựng **BodyMetricListScreen** (Frontend) <br>&emsp; + Liệt kê toàn bộ lịch sử chỉ số cơ thể với ngày tháng định dạng đẹp <br>&emsp; + `WeightChart` hiển thị ở đầu danh sách <br>&emsp; + Chỉnh sửa (điều hướng đến `BodyMetricFormScreen`) và xóa với `ConfirmModal` <br> - Xây dựng **BodyMetricFormScreen** <br>&emsp; + Tạo mới hoặc chỉnh sửa: chiều cao, cân nặng, activity level, goal type <br>&emsp; + Lấy giới tính + tuổi từ trạng thái user trong `authSlice` | 03/13/2026 | 03/13/2026 | |
| 7   | - **Tinh chỉnh Backend endpoint** giai đoạn 1 <br>&emsp; + Thêm annotation `@Valid` và custom constraint validator vào tất cả request DTO <br>&emsp; + Chuẩn hóa phản hồi phân trang: `PageResponse<T>` với `page`, `size`, `totalPages`, `totalElements` <br>&emsp; + Thêm bộ lọc khoảng thời gian `GET /api/sessions/user/{userId}?startDate=&endDate=` cho session history | 03/14/2026 | 03/14/2026 | |
| 8   | - Viết **unit test** (Spring Boot Test + JUnit 5 + Mockito) <br>&emsp; + `UserWorkoutPlanServiceTest`: clone method, activation logic, IDOR prevention <br>&emsp; + `HealthCalculationServiceTest`: công thức BMI/BMR/TDEE với các input edge case <br>&emsp; + `UserWorkoutSessionServiceTest`: active session queries, deactivation behavior | 03/15/2026 | 03/15/2026 | |

### Kết quả đạt được Tuần 9:

* **Backend — Module User Metric**:
  * `BodyMetric` được lưu thành công với validation đầy đủ (chiều cao ≥ 50 cm, cân nặng ≥ 20 kg, tuổi ≥ 10).
  * Tích hợp AWS Secrets Manager để tải an toàn credentials PostgreSQL và JWT secrets khi runtime, mã hóa qua AWS KMS.
  * BMR tính đúng theo phương trình Mifflin-St Jeor; TDEE áp dụng đúng hệ số hoạt động (1.2 – 1.9).
  * `POST /api/metrics/calculate` trả về `HealthCalculationResponse` với BMI, BMR, TDEE và macro gợi ý (gam protein/carbs/fat theo goal type).
  * Các endpoint lịch sử trả về bản ghi được sắp xếp theo `createdAt DESC`.
  * Tinh chỉnh endpoint giai đoạn 1 hoàn tất: tất cả DTO có ràng buộc `@Valid`, phân trang chuẩn hóa với `PageResponse<T>`, bộ lọc khoảng thời gian đã thêm vào session history.
  * Unit test viết xong và pass: `UserWorkoutPlanServiceTest`, `HealthCalculationServiceTest`, `UserWorkoutSessionServiceTest` đều bao phủ edge case với Mockito-based mocking.
* **Frontend — Health Dashboard**:
  * `HealthDashboardScreen` wheel picker mang lại UX mượt mà khi nhập chiều cao/cân nặng.
  * Kết quả hiển thị ngay trong `HealthResultCard` sau khi có phản hồi API, với logic tối ưu để tránh tính lại BMI/BMR thừa trên mỗi lần render.
  * Xây dựng bằng `react-native-gifted-charts`, đảm bảo hiệu năng cao và khả năng tùy chỉnh tốt.
  * `BMITrendChart` tô màu các điểm dữ liệu: thiếu cân (xanh dương), bình thường (xanh lá), thừa cân (vàng), béo phì (đỏ).
  * `WeightChart` hiển thị đường moving average khi người dùng có ≥ 7 điểm dữ liệu.
  * `BodyMetricListScreen` hiển thị toàn bộ lịch sử đo lường với chỉnh sửa/xóa trực tiếp.

### Kiến thức AWS đã học:

* Nắm phương pháp quản lý secret bằng AWS Secrets Manager hoặc SSM Parameter Store thay vì phân phối plaintext tồn tại lâu dài.
* Nghiên cứu các kiến thức cơ bản về AWS KMS bao gồm mã hóa at-rest, ranh giới key policy, và mối quan hệ giữa IAM với quyền sử dụng key.
* Hiểu quy trình xoay vòng secret và các cân nhắc cấp độ ứng dụng cần thiết khi một giá trị thay đổi.
* Củng cố thực hành tách biệt môi trường để credential của dev, staging và production không bị trộn lẫn nhầm lẫn.
* Nắm giá trị audit của CloudTrail trong việc theo dõi các thay đổi đối với cấu hình nhạy cảm và ranh giới quyền.
* Áp dụng nguyên tắc giảm thiểu credential tồn tại lâu dài bằng cách ưu tiên role-based access ngắn hạn ở bất cứ đâu có thể.
* Kết nối trực tiếp các biện pháp kiểm soát này với việc bảo vệ thông tin hồ sơ người dùng và dữ liệu sức khỏe mà ứng dụng xử lý.

Tóm lại, tuần 9 tập trung vào các biện pháp kiểm soát bảo mật AWS bảo vệ cả quyền truy cập hệ thống lẫn dữ liệu nhạy cảm của ứng dụng.

### Kế hoạch tuần tiếp theo:

* **Backend**: Triển khai lên AWS (Docker → ECR → ECS Fargate behind ALB), kích hoạt AWS WAF rate limiting và rà soát cấu hình.
* **Frontend**: Xây dựng `ChatScreen` tích hợp AWS Bedrock AI chat và `ProfileScreen` với chỉnh sửa hồ sơ người dùng và quản lý tài khoản.
