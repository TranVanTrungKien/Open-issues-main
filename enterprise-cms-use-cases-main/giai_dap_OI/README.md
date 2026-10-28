# Giải đáp Open Issues

Bộ giải đáp cho 16 issue trong [open-issues.md](../open-issues.md). OI1 được rà soát/chỉnh trực tiếp; OI2–OI16 có file riêng.

**Trạng thái chung:** Các phương án là đề xuất phân tích, chưa phải xác nhận của chủ quyết định. Không đóng issue hoặc dùng giả định chưa chốt làm căn cứ nghiệm thu production. Khi được xác nhận, cập nhật UC/BR/AC và lộ trình chịu ảnh hưởng.

| Issue | Nội dung |
|---|---|
| [OI-01](OI1.md) | Mô hình sản phẩm và hồ sơ pháp lý — đã rà soát |
| [OI-02](OI2.md) | Phạm vi tổ chức và chia sẻ dữ liệu |
| [OI-03](OI3.md) | SSO, MFA, phiên và vô hiệu hóa |
| [OI-04](OI4.md) | Cấp duyệt và thu hồi phê duyệt |
| [OI-05](OI5.md) | Loại nội dung, trường và URL |
| [OI-06](OI6.md) | Đích phân phối và bằng chứng hoàn tất |
| [OI-07](OI7.md) | Quyền thực thi lịch và phục hồi lịch lỡ hạn |
| [OI-08](OI8.md) | Locale, fallback và bản dịch lỗi thời |
| [OI-09](OI9.md) | Tệp tải lên, kiểm tra an toàn và dung lượng |
| [OI-10](OI10.md) | Quyền sử dụng tài nguyên và hết hạn |
| [OI-11](OI11.md) | Lưu giữ, giữ lại và xóa dữ liệu |
| [OI-12](OI12.md) | Quy mô và chỉ tiêu hiệu năng |
| [OI-13](OI13.md) | Backup, RPO/RTO và diễn tập phục hồi |
| [OI-14](OI14.md) | Nhập và di chuyển dữ liệu |
| [OI-15](OI15.md) | Mẫu trang, trợ năng và trình duyệt |
| [OI-16](OI16.md) | Tích hợp bắt buộc và nguyên tử xuyên hệ thống |

## Quan hệ và thứ tự chốt

- Trước P0: OI1/OI2 xác định phạm vi; OI3 xác định danh tính; OI8 xác định mô hình locale; OI16 xác định kết nối nền tảng bắt buộc.
- Trước P1: OI5/OI9 xác định mô hình nội dung và tệp. Thu thập hồ sơ tải OI12 trước Detail Design.
- Trong P2: OI4/OI6/OI7/OI13/OI15 hoàn thiện duyệt, phân phối, lịch, phục hồi và mẫu hiển thị. Kiểm thử tải OI12 kiểm chứng mục tiêu đã chốt trước đó.
- Trước production: OI10 có kế hoạch hết quyền khi dùng tài nguyên có hạn; OI11 có chính sách lưu giữ cho dữ liệu thật, không mặc định đợi P4.
- OI14 vào P1/P2 nếu thay hệ thống cũ lúc go-live. Đa ngôn ngữ, nhiều cấp duyệt, page builder hoặc tích hợp khác phải chuyển sớm nếu là điều kiện go-live.

Mỗi file có vấn đề gốc, câu trả lời đề xuất, cách xử lý, thông tin cần xác nhận và tiêu chí kiểm chứng. Các số chưa có căn cứ (thời gian, dung lượng, tải, RPO/RTO) là đầu vào cần chốt, không được gán tùy ý.
