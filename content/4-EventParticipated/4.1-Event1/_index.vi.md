---
title: "Tổng kết AWS re:Invent 2025 - Phiên bản Việt Nam"
date: 2026-01-27
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Báo cáo tổng kết: "AWS re:Invent 2025 Recap - Vietnam Edition"

### Mục tiêu sự kiện

- Tổng hợp và phân tích chuyên sâu các công bố chiến lược cùng những bước đột phá công nghệ nổi bật nhất từ AWS re:Invent 2025.
- Cập nhật nhanh chóng cho cộng đồng kỹ sư phần mềm Việt Nam về các dịch vụ AWS tân tiến và các hệ chuẩn (paradigms) kiến trúc Cloud hiện đại.
- Cung cấp lăng kính đa chiều về các công nghệ Cloud tiên phong, bao trùm Generative AI, hệ thống Agentic AI tự trị và kiến trúc nền tảng dữ liệu thế hệ mới.

### Các điểm nổi bật chính

#### Generative AI và Agentic Systems trên AWS

Điểm nhấn cốt lõi của sự kiện xoay quanh bước tiến vượt bậc của **Generative AI và các hệ thống AI Agent tự trị** (Autonomous Agents) trên hệ sinh thái AWS.  
Khán giả đã được tiếp cận sâu với **Amazon Bedrock** cùng **họ mô hình Amazon Nova** — cung cấp tập hợp các Foundation Models (FMs) dạng Managed Service, trao quyền cho các kỹ sư tích hợp AI vào ứng dụng một cách liền mạch.

Phiên trình bày đặc biệt xoáy sâu vào **Bedrock Agents**, một framework kiến trúc chuyên biệt để kiến tạo các hệ thống thông minh có khả năng tự động thực thi các luồng công việc (workflows) phức tạp. Các Agent này được trang bị các năng lực cốt lõi:

- **Orchestration và Flow Management** để điều phối linh hoạt các chuỗi tác vụ (multi-step tasks) phức hợp.
- **Contextual Memory System** cho phép lưu trữ và duy trì tính liền mạch của ngữ cảnh hội thoại xuyên suốt các phiên tương tác.
- **Policies và Guardrails** tích hợp các rào chắn bảo mật nhằm kiểm soát chặt chẽ và đảm bảo tính an toàn cho các hành vi sinh văn bản của AI.
- **Evaluation Tooling** cung cấp cơ sở đo lường và đánh giá hiệu năng (benchmarking), liên tục tối ưu hóa độ phản hồi của agent.

Sự hội tụ của các thành phần này tạo bệ phóng vững chắc để các developer kiến tạo những ứng dụng AI thế hệ mới, có khả năng tự động gọi (invoke) các công cụ ngoại vi và tương tác trực tiếp với các kho dữ liệu doanh nghiệp.

---

#### SageMaker Unified Studio và các cải tiến của S3

Một bước tiến mang tính chiến lược khác là sự tiến hóa của **hệ sinh thái phát triển Machine Learning (ML) trên AWS**.  
Việc ra mắt **SageMaker Unified Studio** mang đến một không gian làm việc (workspace) hợp nhất, cho phép **Data Engineers, Data Scientists và AI Engineers** cộng tác mượt mà trong cùng một môi trường IDE đồng nhất.

Song song với đó, những bước nhảy vọt về mặt kiến trúc của **Amazon S3** cũng được công bố:

- **S3 Tables** tối ưu hóa việc lưu trữ dữ liệu thô (raw data) dưới chuẩn định dạng bảng **Apache Iceberg**, qua đó tinh gọn và tăng tốc độ xử lý cho các luồng phân tích dữ liệu (analytics workflows) quy mô lớn.
- **S3 Vector** cung cấp năng lực lưu trữ vector bản địa (native), tối ưu hóa cấu trúc chi phí trong việc quản lý và truy xuất embeddings so với các giải pháp Vector Database truyền thống.

Những bản cập nhật này tái khẳng định tham vọng của AWS trong việc kiến tạo một Data Platform hội tụ, nơi ranh giới giữa lưu trữ, phân tích dữ liệu và huấn luyện AI bị xóa nhòa.

---

#### OpenSearch và Agentic Search

Sự kiện cũng phác họa những năng lực mới nhất của **OpenSearch Serverless**, đặc biệt nhấn mạnh vào khả năng liên kết liền mạch với các hệ sinh thái AI hiện đại.

Những khái niệm nền tảng được đề cập bao gồm:

- Tương thích hoàn toàn với chuẩn giao tiếp **Model Context Protocol (MCP)**.
- Triển khai **Agentic Memory**, trao quyền cho hệ thống AI lưu trữ, học hỏi và tái sử dụng tri thức thu thập được từ các phiên truy vấn (search sessions).
- Cơ chế vận hành các **Specialized Agents** (Agent chuyên biệt) được tinh chỉnh riêng cho những tác vụ phân tích dữ liệu sâu (deep data analysis).

Phần live-demo đã gây ấn tượng mạnh khi minh họa trực quan cách một **Flow Agent** có thể chủ động phân tích các tập dữ liệu bán hàng, tự động trích xuất các thông tin chuyên sâu (actionable insights) thông qua việc tương tác nhịp nhàng với engine tìm kiếm và các công cụ BI.

---

#### Advanced RAG và Multimodal AI

Vượt ra khỏi biên giới của các ứng dụng AI thuần văn bản (text-based), sự kiện đã khai mở kỷ nguyên mới của kiến trúc **Retrieval-Augmented Generation (RAG)** khi tiến lên cấp độ **đa phương thức (Multimodal)**.

Các công nghệ dẫn dắt xu hướng này bao gồm:

- **Nova Multimodal Embeddings**, đột phá trong việc vector hóa (vectorize) các định dạng media phức tạp như hình ảnh và video, đưa chúng vào không gian embedding chung.
- **Bedrock Data Automation**, tự động hóa quá trình trích xuất, phân loại và cấu trúc hóa (structure) thông tin từ các nguồn dữ liệu thô đa phương tiện (unstructured media data).

Sự chuyển mình này tháo gỡ rào cản định dạng, cho phép các hệ thống AI xử lý trực tiếp và suy luận chéo trên **hình ảnh, video và âm thanh**, mở ra không gian phát triển vô tận cho các ứng dụng AI thông minh.

---

#### Hạ tầng AI và chuyên sâu về SageMaker

Phiên chuyên đề cuối cùng bóc tách kiến trúc **hạ tầng (Infrastructure) lõi cần thiết để huấn luyện và vận hành các hệ thống AI ở quy mô siêu lớn (hyperscale)**.

Hệ sinh thái **Amazon SageMaker** chứng kiến sự bổ sung của các công cụ quyền lực:

- **SageMaker HyperPod**, kiến trúc cluster tối ưu hóa việc cung cấp và quản trị các cụm GPU khổng lồ, đảm bảo tính bền bỉ (resilience) cho các workload Deep Learning kéo dài.
- **SageMaker MLflow**, tích hợp chặt chẽ công cụ tracking thí nghiệm (experiment tracking) và quản trị vòng đời (lifecycle management) trọn vẹn cho các mô hình Machine Learning.
- **Bi-directional Streaming (Streaming hai chiều)**, mở khóa tiềm năng xây dựng các ứng dụng AI tương tác **Voice-to-Voice với độ trễ cực thấp (real-time)**.

Bộ giải pháp này là lời khẳng định mạnh mẽ của AWS trong việc xây dựng một nền tảng hạ tầng linh hoạt, đủ sức gánh vác các hệ thống AI thế hệ tiếp theo.

---

#### Một số hình ảnh tại sự kiện

![Check-in sự kiện](/images/4-Event/event2a.jpg "Check-in")
![Giao lưu với người tham dự](/images/4-Event/event2b.jpg "Networking")
![Anh su kien AWS re:Invent 3](/images/4-Event/event1c.jpg "AWS re:Invent 3")

> Nhìn chung, sự kiện đã mang lại cái nhìn tổng quan toàn diện về những thông báo quan trọng tại AWS re:Invent 2025, đặc biệt trong các lĩnh vực Generative AI, kiến trúc agent-based và nền tảng dữ liệu hiện đại. Các phiên chia sẻ cũng cung cấp nhiều insight thực tế về cách áp dụng các dịch vụ AWS vào các hệ thống thực tế như kiến trúc NutriTrack, đồng thời mang đến cơ hội để người tham dự kết nối trực tiếp với các AWS Solution Architect và cộng đồng cloud địa phương.