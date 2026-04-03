---
title: "Blog 6"
date: 2026-03-02
weight: 6
chapter: false
pre: " <b> 3.6. </b> "
---

# Hiểu về IAM cho các Managed AWS MCP Server

Khi các AI agent ngày càng được tích hợp sâu vào quy trình phát triển phần mềm, các tổ chức ngày càng cần một giải pháp để cho phép các agent này tương tác với tài nguyên AWS một cách an toàn và nhất quán. Trong thực tế, điều này có nghĩa là các AI agent nên có khả năng sử dụng cùng một mô hình phân quyền **AWS Identity and Access Management (IAM)** mà các developer đang tin dùng, thay vì buộc các team phải tạo và quản lý một hệ thống authorization (ủy quyền) riêng biệt chỉ dành cho các hoạt động do AI điều khiển. Đồng thời, các tổ chức cũng cần đủ quyền kiểm soát để phân biệt giữa các hành động do người dùng (human user) trực tiếp thực hiện và các hành động do AI agent khởi tạo thay mặt cho người dùng đó.

Trong bài đăng trên AWS Security Blog này, các tác giả giải thích cách AWS đang giải quyết nhu cầu đó đối với **các máy chủ Model Context Protocol (MCP) từ xa được AWS quản lý (AWS-managed remote MCP servers)**. Bài viết giới thiệu các IAM context key (khóa ngữ cảnh) tiêu chuẩn mới giúp các tổ chức áp dụng các policy quản trị khác nhau cho hoạt động AI dựa trên MCP, mô tả một mô hình authorization được đơn giản hóa sát hơn với hành vi của **AWS CLI** và **AWS SDK**, đồng thời hé lộ việc hỗ trợ **VPC endpoint** trong tương lai để bổ sung thêm các tính năng kiểm soát quyền riêng tư (private network) và bảo mật vành đai (perimeter-based controls).

---

## Tổng quan

AWS giải thích rằng tại **AWS re:Invent 2025**, công ty đã ra mắt bản preview của bốn AWS-managed remote MCP server: **AWS**, **EKS**, **ECS**, và **SageMaker**. Các MCP server này được AWS lưu trữ và quản lý, đồng nghĩa với việc khách hàng không cần tự cài đặt hay vận hành hạ tầng MCP server ở local. Thay vào đó, họ được hưởng lợi từ các bản cập nhật tự động, khả năng mở rộng (scalability), độ phục hồi (resiliency) tốt hơn, cũng như khả năng audit thông qua **AWS CloudTrail**.

Một ví dụ điển hình được nhấn mạnh trong bài viết là **AWS MCP Server**, có thể cung cấp quyền truy cập vào tài liệu AWS và thực thi lệnh gọi đến hơn **15.000 AWS API**. Điều này cho phép các AI agent hỗ trợ các tác vụ hạ tầng và vận hành gồm nhiều bước, chẳng hạn như provisioning tài nguyên VPC hoặc cấu hình cảnh báo **Amazon CloudWatch**.

Tuy nhiên, AWS lưu ý rằng khách hàng đã nêu ra hai lo ngại quan trọng khi AI agent bắt đầu đóng vai trò lớn hơn trong workflow phát triển. Thứ nhất, họ muốn các AI agent hoạt động với **các quyền IAM hiện có** của họ, thay vì yêu cầu một framework phân quyền riêng. Thứ hai, họ muốn có sự linh hoạt để áp dụng **các rào cản quản trị khác nhau** đối với hành động do AI điều khiển so với hành động do con người khởi tạo.

Để giải quyết những nhu cầu này, AWS đã giới thiệu hai IAM context key tiêu chuẩn mới cho các AWS-managed MCP server:

- **`aws:ViaAWSMCPService`**
- **`aws:CalledViaAWSMCP`**

Các context key này giúp xác định xem một request có đi qua AWS-managed MCP server hay không, và nếu có, thì MCP server cụ thể nào đã khởi tạo hành động. Theo bài viết, điều này giúp các tổ chức triển khai chiến lược **defense-in-depth (phòng thủ chiều sâu)**, duy trì các audit trail chi tiết và đáp ứng yêu cầu tuân thủ (compliance) bằng cách phân biệt quyền truy cập do AI điều khiển với hành động trực tiếp của người dùng.

Ngoài ra, AWS đã thông báo về việc đơn giản hóa mô hình authorization trong thời gian tới. Khách hàng sẽ sớm không cần phải sử dụng các **IAM action dành riêng cho MCP** như `aws-mcp:InvokeMCP` khi tương tác với các AWS-managed MCP server qua public endpoint. Thay vào đó, hệ thống sẽ hoạt động tương đồng hơn với cách AWS CLI và SDK đang hoạt động, giúp giảm bớt độ phức tạp trong cấu hình mà vẫn tiếp tục dựa vào các IAM policy hiện có để kiểm soát truy cập dịch vụ.

Cuối cùng, AWS chia sẻ rằng việc hỗ trợ **VPC endpoint** cho các AWS-managed MCP server đang được lên kế hoạch trong tương lai. Tính năng này sẽ giúp các tổ chức áp dụng cả kiểm soát **dựa trên danh tính (identity-based)** và **dựa trên mạng (network-based)**, đặc biệt hữu ích trong các môi trường được quản lý nghiêm ngặt.

---

## Sử dụng IAM để phân biệt giữa hành động do con người và do AI thực hiện

Một chủ đề chính của bài viết là nhu cầu kiểm soát chính xác cách các AI agent truy cập tài nguyên AWS của các tổ chức. AWS giới thiệu hai IAM context key tiêu chuẩn cho mục đích này.

### `aws:ViaAWSMCPService`

Đây là một context key kiểu **boolean**. Nó được set thành `true` khi một request đi qua AWS-managed MCP server. Admin có thể sử dụng key này để cho phép (allow) hoặc từ chối (deny) một cách tổng quát các hành động được khởi tạo thông qua bất kỳ AWS-managed MCP server nào.

### `aws:CalledViaAWSMCP`

Đây là một context key kiểu **chuỗi đơn trị (single-valued string)**. Nó xác định service principal của MCP server đã thực hiện lệnh gọi, ví dụ như:

- `aws-mcp.amazonaws.com`
- `eks-mcp.amazonaws.com`
- `ecs-mcp.amazonaws.com`

Điều này cho phép admin xác định các policy cho phép hoặc từ chối quyền truy cập dựa trên **MCP server cụ thể** tham gia vào quá trình. AWS cũng lưu ý rằng khi có thêm các AWS-managed MCP server mới, key này cũng sẽ hỗ trợ các giá trị mới đó, cho phép thiết kế policy chi tiết (granular) hơn theo thời gian.

Các context key này có thể được sử dụng trong cả IAM policy và **service control policy (SCP)** để triển khai các quy tắc bảo vệ tinh chỉnh (fine-grained guardrails).

---

## Ví dụ: Deny mọi action thông qua MCP server

Bài viết đưa ra ví dụ về một tổ chức muốn vô hiệu hóa hoàn toàn quyền truy cập AWS-managed MCP server trên phạm vi toàn bộ AWS Organization hoặc trong một organizational unit (OU) cụ thể. Việc này có thể được thực hiện bằng cách tạo một SCP từ chối mọi hành động bất cứ khi nào request được nhận diện là đi qua MCP server.

Logic ở đây rất đơn giản: nếu `aws:ViaAWSMCPService` là `true`, thì request sẽ bị deny.

Loại policy này rất hữu ích cho các tổ chức muốn có một điểm khởi đầu thận trọng cho việc quản lý AI hoặc cần cấm các hành động thay đổi hạ tầng qua trung gian AI cho đến khi các quy trình review và phê duyệt nội bộ hoàn tất.

---

## Ví dụ: Cho phép Read-only trên Amazon S3 nhưng block các thao tác Delete qua MCP

Một ví dụ khác trong blog chỉ ra cách tổ chức có thể cho phép AI agent thực hiện một số thao tác an toàn nhất định trong khi chặn các thao tác nhạy cảm hơn.

Trong trường hợp này, **AWS MCP Server** được phép thực hiện các thao tác đọc (read) trên **Amazon S3**, chẳng hạn như:

- `s3:GetObject`
- `s3:ListBucket`

Đồng thời, các thao tác xóa (delete) chẳng hạn như:

- `s3:DeleteObject`
- `s3:DeleteBucket`

bị deny một cách tường minh bất cứ khi nào request đi qua một MCP server, bằng cách tiếp tục sử dụng điều kiện `aws:ViaAWSMCPService`.

Điều này minh họa cách các tổ chức có thể cho AI agent hỗ trợ tăng năng suất đối với các hoạt động ít rủi ro trong khi vẫn duy trì quyền kiểm soát chặt chẽ đối với các hành động mang tính phá hủy hoặc có tác động lớn.

---

## Ví dụ: Giới hạn truy cập vào các MCP server cụ thể

Bài viết cũng mô tả cách các admin có thể giới hạn quyền truy cập vào một số dịch vụ AWS nhất định để chúng chỉ có thể được gọi thông qua một MCP server cụ thể.

Ví dụ, một tổ chức có thể quyết định rằng các thao tác **Amazon EKS** chỉ được cho phép khi khởi chạy qua **EKS MCP server**, chứ không phải qua AWS MCP server chung. Trong trường hợp đó, IAM policy có thể chỉ allow các action `eks:*` khi `aws:CalledViaAWSMCP` bằng `eks-mcp.amazonaws.com`, và deny chúng trong các trường hợp khác.

Cách tiếp cận này rất có giá trị khi các MCP server khác nhau được chỉ định cho các phạm vi vận hành khác nhau. Nó cho phép các tổ chức map permission chặt chẽ hơn với các interface AI chuyên dụng thay vì cấp quyền truy cập rộng rãi thông qua một server chung.

---

## Hiểu các thay đổi đối với Public Endpoint Authorization

AWS cho biết họ đã tiếp nhận ý kiến từ khách hàng rằng mô hình authorization ban đầu cho AWS-managed MCP server gây ra sự phức tạp không cần thiết. Để giải quyết, công ty đang đơn giản hóa mô hình này để nó hoạt động giống với AWS CLI và SDK hơn.

Theo cách tiếp cận sắp tới, MCP server vẫn xác thực (authenticate) các request bằng **AWS Signature Version 4 (SigV4)**. Tuy nhiên, thay vì yêu cầu các IAM action riêng biệt dành cho MCP, MCP server sẽ chuyển tiếp (forward) request đến dịch vụ AWS downstream (đầu cuối) cùng với các IAM context key tiêu chuẩn:

- `aws:ViaAWSMCPService`
- `aws:CalledViaAWSMCP`

Dịch vụ AWS downstream sau đó sẽ đánh giá request bằng **các IAM policy hiện có** của khách hàng, nơi có thể tham chiếu trực tiếp đến các context key đó khi cần.

Điều này có nghĩa là các tổ chức có thể tiếp tục sử dụng credential AWS và các permission cấp độ dịch vụ (service-level permissions) mà họ đang quản lý hiện nay. Các AI agent có thể hoạt động trong những ranh giới có sẵn đó, trong khi các admin có được một mô hình authorization đơn giản và quen thuộc hơn. AWS nhấn mạnh rằng điều này giúp loại bỏ nhu cầu phải có các permission riêng cho MCP ở public endpoint và giảm bớt gánh nặng cấu hình.

Bài blog có kèm theo một sơ đồ mô tả luồng authorization đơn giản hóa này, trong đó MCP server đóng vai trò trung gian xác thực request, thêm các context liên quan, và forward nó tới dịch vụ AWS đích để đánh giá policy.

---

## Sử dụng IAM với MCP server và VPC Endpoint

Các tác giả cũng thảo luận về phản hồi từ khách hàng trong các ngành công nghiệp bị quản lý nghiêm ngặt như **dịch vụ tài chính** và **chăm sóc sức khỏe**, nơi kết nối mạng nội bộ thường là một yêu cầu bắt buộc về compliance. Đối với những khách hàng này, chỉ riêng kiểm soát qua IAM (identity-based) có thể là chưa đủ. Họ cũng cần đảm bảo rằng traffic của AI agent không đi ra ngoài internet công cộng.

Để giải quyết vấn đề này, AWS có kế hoạch bổ sung **hỗ trợ VPC endpoint** cho các AWS-managed MCP server. Với khả năng này, khách hàng sẽ có thể kết nối trực tiếp đến các MCP server từ bên trong VPC của họ, giữ cho traffic của AI agent hoàn toàn nằm trong ranh giới mạng nội bộ.

Theo bài viết, điều này sẽ tạo ra một **mô hình authorization hai giai đoạn**:

1. Một bước kiểm tra authorization ở **cấp độ VPC endpoint**
2. Một bước kiểm tra authorization thứ hai ở **cấp độ dịch vụ AWS downstream** bằng các IAM policy

Cách tiếp cận phân lớp này củng cố tính bảo mật bằng cách kết hợp **kiểm soát vành đai mạng (network perimeter controls)** với **kiểm soát danh tính dịch vụ (service-level identity controls)**. AWS giải thích rằng khách hàng sẽ có thể kết hợp VPC endpoint với các IAM context key mới để xây dựng các mô hình quản trị toàn diện, được tinh chỉnh theo nhu cầu bảo mật và tuân thủ của mình.

Chi tiết triển khai chuyên sâu và các pattern mẫu sẽ được cung cấp khi tính năng VPC endpoint chính thức ra mắt.

---

## Những điểm cần lưu ý

Bài viết kết thúc phần thảo luận kỹ thuật với một số khuyến nghị thực tiễn cho khách hàng khi áp dụng authorization dựa trên IAM cho các MCP server.

### Thiết kế IAM policy cẩn thận

AWS khuyên khách hàng chỉ nên cấp những quyền thực sự cần thiết (least privilege). Khi AI agent trở nên thông minh hơn và các pattern sử dụng dần định hình rõ ràng, tổ chức nên liên tục tinh chỉnh các policy và loại bỏ các quyền không dùng đến. Các context key mới đặc biệt hữu ích để phân biệt rạch ròi hành động của AI với hành động trực tiếp của người dùng trong các policy này.

### Đáp ứng các yêu cầu về bảo mật và compliance

Đối với các khách hàng có yêu cầu quản trị hoặc pháp lý nghiêm ngặt, hỗ trợ VPC endpoint trong tương lai sẽ là một tùy chọn quan trọng, vì nó cung cấp các đường dẫn giao tiếp private nằm ngoài những ràng buộc cơ bản dựa trên IAM.

### Bắt đầu nhỏ và tinh chỉnh dần

AWS khuyên bạn nên khởi đầu với một mô hình triển khai phù hợp với nhu cầu hiện tại, bắt đầu bằng **các IAM policy giới hạn chặt chẽ (restrictive)**, sau đó nới lỏng dần các quyền khi các team hiểu rõ hơn về nhu cầu thực tế của AI agent. Bài viết cũng khuyến nghị sử dụng **CloudTrail logs** để giám sát những hành động mà AI agent thực hiện trong thực tế. Dữ liệu audit đó sau đó có thể được dùng để liên tục cải thiện IAM policy theo thời gian.

---

## Về các Tác giả

**Riggs Goodman III** là Principal Partner Solution Architect tại AWS. Trọng tâm hiện tại của ông là bảo mật và mạng lưới AI, nơi ông cung cấp các định hướng kỹ thuật, mô hình kiến trúc, và sự dẫn dắt để giúp khách hàng cũng như đối tác xây dựng các workload AI trên AWS. Trong nội bộ, ông cũng thúc đẩy chiến lược kỹ thuật và cải tiến chéo giữa các service team của AWS để giải quyết những thách thức của khách hàng và đối tác.

**Praneeta Prakash** là Senior Product Manager của mảng AWS Developer Tools. Cô làm việc tại điểm giao thoa giữa hạ tầng cloud và trải nghiệm nhà phát triển, dẫn dắt các sáng kiến chiến lược nhằm định hình cách dev tương tác với các hệ thống cloud, đặc biệt là trong thế giới phát triển AI-native đang bùng nổ. Trọng tâm công việc của cô là làm cho AWS trở nên dễ tiếp cận và trực quan hơn đối với developer ở mọi cấp độ.

**Khaled Sinno** là Principal Engineer tại Amazon Web Services. Công việc hiện tại của ông xoay quanh Identity and Access Management (Quản lý Danh tính và Truy cập) trong AWS và các kiểm soát danh tính & bảo mật rộng hơn trên cloud. Trước đây, ông từng làm việc về tính sẵn sàng và bảo mật cho AWS RDS và đóng góp sâu rộng hơn vào bảo mật cho các dịch vụ database và search. Trước khi gia nhập AWS, ông đã dẫn dắt nhiều team kỹ thuật lớn trong ngành FinTech, chuyên về các hệ thống phân tán cho nền tảng tài chính và giao dịch.

**Shreya Jain** là Senior Technical Product Manager mảng AWS Identity. Cô có niềm đam mê trong việc đơn giản hóa các ý tưởng phức tạp. Ngoài công việc, cô thích tập Pilates, nhảy múa và khám phá các quán cà phê mới.