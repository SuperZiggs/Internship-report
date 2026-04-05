---
title: "Worklog Tuần 5"
date: 2026-02-02
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* **Backend**: Thiết kế và triển khai module `UserWorkoutPlan` — hỗ trợ quản lý kế hoạch tập luyện cá nhân hóa thông qua cơ chế nhân bản (clone) từ system template, áp dụng chiến lược xóa mềm (soft-delete), và đảm bảo logic duy nhất một kế hoạch được kích hoạt (active) tại một thời điểm.
* **Frontend**: Xây dựng tổ hợp giao diện quản lý kế hoạch — bao gồm `MyPlansScreen`, `CreatePlanScreen`, và `PlanEditScreen` được tích hợp tab navigation để tinh chỉnh lộ trình theo từng ngày.
* Trao quyền cho người dùng tự do cá nhân hóa và quản trị lịch trình tập luyện một cách linh hoạt, tối ưu.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Khởi tạo thực thể **UserWorkoutPlan** <br>&emsp; + Định nghĩa schema: `user (@ManyToOne)`, `name (≤150)`, `description`, `goalTypeId (UUID)`, `isActive`, `isDeleted` <br>&emsp; + Tích hợp cơ chế Soft-delete thông qua `@SQLRestriction("is_deleted = false")` <br>&emsp; + Khởi tạo script Flyway **V1**: thiết lập bảng `user_workout_plan` và `user_workout_plan_exercises` | 03/02/2026 | 03/02/2026 | |
| 2   | - Triển khai script Flyway **V2**: bổ sung trường `is_deleted BOOLEAN DEFAULT FALSE` nhằm chuẩn bị cho cơ chế xóa mềm | 03/02/2026 | 03/02/2026 | |
| 3   | - Kiến tạo thực thể **UserWorkoutPlanExercise** <br>&emsp; + Thiết lập mapping chi tiết: `dayOfWeek`, `sets`, `reps`, `restSeconds`, `dayIndex`, `weekIndex`, `orderIndex` <br>&emsp; + Khai báo các mối quan hệ (Reference) tới `Exercise` và `UserWorkoutPlan` | 04/02/2026 | 04/02/2026 | |
| 3   | - Cài đặt business logic tại **UserWorkoutPlanService** <br>&emsp; + Đảm bảo `userId` được trích xuất trực tiếp từ `Jwt.getSubject()` — đóng triệt để rủi ro IDOR <br>&emsp; + **Clone**: Thực thi sao chép sâu (deep copy) toàn bộ `WorkoutPlanExercise` — ngắt hoàn toàn khóa ngoại (FK) với kế hoạch gốc <br>&emsp; + **Activate**: Xử lý logic transaction chuyển đổi trạng thái `isActive=true` cho plan được chọn và `isActive=false` cho phần còn lại | 04/02/2026 | 04/02/2026 | |
| 4   | - Xây dựng **UserWorkoutPlanController** (`/api/user-workout-plans`) <br>&emsp; + API Khởi tạo: `POST /me` (tạo mới), `POST /me/clone/{systemPlanId}` (nhân bản) <br>&emsp; + API Truy xuất: `GET /me`, `GET /me/active`, `GET /{id}` <br>&emsp; + API Cập nhật/Xóa: `PUT /{id}`, `PUT /{id}/activate`, `DELETE /{id}` (áp dụng xóa mềm) <br>&emsp; + API CRUD thao tác chi tiết bài tập trong kế hoạch | 05/02/2026 | 05/02/2026 | |
| 5   | - Phát triển **MyPlansScreen** trên Frontend <br>&emsp; + Hiển thị danh sách kế hoạch trực quan kèm badge định dạng trạng thái active <br>&emsp; + Tích hợp tính năng toggle kích hoạt và thao tác xóa có cảnh báo qua `ConfirmModal` <br> - Phát triển **CreatePlanScreen** <br>&emsp; + Xây dựng form nhập liệu chuẩn mực: tên, mô tả, kèm dropdown lựa chọn goal type | 06/02/2026 | 06/02/2026 | |
| 6   | - Phát triển **PlanEditScreen** trên Frontend <br>&emsp; + Tích hợp Tab bar phân tách theo thứ (Thứ 2 - Chủ nhật) để rà soát lịch tập chi tiết <br>&emsp; + Hỗ trợ thao tác vuốt để sắp xếp (reorder) và xóa bài tập linh hoạt <br>&emsp; + Điều hướng mượt mà sang `PlanExercisePicker` để bổ sung bài tập mới | 07/02/2026 | 07/02/2026 | |

### Kết quả đạt được tuần 5:

* **Backend — module UserWorkoutPlan**:
  * Hệ thống script migration Flyway (V1 & V2) được áp dụng thành công, đảm bảo tính nhất quán của schema cơ sở dữ liệu.
  * Cơ chế xóa mềm (Soft-delete) thông qua `@SQLRestriction` vận hành chính xác — loại bỏ hoàn toàn rủi ro rò rỉ dữ liệu đã xóa trong các truy vấn.
  * Thuật toán Clone hoạt động xuất sắc, tạo ra các bản sao (deep copy) độc lập tuyệt đối từ các template hệ thống — không duy trì khóa ngoại (FK) trỏ ngược, bảo toàn quyền tự do tùy biến của người dùng mà không gây rủi ro toàn vẹn cho dữ liệu gốc.
  * Quy tắc kinh doanh (Business rule) "duy nhất một kế hoạch active" được kiểm soát chặt chẽ ở tầng Service thông qua database transaction.
  * Bảo mật tuyệt đối thông tin định danh: `userId` được trích xuất trực tiếp từ JWT payload, vá triệt để mọi lỗ hổng liên quan đến IDOR.
* **Frontend — Quản lý kế hoạch**:
  * Giao diện `MyPlansScreen` phản ánh đồng bộ, theo thời gian thực trạng thái kích hoạt của các kế hoạch.
  * `CreatePlanScreen` vận hành trơn tru với bộ rules validation nghiêm ngặt phía client trước khi dispatch API submit.
  * `PlanEditScreen` mang lại trải nghiệm người dùng (UX) tối ưu nhờ thiết kế phân trang theo ngày, kết hợp cùng Redux `uiSlice` để đồng bộ hóa trạng thái state một cách mượt mà.

### Kiến thức AWS đã học:

* Nắm bắt chuyên sâu các khái niệm cốt lõi khi vận hành PostgreSQL trên **Amazon RDS**: quản trị bản sao lưu (managed backups), thiết lập cửa sổ bảo trì (maintenance windows), điều chỉnh parameter groups và hiểu rõ các giới hạn dịch vụ (service quotas).
* Thực hành chiến lược cô lập cơ sở dữ liệu: triển khai database hoàn toàn trong Private Subnet và thắt chặt Security Group, chỉ cấp quyền truy cập nội bộ cho Backend.
* Nghiên cứu cơ chế snapshot và khả năng khôi phục dữ liệu theo thời điểm (Point-in-Time Recovery - PITR) nhằm đảm bảo an toàn tuyệt đối cho lộ trình tập luyện và dữ liệu cá nhân của người dùng.
* Thiết lập kỷ luật quản lý schema nghiêm ngặt: coi các script Flyway như một loại artifact deploy (deployment artifact), bắt buộc phải có tính tất định (deterministic) xuyên suốt các môi trường.
* Nhận thức rõ bản chất tài nguyên hữu hạn của các kết nối đến cloud database, từ đó thiết lập và tối ưu hóa cơ chế Connection Pooling phía ứng dụng.
* Tìm hiểu chiến lược mở rộng năng lực đọc (read scaling) thông qua Read Replicas, chuẩn bị sẵn sàng kiến trúc cho các truy vấn thống kê và báo cáo phức tạp trong tương lai.
* Tích hợp tư duy giữa chiến lược sao lưu/khôi phục dữ liệu trên Cloud với các yêu cầu về kiểm toán (audit), khả năng phục hồi do lỗi con người và mục tiêu tối thượng là củng cố niềm tin của người dùng.

Tóm lại, tuần 5 gắn thiết kế dữ liệu của dự án với các nguyên tắc vận hành database managed service trên AWS.

### Kế hoạch tuần tiếp theo:

* **Backend**: Khởi tạo và phát triển module `UserWorkoutSession` cùng `WorkoutLog` nhằm lưu trữ và theo dõi sát sao tiến trình của từng buổi tập thực tế.
* **Frontend**: Triển khai `PlanDetailScreen` (dashboard tổng quan cho kế hoạch đang active) và `WorkoutSessionScreen` (không gian tương tác trực tiếp trong buổi tập, được quản lý chặt chẽ bởi Redux session state).