---
title: Cấu Trúc Của Một Dự Án Revit Addin
date: 2022-02-16 00:00:00 Z
layout: post
tag:
    - Revit Addin
img: revit-addin-structure/panel.jpg
---

Xin chào các bạn. Bài viết trước mình đã giới thiệu về [Revit Macro]({% link _posts/2022-02-09-how-to-write-macro.md %}). Revit Macro được tích hợp sẵn trong Revit nên ta có thể dễ dàng sử dụng mà không cần cài thêm bất kì phần mềm thứ 3 nào. Nhưng với những dự án lớn, Revit Macro rất khó mở rộng và bảo trì. Rất may, Revit cung cấp cho chúng ta thêm một sự lựa chọn là Revit Addin. Nó như là một ứng dụng mở rộng của Revit giúp chúng ta dễ dàng phát triển, bảo trì code. Để tạo một Revit Addin sẽ lằng nhằng hơn một chút vì ta phải cài đặt thêm các phần mềm liên quan như Visual Studio, .Net Framework, Addin Manager... Các bạn có thể tham khảo cách cài đặt môi trường và tạo mới một dự án ở [đây](https://chuongmep.com/Start-With-RevitAPI). 
Bạn đã cài đặt xong mọi thứ cần thiết? OK, giờ chúng ta có thể bắt đầu tìm hiểu cấu trúc của một dự án Revit Addin.
## Bước 1: TẠO DỰ ÁN
Mình sử dụng Visual Studio 2017 và Revit 2019 để tạo dự án, các bạn có thể thoải mái sử dụng các phiên bản khác vì cách tạo vẫn tương tự nhau.
Đầu tiên, vào Visual Studio chọn New Project

![img1](/assets/img/revit-addin-structure/img1.png)

 Chọn Class Library .NET Framework và đặt tên cho dự án, ở đây mình tạm đặt là **FirstRevitAddin**. Sau đó ấn nút ok, Visual Studio sẽ tạo cho chúng ta một dự án. Revit Addin về cơ bản là một Class Library được Revit đọc và thực thi.

