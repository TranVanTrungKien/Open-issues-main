# Open Issues và quyết định đã chốt

Theo dõi các vấn đề nghiệp vụ cần làm rõ. Các chi tiết kỹ thuật triển khai có thể tiếp tục được làm rõ riêng mà không giữ mở một quyết định nghiệp vụ đã được xác nhận.

| ID | Vấn đề | Câu hỏi cần trả lời / kết quả | Người quyết định | Hạn/điểm chặn | Trạng thái | Nguồn |
|---|---|---|---|---|---|---|
| OI-01 | Định dạng phân phối & quản lý hồ sơ pháp lý | Đã chốt: Website có đăng nhập; người dùng khách chưa đăng nhập không được truy cập tài liệu. Chọn CMS hybrid tích hợp DMS và CLM qua API hiện có. CMS quản lý nội dung Website và quyền; chỉ tham chiếu hồ sơ. DMS là nguồn chuẩn cho tài liệu/hồ sơ và phiên bản được quản lý trong DMS; CLM là nguồn chuẩn cho dữ liệu và vòng đời hợp đồng được quản lý trong CLM. Khi hệ thống nguồn không khả dụng, hiển thị lỗi. | Người dùng/PO | Trước P0 (theo baseline) | Closed — quyết định nghiệp vụ đã xác nhận ngày 2026-10-29 | [Quy mô CMS doanh nghiệp](chatgpt-conversation://6abe2409-0168-83ec-9477-911dbce7a104); [Notion Open Issues](https://app.notion.com/p/3ecebed0878d8188b258ed90f6f79006) |

## Ghi chú sau khi đóng

- P4 chỉ là giả định tạm thời, chưa được xác nhận là lịch triển khai.
- Hợp đồng API, ánh xạ hồ sơ và luồng kỹ thuật chuyển tiếp/kiểm tra quyền vẫn cần được xác định trong thiết kế tích hợp. Đây là chi tiết kỹ thuật tiếp theo, không thay đổi quyết định nghiệp vụ đã đóng tại OI-01.

## Quy tắc

- Giữ nguyên ý nghĩa và mã ID từ nguồn.
- Ghi riêng lựa chọn đề xuất, giả định và quyết định đã được xác nhận.
- Không coi dấu chọn trong bảng làm việc là phê duyệt chính thức nếu nguồn không nói rõ.
