---
title: "Worklog Tuần 3"
date: 2026-01-19
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* **Backend**: Thiết lập tầng cơ sở hạ tầng chung (Common Infrastructure) — chuẩn hóa cơ chế quản lý ngoại lệ (Exception Handling), cấu hình CORS toàn cục và khởi tạo module `GoalType`.
* **Frontend**: Xây dựng bộ khung điều hướng (Navigation Architecture) toàn diện và triển khai luồng Onboarding đa bước (Multi-step Wizard) nhằm tối ưu trải nghiệm người dùng ban đầu.
* Đảm bảo tính liền mạch của luồng người dùng (User Flow) cốt lõi: từ Đăng nhập → Onboarding → Giao diện chính, tạo nền tảng vững chắc trước khi mở rộng các tính năng nghiệp vụ sâu hơn.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Triển khai **com.example.fitme.common.exception.GlobalExceptionHandler** (`@RestControllerAdvice`) <br>&emsp; + Bắt và xử lý `MethodArgumentNotValidException` → bóc tách lỗi validation chi tiết theo từng trường (field-level) <br>&emsp; + Bắt các lỗi `EntityNotFoundException` và các business exception tùy chỉnh <br>&emsp; + Đóng gói toàn bộ payload lỗi vào chuẩn `ApiResponse` envelope | 20/01/2026 | 20/01/2026 | |
| 2   | - Cấu hình **CorsConfig** toàn cục <br>&emsp; + Cấp quyền truy cập (allow origin) cho môi trường local dev và frontend production <br>&emsp; + Cấu hình Allow headers: `Authorization`, `Content-Type` <br>&emsp; + Cho phép toàn bộ các phương thức HTTP (GET, POST, PUT, DELETE, OPTIONS) | 20/01/2026 | 20/01/2026 | |
| 3   | - Xây dựng module **GoalType** <br>&emsp; + Thiết kế Entity: `name (UNIQUE)`, `description` <br>&emsp; + Khởi tạo các lớp Repository, Service và Controller <br>&emsp; + Cung cấp các Endpoints (**public**): `POST /api/goal-types`, `GET /api/goal-types`, `GET /api/goal-types/{id}`, `GET /api/goal-types/by-name/{name}` | 21/01/2026 | 21/01/2026 | |
| 4   | - Phát triển **RootNavigator** cho Frontend (thư mục `src/navigation/`) <br>&emsp; + Lắng nghe trạng thái `isAuthenticated` và `hasCompletedOnboarding` từ Redux `authSlice` <br>&emsp; + Tự động điều hướng linh hoạt giữa `AuthStack` / `OnboardingStack` / `MainTabs` <br> - Xây dựng **AuthStack**: Tích hợp `WelcomeScreen` và `LoginScreen` <br> - Xây dựng **MainTabs** tích hợp `CustomTabBar` <br>&emsp; + Phân bổ 4 tab chính: Trang chủ, Tập luyện, Dinh dưỡng, Sức khỏe <br>&emsp; + Thiết kế nút Action (Chat) nổi ở vị trí trung tâm, chia tỷ lệ tab đối xứng | 22/01/2026 | 22/01/2026 | <https://reactnavigation.org/> |
| 5   | - Triển khai **OnboardingStack** (Wizard 5 bước thu thập dữ liệu) <br>&emsp; + Bước 1: Giới tính, Ngày sinh, Chiều cao, Cân nặng <br>&emsp; + Bước 2: Tần suất hoạt động thể chất và Phân loại đặc thù công việc <br>&emsp; + Bước 3: Mục tiêu tập luyện, Cân nặng kỳ vọng, Vùng cơ thể ưu tiên cải thiện <br>&emsp; + Bước 4: Chế độ dinh dưỡng, Dị ứng thực phẩm, Số lượng bữa ăn/ngày, Lượng nước tiêu thụ <br>&emsp; + Bước 5: Kinh nghiệm tập luyện, Không gian tập, Tần suất và Thời lượng mỗi buổi | 23/01/2026 | 23/01/2026 | |
| 6   | - Tích hợp **CaloriesScreen** và **TrainingScreen** vào giao diện tổng kết onboarding <br> - Xử lý sự kiện hoàn tất onboarding: kích hoạt `dispatch(completeOnboarding())` và đồng bộ hóa hồ sơ người dùng qua API `POST /user/sync` | 24/01/2026 | 24/01/2026 | |

### Kết quả đạt được tuần 3:

* **Backend — Common Layer**:
  * `GlobalExceptionHandler` đã bao quát toàn bộ các ngoại lệ phát sinh; đảm bảo payload trả về client luôn tuân thủ cấu trúc `ApiResponse` đồng nhất.
  * Chính sách CORS được áp dụng toàn cục — cho phép frontend hoạt động tại `localhost:8081` giao tiếp mượt mà với API server tại `localhost:8080`.
* **Backend — Module GoalType**:
  * Thực thể `GoalType` được lưu trữ thành công vào cơ sở dữ liệu; hỗ trợ các phân loại cơ bản: **Giảm mỡ**, **Tăng cơ**, **Duy trì cân nặng**.
  * Triển khai thành công 4 public endpoints phục vụ các thao tác: tạo mới, truy xuất danh sách, tra cứu theo ID và tên mục tiêu.
  * Hoàn thiện module nền tảng, tạo cơ sở để ánh xạ (map) các kế hoạch tập luyện (workout plans) với mục tiêu cá nhân hóa của người dùng.
* **Frontend — Navigation**:
  * Hệ thống `RootNavigator` vận hành trơn tru, điều hướng chính xác tuyệt đối dựa trên luồng xác thực (auth) và trạng thái onboarding.
  * Giao diện `MainTabs` hiển thị `CustomTabBar` trực quan, nổi bật với nút Chat trung tâm (Floating Action Button).
  * Khởi tạo và đăng ký đầy đủ các stack điều hướng cốt lõi: `AuthStack`, `OnboardingStack`, `WorkoutStack`, `DietStack`, `HealthStack`.
* **Frontend — Onboarding**:
  * Quy trình Wizard 5 bước vận hành hoàn chỉnh với các hiệu ứng chuyển đổi (slide animation) mượt mà.
  * Hệ thống thu thập thành công và lưu trữ cấu trúc dữ liệu người dùng (nhân trắc học, mục tiêu, sở thích dinh dưỡng) một cách đầy đủ.
  * Luồng kết thúc onboarding kích hoạt chuẩn xác action `completeOnboarding()` và đồng bộ dữ liệu hai chiều với Backend thông qua API một cách hoàn hảo.

### Kiến thức AWS đã học:

* Nắm vững bản chất và tác động của CORS khi kiến trúc frontend và backend được phân tán trên các domain hoặc subdomain độc lập trong hệ sinh thái Cloud.
* Hiểu rõ cơ chế preflight request (`OPTIONS`) và phương pháp khắc phục các rủi ro chặn (block) truy cập do cấu hình thiếu HTTP method/header trên môi trường production.
* Rèn luyện tư duy phân hoạch bảo mật API: tách bạch rõ ràng các endpoint public, authenticated và admin-only, tạo tiền đề vững chắc cho việc tích hợp **Amazon API Gateway** trong tương lai.
* Nhận thức sâu sắc giá trị của việc chuẩn hóa response envelope trong công tác giám sát (monitoring), giúp tối ưu hóa việc truy vết log và phân tích lỗi trên **Amazon CloudWatch**.
* Áp dụng tư duy tương quan luồng yêu cầu (request correlation) thông qua việc gắn `requestId` hoặc `traceId`, hỗ trợ đắc lực cho công tác gỡ lỗi (debug) trong hệ thống phân tán.
* Hiểu rõ lợi ích của mô hình xử lý ngoại lệ tập trung (centralized exception handling): đồng nhất hóa các pattern lỗi, từ đó thiết lập các cảnh báo (alarms) tự động chính xác và hiệu quả hơn.
* Tái cấu trúc mã nguồn hướng tới khả năng quan sát (observability-driven design) bằng cách chuẩn hóa toàn diện định dạng log và payload phản hồi.

Nhìn chung, tuần 3 chú trọng việc ứng dụng các nguyên tắc kiến trúc Cloud và thiết kế API chuẩn mực để nâng cao tính ổn định cũng như khả năng giám sát của hệ thống về sau.

### Kế hoạch tuần tiếp theo:

* **Backend**: Khởi tạo và thiết kế phân hệ System Workout — bao gồm các entity `MuscleGroup`, `Exercise`, `WorkoutPlan`, `WorkoutPlanExercise` cùng với hệ thống API CRUD dành cho admin và các public read endpoints cho client.
* **Frontend**: Triển khai giao diện `SuggestedPlanScreen` (Wizard 3 bước đề xuất kế hoạch tập luyện cá nhân hóa) và component `PlanExercisePicker`.