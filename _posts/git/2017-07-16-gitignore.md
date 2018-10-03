---
layout: blog
istop: true
title: "Git bỏ qua các quy tắc và  các quy tắcđúng cho .gitignore"
background-image: image/git.jpg
date:  2017-07-16 23:45:56
category: Git
tags:
- github
- git
- gitignore
- Git bỏ qua các quy tắc
---

# Nhận ra nhu cầu
Trong git nhập nếu bạn muốn bỏ qua một tập tin hoặc thư mục, bạn không muốn tập tin này hoặc thư mục vào kho, bạn có thể sử dụng các tập tin sửa đổi trong phương pháp thư mục gốc .gitignore (nếu không, bạn cần phải tự tạo ra tập tin này cho mình). Mỗi dòng của tệp này chứa một quy tắc phù hợp như:

# Tạo tệp gitignore

```
touch .gitignore
```
## Lưu ý: Git bỏ qua các quy tắc


```
# Đây là một bình luận - sẽ bị bỏ qua bởi Git
 
*.a       # Bỏ qua tất cả các tệp kết thúc bằng .a
!lib.a    # Nhưng ngoại trừ lib.a
/-liberxuesite     # Chỉ cần bỏ qua tệp liberxuesite trong thư mục gốc của dự án, không bao gồm subdir / liberxuesite
liberxue/    # Bỏ qua thư mục liberxue/ Tất cả các tệp trong thư mục và chính thư mục đó
doc/*.txt # Bỏ qua doc / notes.txt nhưng không bỏ qua doc / server / arch.txt
```
## Gitignore bỏ qua lý do quy tắc không có hiệu lực

Các quy tắc rất đơn giản, không làm quá nhiều để giải thích, nhưng đôi khi trong quá trình phát triển dự án, một ý thích bất ngờ muốn thư mục hay tập tin nào đó thêm để bỏ qua các quy tắc, quy định như đã nêu ở trên sau khi phát hiện không có hiệu lực vì chỉ bỏ qua .gitignore Các tệp không được theo dõi, nếu một số tệp đã được bao gồm trong quản lý phiên bản, sửa đổi .gitignore không hợp lệ. Sau đó, giải pháp là xóa bộ nhớ cache cục bộ (thay đổi trạng thái không được theo dõi), và sau đó gửi:

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'

```
