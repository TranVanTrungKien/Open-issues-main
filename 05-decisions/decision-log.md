# Decision Log

| ID | Quyết định | Trạng thái | Người quyết định | Ngày | Lý do | Nguồn |
|---|---|---|---|---|---|---|
| BR-01 | CMS hybrid quản lý nội dung Website có đăng nhập; người dùng khách chưa đăng nhập không truy cập được tài liệu. CMS quản lý quyền Website và chỉ tham chiếu hồ sơ qua API. DMS là nguồn chuẩn cho tài liệu/hồ sơ và phiên bản được quản lý trong DMS; CLM là nguồn chuẩn cho dữ liệu và vòng đời hợp đồng được quản lý trong CLM. Khi DMS/CLM không khả dụng, hiển thị lỗi. | Confirmed | Người dùng/PO | 2026-10-29 | Chốt ranh giới nghiệp vụ CMS–DMS–CLM, kênh Website và truy cập tài liệu | [Quy mô CMS doanh nghiệp](chatgpt-conversation://6abe2409-0168-83ec-9477-911dbce7a104) |

## Trạng thái hợp lệ

- **Proposed:** đang đề xuất, chưa được xác nhận.
- **Confirmed:** có bằng chứng xác nhận và ghi rõ người/nguồn xác nhận.
- **Superseded:** đã được thay thế bởi quyết định khác; cần liên kết quyết định mới.

> Việc merge PR không đồng nghĩa với phê duyệt nghiệp vụ từ khách hàng hoặc PO.

**Ghi chú:** OI-01 đã đóng ở cấp quyết định nghiệp vụ. P4 vẫn là giả định tạm thời; chi tiết API và luồng kỹ thuật ủy quyền thuộc thiết kế tích hợp.
