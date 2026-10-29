# Tài nguyên số

**Domain:** `AST` · **Bắt đầu:** P1 · **Số use case:** 5
**Trạng thái:** Baseline đề xuất v0.1; riêng quyết định chia sẻ tài nguyên chéo site tại OI-02 đã được chốt.

Quản lý tệp, metadata, phiên bản và quyền sử dụng. Không tự công khai tài nguyên đang ở bản nháp.
**Quyết định OI-02:** Tài nguyên được cô lập theo site mặc định và chỉ được site khác sử dụng khi có cấp quyền chia sẻ rõ ràng, còn hiệu lực và được audit.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-AST-01](#uc-ast-01) | Tải tài nguyên số lên kho | P1 |
| [UC-AST-02](#uc-ast-02) | Mô tả và tổ chức tài nguyên | P1 |
| [UC-AST-03](#uc-ast-03) | Thay thế tệp bằng phiên bản mới | P3 |
| [UC-AST-04](#uc-ast-04) | Kiểm tra nơi sử dụng tài nguyên | P2 |
| [UC-AST-05](#uc-ast-05) | Lưu trữ tài nguyên không còn dùng mới | P1 |

## UC-AST-01

**Tải tài nguyên số lên kho**

**Triển khai:** P1. **Actor:** Biên tập viên hoặc quản trị tài nguyên.
**Mục tiêu:** Có tài nguyên được kiểm tra và định danh riêng để tham chiếu.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tải tài nguyên số lên kho”.
**Tiền điều kiện:** Có quyền tải lên phạm vi đã chọn.
**Thông tin đầu vào:** Tệp; tên; phạm vi; loại tài nguyên; metadata bắt buộc.

**Luồng chính**

1. Chọn tệp và phạm vi lưu.
2. Theo dõi kiểm tra kích thước, loại tệp và an toàn.
3. Bổ sung metadata bắt buộc.
4. Nhận trạng thái sẵn sàng sử dụng hoặc bị từ chối.

**Luồng thay thế và ngoại lệ**

- E01: Tệp không hợp lệ hoặc không an toàn: cách ly, không cho dùng công khai.
- E02: Kiểm tra an toàn chưa hoàn tất: giữ trạng thái chờ.

**BR-AST-01:** Tệp mới không tự công khai. Không tin chỉ vào phần mở rộng hoặc Content-Type do người tải khai báo; không ghi đè tài nguyên cùng tên.
**Kết quả thành công:** Có tài nguyên được kiểm tra và định danh riêng để tham chiếu.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-AST-01-01/v1:** Given tệp đang chờ kiểm tra; When gắn vào đợt xuất bản; Then phát hành bị chặn.
- **AC-AST-01-02/v1:** Given hai tệp cùng tên; When tải lên; Then không ghi đè tệp cũ.
- **AC-AST-01-03/v1:** Given chưa được cấp quyền đọc; When mở địa chỉ tệp nháp trực tiếp; Then không đọc được.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04).

## UC-AST-02

**Mô tả và tổ chức tài nguyên**

**Triển khai:** P1. **Actor:** Quản trị tài nguyên hoặc biên tập viên.
**Mục tiêu:** Tài nguyên được tổ chức và có căn cứ sử dụng rõ ràng.
**Kích hoạt:** Người dùng yêu cầu thực hiện “mô tả và tổ chức tài nguyên”.
**Tiền điều kiện:** Có quyền sửa metadata hoặc vị trí quản lý.
**Thông tin đầu vào:** Tên; mô tả; văn bản thay thế; tác giả; quyền sử dụng; ngày hết quyền; thư mục hoặc bộ sưu tập.

**Luồng chính**

1. Mở tài nguyên.
2. Chỉnh metadata và thông tin quyền sử dụng.
3. Chọn thư mục hoặc bộ sưu tập.
4. Xem ảnh hưởng trước khi lưu.

**Luồng thay thế và ngoại lệ**

- E01: Thiếu thông tin bắt buộc: chưa cho sử dụng tại vị trí yêu cầu.
- E02: Đổi phạm vi làm người khác mất quyền: cảnh báo và kiểm tra thẩm quyền.

**BR-AST-02:** Thư mục không tự định nghĩa quyền nếu chưa có chính sách. Chia sẻ tài nguyên phải chỉ rõ site đích và quyền sử dụng; không tự cấp quyền sửa metadata, thay tệp hoặc đổi chủ sở hữu nguồn. Metadata làm thay đổi bản hiển thị phải được phiên bản hóa hoặc chụp theo bản phát hành.
**Kết quả thành công:** Tài nguyên được tổ chức và có căn cứ sử dụng rõ ràng.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-AST-02-01/v1:** Given hình có ý nghĩa nhưng thiếu văn bản thay thế ở vị trí bắt buộc; When phát hành; Then báo thiếu dữ liệu.
- **AC-AST-02-02/v1:** Given chỉ đổi bộ sưu tập; When mở liên kết hợp lệ cũ; Then định danh tệp không đổi.
- **AC-AST-02-03/v1:** Given quyền sử dụng đã hết; When phát hành mới; Then bị chặn.

**Use case liên quan / phụ thuộc:** [UC-AST-01](08-assets.md#uc-ast-01).

## UC-AST-03

**Thay thế tệp bằng phiên bản mới**

**Triển khai:** P3. **Actor:** Quản trị tài nguyên.
**Mục tiêu:** Có phiên bản tài nguyên mới, bản cũ vẫn phục vụ nơi đang tham chiếu.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thay thế tệp bằng phiên bản mới”.
**Tiền điều kiện:** Có quyền tạo phiên bản tài nguyên. Tệp mới được kiểm tra như tệp tải lên.
**Thông tin đầu vào:** Tài nguyên; tệp mới; lý do; nội dung cần cập nhật.

**Luồng chính**

1. Chọn tài nguyên và tải bản thay thế.
2. Kiểm tra an toàn cùng metadata.
3. Xem nơi đang sử dụng bản cũ.
4. Lưu phiên bản mới và chọn nội dung cần cập nhật qua bản nháp.

**Luồng thay thế và ngoại lệ**

- E01: Tệp mới không đạt kiểm tra: giữ nguyên bản cũ.
- E02: Nội dung đang công khai: không thay ngay tệp mà người đọc đang nhận.

**BR-AST-03:** Bản phát hành phải biết phiên bản tài nguyên đã dùng. Không sử dụng thay tệp để vượt phê duyệt nội dung.
**Kết quả thành công:** Có phiên bản tài nguyên mới, bản cũ vẫn phục vụ nơi đang tham chiếu.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-AST-03-01/v1:** Given tệp v1 đang công khai; When tải v2; Then người đọc vẫn nhận v1 đến khi phát hành cập nhật.
- **AC-AST-03-02/v1:** Given v2 không an toàn; When hoàn tất kiểm tra; Then v2 không được dùng.
- **AC-AST-03-03/v1:** Given xem lịch sử; When chọn v1; Then biết nội dung nào đang dùng v1.

**Use case liên quan / phụ thuộc:** [UC-AST-01](08-assets.md#uc-ast-01), [UC-AST-04](08-assets.md#uc-ast-04), [UC-VER-01](10-versioning.md#uc-ver-01).

## UC-AST-04

**Kiểm tra nơi sử dụng tài nguyên**

**Triển khai:** P2. **Actor:** Quản trị tài nguyên hoặc người xuất bản.
**Mục tiêu:** Người dùng đánh giá được ảnh hưởng trước khi thay thế hoặc xóa.
**Kích hoạt:** Người dùng yêu cầu thực hiện “kiểm tra nơi sử dụng tài nguyên”.
**Tiền điều kiện:** Có quyền kiểm tra phụ thuộc trong phạm vi.
**Thông tin đầu vào:** Tài nguyên hoặc phiên bản; bộ lọc trạng thái và site.

**Luồng chính**

1. Chọn tài nguyên.
2. Xem nội dung, phiên bản, trang và lịch tham chiếu.
3. Phân biệt tham chiếu nháp, được duyệt và đang công khai.
4. Mở đối tượng được phép để xử lý.

**Luồng thay thế và ngoại lệ**

- E01: Có phụ thuộc ngoài phạm vi: báo đang bị chặn mà không lộ dữ liệu chi tiết.
- E02: Chưa xác minh hết phụ thuộc: không khẳng định tài nguyên có thể xóa.

**BR-AST-04:** Kiểm tra dùng dữ liệu phụ thuộc chính thức, không chỉ dựa chỉ mục tìm kiếm. Mức tối thiểu được làm cùng P1; màn hình đầy đủ ở P2.
**Kết quả thành công:** Người dùng đánh giá được ảnh hưởng trước khi thay thế hoặc xóa.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-AST-04-01/v1:** Given tài nguyên được lịch hẹn tham chiếu; When kiểm tra; Then lịch được tính là phụ thuộc.
- **AC-AST-04-02/v1:** Given tài nguyên có phụ thuộc tại site khác ngoài quyền quản lý; When xem hoặc thu hồi chia sẻ; Then thấy cảnh báo nhưng không thấy nội dung mật.
- **AC-AST-04-03/v1:** Given chỉ mục tìm kiếm chưa cập nhật; When kiểm tra xóa; Then phụ thuộc thực tế vẫn chặn xóa.

**Use case liên quan / phụ thuộc:** [UC-AST-01](08-assets.md#uc-ast-01), [UC-CNT-01](09-content.md#uc-cnt-01).

## UC-AST-05

**Lưu trữ tài nguyên không còn dùng mới**

**Triển khai:** P1. **Actor:** Quản trị tài nguyên.
**Mục tiêu:** Tài nguyên không được gán mới, phụ thuộc cũ vẫn được bảo toàn cho đến khi xử lý.
**Kích hoạt:** Người dùng yêu cầu thực hiện “lưu trữ tài nguyên không còn dùng mới”.
**Tiền điều kiện:** Có quyền lưu trữ tài nguyên và kiểm tra nơi sử dụng.
**Thông tin đầu vào:** Tài nguyên; lý do; cách xử lý phụ thuộc.

**Luồng chính**

1. Chọn tài nguyên cần ngừng dùng mới.
2. Xem phụ thuộc hiện có.
3. Xác nhận lưu trữ và cấm gán mới.
4. Theo dõi các nơi còn sử dụng để xử lý sau.

**Luồng thay thế và ngoại lệ**

- E01: Còn tham chiếu đang công khai: không xóa bytes hoặc làm hỏng bản công khai.
- E02: Cần thu hồi khẩn cấp do không an toàn: dùng quy trình thu hồi bản phát hành liên quan.

**BR-AST-05:** Lưu trữ khác xóa vĩnh viễn. Xóa vĩnh viễn thuộc Governance sau khi đủ điều kiện.
**Kết quả thành công:** Tài nguyên không được gán mới, phụ thuộc cũ vẫn được bảo toàn cho đến khi xử lý.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-AST-05-01/v1:** Given tài nguyên đã lưu trữ; When gán mới vào bài; Then bị chặn.
- **AC-AST-05-02/v1:** Given tài nguyên còn trong bản công khai; When lưu trữ; Then bản công khai không bị hỏng âm thầm.
- **AC-AST-05-03/v1:** Given yêu cầu xóa bytes còn phụ thuộc; When thực hiện; Then bị chặn.

**Use case liên quan / phụ thuộc:** [UC-AST-01](08-assets.md#uc-ast-01).
