---
title: "Cloud Mastery 2026 #1 AI From Scratch"
date: 2026-03-14
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch Sự kiện: "Ứng dụng AI Agent, Prompt Engineering và AIoT trên AWS"

### Mục Đích Của Sự Kiện

- Phân tích những hạn chế của các Mô hình Ngôn ngữ Lớn (LLM) độc lập và tiếp cận giải pháp khắc phục triệt để thông qua kiến trúc AI Agent.
- Làm chủ nghệ thuật giao tiếp với AI thông qua các kỹ thuật Prompt Engineering tiêu chuẩn, nhằm tối ưu hóa chi phí token và nâng cao chất lượng phản hồi.
- Khám phá tiềm năng ứng dụng thực tiễn của hệ sinh thái AIoT khi tích hợp liền mạch với các dịch vụ Cloud từ AWS (điển hình như IoT Core và Amazon Rekognition).

### Danh Sách Diễn Giả

- **Bành Cẩm Vinh** - Diễn giả chủ đề: Kiến tạo AI Agent với framework Strands
- **Nguyễn Tuấn Thịnh** - DevOps Engineer, Diễn giả chủ đề: Tự động hóa Prompt Engineering (Automated Prompt Engineering)
- **Aiden Dinh & Trần Vũ Bảo Ngọc** - Operation Engineers tại Katalon, Diễn giả chủ đề: Ứng dụng thực tiễn các dự án AIoT

### Nội Dung Nổi Bật

#### Xây dựng AI Agent với Strands

Các Mô hình Ngôn ngữ Lớn (LLM) độc lập thường bộc lộ điểm yếu do thiếu vắng dữ liệu thời gian thực và không có khả năng tương tác trực tiếp với các hệ thống ngoại vi. Kiến trúc AI Agent ra đời nhằm giải quyết triệt để rào cản này với các năng lực cốt lõi:
- **Lập luận đa bước (Multi-step reasoning):** Khả năng tự động lập kế hoạch và thực thi các chuỗi luồng công việc (workflows) phức tạp.
- **Tích hợp công cụ (Tool integration):** Cấp quyền cho AI chủ động truy xuất API, truy vấn cơ sở dữ liệu và giao tiếp với các dịch vụ bên ngoài.
- Ứng dụng framework **Strands Agents** vận hành dựa trên cơ chế **Agentic Loop** (vòng lặp thực thi công cụ), kết hợp chặt chẽ giữa System Prompts và Knowledge Base để tự chủ đưa ra quyết định và phản hồi linh hoạt theo sự thay đổi của ngữ cảnh.

![Mô hình hoạt động của AI Agent](/images/4-Event/Agent.png "AI Agent Architecture")

#### Kỹ thuật Prompt Engineering Tự động

Giao tiếp hiệu quả với AI là một nghệ thuật đòi hỏi tính chuẩn xác cao. Việc sử dụng các câu lệnh (prompt) mơ hồ không chỉ đem lại kết quả kém chất lượng mà còn gây lãng phí tài nguyên (token) và thiếu đi sự nhất quán.
Một cấu trúc Prompt tiêu chuẩn cần hội tụ đủ 7 thành phần thiết yếu:
1. **Role** (Định danh vai trò chuyên gia cho AI)
2. **Instruction** (Chỉ thị nhiệm vụ rõ ràng và tường minh)
3. **Context** (Cung cấp ngữ cảnh nền tảng)
4. **Input Data** (Dữ liệu đầu vào cần xử lý)
5. **Output Format** (Quy định chặt chẽ định dạng dữ liệu đầu ra)
6. **Examples** (Cung cấp ví dụ mẫu theo phương pháp Few-shot)
7. **Constraints** (Thiết lập các rào chắn và giới hạn bắt buộc tuân thủ)

Kiến trúc hệ thống quản lý Prompt tối ưu trên AWS được thiết kế bao gồm: **Amazon DynamoDB** (lưu trữ metadata với độ trễ phản hồi tính bằng mili-giây), **Amazon CloudWatch** (giám sát toàn diện log, độ trễ và tỷ lệ lỗi), kết hợp cùng nền tảng lõi **Amazon Bedrock**.

![Cấu trúc Prompt và Kiến trúc AWS](/images/4-Event/prompt.png "Prompt Engineering & AWS")

#### Dự án AIoT: Quản lý Tủ đồ Thông minh (Smart Locker)

Số hóa và tự động hóa quy trình mượn trả đồ thủ công tại các câu lạc bộ thông qua hệ thống tủ đồ thông minh:
- **Hạ tầng phần cứng (Hardware):** Triển khai Raspberry Pi đóng vai trò là Controller và MQTT Broker nội bộ; sử dụng Arduino để thu thập tín hiệu cảm biến; kết hợp đồng bộ các module Reed Switch, RFID Card Reader và Camera giám sát.
- **Tích hợp kiến trúc AWS Cloud:** - **AWS IoT Core:** Đóng vai trò là hub trung tâm, định tuyến các sự kiện cảm biến (quét thẻ RFID, tín hiệu đóng/mở cửa) đến AWS Lambda và lưu trữ tại DynamoDB, đảm bảo khả năng mở rộng đàn hồi mà không phụ thuộc vào server cục bộ.
  - **Amazon Rekognition:** Ứng dụng Computer Vision để phân tích hình ảnh, đối chiếu khuôn mặt người mượn với cơ sở dữ liệu thành viên nhằm cấp quyền truy cập bảo mật.

![Sơ đồ kiến trúc phần cứng và AWS cho dự án AIoT](/images/4-Event/AIot.png "AIoT Smart Locker Architecture")

#### Một số hình ảnh khi tham gia sự kiện

![Ảnh check-in sự kiện](/images/4-Event/event1a.jpg "Check-in")
![Ảnh chụp cùng diễn giả](/images/4-Event/event1b.jpg "Chụp cùng diễn giả")
![Ảnh chụp cùng diễn giả](/images/4-Event/event2c.jpg "Chụp cùng diễn giả")

> Tổng thể, sự kiện không chỉ cung cấp lăng kính sâu sắc về các xu hướng công nghệ AI tiên phong như Agentic Systems và Prompt Engineering, mà còn mang đến những bài học thực chiến giá trị về phương pháp hội tụ phần cứng IoT với hạ tầng cloud linh hoạt của AWS nhằm giải quyết triệt để các bài toán thực tế.