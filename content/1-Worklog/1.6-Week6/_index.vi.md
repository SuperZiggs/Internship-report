---
title: "Worklog Tuần 6"
date: 2026-02-09
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* **Backend**: Khởi tạo và phát triển module `UserWorkoutSession` cùng `WorkoutLog` nhằm theo dõi và lưu trữ sát sao dữ liệu (telemetry) của quá trình tập luyện thực tế.
* **Frontend**: Thiết kế và triển khai giao diện `PlanDetailScreen` (dashboard tổng quan) và `WorkoutSessionScreen` — tối ưu hóa trải nghiệm tương tác trực tiếp trong từng buổi tập (live-workout).
* Hoàn thiện toàn diện vòng đời cốt lõi của người dùng (core user loop): từ khởi tạo kế hoạch → tiến hành buổi tập → ghi nhận dữ liệu từng hiệp (log set) → kết thúc và thống kê.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Kiến tạo thực thể **UserWorkoutSession** <br>&emsp; + Định nghĩa schema: `user`, `userWorkoutPlan (@ManyToOne)`, `workoutDate (LocalDate)`, `isActive`, `weekIndex`, `dayIndex` <br>&emsp; + Thiết lập cơ chế Cascade cho `WorkoutLog` thông qua `@OneToMany(cascade = ALL)` để quản lý vòng đời dữ liệu đồng bộ | 10/02/2026 | 10/02/2026 | |
| 3   | - Phát triển thực thể **WorkoutLog** <br>&emsp; + Schema chi tiết: `userWorkoutSession`, `exercise`, `setNumber`, `reps`, `weight (Float)`, `durationSeconds` <br> - Triển khai **UserWorkoutSessionController** (`/api/sessions`) <br>&emsp; + Xây dựng các API: Khởi tạo (Create), Truy xuất theo ID/user/date, Lấy session đang active, Hủy kích hoạt (Deactivate) và Ghi nhận log hiệp tập (Add log) | 11/02/2026 | 11/02/2026 | |
| 4   | - Phát triển **PlanDetailScreen** trên Frontend <br>&emsp; + Hiển thị trực quan kế hoạch đang active, tích hợp bộ chọn ngày (Thứ 2 – Chủ Nhật) <br>&emsp; + Nút Action "Bắt đầu tập": trigger API tạo session → điều hướng liền mạch sang `WorkoutSessionScreen` <br>&emsp; + Hiển thị banner cảnh báo nếu tồn tại một session đang dang dở <br>&emsp; + Điều hướng mượt mà sang `SessionCalendar` để tra cứu lịch sử tập luyện | 12/02/2026 | 12/02/2026 | |
| 5   | - Triển khai **workoutSessionSlice** (Redux Toolkit) <br>&emsp; + Cấu trúc State: `sessionId`, `exercises[]`, `currentExIndex`, `currentSet`, `completedSets[]`, `phase`, `restSeconds` <br>&emsp; + Định nghĩa Actions: `initializeSession`, `logSetStart/Success/Failure`, `finishRest`, `skipExercise`, `resetSession` <br>&emsp; + Tích hợp `AsyncStorage` để persist (lưu trữ bền bỉ) state của session — tự động khôi phục (hydrate) trạng thái khi reload ứng dụng | 13/02/2026 | 13/02/2026 | |
| 6   | - Xây dựng giao diện tương tác **WorkoutSessionScreen** <br>&emsp; + `ActiveExerciseCard`: render trực quan tên bài tập, tiến độ hiệp (set progress) và mục tiêu rep <br>&emsp; + `LogSetSheet`: form nhập liệu rep & khối lượng → kích hoạt API `addWorkoutLog` <br>&emsp; + `RestTimer`: đồng hồ đếm ngược thông minh, tự động chuyển bài khi kết thúc thời gian nghỉ <br> - Phát triển **WorkoutSuccessScreen** <br>&emsp; + Màn hình Trophy (chúc mừng) kèm thống kê tóm tắt hiệu suất buổi tập <br>&emsp; + Can thiệp sâu để vô hiệu hóa (intercept) nút back cứng của thiết bị, tránh thoát nhầm luồng | 14/02/2026 | 14/02/2026 | |

### Kết quả đạt được tuần 6:

* **Backend — module Session**:
  * Các thực thể `UserWorkoutSession` và `WorkoutLog` được ánh xạ và lưu trữ thành công vào cơ sở dữ liệu; cơ chế cascade DELETE vận hành chuẩn xác, ngăn ngừa rò rỉ dữ liệu (data leaks) khi xóa session.
  * Các API cốt lõi (khởi tạo, vô hiệu hóa, và ghi nhận log set) hoạt động ổn định và đáp ứng đúng logic nghiệp vụ.
  * Khả năng truy vấn (query) session theo ngày cụ thể và trích xuất session đang active hoạt động với độ chính xác cao.
* **Frontend — Trải nghiệm Live-Workout**:
  * `PlanDetailScreen` tải mượt mà dữ liệu kế hoạch đang active; logic lọc bài tập theo `dayOfWeek` thông qua bộ chọn ngày hoạt động hoàn hảo.
  * Redux `workoutSessionSlice` đóng vai trò là "Single Source of Truth", quản lý trọn vẹn state của buổi tập và persist an toàn xuống `AsyncStorage`.
  * Giao diện `WorkoutSessionScreen` dẫn dắt người dùng xuyên suốt lộ trình bài tập/hiệp tập một cách trực quan, kết hợp mượt mà với bộ đếm ngược thời gian nghỉ (rest timer).
  * **Tối ưu hóa hiệu năng UI**: Xử lý triệt để bài toán *stale closure* (đóng gói sai trạng thái biến) bên trong vòng lặp đếm giờ, đồng thời refactor và gộp các `useEffect` dư thừa. Áp dụng chiến lược dùng `useRef` để theo dõi trạng thái active session, đảm bảo giao diện luôn duy trì tốc độ khung hình lý tưởng (60fps) mà không làm nghẽn (block) main thread.
  * Vòng lặp tính năng tập luyện (workout loop) đã vượt qua các kịch bản kiểm thử toàn trình (end-to-end testing) một cách xuất sắc.

### Kiến thức AWS đã học:

* Làm chủ nghệ thuật thiết kế Structured Logging cho môi trường Cloud: chuẩn hóa các trường dữ liệu cốt lõi (severity, requestId, userId, endpoint, errorCode) nhằm tối ưu hóa tốc độ truy xuất và lọc log trên **Amazon CloudWatch**.
* Xác định và giám sát các chỉ số hiệu năng trọng yếu (SLIs) cho luồng live-workout: độ trễ (latency) khi khởi tạo session, tỷ lệ thành công khi ghi nhận log set, và tính nhất quán của trạng thái active session.
* Nắm vững triết lý thiết lập cảnh báo chủ động (Proactive Alarming) qua CloudWatch Alarms: tập trung vào các chỉ số phân vị p95 latency, tỷ lệ HTTP 5xx errors, và các tín hiệu cạn kiệt tài nguyên database.
* Kiến tạo các dashboard vận hành mang tính hành động (actionable dashboards), ưu tiên các số liệu phục vụ quyết định (decision-making) thay vì hiển thị dàn trải các metric không trọng tâm.
* Thực hành bài bản quy trình quản lý sự cố (Incident Response): từ khâu phát hiện (detection), phân loại (triage), giảm thiểu thiệt hại (mitigation), cho đến nghiệm thu và lập báo cáo hậu kiểm (post-mortem).
* Quán triệt nguyên lý "Rollback-First" (ưu tiên khôi phục phiên bản ổn định) khi đối mặt với các bản release lỗi, nhằm bảo vệ trải nghiệm người dùng trước khi tiến hành điều tra nguyên nhân gốc rễ (root cause analysis).
* Xây dựng định hướng kiến trúc sẵn sàng tích hợp **Amazon SNS** (hoặc các kênh notification tương đương) để định tuyến các cảnh báo hệ thống cấp độ Critical (nghiêm trọng) trong các phase tiếp theo.

Tóm lại, tuần 6 đã nâng góc nhìn của team từ việc đơn thuần phát triển tính năng sang tư duy vận hành (ops) và giám sát (monitoring) hệ thống một cách chủ động trên môi trường cloud.

### Kế hoạch tuần tiếp theo:

* **Backend**: Khởi tạo module quản lý Media — thiết kế thực thể `Image`, tích hợp sâu với **Amazon S3** thông qua AWS SDK, và xây dựng `ImageController` phục vụ việc định tuyến hình ảnh cho các module bài tập, thực phẩm và kế hoạch.
* **Frontend**: Phát triển `SessionDetailScreen` (trang chi tiết hiệu suất buổi tập) và `SessionCalendarScreen` (giao diện lịch tháng trực quan hóa mật độ tập luyện bằng hệ thống marker/chấm lịch).