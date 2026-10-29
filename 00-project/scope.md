# Project Scope

**Trạng thái:** [Đã xác định] — phạm vi OI-01 được người dùng xác nhận ngày 2026-10-29.

## Mục tiêu

- Xây dựng CMS hybrid để quản lý nội dung Website và tích hợp các hệ thống hồ sơ hiện có.

## Trong phạm vi (In Scope)

- Website yêu cầu đăng nhập để truy cập tài liệu; người dùng khách chưa đăng nhập không được truy cập tài liệu.
- CMS quản lý nội dung Website và quyền truy cập Website.
- CMS tham chiếu tài liệu/hồ sơ qua API hiện có của DMS và CLM; CMS không làm nguồn dữ liệu chuẩn.
- DMS là nguồn dữ liệu chuẩn cho tài liệu/hồ sơ và các phiên bản được quản lý trong DMS.
- CLM là nguồn dữ liệu chuẩn cho dữ liệu hợp đồng và vòng đời hợp đồng được quản lý trong CLM.
- Nếu DMS hoặc CLM không khả dụng, hệ thống hiển thị lỗi thay vì giả lập truy cập thành công.

## Ngoài phạm vi (Out of Scope)

- Dùng CMS làm hệ thống quản lý hồ sơ pháp lý chuyên dụng hoặc nguồn dữ liệu chuẩn thay DMS/CLM.
- Coi P4 là lịch triển khai đã được xác nhận.

## Stakeholders và người ra quyết định

- Người dùng/Chủ sản phẩm (PO): xác nhận quyết định nghiệp vụ OI-01.

## Ràng buộc và phụ thuộc

- DMS và CLM là các hệ thống do công ty phát triển từ trước và đã có API (theo xác nhận trước đó).
- Thời điểm P4 chỉ là giả định tạm thời.
- Chi tiết API và luồng kỹ thuật chuyển tiếp/kiểm tra quyền cần được xác định trong thiết kế tích hợp.

## Câu hỏi còn mở

- Không còn câu hỏi nghiệp vụ thuộc phạm vi OI-01.
- Chi tiết hợp đồng API, ánh xạ hồ sơ và luồng kỹ thuật ủy quyền là đầu vào thiết kế tích hợp, không làm thay đổi quyết định phạm vi OI-01.

## Nguồn tham chiếu

- Notion: [Open Issues](https://app.notion.com/p/3ecebed0878d8188b258ed90f6f79006) (baseline; chưa đồng bộ xác nhận mới).
- ChatGPT: cuộc trò chuyện “Quy mô CMS doanh nghiệp”, xác nhận của người dùng ngày 2026-10-29.
