---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Một lỗi AWS IAM khiến mình hiểu rằng: "Role có quyền" chưa chắc dịch vụ đã dùng được

Bài viết chia sẻ một kinh nghiệm thực tế khi làm việc với AWS IAM, giải thích vì sao một IAM Role dù đã được cấp đầy đủ quyền vẫn có thể không hoạt động nếu cấu hình chưa chính xác. Đồng thời, bài viết làm rõ vai trò của **Trust Policy**, **Permission Policy** và **iam:PassRole** trong quá trình một dịch vụ AWS sử dụng IAM Role.

Các nội dung chính của bài viết:

* Phân biệt **Trust Policy** và **Permission Policy** trong IAM Role.
* Hiểu cách Trust Policy xác định dịch vụ AWS nào được phép assume IAM Role.
* Tìm hiểu cách Permission Policy quy định các hành động mà Role được phép thực hiện trên tài nguyên AWS.
* Giải thích mục đích của quyền **iam:PassRole** khi cấu hình các dịch vụ AWS.
* Phân tích rủi ro khi cấp quyền `iam:PassRole` quá rộng.
* Checklist kiểm tra nhanh khi một AWS Service không thể sử dụng IAM Role:
  * Kiểm tra Trust Policy.
  * Kiểm tra Permission Policy.
  * Kiểm tra Resource ARN.
  * Xác minh quyền `iam:PassRole`.
  * Kiểm tra Permission Boundary và Explicit Deny.
* Giới thiệu các thực hành bảo mật IAM theo khuyến nghị của AWS như nguyên tắc **Least Privilege** và sử dụng **IAM Roles** thay cho Access Key dài hạn.

Bài viết nhấn mạnh rằng để một IAM Role hoạt động đúng, không chỉ cần cấp đủ quyền mà còn phải cấu hình chính xác **dịch vụ nào được phép sử dụng Role**, **ai được phép truyền Role** và **Role được phép thao tác trên tài nguyên nào**.

### Link bài viết

https://www.facebook.com/groups/660548818043427/?multi_permalinks=2229747654456861&ref=share

### Tài liệu tham khảo

* AWS IAM Security Best Practices  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html

* IAM Roles User Guide  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html

* Grant Permission to Pass a Role to an AWS Service  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html

* Policies and Permissions in IAM  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html

* Troubleshoot IAM Roles  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html

