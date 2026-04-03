---
title: "Worklog Tuần 1"
date: 2026-01-05
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Khởi động dự án **myFit** — ứng dụng quản lý sức khỏe & thể dục toàn diện.
* Khởi tạo repository cho Backend (Spring Boot) và Frontend (React Native + Expo).
* Cài đặt môi trường phát triển local: Java 17, PostgreSQL qua Docker, Node.js, Expo CLI.
* Thống nhất kiến trúc hệ thống, phân chia module và tech stack với nhóm.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Kickoff dự án: xác định phạm vi, tính năng và yêu cầu MVP <br>&emsp; + Tính năng cốt lõi: kế hoạch tập luyện, theo dõi dinh dưỡng, chỉ số cơ thể, ghi log buổi tập <br>&emsp; + Xác thực qua AWS Cognito <br>&emsp; + Admin panel quản lý bài tập & kế hoạch | 06/01/2026 | 06/01/2026 | |
| 3   | - Khởi tạo dự án **Backend** Spring Boot 3 + Maven <br>&emsp; + Thêm dependencies: `spring-boot-starter-web`, `spring-data-jpa`, `spring-security` <br>&emsp; + Cấu hình `pom.xml`: Flyway, Lombok, jjwt 0.11.5, AWS SDK v1 <br>&emsp; + Tổ chức package: `common/`, `config/`, `module/` | 07/01/2026 | 07/01/2026 | <https://start.spring.io/> |
| 4   | - Cài PostgreSQL local bằng Docker Compose <br>&emsp; + Viết `docker-compose.yml` với service `postgres:15` <br>&emsp; + Cấu hình `application.properties`: datasource, JPA `ddl-auto=create-drop`, Flyway <br>&emsp; + Tạo file `.env` / `.env.example` quản lý secrets <br> - Định nghĩa `EntityBase` `@MappedSuperclass`: `id (UUID)`, `createdAt`, `updatedAt` | 08/01/2026 | 08/01/2026 | <https://docs.docker.com/compose/> |
| 4   | - Khởi tạo dự án **Frontend** React Native + Expo ~54 với TypeScript <br>&emsp; + Dùng `npx create-expo-app --template` <br>&emsp; + Cấu hình `tsconfig.json`, `babel.config.js`, `metro.config.js` <br>&emsp; + Cài đặt NativeWind v4 và cấu hình `tailwind.config.js` | 08/01/2026 | 08/01/2026 | <https://docs.expo.dev/> |
| 5   | - Thiết kế sơ đồ **Entity-Relationship** <br>&emsp; + Các entity: UserProfile, Food, Meal, Exercise, WorkoutPlan, BodyMetric, Session, Image… <br>&emsp; + Xác định quan hệ và cardinality <br> - Tạo **ApiResponse\<T\>** thống nhất: `code`, `message`, `result`, `timestamp`, `path` | 09/01/2026 | 09/01/2026 | |
| 6   | - Thiết lập **Redux Toolkit** store cho Frontend <br>&emsp; + Cài `@reduxjs/toolkit`, `react-redux` <br>&emsp; + Kết nối `store/index.ts` và `providers.tsx` vào `App.tsx` <br> - Cấu hình **Axios** client với base URL từ env <br> - Cấu hình **TanStack React Query** v5 `QueryClient` | 10/01/2026 | 10/01/2026 | <https://redux-toolkit.js.org/> |

### Kết quả đạt được tuần 1:

* Khởi tạo thành công skeleton cho cả backend lẫn frontend, chạy được trên máy local.
* **Backend (Spring Boot)**:
  * Ứng dụng khởi động và kết nối PostgreSQL qua Docker Compose (`docker-compose up -d`).
  * `EntityBase` với UUID primary key và audit timestamps sẵn sàng tái sử dụng cho mọi entity.
  * `ApiResponse<T>` chuẩn hóa response envelope cho tất cả các endpoint tương lai.
  * Maven build thành công, không có lỗi compile.
* **Frontend (React Native + Expo)**:
  * App render được trên Android emulator qua `npx expo start`.
  * NativeWind v4 cấu hình thành công — dùng được TailwindCSS trong RN components.
  * Redux store và React Query `QueryClient` kết nối qua `providers.tsx`.
  * Axios client đọc base URL từ `.env` (`EXPO_PUBLIC_BACKEND_API_URL=http://localhost:8080`).
* Môi trường Docker Compose có thể tái tạo nhất quán trên các máy khác nhau trong nhóm.
* Pattern quản lý secrets qua `.env` được thiết lập — không có credential hardcode trong source code.

### Kiến thức AWS đã học:

* Nắm cách thiết lập tài khoản AWS theo hướng production ngay từ đầu: bật MFA, tách quyền quản trị, và thiết kế IAM theo nguyên tắc least privilege cho cả lập trình viên lẫn CI.
* Hiểu cách chia định danh theo đúng mục đích sử dụng: người dùng thao tác thủ công, role deploy, và role runtime của ứng dụng để giảm blast radius.
* Thực hành dùng AWS CLI profile cho `dev` và `staging`, kết hợp cố định region để tránh thao tác nhầm môi trường.
* Hiểu cơ chế credential provider chain và lý do không bao giờ được commit access key hoặc secret key vào repository.
* Áp dụng chuẩn tagging như `Project`, `Environment`, `Owner`, `CostCenter` để phục vụ quản trị chi phí và vận hành sau này.
* Nắm Shared Responsibility Model ở mức thực tế: AWS chịu trách nhiệm hạ tầng nền, còn team vẫn chịu trách nhiệm code, IAM và bảo mật secret.
* Hình thành tư duy cloud-ready cho cấu hình hệ thống: toàn bộ giá trị nhạy cảm tách khỏi source code và sẵn sàng chuyển sang Secrets Manager hoặc SSM.

Tóm lại, tuần 1 đặt nền tảng tư duy vận hành AWS bài bản trước khi đi vào từng dịch vụ cụ thể.

### Kế hoạch tuần tiếp theo:

* **Backend**: Tích hợp AWS Cognito — cấu hình `SecurityConfig` với JWT resource server, viết `OAuth2TokenValidator` tùy chỉnh, xây dựng `UserProfile` entity + `UserProfileController`.
* **Frontend**: Xây dựng luồng Authentication — `LoginScreen` với PKCE OAuth qua `expo-auth-session`, lưu token vào `expo-secure-store`, quản lý state qua `authSlice`.
