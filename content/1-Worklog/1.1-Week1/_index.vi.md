---
title: "Báo cáo Công việc Tuần 1"
date: 2026-01-05
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Định hướng Tuần 1

* Bắt đầu triển khai dự án **myFit** - nền tảng ứng dụng chăm sóc sức khỏe và tập luyện toàn diện.
* Khởi tạo và cấu hình repository cho cả hai phía: Backend (sử dụng Spring Boot) và Frontend (sử dụng React Native kết hợp Expo).
* Chuẩn bị môi trường dev tại local bao gồm: Java 17, Node.js, Expo CLI và database PostgreSQL chạy trên Docker.
* Chốt phương án về kiến trúc tổng thể, công nghệ sử dụng (tech stack) và cấu trúc phân chia module cùng toàn team.

### Chi tiết công việc thực hiện trong tuần
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Họp khởi động (Kickoff): chốt phạm vi, các tính năng và yêu cầu cho phiên bản MVP <br>&emsp; + Tính năng chính: lộ trình tập, quản lý dinh dưỡng, theo dõi chỉ số cơ thể, lưu lịch sử tập luyện <br>&emsp; + Đăng nhập/Xác thực qua AWS Cognito <br>&emsp; + Xây dựng trang Admin quản lý dữ liệu bài tập | 06/01/2026 | 06/01/2026 | |
| 3 | Set up project **Backend** dùng Spring Boot 3 và Maven <br>&emsp; + Thêm các thư viện cần thiết: `spring-boot-starter-web`, `spring-data-jpa`, `spring-security` <br>&emsp; + Chỉnh sửa `pom.xml` tích hợp Flyway, Lombok, jjwt 0.11.5 và AWS SDK v1 <br>&emsp; + Phân chia cấu trúc thư mục theo `com.example.fitme`: `common/`, `config/`, `module/` | 07/01/2026 | 07/01/2026 | <https://start.spring.io/> |
| 4 | Triển khai PostgreSQL local qua Docker Compose <br>&emsp; + Viết file `docker-compose.yml` chạy service `postgres:15` <br>&emsp; + Cài đặt `application.properties`: thông tin db, JPA `ddl-auto=create-drop`, cấu hình Flyway <br>&emsp; + Thiết lập file `.env` và `.env.example` để bảo mật thông tin <br>&emsp; + Xây dựng class `EntityBase` với `@MappedSuperclass` chứa: `id (UUID)`, `createdAt`, `updatedAt` | 08/01/2026 | 08/01/2026 | <https://docs.docker.com/compose/> |
| 4 | Set up project **Frontend** bằng React Native + Expo SDK 54 (dùng TypeScript) <br>&emsp; + Chạy lệnh `npx create-expo-app --template` <br>&emsp; + Tùy chỉnh các file `tsconfig.json`, `babel.config.js`, `metro.config.js` <br>&emsp; + Tích hợp NativeWind v4 và update `tailwind.config.js` | 08/01/2026 | 08/01/2026 | <https://docs.expo.dev/> |
| 5 | Xây dựng mô hình quan hệ thực thể (**ERD**) <br>&emsp; + Định nghĩa các bảng: UserProfile, Food, Meal, Exercise, WorkoutPlan, BodyMetric, Session, Image… <br>&emsp; + Làm rõ mối quan hệ (cardinality) giữa các thực thể <br>&emsp; + Thiết kế class **ApiResponse<T>** chuẩn hóa gồm: `code`, `message`, `result`, `timestamp`, `path` | 09/01/2026 | 09/01/2026 | |
| 6 | Cấu hình **Redux Toolkit** để quản lý state Frontend <br>&emsp; + Tải các package `@reduxjs/toolkit`, `react-redux` <br>&emsp; + Wrap `store/index.ts` và `providers.tsx` vào file gốc `App.tsx` <br>&emsp; + Set up **Axios** gọi API với base URL đọc từ file env <br>&emsp; + Khởi tạo **TanStack React Query** v5 với `QueryClient` | 10/01/2026 | 10/01/2026 | <https://redux-toolkit.js.org/> |

### Thành quả Tuần 1

* Đã xây dựng thành công bộ khung (skeleton) cơ bản cho cả backend và frontend, đảm bảo vận hành tốt trên môi trường local.
* **Backend (Spring Boot)**:
  * Hệ thống boot thành công và kết nối ổn định với PostgreSQL chạy ngầm qua Docker Compose (`docker-compose up -d`).
  * Class `EntityBase` tích hợp khóa chính UUID cùng các trường thời gian audit đã hoàn thiện, sẵn sàng cho việc kế thừa.
  * Đã chuẩn hóa định dạng trả về cho API thông qua class wrapper `ApiResponse<T>`.
  * Quá trình build bằng Maven diễn ra trơn tru, không gặp lỗi biên dịch.
* **Frontend (React Native + Expo)**:
  * Giao diện app đã render thành công trên máy ảo Android thông qua lệnh `npx expo start`.
  * NativeWind v4 hoạt động ổn định, hỗ trợ viết class TailwindCSS trực tiếp trong các component React Native.
  * Quản lý state (Redux store) và fetch data (React Query `QueryClient`) đã được tích hợp đầy đủ vào hệ thống qua `providers.tsx`.
  * Axios đọc thành công cấu hình base URL từ file môi trường `.env` (`EXPO_PUBLIC_BACKEND_API_URL=http://localhost:8080`).
* Đảm bảo được tính nhất quán của môi trường Docker Compose, giúp các thành viên trong team dễ dàng setup mà không bị sai lệch cấu hình.
* Áp dụng chuẩn quản lý biến môi trường qua `.env`, loại bỏ hoàn toàn việc hardcode dữ liệu nhạy cảm, tạo đà thuận lợi cho việc tích hợp Backend API và CloudFront trong tương lai.

### Những kiến thức AWS tích lũy được

* Hiểu rõ quy trình thiết lập tài khoản AWS chuẩn production: kích hoạt MFA, phân quyền quản trị rõ ràng, và áp dụng nguyên tắc đặc quyền tối thiểu (least privilege) cho các role của dev cũng như CI.
* Nắm bắt phương pháp tách biệt các loại định danh (người dùng manual, role cho deploy, role cho runtime) nhằm hạn chế tối đa phạm vi ảnh hưởng (blast radius) khi có rủi ro.
* Thành thạo việc dùng AWS CLI profile cho các môi trường `dev` và `staging`, đồng thời set cứng region để phòng ngừa rủi ro thao tác nhầm.
* Hiểu sâu về credential provider chain và nhận thức rõ tầm quan trọng của việc không bao giờ đưa access key/secret key lên source code.
* Thực hành gắn thẻ (tagging) tài nguyên theo chuẩn (`Project`, `Environment`, `Owner`, `CostCenter`) để tối ưu việc theo dõi chi phí và quản lý hệ thống.
* Nắm vững Mô hình Trách nhiệm Chung (Shared Responsibility Model): AWS lo phần cứng hạ tầng, trong khi team tự chịu trách nhiệm về code, IAM policies và bảo mật dữ liệu nhạy cảm.
* Xây dựng tư duy thiết kế hệ thống tương thích với Cloud (cloud-ready): mọi thông tin cấu hình nhạy cảm đều được bóc tách khỏi code, sẵn sàng để chuyển dịch sang AWS Secrets Manager hoặc Systems Manager (SSM).

Nhìn chung, tuần đầu tiên đã giúp định hình tư duy quản trị hạ tầng AWS một cách chuyên nghiệp trước khi tiến sâu vào từng dịch vụ riêng biệt.

### Nhiệm vụ dự kiến cho tuần tới

* **Backend**: Bắt đầu kết nối AWS Cognito - setup `SecurityConfig` hỗ trợ JWT resource server, phát triển bộ kiểm tra token `OAuth2TokenValidator` riêng, đồng thời hoàn thiện entity và controller cho `UserProfile`.
* **Frontend**: Phát triển tính năng Đăng nhập/Xác thực - làm `LoginScreen` áp dụng chuẩn PKCE OAuth qua thư viện `expo-auth-session`, lưu trữ token an toàn với `expo-secure-store`, và kiểm soát trạng thái đăng nhập qua `authSlice`.