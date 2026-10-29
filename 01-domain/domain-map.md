# Domain Map — Enterprise CMS

**Trạng thái:** Proposed — chờ review và xác nhận  
**Phạm vi:** Enterprise CMS, đợt phát hành đầu tiên

## 1. Mục tiêu

Phân chia hệ thống theo trách nhiệm nghiệp vụ ở mức tối thiểu. Mỗi Domain đại diện cho một nhóm trách nhiệm; không tách Domain chỉ vì có module kỹ thuật, API hoặc màn hình riêng.

## 2. Danh sách Domain đề xuất

| Domain | Trách nhiệm chính | Phạm vi đề xuất |
|---|---|---|
| Content Management | Quản lý nội dung trong CMS | Nội dung do CMS quản lý; không sở hữu dữ liệu chuẩn của hồ sơ pháp lý hoặc hợp đồng |
| Site & Publishing | Quản lý Website và việc xuất bản nội dung | Website là kênh đọc duy nhất của đợt đầu; Website có đăng nhập |
| Access Control | Quản lý xác thực và quyền truy cập trong phạm vi CMS/Website | Chi tiết phối hợp quyền với DMS/CLM chưa được xác định |
| Integration | Kết nối CMS với hệ thống bên ngoài | Dự kiến tích hợp DMS và CLM qua API ở P4; thời điểm P4 chưa phải cam kết chính thức |

## 3. Ranh giới trách nhiệm đã xác định

- CMS quản lý nội dung Website.
- DMS là nguồn dữ liệu chuẩn cho tài liệu/hồ sơ và phiên bản được quản lý trong DMS.
- CLM là nguồn dữ liệu chuẩn cho dữ liệu hợp đồng và vòng đời hợp đồng được quản lý trong CLM.
- CMS chỉ tham chiếu hồ sơ qua API, không trở thành nguồn dữ liệu chuẩn thay cho DMS hoặc CLM.
- Việc tích hợp DMS/CLM được dự kiến ở P4, không được hiểu là đã triển khai trong đợt đầu.

## 4. Điểm chưa xác định

- Ranh giới chi tiết giữa Content Management và Site & Publishing.
- Cách phân quyền và phối hợp quyền giữa CMS/Website với DMS/CLM.
- Danh sách Use Case thuộc từng Domain.
- Thời điểm triển khai tích hợp DMS/CLM chính thức.

Các điểm trên không được tự suy diễn thành yêu cầu đã được xác nhận.

## 5. Nguyên tắc sử dụng Domain Map

- Domain Map là mô hình phân chia trách nhiệm nghiệp vụ, không phải sơ đồ kiến trúc kỹ thuật.
- Use Case sẽ được ánh xạ vào Domain sau khi cấu trúc này được xác nhận.
- Khi có thông tin mới, cần ghi rõ nội dung đã xác nhận, giả định và điểm chưa rõ.
- Không xem đề xuất này là quyết định cuối cùng khi chưa được người có thẩm quyền phê duyệt.
