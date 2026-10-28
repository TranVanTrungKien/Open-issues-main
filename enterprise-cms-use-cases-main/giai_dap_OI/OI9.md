# OI-09: Tệp tải lên, kiểm tra an toàn và dung lượng

**Trạng thái:** Đề xuất giải đáp, chưa được khách hàng xác nhận. Không dùng giả định chưa chốt làm cam kết production.

[Nguồn issue](../open-issues.md#oi-09) · [Mục lục](README.md) · [Quy tắc chung](../common-rules.md) · [Lộ trình](../implementation-roadmap.md)

## 1. Vấn đề gốc

**Cần chốt:** Loại tệp, giới hạn tải, kiểm tra an toàn, ảnh/video cần xử lý và dung lượng dự kiến.
**Baseline:** Tệp chưa đạt kiểm tra không được phát hành; không tin định dạng khai báo từ phía tải lên.
**Chủ quyết định:** Bảo mật và quản trị tài nguyên.
**Hạn chốt:** Trước P1 và xác minh trước production.

## 2. Trả lời đề xuất

Đề xuất lần đầu hỗ trợ JPEG/PNG/WebP và PDF nếu có nhu cầu tài liệu. Video dùng liên kết nguồn được phép cho đến khi có yêu cầu xử lý riêng. Danh sách chờ bảo mật/chủ tài nguyên xác nhận.

## 3. Cách triển khai và ngoại lệ

- Dùng allowlist, kiểm tra phần mở rộng, loại nhận diện từ nội dung và khả năng phân tích tệp; không tin tên hoặc Content-Type do client khai báo. Tệp không khớp/không hỗ trợ bị từ chối.
- Chốt dung lượng mỗi loại, số tệp mỗi lần, kích thước ảnh, quota và thời gian xử lý từ tệp mẫu và năng lực hạ tầng. Dự báo kho theo OI12; không đặt số tùy ý thành cam kết.
- Tệp mới vào vùng cách ly, chưa có URL công khai dùng được. Quét an toàn và kiểm tra nội dung trước khi đủ điều kiện sử dụng/phát hành.
- Bộ quét lỗi, timeout hoặc không kết luận được: giữ cách ly, cho xử lý lại có audit, không coi sạch. Thumbnail/biến thể tạo qua quy trình xử lý cách ly và vẫn chịu quyền truy cập/điều kiện phát hành.
- Không lộ tệp, thumbnail hoặc metadata nhạy cảm qua search, preview, cache. Tên trùng không ghi đè; bản phát hành giữ đúng phiên bản tài nguyên.
- Theo dõi tệp gốc, biến thể, phiên bản và cách ly. Dọn tệp lỗi theo chính sách lưu giữ; ngừng dùng không đồng nghĩa xóa vĩnh viễn.

## 4. Thông tin cần xác nhận

Allowlist; giới hạn/quota; dịch vụ kiểm tra và người xử lý lỗi; yêu cầu ảnh/video; tệp mẫu và tăng trưởng. Chốt trước P1, kiểm chứng trước production.

Chủ quyết định và hạn chốt theo vấn đề gốc. Khi xác nhận, ghi người, ngày, phạm vi và các giá trị cụ thể; cập nhật UC/BR/AC và lộ trình nếu khác baseline, rồi mới đóng issue.

## 5. Tiêu chí kiểm chứng đề xuất

Tệp thực thi đổi đuôi ảnh không được phát hành. Tệp quá giới hạn bị chặn. Quét lỗi giữ cách ly; URL và thumbnail không công khai. Tải bản mới không thay tệp trong bản phát hành cũ.

## 6. Tài liệu liên quan

[08-assets](../domains/08-assets.md) · [09-content](../domains/09-content.md)

Tiêu chí trên bổ sung kiểm chứng phương án, không tự thay thế AC chính thức trong domain.
