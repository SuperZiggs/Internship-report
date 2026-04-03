---
title: "Blog 3"
date: 2026-03-13
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Hai mươi năm Amazon S3 và xây dựng những gì tiếp theo

Amazon Simple Storage Service (Amazon S3) kỷ niệm **20 năm** là một trong những dịch vụ nền tảng quan trọng nhất của AWS. Ban đầu ra mắt vào **14 tháng 3 năm 2006**, Amazon S3 bắt đầu với một giá trị đề xuất rất đơn giản: cung cấp **storage cho internet** thông qua một web service interface cho phép developer lưu trữ và truy xuất **bất kỳ lượng dữ liệu nào, bất kỳ lúc nào, từ bất kỳ đâu trên web**. Khi ra mắt, dịch vụ gần như không được quảng bá rầm rộ, nhưng theo thời gian nó đã trở thành một trong những **building block quan trọng nhất của cloud computing**. 

Tầm nhìn ban đầu phía sau Amazon S3 không chỉ là cung cấp **object storage**, mà còn mang đến cho developer một primitive **đơn giản và đáng tin cậy**, giúp loại bỏ gánh nặng vận hành. Ở mức cơ bản, S3 giới thiệu các operation đơn giản như **PUT** để lưu object và **GET** để truy xuất chúng sau này. Tuy nhiên, điểm đổi mới sâu hơn nằm ở triết lý **trừu tượng hóa các công việc hạ tầng nặng và không tạo ra sự khác biệt**, để builder có thể tập trung xây dựng ứng dụng và dịch vụ thay vì phải quản lý infrastructure storage. 

Ngay từ đầu, Amazon S3 được định hướng bởi năm nguyên tắc cốt lõi vẫn còn định nghĩa dịch vụ này cho đến hôm nay: **security, durability, availability, performance và elasticity**. Security đảm bảo dữ liệu được bảo vệ mặc định. Durability được thiết kế ở mức **99.999999999% (11 số 9)**. Availability được xây dựng ở mọi layer của hệ thống với giả định rằng lỗi sẽ xảy ra và cần được xử lý một cách graceful. Performance được thiết kế để dịch vụ có thể lưu trữ **khối lượng dữ liệu cực lớn mà không bị suy giảm hiệu năng**, trong khi elasticity cho phép S3 **tự động scale** khi lượng dữ liệu tăng hoặc giảm mà không cần can thiệp thủ công. Những nguyên tắc này đã giúp S3 trở thành một dịch vụ “**just works**” cho hàng triệu khách hàng. 

---

## Những ngày đầu của Amazon S3

Khi Amazon S3 lần đầu ra mắt, nó đã vận hành ở quy mô khá lớn so với thời điểm đó. Ban đầu dịch vụ cung cấp khoảng **1 petabyte** tổng dung lượng lưu trữ, được phân phối trên khoảng **400 storage node**, **15 rack**, và **3 data center**, với tổng băng thông khoảng **15 Gbps**. Hệ thống được thiết kế để lưu trữ **hàng chục tỷ object**, và kích thước object tối đa khi đó là **5 GB**. Giá storage lúc đó là **0.15 USD mỗi gigabyte**. 

Dù những con số này rất ấn tượng vào năm 2006, chúng khá khiêm tốn so với quy mô mà Amazon S3 hỗ trợ ngày nay. Tuy vậy, các lựa chọn thiết kế ban đầu đã đặt nền móng cho một dịch vụ có thể **tiến hóa mạnh mẽ trong hai thập kỷ** mà vẫn **không phá vỡ backward compatibility** cho người dùng. Một trong những điểm đáng chú ý nhất của S3 là **code viết cho nó từ năm 2006 vẫn có thể chạy được đến hôm nay**. Mặc dù infrastructure, phần cứng storage và code xử lý request đã trải qua nhiều thế hệ thay đổi, AWS vẫn duy trì khả năng tương thích trong khi tiếp tục đổi mới bên trong hệ thống.

---

## Amazon S3 ngày nay: quy mô vượt ngoài tưởng tượng

Hai mươi năm sau, Amazon S3 đã mở rộng vượt xa quy mô ban đầu. Hiện nay nó lưu trữ **hơn 500 nghìn tỷ object** và phục vụ **hơn 200 triệu request mỗi giây** trên toàn cầu. Dịch vụ vận hành trên **hàng trăm exabyte dữ liệu** trong **123 Availability Zone** trải rộng qua **39 AWS Region**, phục vụ **hàng triệu khách hàng** trên toàn thế giới. Kích thước object tối đa cũng tăng đáng kể, từ **5 GB** khi ra mắt lên **50 TB**, tương đương mức tăng **10.000 lần**. 

Song song với đó, chi phí storage cũng giảm mạnh theo thời gian. Giá đã giảm từ **0.15 USD mỗi gigabyte** năm 2006 xuống chỉ còn **hơn 0.02 USD mỗi gigabyte** hiện nay, tương đương khoảng **85% giảm giá**. AWS cũng mở rộng khả năng tối ưu chi phí thông qua nhiều storage tier khác nhau. Ví dụ, khách hàng sử dụng **Amazon S3 Intelligent-Tiering** đã tiết kiệm tổng cộng hơn **6 tỷ USD chi phí storage** so với việc dùng **Amazon S3 Standard**.

Một cột mốc quan trọng khác là ảnh hưởng của **S3 API**. Qua nhiều năm, API này đã trở thành **chuẩn tham chiếu của ngành** cho object storage. Nhiều vendor hiện cung cấp hệ thống storage tương thích S3, nghĩa là kiến thức, tooling và best practice phát triển xung quanh Amazon S3 thường có thể áp dụng sang các môi trường storage khác. Sự phổ biến này phản ánh **tính ổn định lâu dài và ảnh hưởng lớn của dịch vụ**. 

---

## Engineering phía sau S3 ở quy mô cực lớn

Khả năng vận hành của Amazon S3 ở quy mô khổng lồ như vậy phụ thuộc vào **đổi mới kỹ thuật liên tục**. Một phần quan trọng của durability trong S3 đến từ một hệ thống **microservices phân tán** liên tục kiểm tra **từng byte dữ liệu** trên toàn bộ storage fleet. Các dịch vụ audit này phát hiện dấu hiệu suy giảm dữ liệu và tự động kích hoạt hệ thống repair khi cần thiết. AWS mô tả S3 được thiết kế để **không mất dữ liệu (lossless)**, với mục tiêu durability **11 số 9** phản ánh cách các hệ thống replication và repair được thiết kế và vận hành.

AWS cũng áp dụng **formal methods và automated reasoning** cho một số phần của môi trường production S3. Ví dụ, khi engineer cập nhật code trong **indexing subsystem**, các chứng minh tự động được dùng để xác minh rằng **consistency guarantees** vẫn chính xác. Các phương pháp tương tự cũng được sử dụng trong những khu vực như **cross-Region replication** và **access policies**, giúp AWS **kiểm chứng hành vi quan trọng bằng toán học** trong một dịch vụ mà tính đúng đắn là cực kỳ quan trọng.

Trong **8 năm qua**, AWS cũng đã dần viết lại các thành phần quan trọng về hiệu năng trong **S3 request path** bằng **Rust**. Điều này bao gồm các hệ thống **blob movement** và **disk storage**, và vẫn đang tiếp tục mở rộng sang các component khác. Rust không chỉ cung cấp **hiệu năng runtime mạnh**, mà còn đảm bảo **memory safety và type safety ở compile-time**, loại bỏ cả một lớp bug phổ biến. Ở quy mô của Amazon S3, những lợi ích về reliability và correctness này đặc biệt có giá trị.

Một triết lý thiết kế khác được nhấn mạnh trong blog là **scale tự nó trở thành lợi thế**. Các engineer của AWS thiết kế S3 sao cho **sự tăng trưởng giúp cải thiện toàn bộ hệ thống**. Khi workload tăng và đa dạng hơn, chúng trở nên **ít tương quan hơn (de-correlated)**, điều này có thể cải thiện độ tin cậy và độ bền vững cho tất cả khách hàng. Tư duy này là một phần quan trọng giúp S3 tiếp tục phát triển trong khi vẫn duy trì sự ổn định suốt hai thập kỷ. 

---

## Hướng tới tương lai: tương lai của Amazon S3

Hiện nay AWS định vị Amazon S3 **không chỉ là object storage**. Tầm nhìn dài hạn là biến S3 thành **nền tảng chung cho mọi workload dữ liệu và AI**. Ý tưởng là khách hàng chỉ cần **lưu dữ liệu một lần trong S3**, sau đó có thể làm việc trực tiếp với dữ liệu đó mà **không cần di chuyển sang nhiều hệ thống chuyên biệt khác nhau**. Cách tiếp cận này giúp giảm độ phức tạp, giảm chi phí, và tránh việc tạo ra nhiều bản sao dữ liệu trùng lặp trong các môi trường khác nhau.

Những sản phẩm ra mắt gần đây cho thấy AWS đang mở rộng S3 theo hướng đó:

- **S3 Tables** cung cấp **Apache Iceberg tables được quản lý hoàn toàn**, với cơ chế bảo trì tự động giúp cải thiện hiệu quả truy vấn và giảm chi phí storage theo thời gian.
- **S3 Vectors** bổ sung khả năng **lưu trữ vector native** cho các workload **semantic search** và **retrieval-augmented generation (RAG)**, hỗ trợ tối đa **2 tỷ vector mỗi index** với **độ trễ truy vấn dưới 100 ms**.
- **S3 Metadata** giới thiệu khả năng **metadata tập trung** để khám phá dữ liệu tức thì, giảm nhu cầu phải recursively list các bucket rất lớn và cải thiện **time-to-insight** cho các kịch bản data lake. 

Blog cũng nhấn mạnh tốc độ adoption mạnh mẽ của các khả năng mới này. Chỉ trong **5 tháng**, từ **tháng 7 đến tháng 12 năm 2025**, khách hàng đã tạo hơn **250.000 index**, ingest hơn **40 tỷ vector**, và thực thi hơn **1 tỷ query** với **S3 Vectors**. Những khả năng này được thiết kế để mở rộng phạm vi của S3 sang **analytics và AI**, trong khi vẫn giữ **cấu trúc chi phí quen thuộc và sự đơn giản trong vận hành** của dịch vụ. 

---

## Kết luận

Lịch sử 20 năm của Amazon S3 phản ánh cả **sự liên tục và sự chuyển đổi**. Từ một dịch vụ storage đơn giản cho developer, nó đã phát triển thành một **nền tảng toàn cầu** hỗ trợ **hàng trăm exabyte dữ liệu**, **hàng trăm triệu request mỗi giây**, và một hệ sinh thái rộng lớn gồm ứng dụng, hệ thống analytics và workload AI. Dù tăng trưởng mạnh mẽ như vậy, các nguyên tắc cốt lõi **security, durability, availability, performance và elasticity** vẫn không thay đổi. 
Câu chuyện của Amazon S3 cũng đáng chú ý vì AWS đã **giữ được backward compatibility** trong khi liên tục thiết kế lại hệ thống bên trong. Khách hàng được hưởng lợi từ **tính năng mới và chi phí tốt hơn** mà **không cần viết lại ứng dụng cũ**. Trong tương lai, AWS xem S3 **không chỉ là storage**, mà là **nền tảng chung để xây dựng các hệ thống dữ liệu lớn và AI thế hệ tiếp theo**. 

---

## Về tác giả

**Sébastien Stormacq** là tác giả của bài viết AWS News Blog này. Ông đã viết code từ khi lần đầu sử dụng **Commodore 64** vào giữa những năm 1980. Tại AWS, ông tập trung vào việc **truyền cảm hứng cho builder** để khai thác giá trị từ AWS Cloud thông qua sự kết hợp của **đam mê, advocacy cho khách hàng, sự tò mò và sáng tạo**. Các lĩnh vực quan tâm của ông bao gồm **software architecture, developer tools và mobile computing**. 