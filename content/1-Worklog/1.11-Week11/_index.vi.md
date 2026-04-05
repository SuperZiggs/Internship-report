---
title: "Worklog Tuần 11"
date: 2026-03-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu Tuần 11:

* Thực thi chiến dịch **kiểm thử tích hợp toàn trình (End-to-End Integration Testing)** bao phủ toàn bộ hệ sinh thái ứng dụng myFit nhằm cam kết chất lượng đầu ra.
* Biên soạn **hệ thống tài liệu kỹ thuật chuyên sâu** — bao gồm README Backend, Cẩm nang phát triển Frontend (Development Guide) và Sơ đồ kiến trúc tổng thể.
* Tiến hành **khử nợ kỹ thuật (Technical Debt Cleanup)**, loại bỏ triệt để các tàn dư debug, tối ưu hóa mã nguồn và đưa dự án vào trạng thái sẵn sàng bàn giao (handover-ready).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | --------------- | ------------------ |
| 2   | - **Kiểm thử tích hợp End-to-End** — Backend <br>&emsp; + Rà soát toàn bộ API endpoints với các kịch bản dữ liệu hợp lệ (happy path) và dị thường (edge cases) <br>&emsp; + Xác thực tiến trình migration Flyway V1/V2/V3 vận hành trơn tru trên cơ sở dữ liệu trắng <br>&emsp; + Đánh giá hiệu năng khởi động lạnh (cold start) qua Docker Compose: đảm bảo cụm `db` + `api` đạt trạng thái healthy dưới 30s <br>&emsp; + Kiểm toán file `application.properties` — triệt tiêu nguy cơ rò rỉ các cấu hình môi trường dev lên production | 23/03/2026 | 23/03/2026 | |
| 3   | - **Kiểm thử tích hợp End-to-End** — Frontend (6 user journeys cốt lõi) <br>&emsp; + Luồng 1: Xác thực → Thu thập dữ liệu Onboarding → Dashboard trang chủ <br>&emsp; + Luồng 2: Khởi tạo kế hoạch cá nhân → Nhân bản (clone) template hệ thống → Kích hoạt lịch tập <br>&emsp; + Luồng 3: Mở phiên tập (live-session) → Ghi nhận telemetry từng hiệp → Quản lý rest timer → Hoàn tất buổi tập <br>&emsp; + Luồng 4: Phân bổ thực phẩm vào 4 bữa tiêu chuẩn → Đối chiếu tổng macro/calo trong ngày <br>&emsp; + Luồng 5: Cập nhật nhân trắc học → Xử lý thuật toán BMI/BMR/TDEE → Trực quan hóa qua biểu đồ <br>&emsp; + Luồng 6: Tương tác Bedrock AI Assistant → Kiểm định chất lượng phản hồi ngôn ngữ tự nhiên (tiếng Việt) | 24/03/2026 | 24/03/2026 | |
| 4   | - Biên soạn **Backend README** (`myFit-api/README.md`) <br>&emsp; + Đặc tả tổng quan dự án & thiết kế kiến trúc (Spring Boot ↔ PostgreSQL ↔ Cognito ↔ S3) <br>&emsp; + Phân tách tài liệu theo module: Auth, Food, SystemWorkout, UserWorkoutPlan, Session, UserMetric, Media, GoalType <br>&emsp; + Chuẩn hóa quy trình bootstrap: prerequisites, thiết lập `.env`, bộ lệnh Docker Compose <br>&emsp; + Xây dựng API Reference: định tuyến, phương thức, payload chuẩn, và yêu cầu authorization | 25/03/2026 | 25/03/2026 | |
| 4   | - Hoàn thiện **Frontend Development Guide** (`guide.md`) <br>&emsp; + Khái quát hệ sinh thái công nghệ (Tech Stack) <br>&emsp; + Lập bản đồ kiến trúc điều hướng (Navigation Tree: AuthStack / OnboardingStack / MainTabs) <br>&emsp; + Quy trình setup local: cấu hình `.env`, lệnh khởi chạy Expo <br>&emsp; + Mapping chi tiết tính năng theo từng màn hình (Screen Dictionary) <br>&emsp; + Tài liệu hóa cấu hình tích hợp AWS Cognito PKCE và AWS Bedrock | 25/03/2026 | 25/03/2026 | |
| 5   | - **Dọn dẹp mã nguồn (Code Cleanup)** — Backend <br>&emsp; + Rà quét và loại bỏ toàn bộ các thẻ `TODO`, `FIXME`, cùng các lệnh debug `System.out.println` <br>&emsp; + Bổ sung Javadoc cho các service và logic nghiệp vụ phức tạp <br>&emsp; + Kiểm toán cấu hình Spring Security, ngăn chặn rủi ro mở nhầm các public routes <br>&emsp; + Thẩm định quá trình build: thực thi `mvn clean package -DskipTests` → đảm bảo xuất xưởng artifact JAR hoàn chỉnh | 26/03/2026 | 26/03/2026 | |
| 5   | - Tối ưu hóa **Response Interceptor** + **UX Polish** + **Final Bug Fix** <br>&emsp; + Hoàn thiện cơ chế Token Refresh đa luồng: đưa các request lỗi `401` đồng thời vào queue, thực thi một phiên refresh duy nhất, sau đó tái kích hoạt (retry) toàn bộ hàng đợi <br>&emsp; + Triển khai `NotificationBox` như một Global Alert Proxy, thay thế hoàn toàn `Alert.alert` native <br>&emsp; + Tích hợp cơ chế Pull-to-refresh cho `HomeScreen` + tinh chỉnh logic hiển thị skeleton loading trên `HealthDashboardScreen` <br>&emsp; + Chuẩn hóa bản địa hóa (localization) cho các giá trị enum (Activity Level, Goal Type) <br>&emsp; + Đồng nhất thiết kế loading spinner và fallback errors trên toàn hệ thống | 26/03/2026 | 26/03/2026 | |
| 6   | - **Dọn dẹp mã nguồn (Code Cleanup)** — Frontend <br>&emsp; + Purge toàn bộ `console.log` và các debug statements <br>&emsp; + Thực thi Eslint và giải quyết triệt để các cảnh báo (warnings) còn tồn đọng <br>&emsp; + Loại bỏ (tree-shaking) các thư viện và imports dư thừa <br>&emsp; + Vá lỗi hiển thị (empty state) của `BMITrendChart` <br>&emsp; + Thử nghiệm export production: `npx expo export` → xác nhận Zero TypeScript Error <br> - Tổ chức **Retrospective**: đúc kết bài học kỹ thuật, đánh giá các quyết định kiến trúc, và phác thảo lộ trình nâng cấp (Future Roadmap) | 27/03/2026 | 27/03/2026 | |

### Thành tựu Tuần 11:

* **Kiểm thử tích hợp (Integration Testing)**:
  * 100% API endpoints của hệ thống vượt qua các kịch bản kiểm thử biên (edge cases) và kiểm thử phá hoại (negative tests).
  * Quy trình khởi động cụm hạ tầng qua Docker Compose đạt độ ổn định cao — toàn bộ services vào trạng thái ready dưới 25 giây.
  * Cơ chế quản trị DB schema qua Flyway (V1-V3) chứng minh tính tất định (deterministic) khi chạy mượt mà trên môi trường sạch (clean database).
  * Toàn bộ 6 luồng trải nghiệm cốt lõi của người dùng đều vận hành trơn tru, không phát sinh bất kỳ block-bug nào.
* **Tài liệu dự án (Documentation)**:
  * Hệ thống tài liệu Backend được thiết kế chuyên nghiệp, minh bạch hóa toàn bộ cấu trúc biến môi trường và đặc tả API.
  * Cẩm nang Frontend cung cấp góc nhìn toàn diện về kiến trúc điều hướng và phương pháp liên kết với các Managed Services của AWS.
  * Sơ đồ luồng dữ liệu (Data Flow) được ánh xạ rõ ràng: Spring Boot ↔ PostgreSQL ↔ AWS Cognito ↔ AWS S3 ↔ React Native ↔ AWS Bedrock.
* **CI/CD & Chất lượng mã nguồn**:
  * Tích hợp thành công GitHub Actions workflows để tự động hóa toàn chu trình: Linting → Testing → Building → Rolling Deployment lên **Amazon ECS Fargate**.
  * Mã nguồn đạt trạng thái tinh gọn, sạch bóng các tàn dư debug (console log/print).
  * Tiến trình kiểm định kiểu tĩnh của TypeScript (`npx expo export`) pass hoàn toàn với 0 lỗi cảnh báo.
  * Pipeline Maven tự động đóng gói và lưu trữ artifact JAR một cách ổn định, sẵn sàng cho khâu containerization.
* **Đúc kết từ Retrospective — Các bài học lõi**:
  * **Chống rò rỉ dữ liệu (IDOR Prevention)**: Việc ủy quyền trích xuất trực tiếp `sub` từ JWT payload là chốt chặn bảo mật không thể thỏa hiệp đối với các REST API thao tác trên tài nguyên cá nhân.
  * **Chiến lược Soft Delete**: Áp dụng `@SQLRestriction` mang lại phương án quản lý vòng đời dữ liệu an toàn, trao quyền khôi phục (recovery) và duy trì tính toàn vẹn lịch sử tốt hơn hẳn so với hard delete.
  * **Quản trị Stateless JWT**: Mô hình này giải phóng Backend khỏi gánh nặng quản lý session, nhưng đòi hỏi kiến trúc Frontend phải cực kỳ tinh tế trong khâu xử lý thu hồi (force logout) và làm mới token (refresh queue) để không làm vỡ UX.
  * **Hệ sinh thái React Native + NativeWind**: Sự kết hợp này mang lại vận tốc phát triển (development velocity) vượt trội, đưa sức mạnh của TailwindCSS vào môi trường ứng dụng native.
  * **Kiến trúc State Management**: Sự phân bổ trách nhiệm rạch ròi giữa **Redux** (quản lý trạng thái tĩnh: Auth/Session) và **React Query** (quản lý server cache, tự động đồng bộ ngầm) đã chứng minh được tính hiệu quả và khả năng mở rộng sâu của Frontend.

### Kiến thức AWS đã học:

* Áp dụng khung đánh giá **AWS Well-Architected Framework**, đối chiếu kiến trúc hiện tại qua lăng kính 5 trụ cột cốt lõi: Security, Reliability, Performance Efficiency, Cost Optimization, và Operational Excellence.
* Thiết lập các tiêu chuẩn **Độ tin cậy (Reliability)** thông qua kịch bản kiểm tra health check chuỗi cung ứng, lập kế hoạch sao lưu/phục hồi (backup/restore), và thiết lập các chỉ tiêu RPO/RTO thực tiễn.
* Thấm nhuần triết lý **FinOps**: chủ động rà soát, right-sizing tài nguyên, thiết lập lifecycle policies cho S3, cài đặt cảnh báo ngân sách (budget alarms) và dọn dẹp các artifact không sử dụng.
* Hoàn thiện bộ tiêu chuẩn **Bảo mật chiều sâu (Defense in Depth)**: kiểm toán các ranh giới IAM, thắt chặt token policies, quy hoạch phạm vi mã hóa KMS, và củng cố quản trị log (audit logs).
* Thiết kế **Cloud Operations Runbook**: chuẩn hóa cẩm nang ứng phó cho các kịch bản deployment, rollback khẩn cấp, khắc phục sự cố (troubleshooting), và bảo trì định kỳ.
* Xác lập các tiêu chí **Sẵn sàng bàn giao (Handover Readiness)**: bảo đảm tính tái lập của môi trường, minh bạch hóa cấu hình, và vạch rõ lằn ranh trách nhiệm (Shared Responsibility).
* Kiến tạo **Lộ trình Cloud tương lai (Future Roadmap)**: đề xuất chiến lược tối ưu hóa ECS Auto-scaling, nâng cấp rule WAF trên ALB, cấu hình nâng cao cho CloudFront CDN, và triển khai hệ thống giám sát (observability) toàn diện.

Nhìn chung, tuần 11 là sự hội tụ, chuyển hóa những kiến thức AWS thực chiến thành một quy trình đánh giá chất lượng (quality assurance) và một bản lề vững chắc cho giai đoạn bàn giao hệ thống.

### Kế hoạch tuần tiếp theo:

* Hoàn tất quá trình tự đánh giá (Self-assessment) và tiếp thu phản hồi.
* Hoàn thiện và nộp báo cáo thực tập chính thức.