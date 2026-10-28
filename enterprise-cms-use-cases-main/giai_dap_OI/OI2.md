# OI-02: Phạm vi tổ chức và chia sẻ dữ liệu

**Trạng thái:** Đề xuất giải đáp, chưa được khách hàng xác nhận. Không dùng giả định chưa chốt làm cam kết production.

[Nguồn issue](../open-issues.md#oi-02) · [Mục lục](README.md) · [Quy tắc chung](../common-rules.md) · [Lộ trình](../implementation-roadmap.md)

## 1. Vấn đề gốc

**Cần chốt:** Một doanh nghiệp nhiều đơn vị hay SaaS đa tenant? Dữ liệu có được chia sẻ giữa các site không?
**Baseline:** Một workspace doanh nghiệp, nhiều đơn vị và site. Không có chia sẻ chéo phạm vi mặc định.
**Chủ quyết định:** Chủ sản phẩm và bảo mật.
**Hạn chốt:** Trước mô hình dữ liệu P0. SaaS đa tenant là thay đổi phạm vi kiến trúc, không chỉ thêm bộ lọc.

## 2. Trả lời đề xuất

Một doanh nghiệp, một workspace có nhiều đơn vị/site; chưa làm SaaS đa tenant. Mặc định không chia sẻ chéo site.

## 3. Cách triển khai và ngoại lệ

- Ghi phạm vi workspace, đơn vị và site từ P0; quyền theo hành động, site, nhóm, loại nội dung và locale. Thuộc một đơn vị không tự có quyền mọi site của đơn vị đó.
- Nếu cần kho dùng chung, cấp quyền đọc/tái sử dụng rõ cho từng site; quyền dùng không cấp quyền sửa nguồn, duyệt hoặc xuất bản ở site khác.
- Bản phát hành khóa phiên bản phụ thuộc; sửa tài nguyên nguồn không đổi âm thầm bản đang công khai.
- Có doanh nghiệp thứ hai phải đánh giá kiến trúc cách ly danh tính, dữ liệu, tệp, backup và vận hành; không chỉ thêm bộ lọc tenant.

## 4. Thông tin cần xác nhận

Danh sách đơn vị/site và chủ sở hữu; dữ liệu cần chia sẻ; người cấp/thu hồi quyền; có chia sẻ ngay lần đầu hay không.

Chủ quyết định và hạn chốt theo vấn đề gốc. Khi xác nhận, ghi người, ngày, phạm vi và các giá trị cụ thể; cập nhật UC/BR/AC và lộ trình nếu khác baseline, rồi mới đóng issue.

## 5. Tiêu chí kiểm chứng đề xuất

Người site A không thấy dữ liệu, số lượng hoặc thumbnail của B qua URL/API/tìm kiếm. Cấp quyền dùng một tệp không lộ cả kho. Thu hồi quyền chặn sử dụng mới ở hành động tiếp theo; tác động tới bản công khai xử lý theo OI10.

## 6. Tài liệu liên quan

[01-identity-access](../domains/01-identity-access.md) · [02-administration](../domains/02-administration.md) · [04-site-delivery](../domains/04-site-delivery.md)

Tiêu chí trên bổ sung kiểm chứng phương án, không tự thay thế AC chính thức trong domain.
