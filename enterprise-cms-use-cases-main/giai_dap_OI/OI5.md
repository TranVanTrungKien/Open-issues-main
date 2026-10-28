# OI-05: Loại nội dung, trường và URL

**Trạng thái:** Đề xuất giải đáp, chưa được khách hàng xác nhận. Không dùng giả định chưa chốt làm cam kết production.

[Nguồn issue](../open-issues.md#oi-05) · [Mục lục](README.md) · [Quy tắc chung](../common-rules.md) · [Lộ trình](../implementation-roadmap.md)

## 1. Vấn đề gốc

**Cần chốt:** Các loại nội dung đầu tiên, trường bắt buộc, quan hệ, kích thước, quy tắc URL và dữ liệu nhạy cảm.
**Baseline:** Chưa áp đặt Article/News/Product là yêu cầu chính thức. Có thể dùng một loại mẫu để thử lát cắt đầu.
**Chủ quyết định:** Chủ nội dung và BA.
**Hạn chốt:** Trước P1.

## 2. Trả lời đề xuất

Dùng một loại mẫu Bài viết thông tin để thử P1, chờ chủ nội dung xác nhận loại chính thức. Không coi Article/News/Product đều là yêu cầu bắt buộc.

## 3. Cách triển khai và ngoại lệ

| Trường đề xuất | Quy tắc khi gửi duyệt |
|---|---|
| Mã, phiên bản, site, locale, chủ sở hữu | Có phạm vi hợp lệ; hệ thống quản lý mã/phiên bản |
| Tiêu đề, nội dung | Bắt buộc; giới hạn độ dài cần chốt |
| Slug | Bắt buộc cho trang có URL; duy nhất trong site + locale + không gian đường dẫn |
| Tóm tắt, ảnh, tệp | Tùy chọn trừ khi mẫu hiển thị bắt buộc; tài nguyên hợp lệ |
| Danh mục/thẻ, quan hệ | Tập giá trị, số lượng và đích tham chiếu được cho phép |

- Nháp có thể chưa đầy đủ theo chính sách; gửi duyệt phải thỏa mô hình. Mã trường ổn định, tách nhãn hiển thị; nội dung biết phiên bản mô hình.
- Đề xuất URL `/{locale}/{slug}` trong site. Chốt chuẩn hóa chữ, dấu và ký tự đặc biệt; đổi slug cần phương án URL cũ. Metadata/đường dẫn công khai gắn bản phát hành.
- Quan hệ bắt buộc phải hợp lệ khi phát hành; không tự xuất bản nháp được tham chiếu. Nội dung dùng cấu trúc/định dạng cho phép, không chèn mã tùy ý.
- Chỉ dùng dữ liệu mẫu không nhạy cảm trong thử nghiệm. Phân loại trường nhạy cảm và quyền xem/xuất/tìm trước dữ liệu thật.

## 4. Thông tin cần xác nhận

Loại thực tế và mẫu dữ liệu; trường bắt buộc, độ dài/kích thước tối đa, quan hệ và số lượng; URL; trường nhạy cảm. Chốt trước P1.

Chủ quyết định và hạn chốt theo vấn đề gốc. Khi xác nhận, ghi người, ngày, phạm vi và các giá trị cụ thể; cập nhật UC/BR/AC và lộ trình nếu khác baseline, rồi mới đóng issue.

## 5. Tiêu chí kiểm chứng đề xuất

Thiếu trường bắt buộc không gửi duyệt; slug trùng cùng phạm vi bị chặn. Quan hệ ngoài quyền không lộ qua gợi ý. Sửa nội dung/metadata tạo bản mới, không thay bản công khai.

## 6. Tài liệu liên quan

[06-content-model](../domains/06-content-model.md) · [09-content](../domains/09-content.md) · [04-site-delivery](../domains/04-site-delivery.md)

Tiêu chí trên bổ sung kiểm chứng phương án, không tự thay thế AC chính thức trong domain.
