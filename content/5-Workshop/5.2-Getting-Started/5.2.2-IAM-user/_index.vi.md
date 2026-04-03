---
title : "Tạo một IAM User"
date : 2026-03-25
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
---

Các dịch vụ trong AWS, ví dụ như Athena, yêu cầu bạn cung cấp thông tin xác thực khi truy cập, để dịch vụ có thể xác định xem bạn có đủ quyền hạn truy cập tài nguyên của nó hay không. Giao diện điều khiển (console) cần mật khẩu của bạn. Bạn có thể tạo khóa truy cập (access keys) cho tài khoản AWS để truy cập qua giao diện dòng lệnh hoặc API. Tuy nhiên, tôi không khuyến khích bạn truy cập AWS bằng thông tin xác thực của tài khoản AWS gốc; tôi khuyên bạn nên sử dụng AWS Identity and Access Management (IAM). Hãy tạo một IAM user, sau đó thêm user đó vào nhóm IAM với quyền Administrator hoặc cấp trực tiếp quyền Administrator cho user này. Khi đó, bạn có thể truy cập AWS bằng một URL đặc biệt và bằng thông tin xác thực của IAM user.

Nếu bạn đã đăng ký tài khoản AWS nhưng chưa tạo một IAM user cho chính mình, bạn có thể tạo một user thông qua IAM console. Nếu bạn chưa quen với việc sử dụng console, hãy xem "Working with the AWS Management Console" để có cái nhìn tổng quan.

**Để tạo một IAM user cho bản thân và thêm user vào nhóm Administrators**

1.  Sử dụng địa chỉ email và mật khẩu tài khoản AWS của bạn để đăng nhập với tư cách `root user` của tài khoản AWS vào IAM console tại [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/). Lưu ý: Tôi thực sự khuyên bạn tuân thủ thực tiễn tốt nhất là sử dụng IAM user với quyền Administrator bên dưới và cất giữ thông tin xác thực của root user ở nơi an toàn. Chỉ đăng nhập với tư cách root user để thực hiện một vài tác vụ quản lý tài khoản và quản trị dịch vụ.

---

1.  Trong ngăn điều hướng (navigation pane) của console, hãy chọn **Users**, sau đó chọn **Create user**.
    
2.  Ở mục **User name**, nhập thông tin `Administrator`.
    
3.  Chọn hộp kiểm bên cạnh **AWS Management Console access**, chọn **Custom password**, và gõ mật khẩu của người dùng mới vào textbox. Bạn cũng có thể tuỳ chọn **Require password reset** để buộc người dùng tạo một mật khẩu mới ở lần đăng nhập tiếp theo.
    
4.  Chọn **Next: Permissions**.
    
5.  Trên trang **Set permissions**, chọn **Add user to group**.
    
6.  Chọn **Create group**.
    
7.  Trong hộp thoại **Create group**, tại phần **Group name**, hãy nhập `Administrators`.
    
8.  Tại phần **Filter policies**, chọn hộp kiểm cho **AWS managed - job function**.
    
9.  Trong danh sách policy, chọn hộp kiểm cho **AdministratorAccess**. Sau đó chọn **Create group**.
    
10. Quay trở lại danh sách nhóm (groups list), chọn hộp kiểm cho nhóm bạn vừa tạo. Nhấn **Refresh** nếu cần để cập nhật danh sách nhóm.
     
11. Chọn **Next: Tags** để thêm metadata cho user bằng cách gắn thẻ dữ liệu dưới dạng các cặp key-value.
     
12. Chọn **Next: Review** để xem qua danh sách các thành viên của nhóm được thêm vào người dùng mới. Nếu đã sẵn sàng tiến hành, nhấn chọn **Create user**.
     

---

Bạn có thể sử dụng cùng quy trình này để tạo nhiều nhóm và người dùng hơn, và cấp cho users quyền truy cập tài nguyên AWS của mình.

Để đăng nhập dưới tư cách IAM user mới này, hãy đăng xuất khỏi AWS console, sau đó sử dụng URL bên dưới, với việc thay your\_aws\_account\_id bằng chính mã số tài khoản AWS của bạn không bao gồm dấu gạch ngang (ví dụ, nếu mã số đó là 1234-5678-9012, thì ID tài khoản AWS của bạn là 123456789012):

[https://your\_aws\_account\_id.signin.aws.amazon.com/console/](https://your_aws_account_id.signin.aws.amazon.com/console/) 

Nhập tên IAM user (không phải email của bạn) và password mà bạn mới tạo. Khi bạn đăng nhập, thanh điều hướng sẽ hiển thị "your\_user\_name @ your\_aws\_account\_id".

Nếu không muốn URL trang đăng nhập chứa ID tài khoản của bạn, bạn hoàn toàn có thể chọn cho tài khoản của mình một bí danh (Alias). Từ IAM Console hãy chọn Dashboard ở thanh điều hướng phụ. Tại trang dashboard này, chọn Customize rồi nhập một bí danh, ví dụ tên công ty của bạn. Để có thể đăng nhập với bí danh mới này, sử dụng URL sau:

[https://your\_account\_alias.signin.aws.amazon.com/console/](https://your_account_alias.signin.aws.amazon.com/console/) 

Để xác nhận URL đường dẫn đăng nhập dành cho IAM user của account bạn, chỉ việc kiểm tra mục IAM users sign-in link trong Dashboard của IAM.