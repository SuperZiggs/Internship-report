---
title: "Cloud Mastery 2026 #2 - Kubernetes, IaC và Elixir trong DevOps"
date: 2026-04-04
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

## Báo cáo tổng kết: "Cloud Mastery 2026 #2"

## Mục tiêu sự kiện

* Xây dựng nền tảng tri thức thực tiễn vững chắc về kiến trúc Kubernetes và các phương pháp vận hành (operations) thường nhật.
* Làm chủ các phương pháp luận Infrastructure as Code (IaC) trên nền tảng AWS thông qua AWS CloudFormation, AWS CDK và Terraform.
* Khám phá sức mạnh của Elixir và hệ sinh thái OTP như một phương pháp tiếp cận tối ưu để kiến tạo các hệ thống DevOps đòi hỏi tính đồng thời (high concurrency) và khả năng chịu lỗi (fault tolerance) vượt trội.

## Diễn giả

* **Bao Huynh** - Phiên: Thiết kế kiến trúc Cloud với Kubernetes
* **Nguyen Ta Minh Triet** - Phiên: Elixir như một giải pháp thống nhất cho hạ tầng DevOps có tính đồng thời cao và chịu lỗi
* **Khanh Phuc Thinh Nguyen** - Phiên: Infrastructure as Code với Terraform trên AWS


## Điểm nổi bật chính

### Phiên 1: Thiết kế kiến trúc Cloud với Kubernetes

Trọng tâm của phiên mở màn là mổ xẻ những thách thức cốt lõi trong việc điều phối container (container orchestration), đồng thời làm rõ lý do Kubernetes vươn lên thành tiêu chuẩn công nghiệp (de facto standard) cho việc vận hành ứng dụng container ở quy mô doanh nghiệp.

Các ý chính:
* **Kiến trúc cốt lõi:** Các thành phần Control Plane (etcd, API Server, Scheduler, Controller Manager, **Cloud Controller Manager**) và hệ sinh thái Worker Node (kubelet, kube-proxy, container runtime).
* **Các đối tượng quan trọng:** Làm chủ các object trọng yếu: Pods, ReplicaSets, Deployments, ConfigMaps, Secrets và Jobs.
* **Vận hành cơ bản:** Thực hành vận hành thông qua Kubernetes manifests (cú pháp YAML) và bộ lệnh `kubectl` tiêu chuẩn.
* **Lộ trình học:** Khởi động tại local environment với Minikube, K3s hoặc K3d, tiến tới môi trường Managed Kubernetes như Amazon EKS **(nơi control plane được AWS quản lý toàn diện, giảm thiểu tối đa overhead cấu hình)**. Để thấu hiểu cơ chế bên dưới, tài liệu **"Kubernetes the Hard Way" của Kelsey Hightower** được đặc biệt đề xuất.
* **Công cụ hệ sinh thái:** Ứng dụng Helm để đóng gói/triển khai ứng dụng và K9s để giám sát, điều khiển cluster trực tiếp từ giao diện terminal.

Phiên trình bày đã tạo cầu nối hoàn hảo giữa lý thuyết kiến trúc hàn lâm và lộ trình thực chiến, sẵn sàng áp dụng từ các dự án cá nhân (pet projects) đến môi trường production phức tạp.

![k8s architecture](/images/4-Event/e3k8s.png "k8s Architecture")
![actual image](/images/4-Event/ws2k8s.jpg "actual image")

---

### Phiên 2: Elixir cho hệ thống DevOps đồng thời cao và chịu lỗi

Phiên chuyên đề thứ hai đi sâu vào ngôn ngữ Elixir cùng máy ảo BEAM — giải pháp kiến trúc tối ưu để xây dựng các nền tảng backend đòi hỏi tính sẵn sàng cao và năng lực xử lý đồng thời (concurrency) quy mô lớn.

Các ý chính:
* **Kiến thức nền tảng Elixir:** Triết lý thiết kế dựa trên lập trình hàm (Functional Programming), cấu trúc dữ liệu bất biến (immutable data), pattern matching và kế thừa sức mạnh từ Erlang/BEAM. **Elixir sở hữu cú pháp thanh lịch lấy cảm hứng từ Ruby, nhưng được biên dịch thành bytecode vận hành trên BEAM VM — cơ chế tương tự như Java.**
* **Mô hình đồng thời:** Tận dụng các tiến trình siêu nhẹ (lightweight processes) của BEAM kết hợp cùng bộ scheduler thông minh giúp hệ thống mở rộng linh hoạt. **Một minh chứng kinh điển là Phoenix Framework — có năng lực duy trì và xử lý tới 2 triệu kết nối WebSocket đồng thời trên một máy chủ duy nhất.**
* **Khả năng chịu lỗi với OTP:** Kiến trúc chịu lỗi (Fault tolerance) với cơ chế giám sát tiến trình (supervision trees), triết lý "Let It Crash" trứ danh và khả năng tự phục hồi (self-healing) ưu việt ngay khi runtime xảy ra sự cố.
* **Lợi ích vận hành:** Tích hợp bộ công cụ mạnh mẽ **(như Mix và IEx)** phục vụ từ khâu phát triển đến vận hành, nổi bật với **tính năng nâng cấp mã nguồn nóng (hot code upgrade) cho phép triển khai không gián đoạn (zero-downtime).**
* **Tác động thực tế:** Các case study chứng minh hiệu quả tối ưu chi phí đột phá khi dịch chuyển các workload có throughput cao từ kiến trúc serverless sang Elixir **(ví dụ thực tế: quá trình viết lại một dịch vụ AWS API Gateway/Lambda Node.js sang Elixir đã thu gọn chi phí hạ tầng từ mức hơn 12.000 USD/tháng xuống vỏn vẹn dưới 400 USD/tháng).**

Bài trình bày đã phá vỡ những ranh giới tư duy thông thường của các DevOps stack phổ biến, khẳng định rằng quyết định lựa chọn runtime/language có tác động trực diện và sâu sắc đến cả độ tin cậy lẫn ngân sách hạ tầng.

![benefits](/images/4-Event/e3benefit.png)
![actual image](/images/4-Event/ws2elixir.jpg "actual image")

---

### Phiên 3: Infrastructure as Code với Terraform trên AWS

Phiên bế mạc xoáy sâu vào triết lý Infrastructure as Code (IaC) — giải pháp hiện đại thay thế hoàn toàn phương pháp cấu hình đám mây thủ công (ClickOps) nhiều rủi ro, qua đó đề cao tính tự động hóa, sự nhất quán (consistency) và khả năng tái lập (reproducibility) của hạ tầng.

Các ý chính:
* **Tư duy IaC:** Quy chuẩn hóa hạ tầng Cloud dưới dạng mã nguồn nhằm triệt tiêu các lỗi thao tác của con người và thúc đẩy văn hóa cộng tác.
* **Kiến thức cơ bản về CloudFormation:** Cốt lõi của CFN bao gồm Templates, stacks, cấu trúc template **(như Parameters, Mappings, Conditions, Outputs)**, và cơ chế phát hiện sai lệch (Drift Detection). **Đặc biệt, nhiều Managed Services như AWS Amplify cũng ngầm định sử dụng CloudFormation như một engine triển khai.**
* **Khái niệm AWS CDK:** Khám phá các cấp độ trừu tượng hóa Construct (L1, L2, L3), hệ thống Construct Tree và quy trình triển khai qua CDK CLI. **Lợi thế tuyệt đối của CDK nằm ở việc hỗ trợ khai báo hạ tầng bằng chính các ngôn ngữ lập trình đa năng phổ biến (TypeScript, Python, Java, C#/.Net và Go).**
* **Kiến thức cơ bản về Terraform:** Phân tích cú pháp HCL, chiến lược tổ chức cấu trúc dự án (từ cơ bản đến nâng cao), vòng đời thực thi (`init`, `validate`, `plan`, `apply`, `destroy`), nghệ thuật quản lý state, **và ưu thế vượt trội trong việc triển khai đa đám mây (multi-cloud).**
* **Tiêu chí chọn công cụ:** Định hình quyết định sử dụng IaC dựa trên chiến lược doanh nghiệp (single hay multi-cloud), năng lực lõi của đội ngũ và mức độ tương thích hệ sinh thái. **Một số giải pháp thay thế tiềm năng như OpenTofu và Pulumi cũng được đưa ra bàn luận.**

Phiên chia sẻ đã phác họa bức tranh so sánh sắc nét giữa các công cụ IaC bản địa của AWS (native) và các giải pháp đa đám mây, trang bị cho kỹ sư tư duy ra quyết định chính xác trong các dự án thực tế.

![alt text](/images/4-Event/e3cf.png)


---

## Kết quả và giá trị đạt được

Khép lại sự kiện, bản thân tôi đã định hình được một lăng kính toàn cảnh và có tính hệ thống cao về kỷ nguyên Cloud Engineering hiện đại:
* Làm chủ phương pháp thiết kế kiến trúc và vận hành hệ thống container phân tán với Kubernetes.
* Xây dựng tư duy kiến trúc hướng tới khả năng chịu lỗi và năng lực xử lý đồng thời cực cao tại tầng runtime thông qua Elixir/OTP.
* Quy chuẩn hóa vòng đời quản lý hạ tầng (infrastructure lifecycle) bằng các công cụ IaC tiêu chuẩn trên nền tảng AWS cũng như kiến trúc Multi-cloud.

![actual image](/images/4-Event/ws2team.jpg "actual image")

Sự kiện cũng giúp làm rõ một lộ trình thực tế: bắt đầu với Kubernetes cục bộ, áp dụng IaC một cách nhất quán cho việc triển khai, và cân nhắc các công nghệ runtime có tính chịu lỗi cao như Elixir khi xây dựng các dịch vụ có mức độ đồng thời lớn.

> Nhìn chung, Cloud Mastery 2026 #2 mang lại sự cân bằng tốt giữa nguyên lý kiến trúc, thực hành triển khai và các đánh đổi kỹ thuật trong thực tế xoay quanh Kubernetes, IaC và các hệ thống backend có độ bền cao.