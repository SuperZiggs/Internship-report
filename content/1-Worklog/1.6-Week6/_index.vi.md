---
title: "Worklog Tuần 6"
date: 2026-02-09
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* **Backend**: Xây dựng module `UserWorkoutSession` và `WorkoutLog` để ghi lại hoạt động tập luyện thực tế.
* **Frontend**: Xây dựng `PlanDetailScreen` và `WorkoutSessionScreen` — trải nghiệm tập luyện trực tiếp.
* Hoàn thiện vòng lặp chính: kế hoạch → buổi tập → log set → kết thúc.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Xây dựng **UserWorkoutSession** entity <br>&emsp; + `user`, `userWorkoutPlan (@ManyToOne)`, `workoutDate (LocalDate)`, `isActive`, `weekIndex`, `dayIndex` <br>&emsp; + Cascade to `WorkoutLog` qua `@OneToMany(cascade = ALL)` | 10/02/2026 | 10/02/2026 | |
| 3   | - Xây dựng **WorkoutLog** entity <br>&emsp; + `userWorkoutSession`, `exercise`, `setNumber`, `reps`, `weight (Float)`, `durationSeconds` <br> - Xây dựng **UserWorkoutSessionController** (`/api/sessions`) <br>&emsp; + Create, Get by ID/user/date, Get active, Deactivate, Add log | 11/02/2026 | 11/02/2026 | |
| 4   | - Xây dựng **PlanDetailScreen** (Frontend) <br>&emsp; + Hiển thị kế hoạch active, bộ chọn ngày Thứ 2–CN <br>&emsp; + Nút "Bắt đầu tập": tạo session → chuyển đến `WorkoutSessionScreen` <br>&emsp; + Banner session đang tiến hành nếu có <br>&emsp; + Link đến `SessionCalendar` để xem lịch sử | 12/02/2026 | 12/02/2026 | |
| 5   | - Xây dựng **workoutSessionSlice** (Redux) <br>&emsp; + State: `sessionId`, `exercises[]`, `currentExIndex`, `currentSet`, `completedSets[]`, `phase`, `restSeconds` <br>&emsp; + Actions: `initializeSession`, `logSetStart/Success/Failure`, `finishRest`, `skipExercise`, `resetSession` <br>&emsp; + Lưu session vào `AsyncStorage` — khôi phục khi app reload | 13/02/2026 | 13/02/2026 | |
| 6   | - Xây dựng **WorkoutSessionScreen** (Frontend) <br>&emsp; + `ActiveExerciseCard`: hiển thị tên bài tập, tiến độ set, mục tiêu rep <br>&emsp; + `LogSetSheet`: nhập rep + cân nặng → gọi `addWorkoutLog` API <br>&emsp; + `RestTimer`: đếm ngược, tự chuyển bài tập sau khi nghỉ <br> - Xây dựng **WorkoutSuccessScreen** <br>&emsp; + Trophy screen với tóm tắt buổi tập <br>&emsp; + Chặn nút back phần cứng | 14/02/2026 | 14/02/2026 | |

### Kết quả đạt được tuần 6:

* **Backend — Module Session**:
  * Entity `UserWorkoutSession` + `WorkoutLog` lưu thành công với cascade DELETE.
  * API tạo session, deactivate và log set hoạt động đúng.
  * Query theo ngày và lấy active session hoạt động chính xác.
* **Frontend — Tập Thực Tế**:
  * `PlanDetailScreen` load kế hoạch active; bộ chọn ngày lọc bài tập theo `dayOfWeek`.
  * Redux `workoutSessionSlice` quản lý toàn bộ trạng thái buổi tập, lưu AsyncStorage.
  * `WorkoutSessionScreen` hướng dẫn user qua tất cả bài tập/set với timer nghỉ.
  * Vòng lặp tập luyện hoàn chỉnh được test end-to-end.

### Kiến thức AWS đã học:

* Học cách thiết kế structured logging cho cloud với các trường như severity, requestId, userId, endpoint và errorCode để lọc log nhanh trên CloudWatch.
* Xác định các chỉ số quan trọng cho luồng workout realtime như độ trễ tạo session, tỷ lệ ghi log set thành công và tính nhất quán của active session.
* Nắm nguyên tắc tạo CloudWatch Alarm dựa trên p95 latency, tỷ lệ lỗi 5xx và dấu hiệu quá tải database.
* Hiểu cách xây dashboard vận hành theo hướng actionable, phục vụ ra quyết định thay vì chỉ hiển thị quá nhiều metric rời rạc.
* Thực hành tư duy xử lý sự cố cơ bản: phát hiện, phân loại, giảm thiểu, xác nhận và ghi nhận bài học sau sự cố.
* Củng cố nguyên tắc rollback-first khi release lỗi để giảm tác động tới người dùng trước khi điều tra nguyên nhân gốc.
* Chuẩn bị định hướng tích hợp SNS hoặc kênh thông báo tương đương cho các cảnh báo mức critical trong tương lai.

Tóm lại, tuần 6 nâng góc nhìn từ viết tính năng sang vận hành và giám sát hệ thống trên môi trường cloud.

### Kế hoạch tuần tiếp theo:

* **Backend**: Xây dựng module Media — entity `Image`, tích hợp AWS S3, `ImageController` liên kết hình ảnh với bài tập/thực phẩm/kế hoạch.
* **Frontend**: Xây dựng `SessionDetailScreen` và `SessionCalendarScreen` (lịch tháng có chấm buổi tập).
