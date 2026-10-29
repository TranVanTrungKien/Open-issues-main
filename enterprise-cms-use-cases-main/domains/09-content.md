# Biên tập nội dung

**Domain:** `CNT` · **Bắt đầu:** P1 · **Số use case:** 6
**Trạng thái:** Baseline đề xuất v0.1; riêng quyết định chia sẻ nội dung chéo site tại OI-02 đã được chốt.

Quản lý nội dung và vòng đời biên tập. Không phê duyệt hoặc công khai nội dung bằng thao tác lưu.
**Quyết định OI-02:** Nội dung được cô lập theo site mặc định. Site đích chỉ được tham chiếu nội dung từ site nguồn khi có cấp quyền chia sẻ rõ ràng; quyền sử dụng không tự cấp quyền sửa nguồn.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-CNT-01](#uc-cnt-01) | Tạo nội dung nháp | P1 |
| [UC-CNT-02](#uc-cnt-02) | Chỉnh sửa nội dung mà không ghi đè thay đổi khác | P1 |
| [UC-CNT-03](#uc-cnt-03) | Nhân bản nội dung để tái sử dụng | P3 |
| [UC-CNT-04](#uc-cnt-04) | Xem trước một phiên bản nội dung | P1 |
| [UC-CNT-05](#uc-cnt-05) | Lưu trữ nội dung không còn vận hành | P2 |
| [UC-CNT-06](#uc-cnt-06) | Khôi phục nội dung từ kho lưu trữ | P3 |

## UC-CNT-01

**Tạo nội dung nháp**

**Triển khai:** P1. **Actor:** Biên tập viên.
**Mục tiêu:** Có nội dung nháp có thể mở lại và tiếp tục biên tập.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tạo nội dung nháp”.
**Tiền điều kiện:** Có quyền tạo nội dung trong site và locale. Loại nội dung đã kích hoạt.
**Thông tin đầu vào:** Loại nội dung; site; locale; tiêu đề; chủ sở hữu; giá trị trường.

**Luồng chính**

1. Chọn site, locale và loại nội dung.
2. Nhập dữ liệu cùng tài nguyên hoặc nội dung liên quan được phép.
3. Lưu bản nháp.
4. Nhận mã nội dung, phiên bản đầu và trạng thái lưu.

**Luồng thay thế và ngoại lệ**

- E01: Dữ liệu thiếu trường chỉ bắt buộc khi duyệt: cho lưu nháp có cảnh báo.
- E02: Giá trị sai kiểu, tham chiếu ngoài quyền hoặc mô hình đã ngừng dùng: chặn lưu phần không hợp lệ.

**BR-CNT-01:** Bản nháp không có phê duyệt, lịch hay trạng thái công khai. Chủ sở hữu và locale phải được xác định. Tham chiếu chéo site chỉ hợp lệ khi site đích có quyền sử dụng nội dung/tài nguyên được chia sẻ còn hiệu lực.
**Kết quả thành công:** Có nội dung nháp có thể mở lại và tiếp tục biên tập.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-CNT-01-01/v1:** Given dữ liệu hợp lệ; When lưu; Then có nội dung và phiên bản đầu, chưa công khai.
- **AC-CNT-01-02/v1:** Given liên kết tới đối tượng ở site khác không được chia sẻ cho site đích; When lưu; Then bị từ chối.
- **AC-CNT-01-03/v1:** Given gửi lại cùng yêu cầu lưu do mất phản hồi; When xử lý; Then không tạo hai nội dung cho cùng yêu cầu.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04), [UC-MDL-01](06-content-model.md#uc-mdl-01), [UC-LOC-01](05-localization.md#uc-loc-01), [UC-AST-01](08-assets.md#uc-ast-01), [UC-TAX-01](07-taxonomy.md#uc-tax-01).

## UC-CNT-02

**Chỉnh sửa nội dung mà không ghi đè thay đổi khác**

**Triển khai:** P1. **Actor:** Biên tập viên.
**Mục tiêu:** Thay đổi được lưu thành phiên bản mới mà không mất cập nhật đồng thời.
**Kích hoạt:** Người dùng yêu cầu thực hiện “chỉnh sửa nội dung mà không ghi đè thay đổi khác”.
**Tiền điều kiện:** Có quyền sửa nội dung. Đã mở một phiên bản xác định.
**Thông tin đầu vào:** Nội dung; phiên bản đang sửa; giá trị mới; lý do thay đổi nếu bắt buộc.

**Luồng chính**

1. Mở bản làm việc và xem trạng thái.
2. Sửa trường, phân loại và quan hệ được phép.
3. Lưu với căn cứ phiên bản đã mở.
4. Nhận phiên bản mới hoặc thông báo xung đột.

**Luồng thay thế và ngoại lệ**

- E01: Người khác đã lưu trước: hiển thị khác biệt, yêu cầu nạp lại hoặc hợp nhất có chủ ý.
- E02: Phiên bản đang được duyệt: baseline yêu cầu rút yêu cầu trước khi sửa.

**BR-CNT-02:** Không sửa trực tiếp phiên bản bất biến. Chỉnh bản đã duyệt tạo bản nháp mới, không mang theo phê duyệt hoặc lịch của bản cũ.
**Kết quả thành công:** Thay đổi được lưu thành phiên bản mới mà không mất cập nhật đồng thời.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-CNT-02-01/v1:** Given A và B mở v2, A đã lưu v3; When B lưu từ v2; Then không ghi đè v3 im lặng.
- **AC-CNT-02-02/v1:** Given v2 đang công khai; When lưu v3; Then người đọc vẫn thấy v2.
- **AC-CNT-02-03/v1:** Given v2 đã duyệt; When tạo v3; Then v3 chưa được duyệt.
- **AC-CNT-02-04/v1:** Given phiên bản đang ở yêu cầu duyệt mở; When sửa trực tiếp; Then phải rút yêu cầu trước theo baseline.

**Use case liên quan / phụ thuộc:** [UC-CNT-01](09-content.md#uc-cnt-01), [UC-VER-01](10-versioning.md#uc-ver-01).

## UC-CNT-03

**Nhân bản nội dung để tái sử dụng**

**Triển khai:** P3. **Actor:** Biên tập viên.
**Mục tiêu:** Có nội dung mới để biên tập mà không ảnh hưởng nguồn.
**Kích hoạt:** Người dùng yêu cầu thực hiện “nhân bản nội dung để tái sử dụng”.
**Tiền điều kiện:** Có quyền đọc nguồn và tạo nội dung trong phạm vi đích.
**Thông tin đầu vào:** Nguồn và phiên bản; site hoặc locale đích; tên mới; cách xử lý tham chiếu.

**Luồng chính**

1. Chọn phiên bản nguồn.
2. Chọn phạm vi đích.
3. Xem trường và tham chiếu có thể sao chép.
4. Xác nhận tạo nội dung nháp mới.

**Luồng thay thế và ngoại lệ**

- E01: Tài nguyên nguồn không có quyền dùng ở đích: yêu cầu thay hoặc bỏ tham chiếu.
- E02: Đường dẫn bị trùng: không sao chép đường dẫn nguyên trạng.

**BR-CNT-03:** Nội dung mới có định danh riêng. Không sao chép phê duyệt, lịch xuất bản, lịch sử tác giả hoặc quyền truy cập một cách mặc định.
**Kết quả thành công:** Có nội dung mới để biên tập mà không ảnh hưởng nguồn.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-CNT-03-01/v1:** Given nguồn đã xuất bản; When nhân bản; Then bản sao là nháp chưa duyệt.
- **AC-CNT-03-02/v1:** Given tài nguyên không dùng được ở đích; When xác nhận; Then báo rõ tham chiếu cần xử lý.
- **AC-CNT-03-03/v1:** Given sửa bản sao; When xem nguồn; Then nguồn không đổi.

**Use case liên quan / phụ thuộc:** [UC-CNT-01](09-content.md#uc-cnt-01), [UC-IAM-04](01-identity-access.md#uc-iam-04).

## UC-CNT-04

**Xem trước một phiên bản nội dung**

**Triển khai:** P1. **Actor:** Biên tập viên, người duyệt hoặc người được cấp quyền xem trước.
**Mục tiêu:** Người có quyền kiểm tra được đúng phiên bản trước khi duyệt hoặc phát hành.
**Kích hoạt:** Người dùng yêu cầu thực hiện “xem trước một phiên bản nội dung”.
**Tiền điều kiện:** Có quyền xem đúng phiên bản và phạm vi.
**Thông tin đầu vào:** Nội dung; phiên bản; site; locale; mẫu hoặc kênh xem trước.

**Luồng chính**

1. Chọn phiên bản muốn xem.
2. Chọn cách hiển thị phù hợp kênh.
3. Mở bản xem trước có nhãn phiên bản rõ ràng.
4. Kiểm tra nội dung và các tài nguyên liên quan.

**Luồng thay thế và ngoại lệ**

- E01: Thiếu mẫu hoặc thành phần: hiển thị lỗi xem trước cụ thể.
- E02: Liên kết xem trước hết hạn hoặc người xem mất quyền: từ chối truy cập.

**BR-CNT-04:** Xem trước không xuất bản. P1 dùng mẫu cố định hoặc trình xem có cấu trúc; công cụ thiết kế trang tự do ở P3.
**Kết quả thành công:** Người có quyền kiểm tra được đúng phiên bản trước khi duyệt hoặc phát hành.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-CNT-04-01/v1:** Given chọn v3; When xem trước; Then nhãn và nội dung đều thuộc v3.
- **AC-CNT-04-02/v1:** Given người chưa có quyền mở đường xem trước; When truy cập; Then bị từ chối.
- **AC-CNT-04-03/v1:** Given xem trước thành công; When khách mở site; Then trạng thái công khai không đổi.

**Use case liên quan / phụ thuộc:** [UC-CNT-01](09-content.md#uc-cnt-01), [UC-SITE-01](04-site-delivery.md#uc-site-01), [UC-VER-01](10-versioning.md#uc-ver-01).

## UC-CNT-05

**Lưu trữ nội dung không còn vận hành**

**Triển khai:** P2. **Actor:** Chủ sở hữu hoặc quản trị nội dung.
**Mục tiêu:** Nội dung ngừng được biên tập và sử dụng mới theo baseline, còn tra cứu lịch sử.
**Kích hoạt:** Người dùng yêu cầu thực hiện “lưu trữ nội dung không còn vận hành”.
**Tiền điều kiện:** Có quyền lưu trữ. Đã xác định bản công khai, lịch và yêu cầu duyệt đang mở.
**Thông tin đầu vào:** Nội dung; lý do; cách xử lý công việc đang mở.

**Luồng chính**

1. Mở báo cáo phụ thuộc và trạng thái.
2. Gỡ bản công khai, hủy lịch và rút yêu cầu duyệt khi cần.
3. Xác nhận lưu trữ sau khi điều kiện đã đạt.
4. Xem nội dung ở kho lưu trữ.

**Luồng thay thế và ngoại lệ**

- E01: Bản công khai chưa gỡ hoàn tất: không xác nhận lưu trữ như đã ẩn khỏi công chúng.
- E02: Còn lịch hoặc yêu cầu duyệt: yêu cầu xử lý rõ, không âm thầm bỏ.

**BR-CNT-05:** Lưu trữ không xóa lịch sử và không thay chính sách lưu giữ. Người không có quyền lưu trữ không thể dùng thao tác xóa để thay thế.
**Kết quả thành công:** Nội dung ngừng được biên tập và sử dụng mới theo baseline, còn tra cứu lịch sử.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-CNT-05-01/v1:** Given còn lịch đang hiệu lực; When lưu trữ; Then bị chặn cho đến khi hủy hoặc xử lý lịch.
- **AC-CNT-05-02/v1:** Given gỡ chưa hoàn tất; When lưu trữ; Then không báo nội dung đã biến mất khỏi mọi kênh.
- **AC-CNT-05-03/v1:** Given lưu trữ thành công; When xem lịch sử; Then các phiên bản còn nguyên.

**Use case liên quan / phụ thuộc:** [UC-PUB-03](13-publishing.md#uc-pub-03), [UC-PUB-04](13-publishing.md#uc-pub-04), [UC-WF-05](12-workflow.md#uc-wf-05), [UC-VER-01](10-versioning.md#uc-ver-01).

## UC-CNT-06

**Khôi phục nội dung từ kho lưu trữ**

**Triển khai:** P3. **Actor:** Chủ sở hữu hoặc quản trị nội dung.
**Mục tiêu:** Nội dung trở lại dạng nháp có thể xử lý theo quy trình hiện hành.
**Kích hoạt:** Người dùng yêu cầu thực hiện “khôi phục nội dung từ kho lưu trữ”.
**Tiền điều kiện:** Nội dung còn được lưu giữ và người dùng có quyền khôi phục.
**Thông tin đầu vào:** Nội dung; phiên bản làm căn cứ; chủ sở hữu; phạm vi đích.

**Luồng chính**

1. Chọn nội dung đã lưu trữ.
2. Kiểm tra mô hình, tài nguyên và phạm vi hiện hành.
3. Xác nhận chủ sở hữu và cách xử lý dữ liệu không tương thích.
4. Khôi phục để tiếp tục bằng bản nháp.

**Luồng thay thế và ngoại lệ**

- E01: Mô hình không còn hỗ trợ: yêu cầu chuyển đổi trước khi gửi duyệt.
- E02: Tài nguyên đã bị xóa hoặc hết quyền: chỉ rõ phần cần thay.

**BR-CNT-06:** Không tự khôi phục phê duyệt, lịch hoặc trạng thái công khai cũ.
**Kết quả thành công:** Nội dung trở lại dạng nháp có thể xử lý theo quy trình hiện hành.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-CNT-06-01/v1:** Given nội dung từng công khai; When khôi phục; Then không tự công khai.
- **AC-CNT-06-02/v1:** Given lịch cũ từng tồn tại; When khôi phục; Then lịch không tự chạy lại.
- **AC-CNT-06-03/v1:** Given tài nguyên đã bị xóa; When gửi duyệt; Then báo tham chiếu không hợp lệ.

**Use case liên quan / phụ thuộc:** [UC-CNT-05](09-content.md#uc-cnt-05), [UC-CNT-01](09-content.md#uc-cnt-01).
