# OI-08: Locale, fallback và bản dịch lỗi thời

**Trạng thái:** Đề xuất giải đáp, chưa được khách hàng xác nhận. Không dùng giả định chưa chốt làm cam kết production.

[Nguồn issue](../open-issues.md#oi-08) · [Mục lục](README.md) · [Quy tắc chung](../common-rules.md) · [Lộ trình](../implementation-roadmap.md)

## 1. Vấn đề gốc

**Cần chốt:** Ngôn ngữ bắt buộc, fallback, trường không được fallback, và thế nào là bản dịch lỗi thời.
**Baseline:** Một locale P0; locale độc lập về duyệt và phát hành; không fallback sang nháp.
**Chủ quyết định:** Chủ nội dung và nhóm dịch.
**Hạn chốt:** Quy tắc mô hình ở P0; quy trình dịch trước P3. Đa ngôn ngữ bắt buộc khi go-live phải chuyển vào P2.

## 2. Trả lời đề xuất

Đề xuất locale mặc định vi-VN, chờ chủ nội dung xác nhận. Một locale ở P0, mỗi locale duyệt/phát hành độc lập; mặc định chưa bật fallback.

## 3. Cách triển khai và ngoại lệ

- Ghi locale vào mô hình từ P0. Nếu nhiều ngôn ngữ bắt buộc go-live, đưa LOC-02/03/04 vào P2 thay vì P3.
- Khi bật fallback, cấu hình chuỗi rõ ràng và chặn chu trình. Chỉ dùng bản đã phát hành, còn hiệu lực, đúng quyền đọc; không dùng nháp hoặc vượt quyền kênh.
- Đề xuất không fallback trường thông báo bắt buộc theo thị trường, điều kiện sử dụng và trường riêng của locale. Danh sách cuối do chủ nội dung xác nhận; thiếu trường bắt buộc thì báo thiếu, không ghép sai nghĩa.
- Ưu tiên fallback cả bản đã duyệt. Fallback từng trường chỉ triển khai nếu quy tắc bảo toàn ngữ cảnh được xác nhận; người đọc biết ngôn ngữ thực tế.
- Bản dịch lưu phiên bản nguồn. Khi phiên bản nguồn được chọn làm chuẩn thay đổi, đánh dấu bản dịch dùng nguồn cũ cần rà soát; không mặc định mọi lần lưu nháp đều đổi nguồn chuẩn.
- Chặn phát hành mới bản dịch lỗi thời đến khi cập nhật hoặc người có quyền xác nhận không bị ảnh hưởng và lưu căn cứ. Nguồn đổi không tự ghi đè/gỡ bản dịch công khai.

## 4. Thông tin cần xác nhận

Locale mặc định/bắt buộc, sự kiện chọn nguồn chuẩn, chuỗi fallback và trường cấm, người xác nhận không bị ảnh hưởng, chính sách bản công khai lỗi thời. Mô hình ở P0; quy trình trước P3 hoặc P2 nếu bắt buộc go-live.

Chủ quyết định và hạn chốt theo vấn đề gốc. Khi xác nhận, ghi người, ngày, phạm vi và các giá trị cụ thể; cập nhật UC/BR/AC và lộ trình nếu khác baseline, rồi mới đóng issue.

## 5. Tiêu chí kiểm chứng đề xuất

Chu trình fallback bị chặn; chỉ có nháp không trả nội dung. Duyệt nguồn không cấp duyệt bản dịch. Một locale bị từ chối không làm locale khác bị gỡ. Nguồn chuẩn đổi tạo cảnh báo và chặn phát hành mới bản dịch chưa rà soát.

## 6. Tài liệu liên quan

[05-localization](../domains/05-localization.md) · [04-site-delivery](../domains/04-site-delivery.md)

Tiêu chí trên bổ sung kiểm chứng phương án, không tự thay thế AC chính thức trong domain.
