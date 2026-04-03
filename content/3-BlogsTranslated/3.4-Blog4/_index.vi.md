---
title: "Blog 4"
date: 2026-03-12
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Bảo mật AI agents với Policy trong Amazon Bedrock AgentCore

Việc triển khai AI agent một cách an toàn trong các ngành được quản lý chặt chẽ là một thách thức lớn, bởi vì những hệ thống này có thể truy cập dữ liệu nhạy cảm, gọi các công cụ (tools), và thực hiện những hành động có tác động trong thế giới thực. Khác với phần mềm truyền thống, AI agent không chỉ đơn giản thực thi một chuỗi lệnh cố định. Thay vào đó, nó có thể quyết định nên gọi tool nào, truy xuất dữ liệu gì và phản hồi ra sao dựa trên input của người dùng và bối cảnh môi trường. Sự linh hoạt này khiến agent trở nên mạnh mẽ, nhưng đồng thời cũng tạo ra các rủi ro bảo mật mới như truy cập dữ liệu trái phép, giao dịch ngoài ý muốn, prompt injection và việc lạm dụng các hệ thống được kết nối. Trong bài viết AWS *Secure AI agents with Policy in Amazon Bedrock AgentCore*, các tác giả giải thích cách **Policy trong Amazon Bedrock AgentCore** cung cấp một lớp thực thi bảo mật mang tính xác định (deterministic) để bảo vệ AI agent một cách độc lập với khả năng suy luận của chính agent. 

Bài viết sử dụng ví dụ cụ thể về một **agent đặt lịch khám bệnh trong hệ thống y tế**. Y tế là một lĩnh vực đặc biệt phù hợp để minh họa vì các agent trong môi trường này có thể xử lý **Protected Health Information (PHI)**, quản lý lịch hẹn và truy cập hồ sơ bệnh nhân. Trong bối cảnh như vậy, chỉ dựa vào bản thân mô hình để hành xử an toàn là chưa đủ. Các tổ chức cần một cơ chế đảm bảo **ai có thể truy cập cái gì, trong điều kiện nào và với những hạn chế nào** được thực thi một cách nhất quán. Policy trong Amazon Bedrock AgentCore được thiết kế để cung cấp chính xác loại kiểm soát này bằng cách hoạt động ở **runtime thông qua gateway layer**.

---

## Vì sao AI agents cần thực thi policy từ bên ngoài

Một luận điểm chính của bài blog là việc bảo mật AI agent khó hơn so với bảo mật các ứng dụng truyền thống. Phần mềm truyền thống thường có các **code path rõ ràng**, trong khi agent dựa vào **large language models (LLMs)** có khả năng suy luận theo nhiều hướng mở. Điều này có nghĩa là cùng một mô hình có thể hành xử khác nhau tùy thuộc vào prompt, kết quả từ tool, ngữ cảnh xung quanh hoặc thậm chí input mang tính tấn công. Vì LLM có thể **hallucinate** và không tự phân biệt rõ giữa instruction đáng tin cậy và văn bản không đáng tin cậy, các agent dễ bị tấn công **prompt injection** và các hình thức tấn công tương tự. 

Một cách phổ biến để giảm rủi ro là bọc agent trong code ứng dụng để giới hạn các tool có thể được gọi và điều kiện gọi chúng. Tuy nhiên, cách tiếp cận này có một số nhược điểm. Logic bảo mật bị phân tán trong codebase, khiến việc review, audit và bảo trì trở nên khó khăn hơn. Ngoài ra, tính đúng đắn của wrapper code cũng trở thành một phần của **security boundary**. Nếu code này có bug, agent vẫn có thể thực hiện các thao tác không an toàn. Bài viết của AWS đề xuất một mô hình khác: **đưa việc thực thi policy ra hoàn toàn bên ngoài agent** và áp dụng nó trước khi bất kỳ tool nào được gọi. 

Sự tách biệt này rất quan trọng vì policy vẫn có hiệu lực bất kể agent được prompt như thế nào, agent “tin” điều gì, hay logic ứng dụng có lỗi hay không. Gateway sẽ đánh giá mọi request dựa trên các policy đã định nghĩa trước khi cho phép truy cập tool. Nhờ đó, các quy tắc bảo mật trở nên **minh bạch, có thể audit và độc lập với hành vi của LLM**.

---

## Cedar – nền tảng cho policy mang tính xác định

Để việc thực thi policy từ bên ngoài trở nên khả thi, Amazon Bedrock AgentCore sử dụng **Cedar**, một ngôn ngữ authorization được thiết kế vừa hiệu quả cho máy xử lý vừa dễ hiểu đối với con người. Cedar policy mô tả ba thành phần chính: **principal** (ai đang thực hiện request), **action** (hành động đang được yêu cầu), và **resource** (tài nguyên được truy cập). Các điều kiện có thể được thêm vào thông qua mệnh đề `when` để đánh giá ngữ cảnh runtime. 

Bài blog của AWS nhấn mạnh một số lý do khiến Cedar phù hợp cho bảo mật agent. Thứ nhất, nó có cơ chế **default-deny**, nghĩa là mọi request sẽ bị từ chối trừ khi có rule cho phép rõ ràng. Thứ hai, **forbid rule luôn ghi đè permit rule**, cho phép đội bảo mật chặn hoàn toàn các pattern nguy hiểm ngay cả khi tồn tại quyền truy cập rộng hơn. Thứ ba, Cedar được thiết kế **không có side effects và không có vòng lặp**, giúp việc đánh giá nhanh, dự đoán được và dễ phân tích bằng các phương pháp hình thức. Những đặc tính này hỗ trợ việc thực thi policy mang tính xác định, đặc biệt quan trọng trong môi trường AI agent có hành vi rất động. 

Blog cũng giải thích rằng policy trong AgentCore có thể được tạo theo nhiều cách. Developer và đội bảo mật có thể viết Cedar trực tiếp để kiểm soát chi tiết, tạo Cedar từ mô tả policy bằng ngôn ngữ tự nhiên, hoặc sử dụng các công cụ dạng form để định nghĩa rule. Sự linh hoạt này giúp giảm rào cản áp dụng trong khi vẫn giữ được semantics thực thi chính xác. 

---

## Policy trong Amazon Bedrock AgentCore

Policy trong Amazon Bedrock AgentCore hoạt động bằng cách đánh giá mọi request từ agent tới tool thông qua một **policy engine** đã được định nghĩa. Engine này là tập hợp các Cedar policy được gắn với **AgentCore Gateway**. Ở runtime, gateway sẽ chặn request, đánh giá nó dựa trên policy và quyết định có cho phép hay từ chối. Như vậy gateway trở thành **điểm thực thi bảo mật** giữa agent và các tool mà nó muốn gọi. 

Theo bài viết, dịch vụ không chỉ hỗ trợ viết Cedar trực tiếp mà còn hỗ trợ tạo Cedar từ các policy mô tả bằng tiếng Anh đơn giản. Các policy được tạo ra sẽ được kiểm tra tính hợp lệ cú pháp, xác minh với schema của gateway và phân tích các vấn đề tiềm ẩn như quá permissive hoặc quá restrictive. Khả năng này giúp các team chuyển đổi **business rules thành các kiểm soát thực thi được**, đồng thời giảm nguy cơ cấu hình sai. 

Một lợi ích thực tế của mô hình này là tổ chức có thể gắn policy engine ở chế độ **LOG_ONLY** trước. Ở chế độ này, họ có thể quan sát policy sẽ hoạt động ra sao mà không chặn traffic production. Khi xác nhận rằng policy hoạt động đúng như mong muốn, họ có thể chuyển sang chế độ enforcement để thực thi policy thật sự. Cách triển khai theo từng bước này rất hữu ích cho các doanh nghiệp muốn áp dụng kiểm soát nghiêm ngặt mà không làm gián đoạn hệ thống quan trọng. 

---

## Ví dụ agent đặt lịch khám bệnh

Bài blog minh họa các ý tưởng trên bằng một agent đặt lịch khám bệnh trong hệ thống y tế. Agent này hỗ trợ nhiều tool khác nhau, bao gồm:

- `getPatient` để lấy thông tin hồ sơ bệnh nhân  
- `searchImmunization` để truy vấn lịch sử tiêm chủng  
- `bookAppointment` để đặt lịch khám  
- `getSlots` để lấy danh sách khung giờ khám còn trống

Vì các tool này cung cấp khả năng truy cập dữ liệu nhạy cảm, agent phải được quản lý rất cẩn thận. Các tác giả cho thấy nhiều pattern policy có thể kết hợp với nhau để tạo thành một tập policy **an toàn và có thể audit**.

### Policy dựa trên danh tính

Pattern đầu tiên là **identity-scoped access**. Trong hệ thống y tế, bệnh nhân chỉ nên truy cập hồ sơ của chính họ. Ví dụ, khi agent gọi `getPatient`, `patient_id` trong request phải trùng với ID của người dùng đã được xác thực. Một rule tương tự áp dụng cho việc tìm kiếm dữ liệu tiêm chủng. Điều này đảm bảo người dùng không thể yêu cầu agent truy cập hồ sơ của người khác chỉ bằng cách thay đổi giá trị input. 

Blog giải thích rằng các rule như vậy có thể viết trực tiếp bằng Cedar hoặc sinh ra từ mô tả policy bằng ngôn ngữ tự nhiên. Trong cả hai trường hợp, policy cuối cùng sẽ so sánh identity đã xác thực với tham số request và chỉ cho phép hành động nếu chúng khớp nhau. 

### Tách quyền đọc và ghi

Pattern thứ hai là **phân tách quyền read và write dựa trên scope**. Trong nhiều hệ thống y tế, quyền đọc thường rộng hơn quyền ghi. Người dùng có thể được phép xem thông tin nếu có scope như `fhir:read`, trong khi đặt lịch hoặc thay đổi dữ liệu có thể yêu cầu scope riêng như `appointment:write`. Bằng cách tách quyền như vậy, tổ chức có thể giảm nguy cơ thay đổi dữ liệu trái phép trong khi vẫn cho phép truy cập thông tin cần thiết.

Bài viết cũng minh họa cách sử dụng giao diện tạo policy dạng form để xây dựng các rule như vậy bằng cách xác định effect, principal, resource, action và conditions. Điều này hữu ích cho các team muốn viết policy có cấu trúc mà không cần viết Cedar thủ công.

### Kiểm soát rủi ro trong việc đặt lịch

Pattern thứ ba là sử dụng **risk control rõ ràng** để chặn các request nguy hiểm hoặc có khả năng bị lạm dụng. Ví dụ, policy có thể giới hạn việc truy cập khung giờ khám chỉ trong khoảng **9 AM đến 9 PM UTC**. Những request ngoài khoảng thời gian này sẽ bị từ chối. Loại rule này không chỉ liên quan đến authorization mà còn mã hóa **business constraint và cơ chế chống lạm dụng** trực tiếp trong lớp thực thi runtime.

Đây là nơi mô hình **forbid-wins** của Cedar phát huy tác dụng. Ngay cả khi tồn tại permit rule tổng quát hơn, forbid rule vẫn có thể chặn các thao tác rủi ro cao. Điều này giúp tạo ra cơ chế bảo vệ nhiều lớp vừa dễ hiểu cho auditor vừa có thể thực thi trực tiếp ở runtime.

---

## Kiểm thử mô hình thực thi

Các tác giả AWS cũng trình bày một số kịch bản kiểm thử để minh họa cách hệ thống hoạt động. Trong một test, người dùng đã xác thực là `adult-patient-001` và yêu cầu agent lấy thông tin của chính ID đó. Vì request khớp với identity đã xác thực, quyết định policy là **ALLOW** và hồ sơ bệnh nhân được trả về. 

Trong test tiếp theo, cùng người dùng đó yêu cầu thông tin cho `pediatric-patient-001`. Agent, model và tool không thay đổi, nhưng tham số request không còn khớp với identity. Kết quả là gateway từ chối request vì không có permit policy nào phù hợp. Điều này chứng minh rằng **security boundary được thực thi tại gateway**, độc lập với quá trình suy luận của agent.

Một cặp test khác kiểm tra kiểm soát đặt lịch theo thời gian. Khi người dùng yêu cầu khung giờ trong khoảng hợp lệ, ví dụ **2 PM UTC**, forbid condition không khớp nên request được cho phép nếu có permit rule. Nhưng khi request được thực hiện lúc **3 AM UTC**, forbid rule sẽ khớp và request bị từ chối. Các test này cho thấy cách **permit dựa trên identity và forbid dựa trên thời gian** kết hợp để tạo ra kết quả bảo mật mang tính xác định.

---

## Triển khai và vận hành

Để thử ví dụ này, bài blog hướng dẫn clone repository **amazon-bedrock-agentcore-samples** và truy cập vào sample healthcare appointment agent. Repository bao gồm hướng dẫn setup, cấu hình môi trường AWS, triển khai stack và gọi agent end-to-end.

Bài viết cũng liệt kê một số điều kiện cần thiết, bao gồm tài khoản AWS đang hoạt động, sử dụng Region hỗ trợ Policy trong AgentCore và quyền IAM phù hợp để tạo và quản lý policy engine, Cedar policies, tài nguyên tạo policy và gateway association. AWS khuyến nghị trong môi trường production nên giới hạn quyền truy cập vào các resource cụ thể thay vì dùng wildcard rộng.
Về vận hành, quy trình được khuyến nghị là: tạo policy engine, viết policy bằng một trong các phương pháp hỗ trợ, gắn engine vào gateway ở chế độ **LOG_ONLY**, quan sát hành vi qua log, và sau đó chuyển sang chế độ enforcement khi đã xác nhận policy hoạt động đúng. Quy trình này giúp các team áp dụng policy-based enforcement một cách **an toàn và có hệ thống**.

---

## Kết luận

Thông điệp chính của bài blog AWS là **AI agent chỉ đáng tin cậy khi các ranh giới xung quanh nó được thiết lập rõ ràng**. Vì agent có thể suy luận linh hoạt và gọi các tool mạnh mẽ, các kiểm soát bảo mật phải được thực thi **bên ngoài mô hình**. Policy trong Amazon Bedrock AgentCore giải quyết thách thức này bằng cách đặt một lớp policy mang tính xác định và có thể audit tại gateway, nơi mọi tool call được đánh giá trước khi thực thi.

Bằng cách kết hợp Cedar policy, cơ chế default-deny, semantics forbid-overrides và thực thi runtime tại gateway, các tổ chức có thể xây dựng hệ thống agentic **an toàn hơn, minh bạch hơn và dễ audit hơn**. Ví dụ về hệ thống đặt lịch khám bệnh cho thấy cách rule dựa trên identity, phân tách scope và kiểm soát rủi ro theo business có thể phối hợp để bảo vệ các hệ thống nhạy cảm. Với các doanh nghiệp hoạt động trong lĩnh vực được quản lý chặt chẽ, việc tách biệt **khả năng của agent** và **cơ chế thực thi bảo mật** là nền tảng vững chắc cho các AI agent ở cấp độ production.

---

## Về tác giả

**Bharathi Srinivasan** là Generative AI Data Scientist tại AWS Worldwide Specialist Organization. Cô phát triển các giải pháp Responsible AI với trọng tâm là fairness của thuật toán, độ tin cậy của LLM, khả năng giải thích và governance của agent.

**Anil Nadiminti** là Senior Solutions Architect tại AWS tập trung vào lĩnh vực FinTech. Anh làm việc với các tổ chức tài chính doanh nghiệp và các công ty blockchain để cung cấp giải pháp kỹ thuật và chiến lược trong Web3 và DeFi.

**Pushpinder Dua** là Software Development Manager tại AWS Agentic AI. Anh lãnh đạo các sáng kiến liên quan đến governance của hệ thống agentic và sự phát triển của các agentic protocol nhằm cải thiện tính an toàn và độ tin cậy của hệ sinh thái agent.

**Jean-Baptiste Tristan** là Principal Applied Scientist tại AWS Agentic AI. Công việc của ông tập trung vào **an toàn và bảo mật của agentic systems**, kết hợp automated reasoning với generative AI.