# Project Timeline - myFit (12 tuần)

## 1) Tổng quan kế hoạch

- Thời gian dự án: 12 tuần (01/2026 - 04/2026)
- Sản phẩm: myFit (Spring Boot + React Native), tích hợp AWS Cognito, S3, Bedrock
- Mục tiêu cuối kỳ:
  - Hoàn thiện ứng dụng full-stack phục vụ theo dõi tập luyện, dinh dưỡng và sức khỏe
  - Đảm bảo bảo mật, khả năng vận hành và tài liệu bàn giao đầy đủ
  - Trình bày demo cuối kỳ và nộp báo cáo thực tập

## 2) Kế hoạch theo giai đoạn

| Giai đoạn | Tuần | Trọng tâm | Kết quả đầu ra chính |
| --- | --- | --- | --- |
| Khởi tạo nền tảng | 1-2 | Setup hệ thống, auth, profile | Skeleton backend/frontend, Cognito login, UserProfile |
| Xây tính năng cốt lõi | 3-6 | GoalType, workout plans, session/log | Luồng kế hoạch tập và phiên tập hoạt động end-to-end |
| Mở rộng nghiệp vụ | 7-9 | Media, dinh dưỡng, chỉ số sức khỏe | S3 media, meal tracking, BMI/BMR/TDEE dashboard |
| Tăng tốc chất lượng | 10-11 | AI chat, tối ưu UX, test và tài liệu | Chat Bedrock, Home/Profile hoàn thiện, E2E + docs |
| Chốt dự án | 12 | Thuyết trình, phản hồi, bàn giao | Demo cuối, báo cáo thực tập, tổng kết dự án |

## 3) Timeline chi tiết theo tuần

| Tuần | Mục tiêu chính | Hạng mục thực hiện | Deliverable |
| --- | --- | --- | --- |
| Tuần 1 | Khởi động dự án và thiết lập nền tảng kỹ thuật | Tạo repo backend/frontend, setup Docker PostgreSQL, thiết kế ERD, setup Redux + React Query | Nền tảng kỹ thuật chạy local ổn định |
| Tuần 2 | Tích hợp xác thực AWS Cognito | Cấu hình Spring Security JWT, map role từ Cognito, hoàn thiện LoginScreen PKCE, secure token storage | Luồng đăng nhập và phân quyền hoạt động |
| Tuần 3 | Chuẩn hóa backend common layer và navigation app | Global exception handling, CORS, module GoalType, RootNavigator + Onboarding đa bước | Luồng auth -> onboarding -> main app thông suốt |
| Tuần 4 | Xây module bài tập hệ thống | MuscleGroup, Exercise, WorkoutPlan, WorkoutPlanExercise; màn gợi ý giáo án và picker bài tập | Kho dữ liệu bài tập + giáo án mẫu sẵn sàng |
| Tuần 5 | Quản lý kế hoạch tập cá nhân | UserWorkoutPlan CRUD, clone từ system plan, logic active plan duy nhất, màn tạo/sửa kế hoạch | User có thể tự tạo và vận hành giáo án riêng |
| Tuần 6 | Theo dõi buổi tập thực tế | UserWorkoutSession + WorkoutLog, PlanDetail, WorkoutSessionScreen, state session qua Redux | Luồng tập luyện realtime hoàn chỉnh |
| Tuần 7 | Tích hợp media và lịch sử phiên tập | Image module, AWS S3 config, SessionDetail + SessionCalendar, media caching | Hình ảnh hiển thị xuyên suốt các màn hình liên quan |
| Tuần 8 | Theo dõi dinh dưỡng | Food/Meal/MealFood/DailyNutrition, DietScreen + DietHistoryScreen | Theo dõi calo và macro hàng ngày |
| Tuần 9 | Dashboard chỉ số sức khỏe | BodyMetric + HealthCalculation, BMI/BMR/TDEE API, charts sức khỏe | Bộ chỉ số sức khỏe và biểu đồ theo thời gian |
| Tuần 10 | Tích hợp AI và tối ưu chất lượng sản phẩm | ChatScreen + Bedrock, ProfileScreen, HomeScreen, unit test backend, bug fix, token refresh queue | Tăng trải nghiệm người dùng và độ ổn định hệ thống |
| Tuần 11 | Kiểm thử tổng thể và chuẩn bị bàn giao | E2E test 6 user journeys, cập nhật README/guide, cleanup code backend/frontend | Bản release sẵn sàng demo và bàn giao |
| Tuần 12 | Hoàn thiện chương trình thực tập | Thuyết trình cuối kỳ, tự đánh giá, feedback, nộp báo cáo, tổng kết | Hoàn tất deliverable và đóng dự án |

## 4) Mốc quan trọng (Milestones)

- M1 (Cuối Tuần 2): Hoàn thành luồng xác thực và hồ sơ người dùng
- M2 (Cuối Tuần 4): Hoàn thiện hệ thống bài tập và giáo án mẫu
- M3 (Cuối Tuần 6): Hoàn tất luồng tập luyện thực tế end-to-end
- M4 (Cuối Tuần 9): Hoàn thiện tính năng dinh dưỡng + dashboard sức khỏe
- M5 (Cuối Tuần 11): Hoàn tất kiểm thử tích hợp, tài liệu, và release candidate
- M6 (Tuần 12): Demo cuối kỳ, nộp báo cáo và bàn giao

## 5) Phụ thuộc và rủi ro chính

- Phụ thuộc:
  - AWS services (Cognito, S3, Bedrock) và cấu hình IAM/secret đúng chuẩn
  - Môi trường chạy đồng bộ giữa backend, frontend và database
- Rủi ro:
  - Lỗi token/auth ảnh hưởng luồng người dùng
  - Độ trễ hoặc quota của dịch vụ AI khi gọi Bedrock
  - Chậm tiến độ nếu E2E và bug fix dồn vào cuối kỳ
- Kế hoạch giảm thiểu:
  - Chốt API contract sớm, kiểm thử định kỳ theo tuần
  - Theo dõi logging/healthcheck liên tục bằng checklist vận hành
  - Dành buffer tuần 10-11 cho fix lỗi và hardening

## 6) Tiêu chí hoàn thành dự án

- 100% luồng nghiệp vụ chính chạy ổn định: Auth, Workout, Nutrition, Health, AI Chat
- Tài liệu đầy đủ: setup, API, kiến trúc, hướng dẫn bàn giao
- Hoàn thành demo cuối kỳ, self-evaluation, feedback và internship report
