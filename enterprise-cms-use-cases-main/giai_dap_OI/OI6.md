# OI-06: Đích phân phối và bằng chứng hoàn tất

**Trạng thái:** Đề xuất giải đáp, chưa được khách hàng xác nhận. Không dùng giả định chưa chốt làm cam kết production.

[Nguồn issue](../open-issues.md#oi-06) · [Mục lục](README.md) · [Quy tắc chung](../common-rules.md) · [Lộ trình](../implementation-roadmap.md)

## 1. Vấn đề gốc

**Cần chốt:** Đích phân phối, bằng chứng đã phục vụ bản mới, giới hạn trễ xuất bản/gỡ/cache/tìm kiếm.
**Baseline:** Một đích hoạt động trong P2; không báo hoàn tất khi chỉ mới nhận yêu cầu.
**Chủ quyết định:** Chủ sản phẩm và vận hành.
**Hạn chốt:** Trước nghiệm thu P2. Không tự đặt số giây khi chưa có căn cứ.

## 2. Trả lời đề xuất

Một đích hoạt động trong P2 theo kênh được chọn ở OI1. Hoàn tất khi đích thực sự phục vụ đúng bản; nhận yêu cầu/hàng đợi chưa phải thành công.

## 3. Cách triển khai và ngoại lệ

- Hồ sơ đích gồm site, locale, địa chỉ, quyền truy cập, cách xác minh và người vận hành. Tạo site không tự công khai.
- Khóa phiên bản nội dung và phụ thuộc cho mỗi yêu cầu. Phân biệt đã nhận, đang xử lý, đã xác minh, lỗi/chưa xác minh; tên trạng thái chi tiết chốt khi thiết kế.
- Bằng chứng đề xuất: mã lần phát hành, phiên bản mục tiêu, thời điểm và kết quả đọc kiểm tra kênh thực tế có định danh bản. Mất phản hồi thì đối soát trước khi retry cùng mã.
- Gỡ phải xác minh nội dung ngừng phục vụ trong phạm vi cam kết gồm cache quản lý được và search nếu có. Bản người đọc đã tải về nằm ngoài khả năng thu hồi của CMS.
- Lập bảng giới hạn riêng cho xuất bản, gỡ, cache, search: điểm bắt đầu/kết thúc đo, giới hạn, tải, cách đo và người xử lý vi phạm. Đo trên môi trường đại diện trước khi chốt số.
- Search công khai mặc định P3; nếu bắt buộc go-live đưa SRCH-03 vào P2. Tác vụ cũ không ghi đè lần gỡ/phát hành mới.

## 4. Thông tin cần xác nhận

Đích và môi trường cụ thể; bằng chứng; giới hạn trễ từng lớp; phạm vi cache/search; ngưỡng cảnh báo và người trực. Chưa có giá trị được duyệt thì chưa dùng làm cam kết production.

Chủ quyết định và hạn chốt theo vấn đề gốc. Khi xác nhận, ghi người, ngày, phạm vi và các giá trị cụ thể; cập nhật UC/BR/AC và lộ trình nếu khác baseline, rồi mới đóng issue.

## 5. Tiêu chí kiểm chứng đề xuất

Đích chỉ xác nhận đã nhận yêu cầu thì chưa báo hoàn tất. Đọc kênh nhận đúng bản được duyệt. Cache còn phục vụ ngoài giới hạn gỡ thì chưa hoàn tất; retry không nhân đôi phát hành.

## 6. Tài liệu liên quan

[13-publishing](../domains/13-publishing.md) · [04-site-delivery](../domains/04-site-delivery.md) · [11-search](../domains/11-search.md)

Tiêu chí trên bổ sung kiểm chứng phương án, không tự thay thế AC chính thức trong domain.
