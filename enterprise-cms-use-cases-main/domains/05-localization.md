# Ngôn ngữ và bản dịch

**Domain:** `LOC` · **Bắt đầu:** P0 · **Số use case:** 4
**Trạng thái:** Baseline đề xuất v0.1 — chưa phải yêu cầu đã được khách hàng phê duyệt.

Quản lý ngôn ngữ, quan hệ bản dịch và độ mới. Dùng lại Workflow và Publishing, không tạo quy trình duyệt riêng.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-LOC-01](#uc-loc-01) | Thiết lập ngôn ngữ và quy tắc dự phòng | P0 |
| [UC-LOC-02](#uc-loc-02) | Tạo bản dịch từ một phiên bản nguồn | P3 |
| [UC-LOC-03](#uc-loc-03) | Gửi bản dịch để duyệt và phát hành | P3 |
| [UC-LOC-04](#uc-loc-04) | Rà soát bản dịch khi nguồn thay đổi | P3 |

## UC-LOC-01

**Thiết lập ngôn ngữ và quy tắc dự phòng**

**Triển khai:** P0. **Actor:** Quản trị ngôn ngữ.
**Mục tiêu:** Mọi nội dung có locale rõ ràng và quy tắc đọc khi thiếu bản dịch.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thiết lập ngôn ngữ và quy tắc dự phòng”.
**Tiền điều kiện:** Workspace và site được khai báo.
**Thông tin đầu vào:** Locale mặc định; locale cho phép; chuỗi dự phòng; quy tắc thiếu bản dịch.

**Luồng chính**

1. Chọn locale mặc định cho site.
2. Bật các locale sẽ sử dụng.
3. Chọn có cho dùng nội dung dự phòng hay trả về không khả dụng.
4. Kiểm tra vòng lặp và xác nhận.

**Luồng thay thế và ngoại lệ**

- E01: Chuỗi dự phòng có vòng lặp: chặn.
- E02: Tắt locale đang có nội dung công khai: yêu cầu xử lý bản công khai trước.

**BR-LOC-01:** P0 có thể chỉ bật một locale. Không sử dụng bản nháp làm nội dung dự phòng. Các trường không được fallback phải xác định rõ.
**Kết quả thành công:** Mọi nội dung có locale rõ ràng và quy tắc đọc khi thiếu bản dịch.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-LOC-01-01/v1:** Given fallback A sang B và B sang A; When lưu; Then bị chặn.
- **AC-LOC-01-02/v1:** Given locale đích chưa xuất bản và fallback chỉ có nháp; When đọc; Then không nhận bản nháp.
- **AC-LOC-01-03/v1:** Given site chỉ có một locale; When tạo bài; Then locale mặc định được xác định.

**Use case liên quan / phụ thuộc:** [UC-SITE-01](04-site-delivery.md#uc-site-01).

## UC-LOC-02

**Tạo bản dịch từ một phiên bản nguồn**

**Triển khai:** P3. **Actor:** Biên dịch viên.
**Mục tiêu:** Có bản nháp dịch truy ngược được đúng bản nguồn.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tạo bản dịch từ một phiên bản nguồn”.
**Tiền điều kiện:** Có quyền đọc bản nguồn và tạo nội dung ở locale đích.
**Thông tin đầu vào:** Nội dung nguồn; phiên bản nguồn; locale đích; trường cần dịch.

**Luồng chính**

1. Chọn chính xác phiên bản nguồn.
2. Tạo bản nháp liên kết với locale đích.
3. Dịch các trường được phép và giữ quan hệ nội dung.
4. Lưu cùng mã phiên bản nguồn làm căn cứ.

**Luồng thay thế và ngoại lệ**

- E01: Locale đích đã có bản dịch: mở bản hiện có hoặc tạo phiên bản mới, không tạo trùng.
- E02: Thiếu quyền xem nguồn: không cho sao chép dữ liệu.

**BR-LOC-02:** Trạng thái duyệt và công khai độc lập theo locale. Bản dịch không được hưởng phê duyệt của bản nguồn.
**Kết quả thành công:** Có bản nháp dịch truy ngược được đúng bản nguồn.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-LOC-02-01/v1:** Given nguồn là v3; When tạo bản dịch; Then ghi nhận v3 làm căn cứ.
- **AC-LOC-02-02/v1:** Given nguồn đã duyệt; When tạo bản dịch; Then bản dịch vẫn chưa được duyệt.
- **AC-LOC-02-03/v1:** Given người dịch không có quyền locale đích; When lưu; Then bị từ chối.

**Use case liên quan / phụ thuộc:** [UC-LOC-01](05-localization.md#uc-loc-01), [UC-CNT-01](09-content.md#uc-cnt-01), [UC-VER-01](10-versioning.md#uc-ver-01).

## UC-LOC-03

**Gửi bản dịch để duyệt và phát hành**

**Triển khai:** P3. **Actor:** Biên dịch viên, người duyệt locale, người xuất bản.
**Mục tiêu:** Locale đích được phát hành đúng bản duyệt, không tự phát hành locale khác.
**Kích hoạt:** Người dùng yêu cầu thực hiện “gửi bản dịch để duyệt và phát hành”.
**Tiền điều kiện:** Bản dịch có dữ liệu bắt buộc và phiên bản nguồn được xác định.
**Thông tin đầu vào:** Bản dịch; locale; phiên bản; người duyệt; đích phát hành.

**Luồng chính**

1. Kiểm tra bản dịch và độ mới của nguồn.
2. Gửi đúng phiên bản qua Workflow.
3. Người duyệt locale quyết định theo quyền.
4. Người xuất bản phát hành locale đã duyệt qua Publishing.

**Luồng thay thế và ngoại lệ**

- E01: Nguồn đã thay đổi: áp dụng quy tắc chặn hoặc yêu cầu xác nhận độ mới đã được phê duyệt.
- E02: Một locale bị từ chối: không tự đổi trạng thái locale khác.

**BR-LOC-03:** Đây là luồng phối hợp; quyền và trạng thái thuộc Workflow và Publishing. Baseline chặn phát hành bản dịch lỗi thời cho đến khi được cập nhật hoặc xác nhận không bị ảnh hưởng.
**Kết quả thành công:** Locale đích được phát hành đúng bản duyệt, không tự phát hành locale khác.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-LOC-03-01/v1:** Given chỉ locale en được chọn; When xuất bản; Then locale ja không đổi.
- **AC-LOC-03-02/v1:** Given bản dịch chưa được duyệt; When xuất bản; Then bị chặn.
- **AC-LOC-03-03/v1:** Given nguồn thay đổi và chưa xác nhận tác động; When xuất bản bản dịch; Then bị chặn.

**Use case liên quan / phụ thuộc:** [UC-LOC-02](05-localization.md#uc-loc-02), [UC-WF-02](12-workflow.md#uc-wf-02), [UC-WF-03](12-workflow.md#uc-wf-03), [UC-PUB-01](13-publishing.md#uc-pub-01).

## UC-LOC-04

**Rà soát bản dịch khi nguồn thay đổi**

**Triển khai:** P3. **Actor:** Biên dịch viên hoặc người phụ trách ngôn ngữ.
**Mục tiêu:** Bản dịch được cập nhật hoặc có quyết định hợp lệ về độ mới.
**Kích hoạt:** Người dùng yêu cầu thực hiện “rà soát bản dịch khi nguồn thay đổi”.
**Tiền điều kiện:** Có bản dịch tham chiếu phiên bản nguồn cũ.
**Thông tin đầu vào:** Bản dịch; nguồn mới; phạm vi thay đổi; quyết định có cần dịch lại.

**Luồng chính**

1. Mở danh sách bản dịch lỗi thời.
2. So sánh nguồn cũ với nguồn mới.
3. Cập nhật bản nháp dịch hoặc ghi căn cứ không bị ảnh hưởng.
4. Gửi thay đổi hoặc xác nhận qua bước rà soát thích hợp.

**Luồng thay thế và ngoại lệ**

- E01: Nguồn lại thay đổi trong lúc xử lý: tiếp tục đánh dấu lỗi thời với bản nguồn mới nhất đã chọn làm chuẩn.
- E02: Người xử lý thiếu quyền locale: không cho xác nhận thay.

**BR-LOC-04:** Không ghi đè bản dịch công khai chỉ vì nguồn thay đổi. Căn cứ xác nhận không bị ảnh hưởng phải được lưu.
**Kết quả thành công:** Bản dịch được cập nhật hoặc có quyết định hợp lệ về độ mới.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-LOC-04-01/v1:** Given nguồn v4 thay v3; When mở bản dịch từ v3; Then thấy cảnh báo lỗi thời.
- **AC-LOC-04-02/v1:** Given lưu bản dịch mới; When đọc bản công khai; Then bản công khai chưa đổi.
- **AC-LOC-04-03/v1:** Given xác nhận không bị ảnh hưởng; When tra cứu lịch sử; Then có người xác nhận, lý do và phiên bản nguồn.

**Use case liên quan / phụ thuộc:** [UC-LOC-02](05-localization.md#uc-loc-02), [UC-VER-02](10-versioning.md#uc-ver-02), [UC-WF-03](12-workflow.md#uc-wf-03).
