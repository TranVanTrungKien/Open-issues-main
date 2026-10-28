# Phân loại nội dung

**Domain:** `TAX` · **Bắt đầu:** P1 · **Số use case:** 4
**Trạng thái:** Baseline đề xuất v0.1 — chưa phải yêu cầu đã được khách hàng phê duyệt.

Quản lý từ vựng, danh mục, thẻ và quan hệ phân cấp. Không sử dụng danh mục thay cho quyền truy cập.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-TAX-01](#uc-tax-01) | Thiết lập danh mục và thẻ | P1 |
| [UC-TAX-02](#uc-tax-02) | Phân loại một nội dung | P1 |
| [UC-TAX-03](#uc-tax-03) | Hợp nhất danh mục hoặc thẻ trùng | P3 |
| [UC-TAX-04](#uc-tax-04) | Ngừng sử dụng một mục phân loại | P3 |

## UC-TAX-01

**Thiết lập danh mục và thẻ**

**Triển khai:** P1. **Actor:** Quản trị phân loại.
**Mục tiêu:** Có bộ phân loại hợp lệ để gán cho nội dung.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thiết lập danh mục và thẻ”.
**Tiền điều kiện:** Có quyền quản lý bộ phân loại trong phạm vi.
**Thông tin đầu vào:** Bộ phân loại; mã; tên; quan hệ cha-con; thứ tự; locale.

**Luồng chính**

1. Tạo bộ phân loại hoặc chọn bộ có sẵn.
2. Thêm danh mục hoặc thẻ.
3. Sắp xếp quan hệ phân cấp.
4. Kiểm tra mã và cây trước khi lưu.

**Luồng thay thế và ngoại lệ**

- E01: Mã bị trùng trong bộ phân loại: chặn.
- E02: Danh mục trở thành tổ tiên của chính nó: chặn.

**BR-TAX-01:** Danh mục và thẻ không mặc định cấp quyền nội dung. Mã định danh ổn định khi đổi nhãn.
**Kết quả thành công:** Có bộ phân loại hợp lệ để gán cho nội dung.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-TAX-01-01/v1:** Given mã đã tồn tại; When thêm mã trùng trong cùng bộ; Then bị chặn.
- **AC-TAX-01-02/v1:** Given A là cha của B; When chuyển A dưới B; Then bị chặn.
- **AC-TAX-01-03/v1:** Given đổi nhãn danh mục; When xem liên kết nội dung; Then mã liên kết không đổi.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04).

## UC-TAX-02

**Phân loại một nội dung**

**Triển khai:** P1. **Actor:** Biên tập viên.
**Mục tiêu:** Bản nháp có phân loại hợp lệ, có thể gửi duyệt.
**Kích hoạt:** Người dùng yêu cầu thực hiện “phân loại một nội dung”.
**Tiền điều kiện:** Có quyền sửa bản nháp. Bộ phân loại được mô hình nội dung cho phép.
**Thông tin đầu vào:** Nội dung; phiên bản; danh mục hoặc thẻ; danh mục chính nếu cần.

**Luồng chính**

1. Mở bản nháp nội dung.
2. Chọn danh mục hoặc thẻ được phép.
3. Kiểm tra số lượng và danh mục chính.
4. Lưu thành phiên bản nội dung mới.

**Luồng thay thế và ngoại lệ**

- E01: Danh mục đã ngừng sử dụng: không cho gán mới.
- E02: Gán vượt giới hạn của mô hình: chỉ rõ lỗi.

**BR-TAX-02:** Việc gán thuộc bản nội dung; không sửa trực tiếp phân loại của bản đang công khai. Mô hình quyết định có bắt buộc phân loại hay không.
**Kết quả thành công:** Bản nháp có phân loại hợp lệ, có thể gửi duyệt.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-TAX-02-01/v1:** Given danh mục đã ngừng; When gán mới; Then bị từ chối.
- **AC-TAX-02-02/v1:** Given mô hình chỉ cho một danh mục chính; When chọn hai; Then bị chặn.
- **AC-TAX-02-03/v1:** Given sửa phân loại bản nháp; When đọc bài công khai; Then phân loại công khai chưa đổi.

**Use case liên quan / phụ thuộc:** [UC-TAX-01](07-taxonomy.md#uc-tax-01), [UC-MDL-01](06-content-model.md#uc-mdl-01), [UC-CNT-01](09-content.md#uc-cnt-01).

## UC-TAX-03

**Hợp nhất danh mục hoặc thẻ trùng**

**Triển khai:** P3. **Actor:** Quản trị phân loại.
**Mục tiêu:** Danh mục được chuẩn hóa mà vẫn truy vết được các liên kết cũ.
**Kích hoạt:** Người dùng yêu cầu thực hiện “hợp nhất danh mục hoặc thẻ trùng”.
**Tiền điều kiện:** Nguồn và đích cùng phạm vi được phép. Có báo cáo nơi sử dụng.
**Thông tin đầu vào:** Danh mục nguồn; danh mục đích; ánh xạ; kế hoạch cập nhật nội dung.

**Luồng chính**

1. Chọn các mục cần hợp nhất.
2. Xem số nội dung, menu và bản dịch liên quan.
3. Chọn cách ngừng gán mục cũ và tạo thay đổi cho nội dung.
4. Xác nhận và kiểm tra báo cáo.

**Luồng thay thế và ngoại lệ**

- E01: Nguồn hoặc đích tạo cây không hợp lệ: chặn.
- E02: Nội dung không có quyền sửa: ghi rõ chưa xử lý, không tự bỏ qua.

**BR-TAX-03:** Không âm thầm sửa bản công khai; thay đổi hiển thị cần qua phát hành. Mục cũ có ánh xạ hoặc dấu vết để tra cứu lịch sử.
**Kết quả thành công:** Danh mục được chuẩn hóa mà vẫn truy vết được các liên kết cũ.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-TAX-03-01/v1:** Given nguồn có bài công khai; When hợp nhất; Then bản công khai không tự đổi phân loại.
- **AC-TAX-03-02/v1:** Given nội dung ngoài quyền quản trị; When cập nhật hàng loạt; Then báo không xử lý được nội dung đó.
- **AC-TAX-03-03/v1:** Given hoàn tất; When tra cứu mã cũ; Then có thể xác định mục đích và lịch sử hợp nhất.

**Use case liên quan / phụ thuộc:** [UC-TAX-01](07-taxonomy.md#uc-tax-01), [UC-TAX-02](07-taxonomy.md#uc-tax-02), [UC-VER-01](10-versioning.md#uc-ver-01).

## UC-TAX-04

**Ngừng sử dụng một mục phân loại**

**Triển khai:** P3. **Actor:** Quản trị phân loại.
**Mục tiêu:** Không gán mới mục đã ngừng; nội dung hiện có không bị mất.
**Kích hoạt:** Người dùng yêu cầu thực hiện “ngừng sử dụng một mục phân loại”.
**Tiền điều kiện:** Có quyền quản lý mục và đã xem phụ thuộc.
**Thông tin đầu vào:** Mục phân loại; lý do; mục thay thế nếu có.

**Luồng chính**

1. Chọn mục cần ngừng dùng.
2. Xem nội dung và menu liên quan.
3. Chọn giữ lịch sử hoặc tạo kế hoạch chuyển sang mục khác.
4. Xác nhận ngừng gán mới.

**Luồng thay thế và ngoại lệ**

- E01: Còn phụ thuộc lịch sử: không cho xóa cứng trực tiếp.
- E02: Ngừng danh mục cha còn con hoạt động: yêu cầu xử lý cây trước.

**BR-TAX-04:** Ngừng dùng không đồng nghĩa xóa các bài được phân loại. Mục ngừng dùng vẫn được hiển thị trong lịch sử khi có quyền.
**Kết quả thành công:** Không gán mới mục đã ngừng; nội dung hiện có không bị mất.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-TAX-04-01/v1:** Given mục đã ngừng; When chọn cho bài mới; Then không được chấp nhận.
- **AC-TAX-04-02/v1:** Given còn bài liên kết; When xóa cứng; Then bị chặn.
- **AC-TAX-04-03/v1:** Given ngừng dùng thành công; When xem lịch sử bài; Then còn thông tin phân loại cũ.

**Use case liên quan / phụ thuộc:** [UC-TAX-01](07-taxonomy.md#uc-tax-01), [UC-TAX-02](07-taxonomy.md#uc-tax-02).
