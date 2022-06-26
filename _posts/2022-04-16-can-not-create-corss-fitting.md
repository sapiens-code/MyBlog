---
title: Failed To Insert Cross
date: 2022-04-16 00:00:00 Z
layout: post
comments: true
tag:
    - Revit Addin
img: can-not-create-cross-fitting/cable-tray.jpg
---

Xin chào mọi người. Hôm nay mình sẽ nói về cách fix lỗi không tạo được Cross Fitting trong Revit. Thông thường khi tạo cross chúng ra gọi hàm **NewCrossFitting** và truyền vào 4 Connector. Nhưng đôi khi nó throw ra một lỗi rất ức chế với thống báo **Thrown when cross fitting cannot be created** và không có dữ kiện gì thêm nữa, đây là một vấn đề với tài liệu của Revit API.

Điều kiệu API mong muốn là các Connector được xắp xếp theo thứ tự Main-Main-Side-Side, tức là phải đưa 2 Connector đối diện vào trước 2 Connector 2 bên.

Ngoài ra hãy chắc chắn trong Routing References cross đã được set. Fitting không tạo có thể do bạn chưa set Cross trong Routing Preferences. Nếu bạn có vấn đề nào thắc mắc hãy bình luận bên dưới nhé.

[nguồn](https://adndevblog.typepad.com/aec/2015/06/revitiapi-crossfitting-creation-problem-invalidoperationexception-failed-to-insert-cross.html){:target="\_blank"}