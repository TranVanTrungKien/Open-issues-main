# OI-04: Cấp duyệt và thu hồi phê duyệt

**Trạng thái:** Đề xuất giải đáp, chưa được khách hàng xác nhận. Không dùng giả định chưa chốt làm cam kết production.

[Nguồn issue](../open-issues.md#oi-04) · [Mục lục](README.md) · [Quy tắc chung](../common-rules.md) · [Lộ trình](../implementation-roadmap.md)

## 1. Vấn đề gốc

**Cần chốt:** Một hay nhiều cấp duyệt? Có áp dụng maker-checker cho mọi loại nội dung không? Ai được thu hồi duyệt?
**Baseline:** Một cấp độc lập. Người tham gia tạo/sửa trong lần gửi duyệt không tự duyệt.
**Chủ quyết định:** Chủ nội dung và người phụ trách kiểm soát.
**Hạn chốt:** Trước P2. Nếu nhiều cấp bắt buộc, chuyển năng lực này từ P3 về P2.

## 2. Trả lời đề xuất

Một cấp duyệt độc lập cho mọi loại nội dung lần đầu. Người tạo hoặc tham gia sửa trong lần gửi duyệt không tự duyệt, dù có nhiều vai trò.

## 3. Cách triển khai và ngoại lệ

- Tách quyền soạn, duyệt và xuất bản. Người duyệt chỉ được kiêm xuất bản nếu được cấp quyền riêng và vẫn độc lập với người tạo/sửa bản gửi duyệt.
- Lưu người tham gia và phiên bản quy trình tại lần gửi. Ủy quyền/phân công lại không xóa xung đột trách nhiệm.
- Quyết định gắn đúng phiên bản, mô hình, phụ thuộc, site và locale. Sửa sau duyệt tạo bản mới chưa duyệt; nháp mới không tự hủy duyệt của bản cũ.
- Đề xuất cho người đã duyệt hoặc phụ trách kiểm soát có quyền trong phạm vi được thu hồi, bắt buộc lý do. System Admin không tự có quyền này.
- Thu hồi chặn phát hành mới, dừng lịch dùng quyết định đó. Bản đã công khai phải có yêu cầu gỡ riêng và xác minh ở Publishing.
- Nếu nhiều cấp là bắt buộc, chuyển năng lực vào P2, chốt thứ tự, điều kiện từng cấp và cách xử lý yêu cầu sửa.

## 4. Thông tin cần xác nhận

Loại nội dung áp dụng, nhóm duyệt theo phạm vi, người có quyền thu hồi; có bắt buộc nhiều cấp hoặc cấm người duyệt kiêm xuất bản không.

Chủ quyết định và hạn chốt theo vấn đề gốc. Khi xác nhận, ghi người, ngày, phạm vi và các giá trị cụ thể; cập nhật UC/BR/AC và lộ trình nếu khác baseline, rồi mới đóng issue.

## 5. Tiêu chí kiểm chứng đề xuất

Người sửa có vai trò duyệt vẫn bị chặn tự duyệt. Duyệt v2 không áp cho nháp v3. Thu hồi trước giờ lịch làm lịch dừng; thu hồi không báo đã gỡ bản công khai.

## 6. Tài liệu liên quan

[12-workflow](../domains/12-workflow.md) · [01-identity-access](../domains/01-identity-access.md) · [13-publishing](../domains/13-publishing.md)

Tiêu chí trên bổ sung kiểm chứng phương án, không tự thay thế AC chính thức trong domain.
