---
title: "Worklog Tuần 8"
date: 2026-03-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* **Backend**: Khởi tạo và phát triển module (module) Food & Nutrition, đồng thời chuẩn bị nền tảng dữ liệu (data layer) đóng vai trò làm ngữ cảnh động (dynamic context) cho việc tích hợp **Amazon Bedrock AI Coach** trong giai đoạn sắp tới.
* **Frontend**: Xây dựng giao diện `DietScreen`, ứng dụng logic `ensureDailyMeals` nhằm tự động hóa và đảm bảo tính nhất quán của cấu trúc 4 bữa ăn tiêu chuẩn mỗi ngày.
* Triển khai thử nghiệm và tinh chỉnh các kỹ thuật Prompt Engineering chuyên sâu, phục vụ mục đích phân tích dữ liệu dinh dưỡng và đưa ra lộ trình thể hình cá nhân hóa.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Khởi tạo thực thể **Food** (ánh xạ bảng `food`) <br>&emsp; + Định nghĩa schema: `name`, `caloriesPer100g`, `proteinPer100g`, `carbsPer100g`, `fatsPer100g`, `unit` <br>&emsp; + Triển khai `FoodController`: hỗ trợ đầy đủ thao tác CRUD, kết hợp phân trang (pagination) và tìm kiếm động theo từ khóa | 03/03/2026 | 03/03/2026 | |
| 3   | - Thiết kế thực thể **Meal** (bảng `meal`) <br>&emsp; + Thiết lập mapping: `userProfile (@ManyToOne)`, `date (LocalDateTime)`, `mealType (BREAKFAST/LUNCH/SNACK/DINNER)`, `note` <br>&emsp; + Xây dựng `MealController`: cung cấp API khởi tạo, truy xuất theo ID, và bộ lọc linh hoạt theo ngày hoặc loại bữa ăn | 04/03/2026 | 04/03/2026 | |
| 4   | - Triển khai thực thể trung gian **MealFood** (bảng `meal_food`) <br>&emsp; + Ánh xạ dữ liệu: `meal`, `food`, `quantity (gram)` <br>&emsp; + Thiết lập logic tự động tính toán macro (`calories`, `protein`, `carbs`, `fats`) khi khởi tạo dựa trên công thức: `quantity / 100 * per100gValue` <br>&emsp; + Phát triển `MealFoodController`: hỗ trợ luồng thao tác thêm, liệt kê và xóa thực phẩm khỏi bữa ăn <br> - Khởi tạo thực thể **DailyNutrition** và `DailyNutritionController`: tự động tổng hợp và lưu trữ chỉ số dinh dưỡng theo ngày | 05/03/2026 | 05/03/2026 | |
| 5   | - Phát triển giao diện **DietScreen** trên Frontend <br>&emsp; + Render danh sách 4 bữa ăn trong ngày dựa trên cơ chế `ensureDailyMeals` <br>&emsp; + Chi tiết hóa từng bữa: hiển thị danh mục thực phẩm, cột tổng calo và thanh tiến độ (progress bar) <br>&emsp; + Triển khai Modal "Thêm thực phẩm": tích hợp công cụ tìm kiếm, nhập định lượng (gram) và dispatch action `addFoodToMeal` <br>&emsp; + Bổ sung thanh tiến độ (progress bar) phản ánh tổng calo toàn ngày nhằm tối ưu UX <br>&emsp; + Tích hợp cơ chế làm mới dữ liệu (Pull-to-refresh) | 06/03/2026 | 06/03/2026 | |
| 6   | - Phát triển màn hình **DietHistoryScreen** trên Frontend <br>&emsp; + Trình bày lịch tháng trực quan — hỗ trợ nhấn chọn ngày để truy xuất lịch sử bữa ăn tương ứng <br>&emsp; + Liệt kê chi tiết danh mục thực phẩm và tổng lượng calo tương ứng theo từng bữa <br> - Thực thi kiểm thử (test) toàn trình (end-to-end) cho luồng quản lý và theo dõi dinh dưỡng | 07/03/2026 | 07/03/2026 | |

### Kết quả đạt được tuần 8:

* **Backend — module Food & Nutrition**:
  * Thực thể `Food` đã được seed thành công khối lượng lớn dữ liệu thực phẩm thông qua crawler (đạt quy mô hơn 100 món ăn chuẩn).
  * Logic nghiệp vụ tại `MealFood` vận hành hoàn hảo, tự động tính toán chính xác các chỉ số macro ngay khi insert dữ liệu.
  * Cơ chế phân trang tại endpoint `GET /api/foods` hoạt động mượt mà, chuẩn hóa định dạng trả về qua lớp wrapper `PageResponse<T>`.
  * Truy vấn tìm kiếm từ khóa (case-insensitive LIKE query) phản hồi nhanh chóng và chuẩn xác.
* **Frontend — Theo dõi dinh dưỡng**:
  * Giao diện `DietScreen` đảm bảo tự động khởi tạo đủ 4 bữa ăn tiêu chuẩn cho ngày hiện tại.
  * Modal tìm kiếm thực phẩm tích hợp mượt mà cơ chế phân trang (pagination) nhằm tối ưu hiệu năng hiển thị (render).
  * Luồng tương tác thêm thực phẩm: backend tính toán và trả về macro, UI lập tức đồng bộ và cập nhật lại thông số hiển thị mà không có độ trễ.
  * `DietHistoryScreen` truy xuất và kết xuất (render) chính xác dữ liệu lịch sử dinh dưỡng dựa trên thao tác chọn ngày của người dùng.

### Kiến thức AWS đã học:

* Nắm bắt chi tiết toàn bộ luồng giao tiếp với **Amazon Bedrock Runtime**: từ việc định dạng payload request, lựa chọn model ID phù hợp, cho đến kỹ thuật bóc tách (parse) response một cách ổn định và an toàn.
* Phân tích sâu bài toán đánh đổi (trade-off) khi lựa chọn AI model dựa trên ba trục chính: chất lượng sinh văn bản, độ trễ (latency), và chi phí token, được đặt trong bài toán đặc thù của một fitness coach.
* Nâng tầm kỹ năng Prompt Engineering bằng việc áp dụng kiến trúc prompt chuyên nghiệp: thiết lập role instruction, vạch rõ ranh giới ngữ cảnh (boundary) và ép chuẩn định dạng đầu ra (output format) để duy trì tính nhất quán.
* Hình thành tư duy lập trình phòng thủ (defensive programming) khi tích hợp hệ thống AI: kiểm soát chặt chẽ context đầu vào, nhận diện các rủi ro prompt injection cơ bản, đồng thời thiết lập hệ thống fallback message an toàn khi model gặp sự cố.
* Áp dụng các chiến lược phục hồi lỗi tiêu chuẩn như retry, exponential backoff và quản trị timeout budget nhằm duy trì UX của giao diện chat không bị gián đoạn trước các lỗi tạm thời (transient errors) từ cloud.
* Nhận thức rõ vai trò của việc giám sát Service Quotas và quản trị chi phí AI — xem đây là một cấu phần thiết yếu trong khâu vận hành thay vì chỉ là tác vụ theo dõi hóa đơn (billing).
* Xác định hệ thống chỉ số đo lường (metrics) cốt lõi dành riêng cho tính năng AI: tỷ lệ phản hồi thành công (success rate), độ trễ truy vấn (latency), và thống kê phân loại lỗi.

Nhìn chung, tuần 8 đã mở rộng phổ kiến thức AWS từ hạ tầng cơ bản sang việc vận hành các dịch vụ Managed AI, đồng thời giải quyết các thách thức thực tiễn khi đưa AI vào môi trường sản phẩm thực tế.

### Kế hoạch tuần tiếp theo:

* **Backend**: Khởi tạo và phát triển module User Metric — tập trung vào các thực thể `BodyMetric` và `HealthCalculation` tích hợp thuật toán xử lý các chỉ số sinh lý cốt lõi như BMI, BMR và TDEE.
* **Frontend**: Triển khai hệ sinh thái giao diện quản lý sức khỏe, bao gồm `HealthDashboardScreen` (trang tổng quan), `BodyMetricListScreen` (danh sách chỉ số), `BodyMetricFormScreen` (form cập nhật) cùng hệ thống biểu đồ (charts) trực quan hóa dữ liệu lịch sử.