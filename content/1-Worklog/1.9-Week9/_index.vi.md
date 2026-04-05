---
title: "Worklog Tuần 9"
date: 2026-03-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu Tuần 9:

* **Backend**: Khởi tạo và kiến trúc module User Metric — bao gồm thực thể `BodyMetric` để thu thập dữ liệu nhân trắc học và `HealthCalculation` tích hợp thuật toán xử lý các chỉ số sinh lý cốt lõi (BMI, BMR, TDEE).
* **Frontend**: Xây dựng hệ sinh thái giao diện quản lý sức khỏe toàn diện, bao gồm `HealthDashboardScreen` (tích hợp form nhập liệu và tính toán), `BodyMetricListScreen`, `BodyMetricFormScreen`, kết hợp cùng hệ thống biểu đồ (charts) trực quan hóa dữ liệu.
* Chuyển hóa các dữ liệu thô thành thông tin chi tiết, có thể hành động (actionable insights), giúp người dùng dễ dàng theo dõi thể trạng và thiết lập mục tiêu calo chuẩn xác.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Khởi tạo thực thể **BodyMetric** (bảng `body_metric`) <br>&emsp; + Định nghĩa schema: `user (@ManyToOne)`, `heightCm (Float, min=50)`, `weightKg (Float, min=20)`, `age (Integer, min=10)`, `gender (Gender enum)`, `activityLevel (ActivityLevel enum)` <br>&emsp; + Triển khai `BodyMetricController` (`/api/body-metrics`): cung cấp API tạo mới, truy xuất theo user (ưu tiên bản ghi mới nhất) và các thao tác get/update/delete theo ID | 03/09/2026 | 03/09/2026 | |
| 3   | - Thiết kế thực thể **HealthCalculation** (bảng `health_calculation`) <br>&emsp; + Thiết lập mapping: `user`, `bodyMetric (khóa ngoại snapshot tùy chọn)`, `bmi`, `bmr`, `tdee`, `goalType (GoalTypes enum)` <br>&emsp; + Áp dụng công thức chuẩn: <br>&emsp;&emsp; BMI = Cân nặng / (Chiều cao tính bằng m)² <br>&emsp;&emsp; BMR (Mifflin-St Jeor): Nam = 10×w + 6.25×h - 5×age + 5; Nữ = −5 <br>&emsp;&emsp; TDEE = BMR × hệ số vận động | 03/10/2026 | 03/10/2026 | |
| 3   | - Triển khai **HealthMetricsController** (`/api/metrics`) <br>&emsp; + `POST /calculate` — thực thi tính toán và lưu trữ BMI/BMR/TDEE từ payload `CalculateMetricsRequest` <br>&emsp; + Cung cấp các endpoint: `GET /user/{userId}` (truy xuất toàn bộ lịch sử), `GET /user/{userId}/latest`, `GET /{id}` | 03/10/2026 | 03/10/2026 | |
| 4   | - Phát triển **HealthDashboardScreen** trên Frontend <br>&emsp; + Tích hợp `WheelPicker` tối ưu UX nhập liệu chiều cao (cm) và cân nặng (kg) <br>&emsp; + Bố trí Dropdown lựa chọn `ActivityLevel` <br>&emsp; + Phân loại mục tiêu: Cutting / Bulking / Maintain / UP_Power <br>&emsp; + Xử lý luồng submit: gọi chuỗi API `createBodyMetric` + `calculateMetrics` → kết xuất `HealthResultCard` (chứa BMI, BMR, TDEE, Macros) <br>&emsp; + Tích hợp cơ chế fallback điều hướng về Profile nếu khuyết thông tin giới tính/ngày sinh | 03/11/2026 | 03/11/2026 | |
| 5   | - Thiết kế các **health chart components** (`src/components/health/`) <br>&emsp; + `BMITrendChart`: biểu đồ đường hỗ trợ lọc khoảng thời gian (7d/30d/90d/all) + thay đổi màu sắc linh hoạt theo phân vùng BMI <br>&emsp; + `WeightChart`: biểu đồ đường + tích hợp đường trung bình động (moving average) 7 ngày + tính toán khoảng cách đến cân nặng mục tiêu <br>&emsp; + `CalorieEnergyCharts`: biểu đồ đường kép BMR/TDEE hỗ trợ cơ chế chuyển đổi (toggle) <br>&emsp; + `MacrosDisplay`: 3 thanh tiến độ (protein/carbs/fat) đính kèm nhãn định lượng (gam) và % <br>&emsp; + `HealthResultCard`: thẻ tóm tắt tổng hợp toàn bộ các chỉ số sinh lý đã tính toán | 03/12/2026 | 03/12/2026 | |
| 6   | - Hoàn thiện giao diện **BodyMetricListScreen** (Frontend) <br>&emsp; + Liệt kê toàn bộ lịch sử đo lường với định dạng thời gian chuẩn hóa <br>&emsp; + Ghim `WeightChart` trực quan ở đầu danh sách <br>&emsp; + Tích hợp thao tác chỉnh sửa (điều hướng sang `BodyMetricFormScreen`) và xóa an toàn qua `ConfirmModal` <br> - Xây dựng **BodyMetricFormScreen** <br>&emsp; + Hỗ trợ luồng tạo mới/cập nhật: chiều cao, cân nặng, mức độ vận động, loại mục tiêu <br>&emsp; + Tự động trích xuất và đồng bộ giới tính + tuổi từ trạng thái user trong `authSlice` | 03/13/2026 | 03/13/2026 | |
| 7   | - **Tinh chỉnh Backend endpoint** (Phase 1) <br>&emsp; + Áp dụng triệt để annotation `@Valid` và custom constraint validator cho toàn bộ request DTO <br>&emsp; + Chuẩn hóa payload phân trang: bọc dữ liệu bằng `PageResponse<T>` kèm theo `page`, `size`, `totalPages`, `totalElements` <br>&emsp; + Bổ sung bộ lọc thời gian `GET /api/sessions/user/{userId}?startDate=&endDate=` cho tính năng truy xuất lịch sử tập luyện | 03/14/2026 | 03/14/2026 | |
| 8   | - Phủ **Unit test** toàn diện (Spring Boot Test + JUnit 5 + Mockito) <br>&emsp; + `UserWorkoutPlanServiceTest`: kiểm thử logic clone, cơ chế kích hoạt (activation), và khả năng phòng chống IDOR <br>&emsp; + `HealthCalculationServiceTest`: kiểm thử biên (edge cases) cho các công thức BMI/BMR/TDEE <br>&emsp; + `UserWorkoutSessionServiceTest`: xác thực các truy vấn active session và hành vi vô hiệu hóa (deactivation) | 03/15/2026 | 03/15/2026 | |

### Kết quả đạt được Tuần 9:

* **Backend — module User Metric**:
  * Thực thể `BodyMetric` được lưu trữ an toàn, vượt qua toàn bộ lớp validation chặt chẽ (đảm bảo chiều cao ≥ 50 cm, cân nặng ≥ 20 kg, tuổi ≥ 10).
  * Tích hợp thành công **AWS Secrets Manager** để truy xuất động thông tin cấu hình PostgreSQL và JWT secret tại runtime, được bảo mật tuyệt đối qua lớp mã hóa **AWS KMS**.
  * Engine tính toán BMR hoạt động chính xác theo chuẩn phương trình Mifflin-St Jeor; TDEE ánh xạ chuẩn xác hệ số hoạt động thể chất (từ 1.2 đến 1.9).
  * Endpoint `POST /api/metrics/calculate` phản hồi `HealthCalculationResponse` tiêu chuẩn, cung cấp BMI, BMR, TDEE cùng phân bổ macro gợi ý (gam protein/carbs/fat) theo từng mục tiêu cụ thể.
  * Các truy vấn lịch sử được tối ưu hóa, đảm bảo dữ liệu trả về luôn tuân thủ thứ tự `createdAt DESC`.
  * Hoàn tất quá trình chuẩn hóa API Phase 1: mọi DTO đều được kiểm soát bởi `@Valid`, áp dụng envelope `PageResponse<T>` cho phân trang và đưa vào hoạt động bộ lọc thời gian cho session.
  * Hoàn thành bộ Unit test với độ phủ cao: `UserWorkoutPlanServiceTest`, `HealthCalculationServiceTest`, `UserWorkoutSessionServiceTest` đều pass toàn bộ các kịch bản edge case thông qua phương pháp Mockito-based mocking.
* **Frontend — Health Dashboard**:
  * Component Wheel Picker trên `HealthDashboardScreen` mang lại trải nghiệm tương tác (UX) vô cùng mượt mà trong thao tác nhập liệu chiều cao/cân nặng.
  * Tối ưu hóa vòng đời render: `HealthResultCard` cập nhật tức thời khi nhận response, loại bỏ triệt để việc tái tính toán BMI/BMR dư thừa trên main thread.
  * Tích hợp thành công thư viện `react-native-gifted-charts`, kiến tạo hệ thống biểu đồ hiệu năng cao và có khả năng tùy biến sâu.
  * `BMITrendChart` biểu thị dữ liệu sinh động bằng cách chuyển đổi màu sắc theo ngưỡng: xanh dương (thiếu cân), xanh lá (bình thường), vàng (thừa cân), đỏ (béo phì).
  * `WeightChart` thông minh tự động kích hoạt đường trung bình động (moving average) khi người dùng tích lũy đủ 7 điểm dữ liệu.
  * `BodyMetricListScreen` vận hành trơn tru, hiển thị đầy đủ lịch sử đo lường kèm các tác vụ CRUD trực tiếp một cách liền mạch.

### Kiến thức AWS đã học:

* Thấm nhuần tư duy quản lý bí mật (secret management) trên Cloud: chuyển đổi từ việc lưu trữ plaintext dài hạn sang sử dụng **AWS Secrets Manager** hoặc **SSM Parameter Store**.
* Nắm bắt kiến trúc cốt lõi của **AWS KMS**, bao gồm cơ chế mã hóa dữ liệu tại chỗ (at-rest), ranh giới của key policy và mối liên hệ mật thiết giữa IAM với quyền thực thi khóa.
* Hiểu rõ quy trình xoay vòng bí mật (secret rotation) và các chiến lược tái nạp (reload) cấu hình ở cấp độ ứng dụng khi có sự thay đổi.
* Quán triệt nguyên tắc cô lập môi trường (environment isolation), đảm bảo credentials của dev, staging và production không bao giờ bị rò rỉ chéo.
* Khai thác sức mạnh của **AWS CloudTrail** để thiết lập cơ chế kiểm toán (audit), giám sát liên tục mọi hành vi thay đổi đối với cấu hình nhạy cảm và ranh giới quyền hạn.
* Áp dụng nguyên tắc giảm thiểu rủi ro bảo mật bằng cách loại bỏ thông tin xác thực dài hạn, ưu tiên cấp phát role-based access ngắn hạn (temporary credentials) ở mọi điểm chạm hệ thống.
* Gắn kết trực tiếp các tiêu chuẩn bảo mật hạ tầng này vào thực tiễn dự án, tạo nên lớp phòng thủ vững chắc để bảo vệ hồ sơ định danh và dữ liệu sức khỏe nhạy cảm của người dùng.

Tóm lại, tuần 9 đã chuyển dịch trọng tâm từ xây dựng tính năng sang việc thiết lập các biện pháp kiểm soát bảo mật AWS, bảo vệ toàn diện cả quyền truy cập hệ thống lẫn tài sản dữ liệu của ứng dụng.

### Kế hoạch tuần tiếp theo:

* **Backend**: Khởi động quy trình triển khai lên môi trường AWS (Dockerize → ECR → vận hành trên ECS Fargate phía sau ALB), thiết lập AWS WAF để áp dụng rate limiting và thực hiện rà soát toàn bộ cấu hình.
* **Frontend**: Phát triển `ChatScreen` tích hợp trợ lý ảo thông minh qua AWS Bedrock AI và hoàn thiện `ProfileScreen` phục vụ việc cập nhật hồ sơ cá nhân và quản lý tài khoản.