# Tìm kiếm

**Domain:** `SRCH` · **Bắt đầu:** P1 · **Số use case:** 4
**Trạng thái:** Baseline đề xuất v0.1 — chưa phải yêu cầu đã được khách hàng phê duyệt.

Giúp người dùng tìm dữ liệu được phép thấy. Không trở thành nguồn quyết định quyền hoặc trạng thái xuất bản.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-SRCH-01](#uc-srch-01) | Tìm nội dung để biên tập hoặc xử lý | P1 |
| [UC-SRCH-02](#uc-srch-02) | Tìm tài nguyên để tái sử dụng | P1 |
| [UC-SRCH-03](#uc-srch-03) | Tìm kiếm trên kênh người đọc | P3 |
| [UC-SRCH-04](#uc-srch-04) | Lưu và chia sẻ bộ điều kiện tìm kiếm | P3 |

## UC-SRCH-01

**Tìm nội dung để biên tập hoặc xử lý**

**Triển khai:** P1. **Actor:** Nhân viên CMS.
**Mục tiêu:** Người dùng tìm đúng nội dung trong phạm vi được phép.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tìm nội dung để biên tập hoặc xử lý”.
**Tiền điều kiện:** Có quyền đọc ít nhất một phạm vi nội dung.
**Thông tin đầu vào:** Từ khóa; site; locale; loại; trạng thái; chủ sở hữu; thời gian.

**Luồng chính**

1. Nhập từ khóa và bộ lọc.
2. Xem danh sách, số lượng và trạng thái.
3. Sắp xếp hoặc chuyển trang.
4. Mở nội dung để xử lý theo quyền hiện hành.

**Luồng thay thế và ngoại lệ**

- E01: Không có kết quả: hiển thị rỗng và bộ lọc đang dùng.
- E02: Nội dung đã đổi hoặc quyền vừa mất: kiểm tra lại khi mở.

**BR-SRCH-01:** Tất cả kết quả, số lượng, gợi ý và bộ lọc chỉ phản ánh dữ liệu được phép thấy. P1 ưu tiên truy vấn cơ bản; chưa cần máy tìm kiếm riêng.
**Kết quả thành công:** Người dùng tìm đúng nội dung trong phạm vi được phép.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SRCH-01-01/v1:** Given chỉ được site A; When tìm từ khóa có bài ở site B; Then danh sách và số lượng không lộ bài B.
- **AC-SRCH-01-02/v1:** Given mất quyền sau khi kết quả được tải; When mở bài; Then bị từ chối.
- **AC-SRCH-01-03/v1:** Given đổi bộ lọc; When chuyển trang; Then các bộ lọc được giữ đúng.

**Use case liên quan / phụ thuộc:** [UC-CNT-01](09-content.md#uc-cnt-01), [UC-IAM-04](01-identity-access.md#uc-iam-04).

## UC-SRCH-02

**Tìm tài nguyên để tái sử dụng**

**Triển khai:** P1. **Actor:** Biên tập viên hoặc quản trị tài nguyên.
**Mục tiêu:** Tài nguyên được chọn có thể sử dụng trong phạm vi đích.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tìm tài nguyên để tái sử dụng”.
**Tiền điều kiện:** Có quyền đọc kho tài nguyên tương ứng.
**Thông tin đầu vào:** Từ khóa; loại tệp; phạm vi; quyền sử dụng; trạng thái; bộ sưu tập.

**Luồng chính**

1. Nhập điều kiện tìm.
2. Xem thumbnail và metadata được phép.
3. Mở tài nguyên để kiểm tra phiên bản và quyền sử dụng.
4. Chọn tài nguyên hợp lệ cho nội dung.

**Luồng thay thế và ngoại lệ**

- E01: Tài nguyên đang cách ly hoặc bị hạn chế: không cho sử dụng.
- E02: Quyền sử dụng đã hết: nêu lý do không thể chọn.

**BR-SRCH-02:** Thumbnail, tên tệp và số lượng cũng thuộc phạm vi kiểm soát quyền. Kết quả tìm không thay kiểm tra quyền khi gán.
**Kết quả thành công:** Tài nguyên được chọn có thể sử dụng trong phạm vi đích.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SRCH-02-01/v1:** Given tài nguyên riêng tư; When người ngoài phạm vi tìm; Then không thấy cả thumbnail và tên.
- **AC-SRCH-02-02/v1:** Given tệp đang chờ quét; When chọn để phát hành; Then bị chặn.
- **AC-SRCH-02-03/v1:** Given tài nguyên được phép đọc nhưng không dùng ở site đích; When gán; Then bị từ chối.

**Use case liên quan / phụ thuộc:** [UC-AST-01](08-assets.md#uc-ast-01), [UC-IAM-04](01-identity-access.md#uc-iam-04).

## UC-SRCH-03

**Tìm kiếm trên kênh người đọc**

**Triển khai:** P3. **Actor:** Khách truy cập hoặc nhân viên được quyền đọc.
**Mục tiêu:** Người đọc tìm được nội dung còn hiệu lực và được phép đọc.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tìm kiếm trên kênh người đọc”.
**Tiền điều kiện:** Site có nội dung đang công khai hoặc phát hành cho nhóm người đọc cụ thể.
**Thông tin đầu vào:** Từ khóa; locale; loại nội dung; bộ lọc công khai.

**Luồng chính**

1. Nhập từ khóa trên kênh đọc.
2. Nhận kết quả đúng phạm vi người đọc.
3. Xem trích đoạn và mở trang.
4. Điều chỉnh điều kiện khi không có kết quả.

**Luồng thay thế và ngoại lệ**

- E01: Chỉ mục chưa kịp cập nhật sau gỡ: lọc lại khả năng đọc trước khi trả kết quả.
- E02: Không có locale đích: áp dụng fallback đã chốt, không lấy nháp.

**BR-SRCH-03:** Không đưa bản nháp vào tìm kiếm công khai. Gỡ nội dung phải được phản ánh trong thời gian đã chốt; không dùng chỉ mục cũ để vượt quyền.
**Kết quả thành công:** Người đọc tìm được nội dung còn hiệu lực và được phép đọc.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SRCH-03-01/v1:** Given bài chưa xuất bản; When tìm đúng tiêu đề; Then không xuất hiện.
- **AC-SRCH-03-02/v1:** Given bài riêng tư; When người vô danh tìm; Then không thấy kết quả hoặc trích đoạn.
- **AC-SRCH-03-03/v1:** Given bài đã gỡ nhưng chỉ mục cũ còn; When tìm; Then không trả nội dung bị gỡ như đang đọc được.

**Use case liên quan / phụ thuộc:** [UC-SITE-05](04-site-delivery.md#uc-site-05), [UC-SRCH-01](11-search.md#uc-srch-01).

## UC-SRCH-04

**Lưu và chia sẻ bộ điều kiện tìm kiếm**

**Triển khai:** P3. **Actor:** Nhân viên CMS.
**Mục tiêu:** Người dùng tái sử dụng điều kiện mà không bị vượt phạm vi dữ liệu.
**Kích hoạt:** Người dùng yêu cầu thực hiện “lưu và chia sẻ bộ điều kiện tìm kiếm”.
**Tiền điều kiện:** Có quyền sử dụng tìm kiếm và chia sẻ trong nhóm nếu cần.
**Thông tin đầu vào:** Tên bộ lọc; điều kiện; phạm vi cá nhân hoặc nhóm.

**Luồng chính**

1. Tạo điều kiện tìm kiếm.
2. Lưu với tên nhận biết.
3. Chọn người được dùng bộ lọc.
4. Mở lại để chạy trên dữ liệu và quyền hiện hành.

**Luồng thay thế và ngoại lệ**

- E01: Điều kiện chứa trường đã ngừng dùng: cảnh báo cần sửa.
- E02: Người nhận ít quyền hơn: chỉ nhận kết quả theo quyền của chính họ.

**BR-SRCH-04:** Chia sẻ điều kiện không chia sẻ quyền dữ liệu. Không lưu nguyên danh sách kết quả nhạy cảm như mặc định.
**Kết quả thành công:** Người dùng tái sử dụng điều kiện mà không bị vượt phạm vi dữ liệu.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SRCH-04-01/v1:** Given A chia sẻ bộ lọc cho B ít quyền hơn; When B chạy; Then chỉ thấy dữ liệu B được phép.
- **AC-SRCH-04-02/v1:** Given quyền thay đổi sau khi lưu; When mở lại; Then áp dụng quyền mới.
- **AC-SRCH-04-03/v1:** Given trường lọc đã bị bỏ; When chạy; Then có cảnh báo thay vì âm thầm bỏ điều kiện.

**Use case liên quan / phụ thuộc:** [UC-SRCH-01](11-search.md#uc-srch-01).
