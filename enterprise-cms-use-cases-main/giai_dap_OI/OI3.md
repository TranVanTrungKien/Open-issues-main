# OI-03: SSO, MFA, phiên và vô hiệu hóa

**Trạng thái:** Đề xuất giải đáp, chưa được khách hàng xác nhận. Không dùng giả định chưa chốt làm cam kết production.

[Nguồn issue](../open-issues.md#oi-03) · [Mục lục](README.md) · [Quy tắc chung](../common-rules.md) · [Lộ trình](../implementation-roadmap.md)

## 1. Vấn đề gốc

**Cần chốt:** Nhà cung cấp SSO, MFA, thời hạn phiên, tài khoản khẩn cấp, nguồn đồng bộ nhóm và cách vô hiệu hóa.
**Baseline:** Xác thực doanh nghiệp; quyền CMS riêng; quyền bị thu hồi không dùng được ở hành động tiếp theo.
**Chủ quyết định:** Bộ phận quản trị danh tính và bảo mật.
**Hạn chốt:** P0.

## 2. Trả lời đề xuất

Dùng hệ thống danh tính hiện có của doanh nghiệp để xác thực, CMS quản lý quyền riêng. Chưa đủ thông tin để chọn nhà cung cấp hoặc gán thời hạn phiên.

## 3. Cách triển khai và ngoại lệ

- Chốt nhà cung cấp, giao thức, mã định danh ổn định và môi trường thử ở P0; không dùng tên hiển thị làm khóa. SSO thành công chưa có vai trò thì không mở nội dung.
- Đề xuất MFA cho người dùng CMS; bảo mật xác nhận phương thức và ngoại lệ. Chốt riêng thời hạn không hoạt động, thời hạn tuyệt đối, gia hạn và xác thực lại cho thao tác nhạy cảm.
- Lấy nhóm từ danh bạ doanh nghiệp được chọn; quản trị kiểm soát ánh xạ nhóm sang vai trò/phạm vi, không tự biến nhóm bất kỳ thành quản trị CMS.
- Đình chỉ trong CMS có hiệu lực ở hành động tiếp theo, kể cả phiên cũ và lịch; không chỉ chờ token hết hạn. Với thay đổi ở IdP, phải chốt cơ chế đồng bộ/thông báo và độ trễ phát hiện. Không xác minh được quyền hiện hành thì chặn thao tác nhạy cảm.
- Có tài khoản khôi phục được bảo vệ, người giữ bí mật, audit mỗi lần dùng và rà soát sau sự cố. Quyền khôi phục quản trị không bỏ qua maker-checker. Đăng xuất CMS kết thúc phiên CMS, không mặc định đăng xuất mọi ứng dụng SSO.

## 4. Thông tin cần xác nhận

Tên IdP/người vận hành; chính sách MFA; các thời hạn phiên; nguồn nhóm và độ trễ đồng bộ; quy trình khẩn cấp và người kiểm tra nhật ký.

Chủ quyết định và hạn chốt theo vấn đề gốc. Khi xác nhận, ghi người, ngày, phạm vi và các giá trị cụ thể; cập nhật UC/BR/AC và lộ trình nếu khác baseline, rồi mới đóng issue.

## 5. Tiêu chí kiểm chứng đề xuất

SSO chưa có quyền không đọc CMS. Đình chỉ rồi lưu bằng phiên cũ bị chặn; lịch dừng. Đăng xuất rồi dùng lại phiên cũ bị từ chối. Diễn tập khôi phục có audit, không tự cấp quyền duyệt/xuất bản.

## 6. Tài liệu liên quan

[01-identity-access](../domains/01-identity-access.md) · [02-administration](../domains/02-administration.md)

Tiêu chí trên bổ sung kiểm chứng phương án, không tự thay thế AC chính thức trong domain.
