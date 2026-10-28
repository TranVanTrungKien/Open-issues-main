# Quản trị nội dung và kiểm toán

**Domain:** `GOV` · **Bắt đầu:** P0 · **Số use case:** 5
**Trạng thái:** Baseline đề xuất v0.1 — chưa phải yêu cầu đã được khách hàng phê duyệt.

Quản lý dấu vết, chủ sở hữu, lưu giữ và xóa có kiểm soát. Không thay thế lịch sử phiên bản.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-GOV-01](#uc-gov-01) | Tra cứu và xuất dấu vết kiểm toán | P0 |
| [UC-GOV-02](#uc-gov-02) | Bàn giao chủ sở hữu nội dung | P1 |
| [UC-GOV-03](#uc-gov-03) | Thiết lập chính sách lưu giữ | P4 |
| [UC-GOV-04](#uc-gov-04) | Đặt hoặc gỡ yêu cầu giữ lại dữ liệu | P4 |
| [UC-GOV-05](#uc-gov-05) | Phê duyệt và thực hiện xóa vĩnh viễn | P4 |

## UC-GOV-01

**Tra cứu và xuất dấu vết kiểm toán**

**Triển khai:** P0. **Actor:** Kiểm toán viên hoặc quản trị được cấp quyền.
**Mục tiêu:** Người kiểm tra xác định được ai đã làm gì, khi nào, với phiên bản nào.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tra cứu và xuất dấu vết kiểm toán”.
**Tiền điều kiện:** Có quyền đọc audit trong phạm vi cần kiểm tra.
**Thông tin đầu vào:** Đối tượng; người thao tác; hành động; thời gian; kết quả.

**Luồng chính**

1. Chọn điều kiện tra cứu.
2. Xem chuỗi sự kiện theo thời gian.
3. Mở sự kiện để biết người thao tác, phạm vi và phiên bản liên quan.
4. Xuất kết quả khi có quyền xuất audit.

**Luồng thay thế và ngoại lệ**

- E01: Không có quyền với phạm vi: không hiển thị sự kiện chi tiết.
- E02: Thiếu hoặc gián đoạn ghi nhận: hiển thị cảnh báo, không trình bày như lịch sử đầy đủ.

**BR-GOV-01:** Ghi audit từ P0 cho thao tác nhạy cảm; nội dung và xuất bản bổ sung khi được triển khai. Không sửa hoặc xóa sự kiện qua giao diện thông thường. Không ghi mật khẩu, token hoặc bí mật.
**Kết quả thành công:** Người kiểm tra xác định được ai đã làm gì, khi nào, với phiên bản nào.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-GOV-01-01/v1:** Given một lần đổi quyền thành công; When tra cứu audit; Then có người thực hiện, đối tượng, thời điểm và kết quả.
- **AC-GOV-01-02/v1:** Given tài khoản chỉ xem audit site A; When lọc site B; Then không đọc được sự kiện site B.
- **AC-GOV-01-03/v1:** Given mở log đăng nhập hoặc tích hợp; When xem dữ liệu; Then không có mật khẩu hay token nguyên văn.

**Use case liên quan / phụ thuộc:** [UC-IAM-01](01-identity-access.md#uc-iam-01).

## UC-GOV-02

**Bàn giao chủ sở hữu nội dung**

**Triển khai:** P1. **Actor:** Quản trị nội dung hoặc chủ sở hữu được phép bàn giao.
**Mục tiêu:** Nội dung có chủ sở hữu mới, lịch sử trách nhiệm được giữ.
**Kích hoạt:** Người dùng yêu cầu thực hiện “bàn giao chủ sở hữu nội dung”.
**Tiền điều kiện:** Người nhận đang hoạt động và có phạm vi phù hợp.
**Thông tin đầu vào:** Nội dung hoặc danh sách nội dung; người nhận; lý do.

**Luồng chính**

1. Chọn nội dung cần bàn giao.
2. Xem người sở hữu, việc duyệt và lịch liên quan.
3. Chọn người nhận và xác nhận.
4. Xem kết quả từng nội dung và các việc cần tái giao riêng.

**Luồng thay thế và ngoại lệ**

- E01: Người nhận ngoài phạm vi: từ chối bản ghi đó.
- E02: Bàn giao hàng loạt có lỗi một phần: hiển thị rõ thành công và thất bại.

**BR-GOV-02:** Bàn giao không sửa tác giả lịch sử. Không tự chuyển quyền duyệt, quyền xuất bản hoặc phê duyệt cũ.
**Kết quả thành công:** Nội dung có chủ sở hữu mới, lịch sử trách nhiệm được giữ.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-GOV-02-01/v1:** Given bàn giao bài A; When hoàn tất; Then chủ sở hữu đổi nhưng tác giả các phiên bản cũ không đổi.
- **AC-GOV-02-02/v1:** Given người nhận không có quyền site; When bàn giao; Then bị chặn.
- **AC-GOV-02-03/v1:** Given có lịch bị dừng do người cũ bị khóa; When bàn giao; Then lịch không tự chạy lại.

**Use case liên quan / phụ thuộc:** [UC-CNT-01](09-content.md#uc-cnt-01), [UC-IAM-04](01-identity-access.md#uc-iam-04).

## UC-GOV-03

**Thiết lập chính sách lưu giữ**

**Triển khai:** P4. **Actor:** Người phụ trách quản trị dữ liệu.
**Mục tiêu:** Có quy tắc xác định được dữ liệu đủ điều kiện xem xét xóa.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thiết lập chính sách lưu giữ”.
**Tiền điều kiện:** Chính sách đã được người có thẩm quyền chấp thuận.
**Thông tin đầu vào:** Loại dữ liệu; sự kiện bắt đầu tính hạn; thời hạn; ngoại lệ; ngày hiệu lực.

**Luồng chính**

1. Chọn phạm vi dữ liệu và thời hạn.
2. Xem dữ liệu sẽ bị ảnh hưởng.
3. Kiểm tra dữ liệu có yêu cầu giữ lại.
4. Xác nhận chính sách và lưu căn cứ.

**Luồng thay thế và ngoại lệ**

- E01: Thiếu căn cứ hoặc thời hạn chưa chốt: không bật xóa tự động.
- E02: Thay đổi làm nhiều bản ghi hết hạn ngay: yêu cầu xem xét riêng trước khi xóa.

**BR-GOV-03:** Mặc định không bật xóa vĩnh viễn khi chính sách chưa được xác nhận. Chính sách của audit, bản nội dung và tài nguyên có thể khác nhau.
**Kết quả thành công:** Có quy tắc xác định được dữ liệu đủ điều kiện xem xét xóa.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-GOV-03-01/v1:** Given chính sách chưa được duyệt; When bật xóa tự động; Then bị chặn.
- **AC-GOV-03-02/v1:** Given dữ liệu đang được giữ lại; When đến hạn; Then chưa được xóa.
- **AC-GOV-03-03/v1:** Given cập nhật thời hạn; When xác nhận; Then lưu phiên bản chính sách và báo cáo ảnh hưởng.

**Use case liên quan / phụ thuộc:** [UC-GOV-01](03-governance.md#uc-gov-01).

## UC-GOV-04

**Đặt hoặc gỡ yêu cầu giữ lại dữ liệu**

**Triển khai:** P4. **Actor:** Người phụ trách tuân thủ được cấp quyền.
**Mục tiêu:** Dữ liệu được giữ hoặc hết một căn cứ giữ, có thể kiểm toán.
**Kích hoạt:** Người dùng yêu cầu thực hiện “đặt hoặc gỡ yêu cầu giữ lại dữ liệu”.
**Tiền điều kiện:** Có thẩm quyền đặt hoặc gỡ yêu cầu giữ lại.
**Thông tin đầu vào:** Phạm vi dữ liệu; căn cứ; người chịu trách nhiệm; điều kiện kết thúc.

**Luồng chính**

1. Chọn dữ liệu hoặc phạm vi liên quan.
2. Nhập căn cứ và thời điểm hiệu lực.
3. Xác nhận giữ lại hoặc gỡ yêu cầu có sẵn.
4. Xem trạng thái cùng lịch sử thay đổi.

**Luồng thay thế và ngoại lệ**

- E01: Còn yêu cầu giữ lại khác: gỡ một yêu cầu không làm dữ liệu được xóa.
- E02: Dữ liệu đã bị xóa vĩnh viễn: nêu giới hạn, không hứa khôi phục.

**BR-GOV-04:** Yêu cầu giữ lại chặn xóa vĩnh viễn, không tự làm nội dung công khai. Gỡ giữ lại không thực hiện xóa ngay.
**Kết quả thành công:** Dữ liệu được giữ hoặc hết một căn cứ giữ, có thể kiểm toán.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-GOV-04-01/v1:** Given dữ liệu đang bị giữ; When yêu cầu xóa vĩnh viễn; Then bị chặn.
- **AC-GOV-04-02/v1:** Given có hai yêu cầu giữ; When gỡ một; Then dữ liệu vẫn bị giữ.
- **AC-GOV-04-03/v1:** Given gỡ yêu cầu cuối; When hoàn tất; Then dữ liệu chưa tự bị xóa.

**Use case liên quan / phụ thuộc:** [UC-GOV-03](03-governance.md#uc-gov-03).

## UC-GOV-05

**Phê duyệt và thực hiện xóa vĩnh viễn**

**Triển khai:** P4. **Actor:** Người yêu cầu và người phê duyệt xóa độc lập.
**Mục tiêu:** Dữ liệu được xử lý theo quyết định, còn biên nhận kiểm toán tối thiểu được phép giữ.
**Kích hoạt:** Người dùng yêu cầu thực hiện “phê duyệt và thực hiện xóa vĩnh viễn”.
**Tiền điều kiện:** Dữ liệu đã lưu trữ, hết thời hạn, không bị giữ và không còn phụ thuộc cần bảo toàn.
**Thông tin đầu vào:** Danh sách đối tượng; căn cứ; phạm vi xóa; báo cáo phụ thuộc.

**Luồng chính**

1. Lập danh sách đủ điều kiện xóa.
2. Người độc lập xem ảnh hưởng và quyết định.
3. Kiểm tra lại điều kiện ngay trước thực hiện.
4. Thực hiện và nhận báo cáo từng đối tượng.

**Luồng thay thế và ngoại lệ**

- E01: Phát sinh yêu cầu giữ lại hoặc phụ thuộc mới: dừng đối tượng bị ảnh hưởng.
- E02: Một phần không xóa được: báo cáo đúng phần còn lại, không báo hoàn tất toàn bộ.

**BR-GOV-05:** Xóa vĩnh viễn khác lưu trữ. Phạm vi bản sao, chỉ mục và backup theo chính sách đã chốt; không cam kết xóa vật lý tức thời ở mọi nơi.
**Kết quả thành công:** Dữ liệu được xử lý theo quyết định, còn biên nhận kiểm toán tối thiểu được phép giữ.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-GOV-05-01/v1:** Given người yêu cầu tự phê duyệt; When xác nhận; Then bị chặn.
- **AC-GOV-05-02/v1:** Given có hold mới sau phê duyệt; When bắt đầu xóa; Then đối tượng đó không bị xóa.
- **AC-GOV-05-03/v1:** Given một đối tượng còn trên bản phát hành đang dùng; When yêu cầu xóa; Then bị chặn và chỉ rõ phụ thuộc.

**Use case liên quan / phụ thuộc:** [UC-GOV-03](03-governance.md#uc-gov-03), [UC-GOV-04](03-governance.md#uc-gov-04), [UC-CNT-05](09-content.md#uc-cnt-05), [UC-AST-04](08-assets.md#uc-ast-04).
