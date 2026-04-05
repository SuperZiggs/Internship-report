---
title: "Worklog Tuần 10"
date: 2026-03-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu Tuần 10:

* **Frontend**:
  * Xây dựng `ChatScreen` tích hợp **AWS Bedrock (Claude 3.5 Haiku)** với bộ nhớ ngữ cảnh dạng cửa sổ trượt, và `ProfileScreen` với quản lý tài khoản người dùng.
  * Triển khai **logic làm mới token** mạnh mẽ qua Axios interceptor queue để đảm bảo UX mượt mà khi phiên đăng nhập hết hạn.
  * Xây dựng `HomeScreen` làm bảng điều khiển trung tâm, giải quyết tất cả các lỗi tồn đọng và hoàn thiện UX tổng thể trong toàn bộ ứng dụng.
* **Backend**:
  * Chuẩn bị triển khai: Docker Compose health check, đẩy image lên **Amazon ECR**, định nghĩa task **ECS Fargate**, kích hoạt **AWS WAF**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | --------------- | ------------------ |
| 2   | - Tinh chỉnh Backend endpoint <br>&emsp; + Thêm annotation `@Valid` và custom constraint validator vào tất cả request DTO <br>&emsp; + Chuẩn hóa phản hồi phân trang: `PageResponse<T>` với `page`, `size`, `totalPages`, `totalElements` <br>&emsp; + Thêm bộ lọc khoảng ngày `GET /api/sessions/user/{userId}?startDate=&endDate=` cho session history | 03/16/2026 | 03/16/2026 | |
| 3   | - Viết **unit test** (Spring Boot Test + JUnit 5 + Mockito) <br>&emsp; + `UserWorkoutPlanServiceTest`: clone method, activate logic, IDOR prevention <br>&emsp; + `HealthCalculationServiceTest`: công thức BMI/BMR/TDEE cho nhiều input khác nhau <br>&emsp; + `UserWorkoutSessionServiceTest`: active session query, deactivate behavior | 03/17/2026 | 03/17/2026 | |
| 4   | - Xây dựng **chatService** (Frontend) <br>&emsp; + Gọi trực tiếp API AWS Bedrock Runtime (không qua Lambda proxy) <br>&emsp; + Model: `anthropic.claude-3-5-haiku-20241022-v1:0` <br>&emsp; + System prompt tiếng Việt: hóa thân thành huấn luyện viên thể hình <br>&emsp; + Gửi 12 lượt hội thoại gần nhất làm ngữ cảnh (bộ nhớ dạng cửa sổ trượt) | 03/18/2026 | 03/18/2026 | <https://docs.aws.amazon.com/bedrock/> |
| 5   | - Xây dựng **ChatScreen** (Frontend) <br>&emsp; + Giao diện chat toàn màn hình với danh sách bong bóng tin nhắn (người dùng / bot) <br>&emsp; + Hiệu ứng "đang gõ" (3 dấu chấm nảy) trong lúc chờ phản hồi <br>&emsp; + 4 chip tùy chọn nhanh: "Gợi ý bài tập", "Thực đơn hôm nay", "Mục tiêu calo", "Lời khuyên giảm cân" <br>&emsp; + Lời chào ban đầu bằng tiếng Việt từ bot thể hình <br>&emsp; + Hiệu ứng tránh bàn phím <br>&emsp; + Xử lý lỗi `notifyAlert` qua proxy thông báo toàn cục | 03/19/2026 | 03/19/2026 | |
| 6   | - Xây dựng **ProfileScreen** (Frontend) <br>&emsp; + Hiển thị avatar (chữ cái đầu tiên nếu không có ảnh), tên, email, username, ngày sinh, giới tính <br>&emsp; + Modal chỉnh sửa: cập nhật ngày sinh (YYYY-MM-DD) và giới tính qua API `updateUserProfile` + `dispatch(updateUserProfile)` <br>&emsp; + Đăng xuất: `signOut` (thu hồi Cognito) + `dispatch(logout)` + xóa secure storage <br>&emsp; + Xóa tài khoản: `deleteUserProfile` + `signOut` — cả hai đều được bảo vệ bởi `ConfirmModal` | 03/20/2026 | 03/20/2026 | |
| 7   | - Xây dựng **HomeScreen** (Frontend) — 5 luồng dữ liệu song song khi mount màn hình <br>&emsp; + Bữa ăn hôm nay: tổng calo MealFood → thanh tiến độ calo hàng ngày so với mục tiêu 2500 kcal <br>&emsp; + `HealthCalculation` mới nhất → hiển thị BMI <br>&emsp; + `BodyMetric` mới nhất → hiển thị chiều cao/cân nặng hiện tại <br>&emsp; + Tên kế hoạch tập luyện đang hoạt động → liên kết nhanh đến PlanDetail <br>&emsp; + Số phiên tập tuần này → tiến độ hàng tuần so với mục tiêu 4 phiên | 03/21/2026 | 03/21/2026 | |
| 7   | - Chuẩn bị triển khai Backend <br>&emsp; + Tinh chỉnh Docker Compose với `postgres` health check và `Spring Actuator` liveness/readiness probe <br>&emsp; + CI/CD: Đẩy image backend bất biến (immutable) lên **Amazon ECR** <br>&emsp; + Định nghĩa task **ECS Fargate** được ánh xạ tới **ALB** cho runtime serverless <br>&emsp; + Bảo vệ các public endpoint bằng **AWS WAF** | 03/21/2026 | 03/21/2026 | |
| 8   | - Phiên **sửa lỗi toàn diện** trên các màn hình Frontend chính <br>&emsp; + Sửa `WorkoutSessionScreen`: trường hợp ngoại lệ khi hoàn thành tất cả bài tập trước khi đồng hồ đếm ngược kết thúc <br>&emsp; + Sửa `DietScreen`: đảm bảo `ensureDailyMeals` không bị gọi trên mỗi lần render — chuyển vào `useEffect` với deps rỗng <br>&emsp; + Sửa `HealthDashboardScreen`: hiển thị trạng thái loading trong lúc gọi `calculateMetrics` | 03/22/2026 | 03/22/2026 | |


### Thành tựu Tuần 10:

* **Backend**:
  * Tất cả request DTO có ràng buộc `@Valid` và validation đã hoàn thành từ Tuần 9.
  * API được kiểm tra và sẵn sàng: `PageResponse<T>` chuẩn hóa tất cả paginated endpoint, bộ lọc khoảng ngày hoạt động, unit test toàn bộ đều pass.
  * Image production đẩy thành công lên **Amazon ECR**, task definition đã tạo cho **ECS Fargate** đứng sau **ALB**.
  * **AWS WAF** được cấu hình và bảo vệ các public endpoint khỏi các cuộc tấn công phổ biến.
  * Health check trên PostgreSQL và Spring Actuator đảm bảo container khởi động đáng tin cậy.
  * `.env.example` ghi chú đầy đủ hơn 10+ biến môi trường bắt buộc kèm mô tả.
* **Frontend**:
  * `chatService.sendChatToBedrock` gọi trực tiếp AWS Bedrock Runtime bằng thông tin xác thực từ `.env`.
  * Bộ nhớ dạng cửa sổ trượt gồm 12 lượt giúp duy trì ngữ cảnh hội thoại một cách tiết kiệm.
  * Các chip tùy chọn nhanh mang đến tương tác tức thì ngay lần đầu mà không cần gõ phím.
  * Hiệu ứng đang gõ tạo cảm giác trò chuyện tự nhiên; tính năng tránh bàn phím hoạt động tốt trên cả iOS và Android.
  * Dữ liệu hồ sơ được tải từ Redux `authSlice` — không cần thêm lệnh gọi API nào khi mount màn hình.
  * Modal chỉnh sửa cập nhật state cục bộ theo hướng lạc quan (optimistically) và xác nhận qua API.
  * Luồng đăng xuất và xóa tài khoản đều yêu cầu xác nhận — không có nguy cơ mất dữ liệu do vô tình.
  * 5 lệnh gọi API song song `Promise.all` hoàn thành dưới 1.5s trên localhost.
  * Các chỉ báo tiến độ calo hàng ngày + phiên tập hàng tuần giúp người dùng nắm bắt mục tiêu rõ ràng ngay từ cái nhìn đầu tiên.
  * Ảnh chụp nhanh BMI và các chỉ số cơ thể hiển thị ngay trên trang chủ mà không cần chuyển trang.
  * Lỗi trường hợp ngoại lệ khi hoàn thành phiên tập đã được giải quyết — ứng dụng không còn bị treo ở hiệp cuối cùng.
  * `ensureDailyMeals` chỉ được gọi một lần trong lần mount đầu tiên — loại bỏ 4 lệnh gọi API thừa ở mỗi lần render.
  * Hàng đợi token refresh ngăn chặn tình trạng nhấp nháy màn hình đăng xuất khi có nhiều yêu cầu đồng thời nhận mã 401.
  * `NotificationBox` thay thế cho native alert — phong cách thông báo trong ứng dụng nhất quán.
  * Tất cả các trạng thái loading và thông báo lỗi được chuẩn hóa trên toàn ứng dụng.
  * Nhãn tiếng Việt đầy đủ và nhất quán xuyên suốt giao diện người dùng.

### Kiến thức AWS đã học:

* **Backend**:
  * Nắm được toàn bộ luồng tạo artifact cho container: tối ưu hóa Docker build, gắn thẻ image bất biến (immutable) và publish image backend lên Amazon ECR.
  * Hiểu cách ECS Fargate mô hình hóa việc triển khai thông qua task definitions, services, health checks và số lượng task mong muốn.
  * Nghiên cứu về mạng container trên AWS, bao gồm private subnets, truy cập ra ngoài qua NAT và tích hợp ALB target group.
  * Học các mô hình triển khai an toàn như rolling updates và minimum healthy percent để giảm thiểu rủi ro khi release.
  * Thực hành tách biệt cấu hình runtime khỏi chính image thông qua các biến môi trường cấp độ task và secret injection.
  * Hiểu cách các cổng kiểm tra chất lượng (quality gates) của GitHub Actions nên bảo vệ các giai đoạn triển khai AWS bằng cách yêu cầu lint, test và build thành công trước tiên.
  * Kết nối khả năng quan sát khi release với các quyết định rollback để tính an toàn của quá trình triển khai được dựa trên hành vi runtime đo lường được.
  * Tóm lại, đã kết nối kiến thức nền tảng về container AWS trực tiếp với con đường triển khai thực tế cho backend.
* **Frontend**:
  * Tìm hiểu kiến trúc phân phối frontend sử dụng S3 static hosting kết hợp với CloudFront để caching toàn cầu và giảm độ trễ.
  * Nghiên cứu thiết lập HTTPS từ đầu đến cuối: cấp phát chứng chỉ ACM, quy trình xác thực và định tuyến DNS Route 53.
  * Hiểu mô hình Origin Access Control để S3 có thể giữ ở trạng thái private trong khi CloudFront phục vụ lưu lượng người dùng một cách an toàn.
  * Học về cache-key và chiến lược invalidation nhằm cân bằng giữa việc cung cấp UI mới nhất với hiệu suất CDN mạnh mẽ.
  * Nghiên cứu AWS WAF như một lớp bảo vệ web cơ bản chống lại các cuộc tấn công phổ biến và các mẫu lưu lượng rác.
  * Nắm bắt cách xử lý lỗi ở biên (edge) và chiến lược dự phòng cần thiết để hỗ trợ định tuyến SPA mà không làm hỏng các deep link.
  * Thực hành tư duy phát hành frontend với cache busting, kiểm soát lan truyền và phiên bản hóa tài nguyên thân thiện với rollback.
  * Tóm lại, đã tập trung vào lớp phân phối AWS để giúp frontend sẵn sàng cho môi trường production và có thể truy cập toàn cầu.

### Kế hoạch tuần tiếp theo:

* Chạy đầy đủ **kiểm thử tích hợp end-to-end** toàn ứng dụng (tất cả tính năng, tất cả màn hình) — 6 luồng người dùng.
* Viết **tài liệu dự án đầy đủ** (Backend README, Frontend guide, tổng quan kiến trúc).
* **Dọn dẹp code**: xóa debug statement, fix linting, xác nhận CI/CD build pass.
* Tinh chỉnh Response interceptor + UX polish + final bug fix (xử lý trạng thái trống của BMITrendChart).
* Đánh giá retrospective dự án và chuẩn bị bàn giao.
