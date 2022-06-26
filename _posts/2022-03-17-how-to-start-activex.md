---
title: Tương Tác Với Autocad Từ Revit
date: 2022-03-17 00:00:00 Z
layout: post
comments: true
tag:
    - Revit Addin
    - ActiveX
img: how-to-start-activex/autocad-mechanica.jpg

---

Gần đây mình có làm một dự án chuyển đối tượng từ Cad qua Revit và gặp một só khó khăn. khi bạn viết Revit Addin đôi khi có những dữ liệu bạn muốn nhận từ Autocad để xử lý cho Addin của mình. Thông thường ta có thể viết một Addin phụ trong Cad để xuất thông tin ra file để đọc trong Revit. Nhưng Autodesk còn hỗ trợ cho chúng ta một bộ API rất hữu ích để tương tác trực tiếp với Autocad từ môi trường bên ngoài là **Autocad Interop** sử dụng Active X. Active X cho phép bạn tương tác với cả môi trường bên trong và bên ngoài Autocad. Dưới đây là một số code mẫu mình tìm hiểu được, chạy trong môi trường Revit, viết bằng c#.
## Kết nối với Autocad và lấy về Active Document
Để sử dụng bạn phải add hai thư viện là **Autodesk.AutoCAD.Interop** và **Autodesk.AutoCAD.Interop.Common** nằm trong thư mục **C:\Program Files\Autodesk\yourcadversion**.
``` bash
AcadApplication App = (AcadApplication)Marshal.GetActiveObject(AutoCAD.Application);
AcadDocument thisDrawing = App.ActiveDocument;
Message.Show(thisDrawing.FullName);
```
Đoạn mã trên sử dụng lớp Marshal để lấy về Instance của chương trình Cad đang chạy. Hãy lưu ý là đang chạy nhé, vì nến không sẽ gây ra lỗi. Sau đó ta lấy về documnet đang mở và in ra tên đầy đủ của documnet đó. bạn cũng có thể mở documnet bằng lệnh **App.Documents.Open(path);**. Dưới đây là sơ đồ tổng quang của Cad Api.
![img1](/assets/img/how-to-start-activex/sodo.png)
## Thao tác với Document
### Lấy về tất cả Entity
``` bash
AcadApplication App = (AcadApplication)Marshal.GetActiveObject(ProgId);
AcadDocument thisDrawing = App.ActiveDocument;
AcadModelSpace modelSpace = thisDrawing.ModelSpace;
for (int i = 0; i < modelSpace.Count; i++)
{
    AcadEntity item = modelSpace.Item(i);
                
}
```
### Lấy center point của text
``` bash
private void GetText(AcadEntity item)
{
    if (item is AcadText text)
    {
        object cMaxP;
        object cMinP;
        text.GetBoundingBox(out cMinP, out cMaxP);

        var minP = ConvertCadPoint(cMinP as double[]);
        var maxP = ConvertCadPoint(cMaxP as double[]);
        //get text center point
        var centerP = minP.MidPoint(maxP);
    }
}
```
### Lấy entity bằng cách chọn từ Cad
Bạn có thể lấy entity bằng cách quét chọn trực tiếp từ cad. Ngoài ra, chúng ta có thể filter những đối tượng mà ta muốn chọn. Ở đây mình sẽ lọc ra các Polyline.
``` bash
AcadSelectionSet select = null;
try
{
    select = ThisDrawing.SelectionSets.Add("ss2");
}
catch//if selection has exist
{
    select = ThisDrawing.SelectionSets.Item("ss2");
    select.Clear();
}

Int16[] FilterType = new Int16[1];
object[] FilterData = new object[1];
FilterType[0] = 0;
FilterData[0] = "LWPolyline,Polyline";

var entities = select.SelectOnScreen(FilterType, FilterData);
Message.Show(entities.Count());
```

Ngoài ra còn rất nhiều tính năng rất hữu ích mà bạn có thể khám phá như vẽ các đối tượng, chạy ngầm CAD... mà mình không thể nêu hết ở đây được. Nếu bạn muốn tìm hiểu thêm thì mình có để link tài liệu bên dưới của Autodesk nhưng được ví dụ băng VBA và AutoLISP nên những bạn sử dụng C# chịu khó ngâm cứu nhé:grin:. Cảm ơn các bạn đã theo dõi.

https://help.autodesk.com/view/OARX/2022/ENU/
