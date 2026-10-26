# Mô hình nội dung

**Domain:** `MDL` · **Bắt đầu:** P1 · **Số use case:** 4
**Trạng thái:** Baseline đề xuất v0.1 — chưa phải yêu cầu đã được khách hàng phê duyệt.

Quản lý loại nội dung, trường, quan hệ và quy tắc nhập liệu. Không quản lý giá trị của từng bài viết.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-MDL-01](#uc-mdl-01) | Định nghĩa và kích hoạt loại nội dung | P1 |
| [UC-MDL-02](#uc-mdl-02) | Thay đổi mô hình đang được sử dụng | P3 |
| [UC-MDL-03](#uc-mdl-03) | Chuyển nội dung sang mô hình mới | P3 |
| [UC-MDL-04](#uc-mdl-04) | Ngừng sử dụng một loại nội dung | P3 |

## UC-MDL-01

**Định nghĩa và kích hoạt loại nội dung**

**Triển khai:** P1. **Actor:** Người thiết kế nội dung.
**Mục tiêu:** Biên tập viên tạo được nội dung theo mô hình đã kích hoạt.
**Kích hoạt:** Người dùng yêu cầu thực hiện “định nghĩa và kích hoạt loại nội dung”.
**Tiền điều kiện:** Có quyền thiết kế mô hình trong workspace hoặc site.
**Thông tin đầu vào:** Tên loại; trường; kiểu dữ liệu; bắt buộc; giới hạn; tham chiếu; trường theo locale.

**Luồng chính**

1. Tạo mô hình nháp.
2. Khai báo trường và quy tắc nhập liệu.
3. Thử bằng dữ liệu hợp lệ cùng dữ liệu sai.
4. Kích hoạt phiên bản mô hình đã kiểm tra.

**Luồng thay thế và ngoại lệ**

- E01: Mã trường trùng: chặn.
- E02: Quy tắc mâu thuẫn hoặc tham chiếu chưa tồn tại: không cho kích hoạt.

**BR-MDL-01:** Mã trường ổn định, không phụ thuộc nhãn hiển thị. Lưu nháp có thể thiếu trường theo chính sách; gửi duyệt phải thỏa quy tắc bắt buộc.
**Kết quả thành công:** Biên tập viên tạo được nội dung theo mô hình đã kích hoạt.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-MDL-01-01/v1:** Given hai trường trùng mã; When kích hoạt; Then bị chặn.
- **AC-MDL-01-02/v1:** Given thiếu trường bắt buộc; When gửi duyệt bài; Then chỉ rõ trường thiếu.
- **AC-MDL-01-03/v1:** Given trường liên kết chỉ nhận loại A; When chọn loại B; Then không được chấp nhận.

**Use case liên quan / phụ thuộc:** [UC-SITE-01](04-site-delivery.md#uc-site-01), [UC-LOC-01](05-localization.md#uc-loc-01).

## UC-MDL-02

**Thay đổi mô hình đang được sử dụng**

**Triển khai:** P3. **Actor:** Người thiết kế nội dung.
**Mục tiêu:** Có phiên bản mô hình mới và kế hoạch áp dụng có thể kiểm tra.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thay đổi mô hình đang được sử dụng”.
**Tiền điều kiện:** Mô hình đã có nội dung sử dụng.
**Thông tin đầu vào:** Phiên bản hiện tại; thay đổi trường; lý do; cách xử lý dữ liệu cũ.

**Luồng chính**

1. Tạo phiên bản mô hình nháp từ bản hiện tại.
2. Sửa trường và quy tắc.
3. Xem ảnh hưởng tới nội dung, bản dịch và kênh đọc.
4. Xác nhận kế hoạch kích hoạt hoặc yêu cầu chuyển đổi.

**Luồng thay thế và ngoại lệ**

- E01: Xóa trường có dữ liệu: không thực hiện âm thầm.
- E02: Đổi kiểu không tương thích: yêu cầu ánh xạ hoặc giữ phiên bản cũ.

**BR-MDL-02:** Mỗi phiên bản nội dung biết mô hình đã dùng. Thay đổi mô hình không làm bản công khai thay đổi ngay.
**Kết quả thành công:** Có phiên bản mô hình mới và kế hoạch áp dụng có thể kiểm tra.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-MDL-02-01/v1:** Given đổi trường chữ thành số với dữ liệu không chuyển được; When kích hoạt trực tiếp; Then bị chặn.
- **AC-MDL-02-02/v1:** Given xóa trường có dữ liệu; When xác nhận; Then phải có đánh giá ảnh hưởng.
- **AC-MDL-02-03/v1:** Given mô hình mới được tạo; When đọc bản công khai cũ; Then dữ liệu cũ không mất.

**Use case liên quan / phụ thuộc:** [UC-MDL-01](06-content-model.md#uc-mdl-01), [UC-CNT-01](09-content.md#uc-cnt-01).

## UC-MDL-03

**Chuyển nội dung sang mô hình mới**

**Triển khai:** P3. **Actor:** Quản trị nội dung.
**Mục tiêu:** Có nội dung theo mô hình mới để kiểm tra và duyệt lại.
**Kích hoạt:** Người dùng yêu cầu thực hiện “chuyển nội dung sang mô hình mới”.
**Tiền điều kiện:** Có phiên bản mô hình đích và phương án chuyển đổi đã được duyệt.
**Thông tin đầu vào:** Tập nội dung; mô hình đích; ánh xạ; cách xử lý lỗi.

**Luồng chính**

1. Chọn tập nội dung.
2. Chạy mô phỏng và xem lỗi từng trường.
3. Xác nhận chuyển các bản hợp lệ.
4. Kiểm tra các bản nháp mới và báo cáo kết quả.

**Luồng thay thế và ngoại lệ**

- E01: Không thể chuyển một giá trị: giữ nguyên đối tượng đó và báo lỗi.
- E02: Nội dung đã được sửa sau mô phỏng: yêu cầu đánh giá lại đối tượng bị thay đổi.

**BR-MDL-03:** Chuyển đổi tạo phiên bản mới, không sửa lịch sử hoặc tự tái sử dụng phê duyệt. Chính sách từng phần hay toàn bộ được chọn trước.
**Kết quả thành công:** Có nội dung theo mô hình mới để kiểm tra và duyệt lại.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-MDL-03-01/v1:** Given chuyển đổi thành công; When xem lịch sử; Then bản cũ vẫn còn.
- **AC-MDL-03-02/v1:** Given bài đã sửa sau mô phỏng; When chạy chuyển đổi; Then không ghi đè im lặng.
- **AC-MDL-03-03/v1:** Given chuyển đổi tạo v5; When xuất bản v5 chưa duyệt; Then bị chặn.

**Use case liên quan / phụ thuộc:** [UC-MDL-02](06-content-model.md#uc-mdl-02), [UC-VER-01](10-versioning.md#uc-ver-01), [UC-CNT-02](09-content.md#uc-cnt-02).

## UC-MDL-04

**Ngừng sử dụng một loại nội dung**

**Triển khai:** P3. **Actor:** Người thiết kế nội dung.
**Mục tiêu:** Không tạo mới bằng loại đã ngừng, vẫn đọc được dữ liệu lịch sử hợp lệ.
**Kích hoạt:** Người dùng yêu cầu thực hiện “ngừng sử dụng một loại nội dung”.
**Tiền điều kiện:** Có quyền quản trị mô hình. Đã xem các nội dung đang sử dụng.
**Thông tin đầu vào:** Loại nội dung; ngày ngừng dùng; lý do; loại thay thế nếu có.

**Luồng chính**

1. Chọn loại cần ngừng tạo mới.
2. Xem nội dung và quan hệ liên quan.
3. Chọn cách xử lý nội dung hiện có.
4. Xác nhận trạng thái ngừng sử dụng.

**Luồng thay thế và ngoại lệ**

- E01: Còn quan hệ bắt buộc chưa xử lý: chặn xóa định nghĩa.
- E02: Người đang tạo bài theo loại vừa ngừng dùng: báo trạng thái mới khi lưu.

**BR-MDL-04:** Ngừng tạo mới không xóa nội dung lịch sử. Xóa vĩnh viễn định nghĩa có dữ liệu ngoài baseline tự động.
**Kết quả thành công:** Không tạo mới bằng loại đã ngừng, vẫn đọc được dữ liệu lịch sử hợp lệ.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-MDL-04-01/v1:** Given loại đã ngừng; When tạo bài mới; Then bị chặn.
- **AC-MDL-04-02/v1:** Given có bản công khai từ loại đã ngừng; When đọc; Then vẫn đọc theo chính sách hiện có.
- **AC-MDL-04-03/v1:** Given còn phụ thuộc; When xóa định nghĩa; Then bị chặn.

**Use case liên quan / phụ thuộc:** [UC-MDL-01](06-content-model.md#uc-mdl-01), [UC-MDL-02](06-content-model.md#uc-mdl-02).
