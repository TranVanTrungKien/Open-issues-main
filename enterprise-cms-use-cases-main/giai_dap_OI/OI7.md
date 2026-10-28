# OI-07: Quyền thực thi lịch và phục hồi lịch lỡ hạn

**Trạng thái:** Đề xuất giải đáp, chưa được khách hàng xác nhận. Không dùng giả định chưa chốt làm cam kết production.

[Nguồn issue](../open-issues.md#oi-07) · [Mục lục](README.md) · [Quy tắc chung](../common-rules.md) · [Lộ trình](../implementation-roadmap.md)

## 1. Vấn đề gốc

**Cần chốt:** Lịch theo quyền cá nhân hay ủy quyền cấp tổ chức? Xử lý lịch bị lỡ sau sự cố thế nào?
**Baseline:** Quyền người lập còn hiệu lực lúc chạy. Lịch xuất bản quá hạn cần được rà soát lại, không tự phát hành bù.
**Chủ quyết định:** Chủ nội dung và vận hành.
**Hạn chốt:** Trước triển khai lập lịch P2.

## 2. Trả lời đề xuất

Lịch theo quyền cá nhân: người lập còn đủ quyền khi chạy. Lịch xuất bản lỡ hạn sau sự cố phải rà soát, không tự phát hành bù.

## 3. Cách triển khai và ngoại lệ

- Lưu phiên bản cố định, hành động, site/locale/đích, người lập, thời điểm tuyệt đối và múi giờ. Không tự lấy bản mới nhất; đổi múi giờ hiển thị không đổi thời điểm đã xác nhận.
- Kiểm tra lại quyền, duyệt, tài nguyên, hủy và thứ tự thao tác trước chuyển bản. Người mất quyền làm lịch dừng; bàn giao sở hữu không tự tái ủy quyền lịch.
- Sau phục hồi, liệt kê lịch xuất bản lỡ hạn. Người còn quyền đánh giá rồi hủy hoặc xác nhận lịch mới, giữ liên kết lịch cũ để truy vết.
- Lịch gỡ/hết hiệu lực cần chính sách riêng: đề xuất người vận hành còn quyền xử lý gỡ quá hạn ngay sau phục hồi khi xác minh đúng bản đích. Không dùng lệnh gỡ cũ để gỡ bản mới hơn. Nếu người lập mất quyền, dừng lịch và chuyển việc khẩn cho người đủ quyền, không âm thầm dùng quyền hệ thống.
- Với hạn kết thúc bắt buộc, đánh giá thêm cơ chế chặn phục vụ tại đích; phải chấp nhận/giải quyết rủi ro trong khoảng dừng trước khi dùng nội dung thật.
- Lịch cấp tổ chức chỉ bổ sung khi có chính sách quyền thực thi, người chịu trách nhiệm, thời hạn và thu hồi riêng. Hủy cạnh tranh thực thi phải xác định hành động thắng.

## 4. Thông tin cần xác nhận

Định nghĩa lỡ hạn và giới hạn trễ bình thường; chính sách gỡ/hết hiệu lực sau sự cố; người xử lý; có cần lịch cấp tổ chức không. Chốt trước triển khai lịch P2.

Chủ quyết định và hạn chốt theo vấn đề gốc. Khi xác nhận, ghi người, ngày, phạm vi và các giá trị cụ thể; cập nhật UC/BR/AC và lộ trình nếu khác baseline, rồi mới đóng issue.

## 5. Tiêu chí kiểm chứng đề xuất

Lịch v2 không lấy v3. Mất quyền/thu hồi duyệt làm lịch dừng. Sau sự cố dài không tự xuất bản bù. Hủy thành công trước chuyển bản ngăn phát hành; lịch gỡ cũ không gỡ bản mới. Thời gian mơ hồ do đổi giờ mùa phải được xử lý rõ.

## 6. Tài liệu liên quan

[13-publishing](../domains/13-publishing.md) · [01-identity-access](../domains/01-identity-access.md)

Tiêu chí trên bổ sung kiểm chứng phương án, không tự thay thế AC chính thức trong domain.
