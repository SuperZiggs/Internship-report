---
title: "Worklog Tuần 2"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* **Backend**: Tích hợp luồng xác thực bảo mật chuẩn Enterprise sử dụng **Amazon Cognito User Pools** kết hợp cùng Spring Security. Khởi tạo và xây dựng module `UserProfile`.
* **Frontend**: Triển khai toàn diện luồng xác thực qua giao thức **PKCE (Proof Key for Code Exchange)** — bao trùm từ bước hiển thị màn hình đăng nhập, lưu trữ token an toàn đến việc quản lý trạng thái (state) người dùng.
* Thiết lập cơ chế (pattern) quản lý vòng đời của token một cách tối ưu, bao gồm khả năng tự động làm mới (auto-refresh) và xử lý triệt để các tình huống thu hồi quyền truy cập (Token Revocation).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Nghiên cứu chuyên sâu về AWS Cognito User Pools <br>&emsp; + Thiết lập User Pool: cấu hình chính sách mật khẩu (password policy), app clients và chuẩn PKCE <br>&emsp; + Phân tích và làm rõ sự khác biệt giữa ID Token và Access Token <br>&emsp; + Tìm hiểu các JWT claims quan trọng: `sub`, `cognito:groups`, `token_use` | 13/01/2026 | 13/01/2026 | <https://docs.aws.amazon.com/cognito/> |
| 3   | - Tích hợp và cấu hình JWT cho **Spring Security** <br>&emsp; + Bổ sung thư viện `spring-security-oauth2-resource-server` <br>&emsp; + Thiết lập `SecurityConfig`: áp dụng cơ chế stateless, vô hiệu hóa CSRF, kích hoạt CORS <br>&emsp; + Khai báo Cognito issuer-uri tại file `application.properties` <br>&emsp; + Tùy biến `OAuth2TokenValidator` nhằm từ chối nghiêm ngặt các token có `token_use != "access"` | 14/01/2026 | 14/01/2026 | <https://docs.spring.io/spring-security/> |
| 3   | - Phát triển cơ chế trích xuất phân quyền (role) từ JWT <br>&emsp; + Đọc dữ liệu claim `cognito:groups` và ánh xạ sang định dạng `ROLE_<GROUP>` <br>&emsp; + Áp dụng `@PreAuthorize("hasRole('ADMIN')")` để bảo vệ các admin endpoints <br>&emsp; + Chuẩn hóa quy tắc phân quyền: khu vực public, yêu cầu xác thực (authenticated) và dành riêng cho quản trị viên (admin-only) | 14/01/2026 | 14/01/2026 | |
| 4   | - Thiết kế **UserProfile** entity và repository <br>&emsp; + Định nghĩa các trường dữ liệu: `cognitoId (UNIQUE)`, `email (UNIQUE)`, `username`, `name`, `gender`, `birthdate`, `phoneNumber`, `picture`, `emailVerified` <br>&emsp; + Khởi tạo `UserProfileRepository` mở rộng từ interface `JpaRepository` | 15/01/2026 | 15/01/2026 | |
| 4   | - Phát triển **UserProfileService** và **UserProfileController** <br>&emsp; + API `POST /user/sync` — thực hiện upsert profile dựa trên Cognito claims (đảm bảo an toàn trước lỗi IDOR bằng cách dùng `cognitoSub` từ JWT `sub`) <br>&emsp; + Cung cấp các API chuẩn: `GET /user/{id}`, `PUT /user/{id}`, `DELETE /user/{id}` | 15/01/2026 | 15/01/2026 | |
| 5   | - Thiết kế giao diện **LoginScreen** trên Frontend <br>&emsp; + Bố trí nút "Đăng nhập với AWS Cognito" làm phương thức xác thực duy nhất <br>&emsp; + Kích hoạt luồng PKCE thông qua sự kết hợp giữa `expo-auth-session` và `expo-web-browser` <br>&emsp; + Bắt và xử lý redirect callback: quy đổi mã code lấy token <br>&emsp; + Sử dụng thư viện `jwt-decode` để giải mã ID Token, phục vụ việc trích xuất thông tin người dùng | 16/01/2026 | 16/01/2026 | <https://docs.expo.dev/guides/authentication/> |
| 6   | - Triển khai **authSlice** (Redux Toolkit) và cơ chế lưu trữ token <br>&emsp; + Cấu trúc State: `isAuthenticated`, `user`, `token`, `refreshToken`, `hasCompletedOnboarding` <br>&emsp; + Khai báo Actions: `login`, `logout`, `completeOnboarding`, `updateUserProfile` <br>&emsp; + Quản lý lưu trữ token linh hoạt: sử dụng `expo-secure-store` cho mobile và `localStorage` cho web thông qua module `utils/storage.ts` <br> - Thiết lập **request interceptor** trong Axios: tự động đính kèm header `Authorization: Bearer <token>` vào mọi truy vấn | 17/01/2026 | 17/01/2026 | |

### Kết quả đạt được tuần 2:

* **Backend — Bảo mật (Security)**:
  * Hoàn thiện cấu hình Spring Security tích hợp **Amazon Cognito** dưới vai trò JWT issuer, vận hành trơn tru cơ chế phân quyền RBAC theo tiêu chuẩn doanh nghiệp.
  * Triển khai thành công `OAuth2TokenValidator` tùy biến để ngăn chặn việc sử dụng ID Token sai mục đích — hệ thống API hiện chỉ chấp thuận Access Token.
  * Tính năng phân quyền theo role hoạt động chính xác: claim `cognito:groups` được ánh xạ mượt mà thành `ROLE_ADMIN`, đảm bảo cấp quyền quản trị đúng đối tượng.
  * Thiết lập chặt chẽ các ranh giới bảo mật: phân định rõ vùng public cho health check, vùng yêu cầu xác thực chung và khu vực `/admin/**` chỉ dành cho quản trị viên.
* **Backend — Phân hệ UserProfile**:
  * Endpoint `POST /user/sync` thực hiện upsert dữ liệu người dùng từ Cognito JWT claims một cách an toàn, vá triệt để rủi ro liên quan đến lỗ hổng IDOR.
  * Cung cấp đầy đủ bộ thao tác CRUD (`GET`, `PUT`, `DELETE`) thông qua đường dẫn `/user/{id}`, kèm theo các lớp kiểm tra phân quyền nghiêm ngặt.
  * Thực thể `UserProfile` đã được ánh xạ và lưu trữ thành công vào cơ sở dữ liệu PostgreSQL thông qua JPA.
* **Frontend — Luồng xác thực**:
  * Giao diện `LoginScreen` render chuẩn xác; thao tác nhấn nút kích hoạt thành công quy trình mở Cognito Hosted UI an toàn trên trình duyệt mặc định của hệ thống.
  * Cơ chế **PKCE code exchange** vận hành trơn tru từ đầu đến cuối (end-to-end) nhờ `expo-auth-session`, giúp loại bỏ hoàn toàn rủi ro bảo mật do việc phải lưu trữ Secret Key tại client.
  * Module `authSlice` cập nhật trạng thái `isAuthenticated` linh hoạt, hỗ trợ `RootNavigator` điều hướng chính xác giữa các luồng giao diện (stack).
  * Axios interceptor tự động đính kèm Bearer token vào header và xử lý mượt mà việc **quản lý vòng đời Token** (hỗ trợ làm mới ngầm tự động và bắt buộc đăng xuất khi token không còn hiệu lực).

### Kiến thức AWS đã học:

* Đào sâu vào kiến trúc thiết lập của Cognito User Pool, bao gồm: cấu hình app client, các URL cho callback và logout, tối ưu hóa OAuth scope, cũng như chiến lược thiết lập thời hạn token sao cho tối ưu nhất đối với môi trường ứng dụng mobile.
* Nắm vững toàn bộ chu trình hoạt động của giao thức PKCE (từ `code_verifier`, `code_challenge` đến thuật toán băm `S256`), đồng thời hiểu rõ tính tất yếu của cơ chế này khi triển khai trên các public client.
* Phân định rạch ròi vai trò của ID Token và Access Token khi ứng dụng vào API thực tế, thấm nhuần nguyên tắc cốt lõi: Backend chỉ sử dụng Access Token để cấp quyền (authorize).
* Nắm bắt trọn vẹn quy trình thẩm định (validate) JWT, từ việc đối chiếu các claims tiêu chuẩn như `iss`, `aud`, `exp`, `nbf`, `token_use` cho đến thao tác xác minh chữ ký JWK cung cấp bởi Cognito.
* Thành thạo kỹ thuật ánh xạ (map) claim `cognito:groups` sang các phân quyền nội bộ (ví dụ: `ROLE_ADMIN`, `ROLE_USER`), làm tiền đề vững chắc cho việc áp dụng mô hình bảo mật RBAC.
* Hiểu sâu về nghệ thuật quản lý refresh token trên nền tảng di động: từ việc lưu trữ bảo mật, cơ chế xoay vòng token (rotation), đến chiến lược xử lý đăng xuất bắt buộc (force logout) khi quá trình làm mới gặp sự cố.
* Khẳng định và củng cố nguyên tắc thiết kế bất di bất dịch: Sử dụng claim `sub` làm định danh độc nhất và không đổi của người dùng trên toàn bộ các module thuộc Backend.

Tóm lại, tuần 2 đã giúp liên kết chặt chẽ các kiến thức nền tảng về AWS Identity với tư duy xây dựng hệ thống authentication và authorization thực chiến cho dự án.

### Kế hoạch tuần tiếp theo:

* **Backend**: Tập trung xây dựng các lớp nền tảng vững chắc cho hệ thống, bao gồm `GlobalExceptionHandler` để quản lý lỗi tập trung và `CorsConfig`. Bắt tay vào thiết kế và phát triển module `GoalType`.
* **Frontend**: Xây dựng bộ khung điều hướng (navigation) hoàn chỉnh cho ứng dụng, bao gồm `RootNavigator`, `AuthStack`, hệ thống `MainTabs` tích hợp thanh điều hướng tùy chỉnh (custom tab bar), và luồng giới thiệu `OnboardingStack`.