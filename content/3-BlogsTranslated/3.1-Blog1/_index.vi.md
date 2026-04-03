---
title: "Blog 1"
date: 2025-12-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Giới thiệu Amazon Nova Forge: Xây dựng Frontier Model của riêng bạn với Nova

Các tổ chức ngày càng áp dụng AI sinh (generative AI) trong nhiều phần của hoạt động vận hành, từ các công cụ tăng năng suất nội bộ đến những ứng dụng chuyên sâu theo từng lĩnh vực. Khi mức độ áp dụng tăng lên, các hạn chế của những foundation model dùng chung cũng trở nên rõ ràng hơn. Nhiều kịch bản trong doanh nghiệp yêu cầu mô hình phải hiểu sâu hơn về kiến thức nội bộ, thuật ngữ riêng của tổ chức, workflow chuyên biệt và các yêu cầu nghiệp vụ đặc thù — những thứ mà chỉ dùng prompt thông thường khó có thể đáp ứng.

Các phương pháp tùy biến phổ biến như **prompt engineering** và **Retrieval-Augmented Generation (RAG)** hữu ích trong nhiều ứng dụng, nhưng chúng không thực sự thay đổi cách mô hình biểu diễn tri thức bên trong. Những phương pháp này chủ yếu giúp mô hình phản hồi tốt hơn ở thời điểm suy luận (inference time), thay vì thay đổi sâu những gì mô hình đã học. Những cách khác như **supervised fine-tuning** và **reinforcement learning** cũng giúp tùy chỉnh mô hình, nhưng chúng thường được áp dụng sau khi mô hình đã được huấn luyện hoàn chỉnh. Ở giai đoạn đó, việc điều chỉnh mô hình theo các domain rất cụ thể trở nên khó khăn hơn.

Đối với các tổ chức muốn mức độ tùy biến mạnh hơn, **Continued Pre-Training (CPT)** có thể là một giải pháp tự nhiên. Tuy nhiên, nếu CPT chỉ được thực hiện trên dữ liệu độc quyền của doanh nghiệp, mô hình có thể gặp vấn đề **catastrophic forgetting** — tức là mô hình trở nên tốt hơn ở domain mới nhưng lại mất đi những năng lực nền tảng đã học trước đó. Ngoài ra, việc huấn luyện một frontier model từ đầu vẫn quá tốn kém và tiêu tốn tài nguyên đối với hầu hết tổ chức, vì nó yêu cầu dataset cực lớn, hạ tầng tính toán đáng kể và chuyên môn huấn luyện nâng cao. AWS đã giới thiệu **Amazon Nova Forge** để thu hẹp khoảng cách này. Với Nova Forge, khách hàng có thể bắt đầu từ các checkpoint sớm của Amazon Nova, kết hợp dataset riêng với dữ liệu huấn luyện được Amazon Nova tuyển chọn, và xây dựng frontier model tùy chỉnh được host an toàn trên AWS. AWS định vị đây là cách dễ hơn và tiết kiệm chi phí hơn để xây dựng frontier model theo domain cụ thể.

---

## Các trường hợp sử dụng và ứng dụng

Amazon Nova Forge được thiết kế cho các tổ chức đã có sẵn dữ liệu độc quyền, kiến thức chuyên ngành hoặc thông tin vận hành đặc thù và muốn biến những tài sản này thành năng lực AI mạnh hơn. Thay vì chỉ dựa vào một mô hình generic, các tổ chức này có thể tạo ra mô hình phù hợp hơn với thực tế domain của họ.

AWS đưa ra một số kịch bản ví dụ. Trong **manufacturing và automation**, các tổ chức có thể xây dựng mô hình hiểu rõ hơn các quy trình công nghiệp chuyên biệt, dữ liệu máy móc, quy trình vận hành và workflow liên quan đến thiết bị. Trong **research and development**, các công ty có thể tạo mô hình được huấn luyện trên tài liệu nghiên cứu nội bộ, tập dữ liệu riêng và chuyên môn domain mà dataset công khai không có. Trong **content và media**, các team có thể phát triển mô hình phù hợp với giọng điệu thương hiệu, tiêu chuẩn nội dung nội bộ và các yêu cầu moderation. Trong **các ngành chuyên biệt** nói chung, Nova Forge có thể dùng để huấn luyện mô hình hiểu ngôn ngữ chuyên ngành, quy định, best practice và các quy ước kỹ thuật của từng lĩnh vực.

Tùy vào mục tiêu kinh doanh, Nova Forge có thể giúp tổ chức tạo ra năng lực mô hình khác biệt, cải thiện hiệu năng cho task cụ thể, giảm độ trễ trong môi trường production và giảm chi phí tổng thể so với các phương pháp yêu cầu retraining lớn hoặc pipeline thích nghi mô hình bên ngoài.

---

## Cách thức hoạt động của Nova Forge

Amazon Nova Forge được thiết kế để giải quyết các hạn chế của việc tùy chỉnh mô hình truyền thống bằng cách cho phép tổ chức bắt đầu phát triển từ **các checkpoint sớm** trong vòng đời của mô hình. Thay vì chỉ làm việc với một mô hình đã hoàn chỉnh, khách hàng có thể bắt đầu từ các checkpoint **pre-training**, **mid-training** hoặc **post-training**. Điều này mang lại sự linh hoạt lớn hơn trong cách mô hình được điều chỉnh cho một domain chuyên biệt.

Một năng lực cốt lõi của Nova Forge là **data blending**. Các tổ chức có thể kết hợp dataset độc quyền của mình với **dữ liệu được Amazon Nova tuyển chọn** trong tất cả các giai đoạn huấn luyện. AWS cho biết quá trình training này có thể chạy bằng các recipe đã được kiểm chứng trên hạ tầng được quản lý hoàn toàn trong **Amazon SageMaker AI**. Điều này có nghĩa là khách hàng không cần tự xây dựng toàn bộ training stack từ đầu. Thay vào đó, họ có thể sử dụng tooling và workflow do AWS quản lý, trong khi vẫn định hình mô hình bằng dữ liệu riêng của mình.

Chiến lược trộn dữ liệu này đặc biệt quan trọng vì nó giúp giảm hiện tượng catastrophic forgetting so với việc chỉ huấn luyện trên dữ liệu độc quyền thô. Theo AWS, việc kết hợp dữ liệu curated với dữ liệu đặc thù của tổ chức giúp giữ lại các kỹ năng nền tảng như khả năng suy luận chung, khả năng làm theo instruction và các đặc tính an toàn tích hợp sẵn, trong khi vẫn cho phép mô hình hấp thụ kiến thức chuyên biệt của doanh nghiệp. Về thực tế, Nova Forge cố gắng cân bằng hai mục tiêu: giữ lại tính hữu dụng rộng của foundation model mạnh và đồng thời thích nghi sâu hơn với một domain cụ thể.

---

## **Học tăng cường trong các môi trường độc quyền**

Nova Forge cũng hỗ trợ **reinforcement learning (RL)** sử dụng reward function được định nghĩa trong môi trường riêng của tổ chức. Điều này cho phép mô hình học từ feedback được tạo ra trong điều kiện gần với use case thực tế hơn so với các benchmark chung.

Khả năng này hữu ích trong những tình huống mà thành công phụ thuộc vào chuỗi quyết định liên tiếp, feedback đặc thù môi trường hoặc logic reward do doanh nghiệp định nghĩa. AWS giải thích rằng khách hàng có thể sử dụng orchestrator riêng của họ để quản lý **multi-turn rollouts**, cho phép hỗ trợ các workflow RL nâng cao hơn, bao gồm hành vi agent phức tạp và các bài toán ra quyết định tuần tự.

AWS đưa ra ví dụ như sử dụng công cụ hóa học để đánh giá thiết kế phân tử, hoặc mô phỏng robotics nơi hệ thống được thưởng khi hoàn thành nhiệm vụ hiệu quả và bị phạt khi có hành vi không an toàn như va chạm. Những ví dụ này cho thấy Nova Forge không chỉ giới hạn ở việc thích nghi mô hình cho text. Thay vào đó, nó có thể được sử dụng trong những môi trường mà việc cải thiện mô hình phụ thuộc vào tương tác với các công cụ chuyên biệt, simulator hoặc môi trường đánh giá nội bộ. Điều này khiến Nova Forge phù hợp với các ngành nghiên cứu chuyên sâu và hệ thống AI vận hành nâng cao cần nhiều hơn fine-tuning thông thường.

---

## **AI có trách nhiệm và cấu hình an toàn**

Ngoài việc huấn luyện và tùy chỉnh mô hình, Nova Forge còn bao gồm **bộ công cụ responsible AI** tích hợp sẵn. Bộ công cụ này cho phép tổ chức cấu hình hành vi **safety** và **content moderation** của các mô hình tùy chỉnh.

AWS cho biết khách hàng có thể điều chỉnh các thiết lập này để phù hợp với nhu cầu kinh doanh cụ thể trong các lĩnh vực như an toàn, bảo mật và xử lý nội dung nhạy cảm.

Đây là một phần quan trọng của nền tảng vì việc triển khai AI trong doanh nghiệp không chỉ liên quan đến chất lượng mô hình hay hiệu năng benchmark. Trong nhiều tổ chức, việc sẵn sàng đưa vào production còn phụ thuộc vào governance, chính sách moderation và kiểm soát rủi ro nội dung. Bằng cách tích hợp các tùy chọn này trực tiếp trong Nova Forge, AWS cung cấp một framework nơi các tổ chức có thể làm việc cả về tùy chỉnh mô hình lẫn trách nhiệm mô hình trong cùng một môi trường.

---

## **Bắt đầu với Nova Forge**


AWS cho biết Nova Forge tích hợp với các workflow AI hiện có trên AWS, đặc biệt là những workflow xoay quanh **Amazon SageMaker AI** và **Amazon Bedrock**. Khách hàng có thể sử dụng các công cụ và hạ tầng quen thuộc trong SageMaker AI để chạy training job, sau đó import các Nova model tùy chỉnh vào Amazon Bedrock dưới dạng **private models**.

Sự tích hợp này quan trọng vì nó cho phép tổ chức tiếp tục sử dụng cùng một môi trường AWS cho bảo mật, API và vận hành. Thay vì xây dựng một hệ thống cho training và một stack khác cho serving, các team có thể đi từ phát triển mô hình đến triển khai ứng dụng bằng các service đã được kết nối sẵn trong hệ sinh thái AWS. AWS nhấn mạnh rằng điều này mang lại cùng một mô hình bảo mật, API nhất quán và khả năng tích hợp rộng giống như các model khác trong Amazon Bedrock.

Trong **Amazon SageMaker Studio**, người dùng hiện có thể xây dựng frontier model bằng Amazon Nova. Workflow ở mức cao mà AWS mô tả khá đơn giản. Trước tiên, người dùng chọn checkpoint mà họ muốn bắt đầu, chẳng hạn như **pre-trained**, **mid-trained** hoặc **post-trained** checkpoint. Sau đó họ có thể upload dataset của riêng mình hoặc sử dụng dataset đã có sẵn trong workflow.

Sau khi chọn checkpoint và nguồn dữ liệu, người dùng có thể trộn dữ liệu training của mình với các dataset curated do Nova cung cấp. AWS cho biết các dataset curated này được phân loại theo domain và nhằm giúp giữ hiệu năng chung của mô hình đồng thời giảm nguy cơ overfitting hoặc catastrophic forgetting. Đây là bước trung tâm trong cách Nova Forge cân bằng giữa specialization và khả năng tổng quát.

AWS cũng cho biết người dùng có thể tùy chọn áp dụng **Reinforcement Fine-Tuning (RFT)**. Theo bài blog, kỹ thuật này có thể được dùng để cải thiện độ chính xác về mặt factual và giảm hallucination trong các domain cụ thể. Sau khi training hoàn tất, mô hình kết quả có thể được import vào Amazon Bedrock và sử dụng trong các ứng dụng downstream.

---

## **Những điều cần biết**

Tại thời điểm ra mắt, **Amazon Nova Forge** có sẵn trong AWS Region **US East (N. Virginia)**. AWS cho biết dịch vụ này bao gồm quyền truy cập nhiều Nova model checkpoint, recipe để trộn dữ liệu độc quyền với dữ liệu huấn luyện được Amazon Nova tuyển chọn, các recipe huấn luyện đã được thiết lập sẵn, và tích hợp với cả Amazon SageMaker AI và Amazon Bedrock.

Đối với người dùng muốn tìm hiểu sâu hơn về dịch vụ, AWS đề xuất bắt đầu với **Amazon Nova User Guide** và **Amazon SageMaker AI console**. AWS cũng lưu ý rằng các tổ chức cần hỗ trợ sâu hơn hoặc chuyên biệt hơn có thể liên hệ **Generative AI Innovation Center** để được hỗ trợ trong các dự án phát triển mô hình.

---

## **Tại sao lần ra mắt này quan trọng**

Amazon Nova Forge là một bước phát triển đáng chú ý cho các tổ chức muốn kiểm soát nhiều hơn quá trình tạo mô hình mà không phải gánh toàn bộ chi phí huấn luyện frontier model từ đầu. Trong nhiều môi trường doanh nghiệp, giải pháp lý tưởng không phải là một mô hình hoàn toàn generic cũng không phải là một mô hình custom hoàn toàn từ số 0. Thay vào đó, con đường phù hợp thường là một mô hình lai: bắt đầu với một mô hình mạnh có sẵn, thích nghi nó sớm hơn và sâu hơn so với fine-tuning thông thường, giữ lại các năng lực cốt lõi, và triển khai nó trên một nền tảng sẵn sàng cho production.

Nova Forge được thiết kế chính xác cho khoảng trung gian đó. Nó cung cấp quyền truy cập các Nova checkpoint, khả năng trộn dữ liệu curated, hạ tầng training do AWS quản lý, hỗ trợ reinforcement learning trong môi trường riêng, cấu hình responsible AI linh hoạt và triển khai thông qua Amazon Bedrock. Khi kết hợp lại, những khả năng này khiến Nova Forge không chỉ đơn giản là một tính năng fine-tuning. Nó được định vị như một framework rộng hơn để xây dựng **frontier model nhận thức domain (domain-aware frontier models)** đồng thời vẫn đáp ứng các yêu cầu doanh nghiệp về hiệu năng, an toàn và tích hợp vận hành.

---

## Kết luận

Khi các tổ chức tiếp tục áp dụng generative AI, nhu cầu về những mô hình hiểu sâu dữ liệu độc quyền và domain chuyên biệt sẽ ngày càng tăng. Các phương pháp tùy chỉnh tiêu chuẩn vẫn hữu ích, nhưng chúng thường chưa đủ cho những doanh nghiệp có tri thức nội bộ phức tạp, workflow độc đáo hoặc yêu cầu domain có mức độ quan trọng cao.

Amazon Nova Forge giải quyết nhu cầu này bằng cách cho phép khách hàng bắt đầu từ các Nova checkpoint sớm, trộn dữ liệu của họ với dataset curated của Amazon, huấn luyện bằng hạ tầng SageMaker AI được quản lý và triển khai mô hình tùy chỉnh thông qua Amazon Bedrock. AWS giới thiệu đây là cách dễ hơn và tiết kiệm chi phí hơn để xây dựng frontier model kết hợp giữa năng lực nền tảng mạnh và sự phù hợp sâu với domain.

Đối với các doanh nghiệp muốn tiến xa hơn các phương pháp thích nghi nhẹ và hướng tới mức độ chuyên biệt hóa mô hình nghiêm túc hơn, Nova Forge cung cấp một con đường thực tế trong hệ sinh thái AWS. Nó giảm bớt một số rào cản lớn nhất vốn gắn liền với việc phát triển frontier model, đồng thời vẫn cho phép tổ chức kiểm soát đáng kể cách mô hình của họ được huấn luyện, thích nghi, quản trị và triển khai.

## Về tác giả

**Danilo Poccia** làm việc với các startup và các công ty ở nhiều quy mô khác nhau nhằm hỗ trợ đổi mới sáng tạo. Trong vai trò **Chief Evangelist (EMEA) tại Amazon Web Services**, ông giúp mọi người hiện thực hóa các ý tưởng, với trọng tâm đặc biệt vào **kiến trúc serverless**, **lập trình hướng sự kiện (event-driven programming)**, cũng như tác động kỹ thuật và kinh doanh của **machine learning** và **edge computing**. Ông cũng là tác giả của cuốn sách *AWS Lambda in Action*.