# Use Cases

## UC-SITE-05 — Đọc nội dung đã phát hành và tài liệu được tham chiếu

- **Trạng thái:** [Đã xác định] — nghiệp vụ; chi tiết kỹ thuật API còn cần thiết kế
- **Domain:** Site, Content, Integration, Access Control
- **Actor chính:** Người dùng Website đã đăng nhập
- **Mục tiêu:** Đọc nội dung đã phát hành và tài liệu được tham chiếu trong phạm vi quyền hiện hành
- **Trigger:** Người dùng mở nội dung Website có tham chiếu tới hồ sơ
- **Tiền điều kiện:** Người dùng đã đăng nhập; CMS xác định được quyền truy cập; nội dung Website đã được phát hành
- **Luồng chính:**
  1. Người dùng mở nội dung Website đã phát hành.
  2. Người dùng chọn một tham chiếu hồ sơ.
  3. CMS kiểm tra quyền truy cập theo quyền CMS hiện hành.
  4. CMS lấy hồ sơ từ DMS hoặc CLM qua API hiện có.
  5. Website hiển thị hồ sơ được phép truy cập.
- **Luồng thay thế/ngoại lệ:**
  - Người dùng chưa đăng nhập: từ chối truy cập tài liệu và yêu cầu đăng nhập.
  - Người dùng không có quyền: từ chối truy cập tài liệu.
  - DMS hoặc CLM không khả dụng: hiển thị lỗi; không báo truy cập thành công.
- **Business Rules liên quan:** BR-01
- **Acceptance Criteria:**
  - [ ] Người dùng chưa đăng nhập không thể truy cập tài liệu qua Website.
  - [ ] Người dùng đã đăng nhập nhưng không có quyền vẫn bị từ chối truy cập tài liệu.
  - [ ] Tài liệu/hồ sơ và phiên bản được lấy theo nguồn chuẩn tương ứng: DMS hoặc CLM.
  - [ ] Khi hệ thống nguồn không khả dụng, Website hiển thị lỗi và không hiển thị kết quả thành công giả.
- **Nguồn và người xác nhận:** Người dùng/PO; [Quy mô CMS doanh nghiệp](chatgpt-conversation://6abe2409-0168-83ec-9477-911dbce7a104). Luồng kỹ thuật ủy quyền và hợp đồng API cần được xác định trong thiết kế tích hợp.

## Use Case Template

- **ID:** UC-XXX
- **Tên:** [Tên use case]
- **Trạng thái:** [Blind]
- **Domain:** [Domain]
- **Actor chính:** [Cần xác nhận]
- **Mục tiêu:** [Cần xác nhận]
- **Trigger:** [Cần xác nhận]
- **Tiền điều kiện:** [Cần xác nhận]
- **Luồng chính:**
  1. [Bước]
- **Luồng thay thế/ngoại lệ:**
  - [Tình huống]
- **Business Rules liên quan:** [BR-XXX]
- **Acceptance Criteria:**
  - [ ] [Tiêu chí có thể kiểm tra]
- **Nguồn và người xác nhận:** [Cần bổ sung]

> Chỉ điền chi tiết khi có nguồn phù hợp; phần chưa rõ phải được đánh dấu [Giả định] hoặc [Blind].
