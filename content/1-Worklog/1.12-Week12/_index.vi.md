---
title: "Worklog Tuần 12"
date: 2026-03-30
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu Tuần 12

* Thực hiện **bài thuyết trình bảo vệ dự án cuối kỳ** (Final Presentation) trước hội đồng chuyên môn và các bên liên quan.
* Hoàn tất quy trình **tự đánh giá (self-assessment)** và đóng góp ý kiến phản hồi (feedback) mang tính xây dựng.
* **Chính thức nghiệm thu và nộp báo cáo thực tập** tổng kết toàn khóa.
* **Tổng kết chặng đường 12 tuần**, đánh giá những cột mốc đã chinh phục và ăn mừng thành quả dự án.

### Các nhiệm vụ thực hiện trong tuần

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1-6 | - Chuẩn bị **tài liệu bảo vệ dự án** & Nghiệm thu <br>&emsp; + Hoàn tất nộp source code và tài liệu theo chuẩn đầu ra (Hạn chót: 04/04) <br>&emsp; + Xây dựng kịch bản và video demo dự phòng (fallback) cho ứng dụng myFit <br>&emsp; + Tổng kiểm tra (sanity check) hệ thống hạ tầng API và điểm kết nối AWS Bedrock | 30/03/2026 | 04/04/2026 | [Presentation] |
| - | - **Bảo vệ đồ án cuối kỳ** <br>&emsp; + Báo cáo tổng kết tại tầng 26 lúc 7h45 <br>&emsp; + Cấu trúc: 2 phút setup + 10 phút thuyết trình + 10 phút bảo vệ trước hội đồng (Q&A) | 18/04/2026 | 18/04/2026 | [Presentation Recording] |
| 3 | - **Tự đánh giá (Self-Assessment)** <br>&emsp; + Hoàn thiện biểu mẫu đánh giá năng lực cá nhân <br>&emsp; + Nhìn nhận lại quá trình tiến hóa kỹ năng trong 12 tuần <br>&emsp; + Nhận diện các điểm sáng kỹ thuật và vạch ra định hướng nâng cấp chuyên môn | 01/04/2026 | 01/04/2026 | [Evaluation Form] |
| 4 | - **Phản hồi chương trình (Program Feedback)** <br>&emsp; + Đóng góp ý kiến chuyên sâu cho ban tổ chức FCAJ <br>&emsp; + Nhấn mạnh các giá trị thực tiễn thu nhận được <br>&emsp; + Đề xuất các phương án tối ưu hóa lộ trình cho các thế hệ thực tập sinh kế tiếp | 02/04/2026 | 02/04/2026 | [Feedback Form] |

### Thành tựu Tuần 12

* **Bảo vệ dự án thành công:**
  * Trình bày trôi chảy kiến trúc hệ thống và giải pháp nghiệp vụ của myFit trước hội đồng chuyên môn và các mentor FCAJ.
  * Thực hiện live-demo mượt mà hệ sinh thái React Native và năng lực tư vấn vượt trội của AI Chatbot (AWS Bedrock).

* **Nghiệm thu toàn diện:**
  * Hạ tầng Backend (Spring Boot) và Frontend (React Native) đạt trạng thái tích hợp hoàn hảo và vận hành ổn định.
  * Bàn giao trọn bộ tài liệu kỹ thuật chuẩn mực: API specs, sơ đồ kiến trúc (architecture diagram) và cẩm nang triển khai (deployment guide).
  * Repository GitHub được tái cấu trúc gọn gàng, loại bỏ hoàn toàn nợ kỹ thuật (technical debt).

* **Tiến hóa năng lực cá nhân:**
  * Làm chủ hệ sinh thái dịch vụ lõi của AWS (Cognito, S3, Bedrock, Secrets Manager).
  * Tích lũy khối lượng kinh nghiệm đồ sộ về chu trình phát triển **full-stack end-to-end** thực chiến.
  * Mài giũa sắc bén tư duy giải quyết vấn đề (problem-solving), kỹ năng thuyết trình và năng lực biên soạn tài liệu kỹ thuật.
  * Xây dựng và mở rộng mạng lưới quan hệ (networking) giá trị với các AWS Solution Architects và chuyên gia trong ngành.

### Tổng kết toàn bộ kỳ thực tập

**Các giai đoạn trong hành trình 12 tuần:**

| Phase | Weeks | Thành tựu chính |
|------|------|-----------------|
| Nền tảng | 1-2 | Thiết lập hạ tầng, thiết kế DB schema, phát triển REST API cốt lõi |
| Phát triển cốt lõi | 3-6 | Tích hợp xác thực AWS Cognito, thiết lập lưu trữ S3 Media, module System Workouts |
| Mở rộng tính năng | 7-8 | Triển khai module Workout Sessions và engine theo dõi dinh dưỡng |
| Tích hợp AI | 9-10 | Xử lý thuật toán sinh lý học sức khỏe, xây dựng AI Fitness Coach qua AWS Bedrock |
| Tối ưu | 11 | Đánh bóng UI/UX, kiểm thử E2E toàn trình, chuẩn hóa môi trường Docker Compose |
| Hoàn thiện | 12 | Tài liệu hóa hệ thống, bảo vệ đồ án cuối kỳ, bàn giao dự án |

**Hệ sinh thái AWS đã làm chủ:**
* Amazon Cognito, Amazon S3, **Amazon CloudFront (kết hợp bảo mật OAC)**, Amazon Bedrock
* AWS Secrets Manager, kiến trúc phân quyền IAM Policies, AWS KMS
* Tư duy thiết kế hạ tầng cho Amazon ECS (Fargate), Application Load Balancer (ALB) và Amazon Route 53

**Dấu ấn kỹ thuật (Project Metrics):**
* Phát triển và đưa vào vận hành hơn **10 module REST API** phức tạp (Auth, Food, Workout, Metrics,...)
* Thiết kế và kiểm thử thành công **6 luồng hành vi người dùng (user journeys)** end-to-end
* Triển khai thuật toán **bộ nhớ ngữ cảnh cửa sổ trượt (sliding window) lên tới 12 lượt hội thoại** cho AI Assistant
* Tối ưu hóa thời gian khởi động lạnh (cold start) của cụm backend qua Docker Compose xuống **dưới 25 giây**

### Suy ngẫm cuối cùng

Hành trình 12 tuần đồng hành cùng chương trình FCAJ là một bước ngoặt phát triển sự nghiệp sâu sắc. Từ những bản nháp thiết kế PostgreSQL schema sơ khai ở Tuần 1, cho đến lúc chứng kiến một hệ sinh thái React Native tích hợp trí tuệ nhân tạo vận hành trơn tru ở Tuần 12, chặng đường này đã khắc sâu vào tư duy tôi những triết lý kỹ thuật cốt lõi:

1. **Security by design**: Bảo mật không phải là tính năng thêm thắt, mà là nền móng — đặc biệt trong việc thiết lập hàng rào chống IDOR, quản trị cấu trúc JWT stateless và tích hợp xác thực AWS Cognito một cách kín kẽ.
2. **Separation of concerns**: Sự phân định rạch ròi trách nhiệm kiến trúc xuyên suốt hệ thống (sử dụng Redux cho state xác thực, React Query cho server cache, và duy trì tính thuần khiết của các service layer phía backend).
3. **Vận hành cloud theo chuẩn Well-Architected**: Đảm bảo các tiến trình containerization đạt độ ổn định tuyệt đối và vạch ra một lộ trình dịch chuyển lên AWS mạch lạc, tối ưu hóa.
4. **Hợp tác & Tài liệu hóa**: Kỹ năng làm việc nhóm và văn hóa viết tài liệu là chìa khóa tối thượng để bảo đảm khả năng bảo trì và chuyển giao dự án thành công.

Tôi xin gửi lời triân sâu sắc đến các mentor của FCAJ, các chuyên gia AWS Solution Architect cùng những người đồng đội đã kiến tạo nên một kỳ thực tập vô giá. Đây chưa phải là vạch đích, mà chính là **bệ phóng vững chãi cho chặng đường chinh phục các hệ thống Cloud và Full-stack quy mô lớn trong tương lai** của tôi! ☁️🚀