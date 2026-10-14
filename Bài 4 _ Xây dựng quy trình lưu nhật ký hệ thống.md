**Bài 4 : Xây dựng quy trình lưu nhật ký hệ thống** 

### **Phần 1: So sánh**

| Tiêu chí | \> | \>\> |
| ----- | ----- | ----- |
| Cơ chế | Giống Out-File: xóa sạch nội dung cũ rồi ghi mới | Giống Out-File \-Append: ghi tiếp vào cuối file |
| Rủi ro mất dữ liệu | Cao: gõ nhầm cuối tháng thì 30 ngày báo cáo bị thay bằng 1 ngày | Thấp: không xóa dữ liệu cũ, nhưng file phình to và có thể ghi trùng dòng |
| Ngữ cảnh | Ca sáng: chạy 1 lần/ngày, tạo báo cáo mới | Theo dõi 24/7: chạy nhiều lần, cần tích lũy |
| Encoding | UTF-16 (PowerShell 5.1), dễ lỗi ký tự khi mở bằng công cụ không hỗ trợ | Giống \> |

### **Phần 2: Lựa chọn**

Chọn **ghi nối tiếp có chỉ định encoding**:

powershell  
Get-Content server.log | Select-String "ERROR" |  
    Add-Content \-Path daily\_report.txt \-Encoding utf8

**Bởi vì:**

1. Lỗi phát sinh liên tục trong ngày nên cần giữ lại toàn bộ, \> sẽ xóa kết quả các lần trước.  
2. Không có nguy cơ gõ nhầm làm mất báo cáo cả tháng.  
3. \-Encoding utf8 giúp file đọc được trên mọi công cụ và không bị lẫn encoding.

