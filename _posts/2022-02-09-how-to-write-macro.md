---
title: Làm thế nào để viết một Revit Macro đơn giản
date: 2022-02-09 00:00:00 Z
layout: post
img: how-to-write-a-macro/ribbon.png
---

Xin chào! Hôm nay mình sẽ hướng dẫn cách viết một Macro đơn giản cho phần mềm Revit. Macro là một đoạn mã dùng để tự động hóa một hành động trong Revit như tự động tạo, xóa, sữa các đối tượng, quản lý thông tin của dự án, tự động xuất thông tin dự án vv... Giúp cho công việc của các kỹ sư thiết kế trở nên dễ dàng hơn. Mỗi phiên bản Revit điều đã được tích hợp sẵn Macro vì thế bạn không cần cài đặt thêm bất cứ phần mềm nào. Trong các ví dụ dưới đây mình sẽ sử dụng revit 2020 và ngôn ngữ c#, giờ chúng ta sẽ bắt đầu đi vào các bước.
## **Bước 1: Mở Revit Macro Manager**
Đầu tiên các bạn vào dự án, sau đó vào tab **Manage** và chọn icon **Macro Manager**.

![ribon](/assets/img/how-to-write-a-macro/ribbon.png)

Cửa sổ Macro Manager bật lên, bạn sẽ thấy 2 tab là **application** và **tệp dự án** của bạn . Những Macro viết trên tab application sẽ sự dụng được lưu vào cấu hình người dùng của Revit, các Macro này có thể sữ dụng cho bất kỳ mô hình nào nhưng chỉ với người dùng đã tạo nó. Những Macro viết trên tab tệp dự án của bạn có thể sữ dụng cho bất kỳ ai mở mô hình đó.

![img1](/assets/img/how-to-write-a-macro/img1.png)

## **Bước 2: Tạo module**
Các Macro được câu trúc theo dạng **Module**, một Module bao gồm nhiều Macro, tên Module và Macro không được chứa ký tự đặc biệt và khoảng trắng.

![img7](/assets/img/how-to-write-a-macro/img7.png)

Nhấn nút Module trong phần **Create** để tạo một Module mới. Revit hỗ trợ cho chúng ta 4 ngôn ngữ là C#, Python, Ruby và VB.net. Các bạn nhập tên, chọn ngôn ngữ và nhập mô tả(có thể để trống) cho Module và chọn nút **ok**. Sau đó Revit sẽ tự động tạo và mở file bằng chương trình SharpDevelop. giao diện chường trình như hình dưới.

![img1](/assets/img/how-to-write-a-macro/img2.png)

Sau khi có Module bạn có thể tạo một Macro bằng cách nhấn vào Module vừa tạo và chọn nút Macro trong phần Create. Bản chất của Macro giống như một hàm(funtion) trong một tệp Module. Hàm là một cấu trúc cơ bản trong các ngôn ngữ lập trình, mình có thể nói về chúng trong các bài viết khác. Sau khi tạo một Macro, chúng sẽ có cấu trúc như hình dưới.

![img1](/assets/img/how-to-write-a-macro/img3.png)

Theo như trong hình thì Macro của chúng ta là hàm MacroTest() và đó là chỗ chúng ta sẽ viết mã.
## **bước 3: Viết mã**
Bạn gõ vào hàm đoạn mã sau đây, Macro đầu tiên của bạn sẽ đơn giản là in ra màn hình dòng chữ Hello World.
``` bash
public void MacroTest()
{
	TaskDialog.Show("hello","hello world");
}
```
## **bước 4: build**
Sau khi viết xong bạn chọn **build project** trong tab **Build** hoặc nhấp **F9** để build Macro.

![img1](/assets/img/how-to-write-a-macro/img8.png)

Nếu có bất kỳ lỗi hay cảnh báo nào chúng sẽ xuất hiện ở cửa sổ này.

![img1](/assets/img/how-to-write-a-macro/img9.png)

Nếu ngược lại bạn sẽ thấy dòng chữ **Build finished successfully** ỡ góc trái dưới cùng. Quay lại Revit, chọn macro vừa tạo và nhấn nút **Run**.

![img1](/assets/img/how-to-write-a-macro/img4.png)

Và đây là kết quả.

![img1](/assets/img/how-to-write-a-macro/img5.png)

## **Một vài thay đổi**
Vừa rồi chúng ta vừa tạo được một Macro vô cùng đơn giản trong Revit. Sau đây chúng ta sẽ sử dụng Macro để can thiệp sâu hơn vào mô hình.
### Get All Wall
``` bash
public void getAllWall()
{
    //get all wall
    IEnumerable<Element> lstWall = new FilteredElementCollector(Document).OfCategory(BuiltInCategory.OST_Walls)
        .WhereElementIsNotElementType();
    // add the name of wall into a string
    string s = string.Empty;
    foreach (Element element in lstWall) {
        s += element.Name + "\n";
    }
    //show the wall list
    TaskDialog.Show("list wall",s);
}
```
Đoạn code trên cho phép bạn lấy tên của tất cả các Wall từ dự án hiện tại và in ra màn hình, đây là kết quả.

![img1](/assets/img/how-to-write-a-macro/img6.png)

### Delete All Wall
Đây là đoạn mã xóa wall từ danh sách đã lấy được ỡ trên
``` pash
public void deleteAllElement()
{
    //get all walls in project
    ICollection<ElementId> lstWall = new FilteredElementCollector(Document).OfCategory(BuiltInCategory.OST_Walls)
        .WhereElementIsNotElementType().ToElementIds();
    //start a transaction
    using(Transaction tran = new Transaction(Document))
    {
        
        tran.Start("start");
        //delete list wall
        Document.Delete(lstWall);
        tran.Commit();
        
    }
    //notification
    TaskDialog.Show("delete walls","successfully");
}
```


## kết
Viết macro là bước đầu để tự động hóa những công việc lặp đi lặp lại khi vẽ. Có được kỹ năng lập trình Revit sẽ giúp bạn tăng hiệu xuất làm việc lên rất nhiều nhưng đòi hỏi bạn phải kiên trì thực hành và lấy đi một phần thời gian của bạn để làm chủ được nó. Ngoài viết mã bằng Macro ra bạn cũng có thể sử dụng một công cụ trực quan rất mạnh mẽ là Dynamo hoặc viết Addin cho revit. Những bài viết sau mình sẽ viết thêm về lập trình **Revit Addin** và **Dynamo** cũng như tương tác sâu hơn với hệ thống của Revit.
Các bạn cũng có thể tìm những Addin hữu dụng trực tiếp trên store của Autodesk. Nếu có bất kỳ thắc mắc gì hãy liên hệ trực tiếp với mình nhé. Chúc các bạn thành công.


