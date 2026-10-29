# Business Rules

**Nguyên tắc:** Mỗi rule cần có ID ổn định, phát biểu rõ ràng, nguồn và trạng thái xác nhận.

| ID | Business Rule | Domain | Lý do/nguồn gốc | Trạng thái | Người xác nhận | Nguồn |
|---|---|---|---|---|---|---|
| BR-01 | CMS quản lý nội dung Website và chỉ tham chiếu hồ sơ qua API, không làm nguồn dữ liệu chuẩn. DMS là nguồn dữ liệu chuẩn cho tài liệu/hồ sơ và phiên bản được quản lý trong DMS; CLM là nguồn dữ liệu chuẩn cho dữ liệu hợp đồng và vòng đời hợp đồng được quản lý trong CLM. Website yêu cầu đăng nhập để truy cập tài liệu; người dùng khách chưa đăng nhập không được truy cập tài liệu. CMS quản lý quyền. Nếu DMS hoặc CLM không khả dụng, hiển thị lỗi. | Content, Integration, Access Control | Phân định nguồn dữ liệu chuẩn và ranh giới truy cập Website | Confirmed | Người dùng/PO | [Quy mô CMS doanh nghiệp](chatgpt-conversation://6abe2409-0168-83ec-9477-911dbce7a104) |

## Tiêu chí ghi nhận

- Viết quy tắc theo cách có thể kiểm tra.
- Tách rule khỏi giải pháp UI/technical implementation khi có thể.
- Ghi rõ ngoại lệ và điều kiện áp dụng.
- Không chuyển giả định thành rule đã xác nhận nếu chưa có bằng chứng.

**Ghi chú:** P4 là giả định tạm thời, chưa được xác nhận là lịch triển khai.
