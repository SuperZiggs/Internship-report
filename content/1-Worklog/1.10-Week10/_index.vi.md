---
title: "Worklog Tuần 10"
date: 2026-03-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu Tuần 10:

* **Frontend**:
  * Phát triển `ChatScreen` tích hợp trợ lý ảo **AWS Bedrock (Claude 3.5 Haiku)** với cơ chế bộ nhớ ngữ cảnh dạng cửa sổ trượt (sliding window context). Hoàn thiện `ProfileScreen` phục vụ công tác quản lý tài khoản định danh.
  * Triển khai **cơ chế làm mới token (token refresh logic)** mạnh mẽ thông qua Axios interceptor queue, đảm bảo trải nghiệm người dùng (UX) liền mạch và không bị gián đoạn khi phiên đăng nhập hết hạn.
  * Xây dựng `HomeScreen` đóng vai trò là dashboard trung tâm, xử lý triệt để các lỗi tồn đọng và đánh bóng (polish) trải nghiệm UX toàn cục trên ứng dụng.
* **Backend**:
  * Chuẩn bị hạ tầng triển khai production: thiết lập Docker Compose health check, đẩy (push) image lên **Amazon ECR**, cấu hình task definition cho **ECS Fargate**, và kích hoạt lớp khiên bảo vệ **AWS WAF**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | --------------- | ------------------ |
| 2   | - Tối ưu hóa các Backend endpoints <br>&emsp; + Áp dụng triệt để annotation `@Valid` và custom constraint validator cho mọi request DTO <br>&emsp; + Chuẩn hóa cấu trúc phản hồi phân trang bằng envelope `PageResponse<T>` kèm theo các metadata: `page`, `size`, `totalPages`, `totalElements` <br>&emsp; + Tích hợp bộ lọc khoảng thời gian `GET /api/sessions/user/{userId}?startDate=&endDate=` phục vụ tra cứu lịch sử phiên tập | 16/03/2026 | 16/03/2026 | |
| 3   | - Phát triển bộ **Unit test** toàn diện (Spring Boot Test + JUnit 5 + Mockito) <br>&emsp; + `UserWorkoutPlanServiceTest`: kiểm thử logic clone, cơ chế kích hoạt (activate) và khả năng ngăn chặn lỗ hổng IDOR <br>&emsp; + `HealthCalculationServiceTest`: kiểm thử công thức BMI/BMR/TDEE với đa dạng dữ liệu đầu vào (edge cases) <br>&emsp; + `UserWorkoutSessionServiceTest`: xác thực các truy vấn active session và hành vi vô hiệu hóa (deactivate) | 17/03/2026 | 17/03/2026 | |
| 4   | - Kiến tạo **chatService** (Frontend) <br>&emsp; + Giao tiếp trực tiếp với AWS Bedrock Runtime API (không thông qua Lambda proxy) <br>&emsp; + Tích hợp model `anthropic.claude-3-5-haiku-20241022-v1:0` <br>&emsp; + Thiết kế system prompt tiếng Việt: định hình persona huấn luyện viên thể hình chuyên nghiệp <br>&emsp; + Truyền tải 12 lượt hội thoại gần nhất làm ngữ cảnh (áp dụng thuật toán bộ nhớ cửa sổ trượt) | 18/03/2026 | 18/03/2026 | <https://docs.aws.amazon.com/bedrock/> |
| 5   | - Xây dựng giao diện **ChatScreen** (Frontend) <br>&emsp; + Thiết kế màn hình chat toàn phần (full-screen) với UI bong bóng tin nhắn trực quan (user/bot) <br>&emsp; + Tích hợp hiệu ứng typing indicator (3 dấu chấm nảy) trong thời gian chờ inference từ AI <br>&emsp; + Bố trí 4 action chips truy cập nhanh: "Gợi ý bài tập", "Thực đơn hôm nay", "Mục tiêu calo", "Lời khuyên giảm cân" <br>&emsp; + Khởi tạo lời chào mặc định bằng tiếng Việt từ AI coach <br>&emsp; + Tối ưu hóa UX với cơ chế tránh bàn phím (Keyboard Avoiding) <br>&emsp; + Bắt và xử lý lỗi qua hệ thống `notifyAlert` proxy tập trung | 19/03/2026 | 19/03/2026 | |
| 6   | - Phát triển **ProfileScreen** (Frontend) <br>&emsp; + Hiển thị thông tin cá nhân: avatar (fallback chữ cái đầu), tên, email, username, ngày sinh, giới tính <br>&emsp; + Modal chỉnh sửa: cập nhật ngày sinh (YYYY-MM-DD) và giới tính thông qua API `updateUserProfile` kết hợp `dispatch(updateUserProfile)` <br>&emsp; + Luồng đăng xuất: thực thi `signOut` (thu hồi session Cognito) + `dispatch(logout)` + xóa định dạng secure storage <br>&emsp; + Luồng xóa tài khoản: `deleteUserProfile` + `signOut` — bảo vệ nghiêm ngặt qua `ConfirmModal` | 20/03/2026 | 20/03/2026 | |
| 7   | - Triển khai **HomeScreen** (Frontend) — đồng bộ hóa 5 luồng fetch dữ liệu song song khi mount <br>&emsp; + Dinh dưỡng: tổng hợp calo từ `MealFood` → kết xuất thanh tiến độ so với mốc 2500 kcal <br>&emsp; + Sức khỏe: trích xuất `HealthCalculation` mới nhất → hiển thị chỉ số BMI <br>&emsp; + Thể trạng: trích xuất `BodyMetric` mới nhất → phản ánh chiều cao/cân nặng hiện tại <br>&emsp; + Kế hoạch: hiển thị tên Workout Plan đang active → điều hướng nhanh tới `PlanDetail` <br>&emsp; + Tiến độ tuần: thống kê số phiên tập/tuần → đối chiếu với mục tiêu 4 phiên | 21/03/2026 | 21/03/2026 | |
| 7   | - Chuẩn bị môi trường triển khai **Backend** <br>&emsp; + Tinh chỉnh cấu hình Docker Compose tích hợp `postgres` health check và `Spring Actuator` liveness/readiness probes <br>&emsp; + CI/CD pipeline: Build và push immutable image lên **Amazon ECR** <br>&emsp; + Thiết lập task definition cho **ECS Fargate** gắn kết với **ALB** phục vụ kiến trúc serverless runtime <br>&emsp; + Triển khai **AWS WAF** bảo vệ các public endpoints | 21/03/2026 | 21/03/2026 | |
| 8   | - Chiến dịch **sửa lỗi toàn diện** và tối ưu UX trên các màn hình Frontend cốt lõi <br>&emsp; + `WorkoutSessionScreen`: xử lý triệt để edge case khi user hoàn tất toàn bộ bài tập trước khi rest timer kết thúc <br>&emsp; + `DietScreen`: cấu trúc lại logic `ensureDailyMeals` vào `useEffect` với empty deps, ngăn chặn re-render thừa <br>&emsp; + `HealthDashboardScreen`: bổ sung trạng thái loading skeleton trong quá trình chờ API `calculateMetrics` | 22/03/2026 | 22/03/2026 | |


### Thành tựu Tuần 10:

* **Backend**:
  * Toàn bộ request DTO đã được đóng gói constraint `@Valid` và hoàn tất quy trình validation chặt chẽ kế thừa từ Tuần 9.
  * Hệ thống API đạt trạng thái production-ready: chuẩn hóa `PageResponse<T>` cho mọi paginated endpoints, bộ lọc thời gian hoạt động trơn tru, và pass 100% test cases.
  * Image production được đẩy thành công lên **Amazon ECR**; khởi tạo hoàn tất task definition cho **ECS Fargate** vận hành phía sau **ALB**.
  * **AWS WAF** được thiết lập, tạo lớp khiên chắn vững chắc cho các public endpoints trước các rủi ro tấn công web phổ biến.
  * Tích hợp Health check cho PostgreSQL và Spring Actuator, đảm bảo các container khởi chạy và duy trì trạng thái ổn định (resilient).
  * Tài liệu hóa `.env.example` với hơn 10 biến môi trường thiết yếu kèm mô tả kỹ thuật chi tiết.
* **Frontend**:
  * Module `chatService.sendChatToBedrock` thiết lập giao tiếp trực tiếp với AWS Bedrock Runtime, nội suy credential an toàn từ file `.env`.
  * Kiến trúc bộ nhớ cửa sổ trượt (12 lượt hội thoại) duy trì xuất sắc ngữ cảnh (context) AI đồng thời tối ưu hóa chi phí token.
  * Hệ thống action chips cung cấp trải nghiệm tương tác tức thì không yêu cầu thao tác gõ phím từ người dùng.
  * Hiệu ứng typing indicator mang lại cảm giác hội thoại tự nhiên; cơ chế keyboard avoiding hoạt động mượt mà trên cả hệ sinh thái iOS và Android.
  * Dữ liệu profile được hydrate trực tiếp từ Redux `authSlice` — đạt mức zero-API-call khi mount màn hình.
  * Modal chỉnh sửa ứng dụng triết lý optimistic UI update (cập nhật state cục bộ trước, xác nhận API sau), tối đa hóa độ phản hồi của giao diện.
  * Các tác vụ nhạy cảm (đăng xuất, xóa tài khoản) được đưa vào luồng xác nhận đa lớp — vô hiệu hóa hoàn toàn rủi ro mất dữ liệu do thao tác nhầm.
  * Kiến trúc gọi API song song qua `Promise.all` tại HomeScreen đạt hiệu năng ấn tượng, hoàn tất dưới 1.5s trên môi trường local.
  * Dashboard trực quan hóa dữ liệu (calo hàng ngày, tiến độ tập luyện, BMI) cung cấp bức tranh sức khỏe toàn cảnh ngay từ màn hình chính.
  * Khắc phục triệt để lỗi treo ứng dụng (crash) ở hiệp tập cuối cùng trong `WorkoutSessionScreen`.
  * Khử hoàn toàn 4 API calls dư thừa mỗi lần re-render trên `DietScreen` bằng cách kiểm soát chặt chẽ vòng đời của `ensureDailyMeals`.
  * Cơ chế Axios interceptor queue xử lý hoàn hảo tình trạng race condition khi nhiều request đồng loạt nhận mã 401, ngăn chặn lỗi nhấp nháy màn hình đăng xuất.
  * Quy hoạch toàn bộ thông báo sang component `NotificationBox` nội bộ, đồng nhất ngôn ngữ thiết kế ứng dụng.
  * Chuẩn hóa toàn diện các trạng thái loading và error handling xuyên suốt hệ thống.
  * Hoàn thiện khâu bản địa hóa (localization) với bộ nhãn tiếng Việt chuẩn mực, chuyên nghiệp.

### Kiến thức AWS đã học:

* **Backend**:
  * Làm chủ quy trình đóng gói artifact cho container: tối ưu hóa Docker build, gắn tag bất biến (immutable tags) và publish image lên **Amazon ECR**.
  * Hiểu sâu cách **ECS Fargate** trừu tượng hóa (abstract) hạ tầng thông qua task definitions, services, health checks và cơ chế auto-scaling dựa trên desired count.
  * Kiến trúc mạng lưới container trên AWS: thiết lập private subnets, định tuyến egress traffic qua NAT Gateway và cấu hình ALB target group tích hợp.
  * Lên chiến lược triển khai không gián đoạn (zero-downtime) thông qua rolling updates và điều chỉnh tham số minimum healthy percent.
  * Thực hành nguyên lý tách bạch hoàn toàn cấu hình runtime khỏi container image thông qua task-level environment variables và secret injection.
  * Thiết lập rào chắn chất lượng (quality gates) trên CI/CD GitHub Actions: bắt buộc pass toàn bộ khâu linting, testing và building trước khi trigger luồng deploy AWS.
  * Liên kết tư duy giữa khả năng quan sát (observability) lúc release và quyết định rollback, đảm bảo an toàn triển khai dựa trên các hành vi runtime thực tế.
  * Tóm lại, đã chuyển hóa thành công lý thuyết kiến trúc container AWS thành một pipeline triển khai production-grade cho Backend.
* **Frontend**:
  * Lĩnh hội kiến trúc phân phối SPA tĩnh: kết hợp **Amazon S3** static hosting cùng **Amazon CloudFront** để thiết lập caching toàn cầu và tối thiểu hóa độ trễ tải trang.
  * Nắm vững quy trình thiết lập TLS/HTTPS end-to-end: từ việc cấp phát chứng chỉ qua **ACM**, xác thực domain, đến cấu hình định tuyến DNS trên **Route 53**.
  * Triển khai mô hình **Origin Access Control (OAC)**, bảo vệ S3 bucket ở trạng thái private tuyệt đối trong khi vẫn cho phép CloudFront phân phối nội dung an toàn tới người dùng.
  * Thiết kế chiến lược cache-key và invalidation thông minh, giải quyết bài toán trade-off giữa việc cập nhật UI theo thời gian thực và việc tận dụng sức mạnh caching của CDN.
  * Triển khai **AWS WAF** tại tầng edge như một lớp giáp phòng ngự chủ động trước các bot rác và các vector tấn công web phổ biến.
  * Xử lý bài toán client-side routing (SPA) đặc thù bằng cách cấu hình custom error responses trên CloudFront, đảm bảo các deep links luôn hoạt động ổn định.
  * Hình thành tư duy release frontend hiện đại: áp dụng kỹ thuật cache busting, versioning tài nguyên và đảm bảo khả năng rollback tức thì.
  * Tóm lại, đã hoàn thiện kiến trúc phân phối mạng lưới (delivery layer) trên AWS, đưa Frontend vươn lên chuẩn mực của một ứng dụng toàn cầu.

### Kế hoạch tuần tiếp theo:

* Thực thi chiến dịch **Kiểm thử tích hợp toàn trình (End-to-End Testing)** bao phủ mọi màn hình và tính năng — đảm bảo 6 luồng người dùng cốt lõi hoạt động trơn tru.
* Biên soạn **tài liệu dự án chuyên sâu** (bao gồm Backend README, Frontend Development Guide, và Sơ đồ kiến trúc tổng quan).
* **Dọn dẹp Technical Debt (Code Cleanup)**: thanh lọc console/debug statements, khắc phục các cảnh báo linting, và chốt chặn trạng thái CI/CD build pass.
* Thực hiện các bước tinh chỉnh cuối cùng: tối ưu Response Interceptor, đánh bóng UX (UX polish) và sửa các bug còn sót lại (ví dụ: xử lý empty state của `BMITrendChart`).
* Tổ chức buổi họp Retrospective đánh giá toàn dự án và tiến hành các thủ tục bàn giao.