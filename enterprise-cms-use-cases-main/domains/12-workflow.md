# Phê duyệt nội dung

**Domain:** `WF` · **Bắt đầu:** P2 · **Số use case:** 7
**Trạng thái:** Baseline đề xuất v0.1 — chưa phải yêu cầu đã được khách hàng phê duyệt.

Quản lý quyết định phê duyệt theo đúng phiên bản và phạm vi. Phê duyệt không đồng nghĩa xuất bản.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-WF-01](#uc-wf-01) | Thiết lập quy trình phê duyệt nội dung | P2 |
| [UC-WF-02](#uc-wf-02) | Gửi một phiên bản để phê duyệt | P2 |
| [UC-WF-03](#uc-wf-03) | Phê duyệt nội dung được giao | P2 |
| [UC-WF-04](#uc-wf-04) | Yêu cầu sửa lại hoặc từ chối nội dung | P2 |
| [UC-WF-05](#uc-wf-05) | Rút yêu cầu đang chờ duyệt | P2 |
| [UC-WF-06](#uc-wf-06) | Phân công lại người phê duyệt | P3 |
| [UC-WF-07](#uc-wf-07) | Thu hồi phê duyệt không còn hợp lệ | P2 |

## UC-WF-01

**Thiết lập quy trình phê duyệt nội dung**

**Triển khai:** P2. **Actor:** Quản trị quy trình.
**Mục tiêu:** Có quy trình hiệu lực, xác định được ai duyệt nội dung nào.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thiết lập quy trình phê duyệt nội dung”.
**Tiền điều kiện:** Vai trò và phạm vi nội dung đã có. Có người đủ điều kiện duyệt.
**Thông tin đầu vào:** Site; loại nội dung; locale; bước duyệt; người hoặc nhóm duyệt; điều kiện áp dụng.

**Luồng chính**

1. Chọn phạm vi cần áp dụng quy trình.
2. Khai báo bước duyệt và người có thể xử lý.
3. Thử bằng tình huống tạo, trả sửa, từ chối và chấp thuận.
4. Kích hoạt phiên bản quy trình.

**Luồng thay thế và ngoại lệ**

- E01: Không có người đủ quyền hoặc chỉ có chính tác giả: không cho kích hoạt tuyến duyệt đó.
- E02: Đổi quy trình khi còn yêu cầu mở: giữ phiên bản đã gắn hoặc chuyển có kiểm soát, không đổi ngầm.

**BR-WF-01:** P2 dùng một cấp duyệt độc lập. Nhiều cấp và điều kiện phức tạp là mở rộng P3. Không cho tự duyệt phiên bản mình tạo hoặc sửa.
**Kết quả thành công:** Có quy trình hiệu lực, xác định được ai duyệt nội dung nào.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-WF-01-01/v1:** Given quy trình không có người đủ điều kiện; When kích hoạt; Then bị chặn.
- **AC-WF-01-02/v1:** Given yêu cầu mở theo quy trình v1; When kích hoạt v2; Then yêu cầu cũ không bị đổi ngầm.
- **AC-WF-01-03/v1:** Given tác giả có cả vai trò duyệt; When duyệt bản chính mình sửa; Then bị chặn.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04), [UC-MDL-01](06-content-model.md#uc-mdl-01), [UC-VER-01](10-versioning.md#uc-ver-01).

## UC-WF-02

**Gửi một phiên bản để phê duyệt**

**Triển khai:** P2. **Actor:** Biên tập viên.
**Mục tiêu:** Có yêu cầu đang chờ duyệt cho đúng phiên bản, chưa xuất bản.
**Kích hoạt:** Người dùng yêu cầu thực hiện “gửi một phiên bản để phê duyệt”.
**Tiền điều kiện:** Có bản nháp hợp lệ và quy trình áp dụng. Không có yêu cầu đang mở cho cùng nội dung và locale.
**Thông tin đầu vào:** Nội dung; phiên bản; phạm vi dự kiến phát hành; ghi chú gửi duyệt.

**Luồng chính**

1. Kiểm tra dữ liệu, tài nguyên và bản xem trước.
2. Chọn đúng phiên bản cùng phạm vi.
3. Gửi yêu cầu.
4. Xem mã yêu cầu, phiên bản đã khóa và người nhận việc.

**Luồng thay thế và ngoại lệ**

- E01: Thiếu trường bắt buộc hoặc tài nguyên chưa an toàn: chặn.
- E02: Không có người duyệt hợp lệ: giữ bản nháp và báo cần phân công.

**BR-WF-02:** Yêu cầu gắn với phiên bản nội dung, mô hình, tài nguyên, phạm vi và quy trình cụ thể. Baseline phải rút yêu cầu trước khi sửa bản đang duyệt.
**Kết quả thành công:** Có yêu cầu đang chờ duyệt cho đúng phiên bản, chưa xuất bản.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-WF-02-01/v1:** Given gửi v4; When mở yêu cầu; Then người duyệt xem v4, không tự chuyển sang bản mới hơn.
- **AC-WF-02-02/v1:** Given còn tệp chờ kiểm tra; When gửi; Then bị chặn.
- **AC-WF-02-03/v1:** Given cùng thao tác gửi được thử lại; When xử lý; Then không tạo hai yêu cầu mở.

**Use case liên quan / phụ thuộc:** [UC-WF-01](12-workflow.md#uc-wf-01), [UC-CNT-04](09-content.md#uc-cnt-04), [UC-AST-01](08-assets.md#uc-ast-01).

## UC-WF-03

**Phê duyệt nội dung được giao**

**Triển khai:** P2. **Actor:** Người phê duyệt.
**Mục tiêu:** Quyết định hợp lệ được lưu với người và thời điểm.
**Kích hoạt:** Người dùng yêu cầu thực hiện “phê duyệt nội dung được giao”.
**Tiền điều kiện:** Yêu cầu còn mở. Người thao tác đủ quyền và không có xung đột trách nhiệm.
**Thông tin đầu vào:** Yêu cầu; phiên bản; quyết định; ghi chú nếu chính sách yêu cầu.

**Luồng chính**

1. Mở công việc được giao.
2. Xem phiên bản khóa, khác biệt và bản xem trước.
3. Chấp thuận bước mình phụ trách.
4. Xem trạng thái bước tiếp theo hoặc kết quả được duyệt.

**Luồng thay thế và ngoại lệ**

- E01: Người khác đã xử lý: không ghi quyết định thứ hai.
- E02: Quyền bị thu hồi hoặc trở thành xung đột: từ chối quyết định.

**BR-WF-03:** Duyệt chỉ áp dụng đúng phiên bản và phạm vi. Chỉ hoàn tất mọi bước mới đạt trạng thái được duyệt; không tự xuất bản.
**Kết quả thành công:** Quyết định hợp lệ được lưu với người và thời điểm.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-WF-03-01/v1:** Given đã chấp thuận cuối; When mở bài công khai; Then chưa đổi chỉ vì hành động duyệt.
- **AC-WF-03-02/v1:** Given người thao tác đã sửa phiên bản; When chấp thuận; Then bị chặn.
- **AC-WF-03-03/v1:** Given yêu cầu đã bị rút; When xác nhận trên màn hình cũ; Then không được duyệt.
- **AC-WF-03-04/v1:** Given quy trình có hai cấp; When chỉ cấp một chấp thuận; Then phiên bản chưa đủ điều kiện xuất bản.

**Use case liên quan / phụ thuộc:** [UC-WF-02](12-workflow.md#uc-wf-02), [UC-VER-02](10-versioning.md#uc-ver-02).

## UC-WF-04

**Yêu cầu sửa lại hoặc từ chối nội dung**

**Triển khai:** P2. **Actor:** Người phê duyệt.
**Mục tiêu:** Người gửi biết nguyên nhân và bước xử lý tiếp theo.
**Kích hoạt:** Người dùng yêu cầu thực hiện “yêu cầu sửa lại hoặc từ chối nội dung”.
**Tiền điều kiện:** Yêu cầu còn mở và thuộc quyền xử lý.
**Thông tin đầu vào:** Yêu cầu; quyết định trả sửa hoặc từ chối; lý do; trường cần sửa.

**Luồng chính**

1. Mở bản đang được duyệt.
2. Ghi rõ vấn đề và quyết định.
3. Xác nhận trả sửa hoặc từ chối.
4. Theo dõi thông báo tới người gửi.

**Luồng thay thế và ngoại lệ**

- E01: Không nhập lý do: chặn.
- E02: Yêu cầu đã được quyết định đồng thời: báo trạng thái mới.

**BR-WF-04:** Trả sửa cho phép tạo bản nháp để sửa và gửi lại. Từ chối kết thúc yêu cầu; gửi lại phải có yêu cầu mới. Không sửa bản công khai hiện có.
**Kết quả thành công:** Người gửi biết nguyên nhân và bước xử lý tiếp theo.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-WF-04-01/v1:** Given không có lý do; When trả sửa; Then bị chặn.
- **AC-WF-04-02/v1:** Given yêu cầu bị từ chối; When xuất bản dựa vào yêu cầu đó; Then bị chặn.
- **AC-WF-04-03/v1:** Given v1 đang công khai và v2 bị trả sửa; When khách mở bài; Then vẫn thấy v1.

**Use case liên quan / phụ thuộc:** [UC-WF-02](12-workflow.md#uc-wf-02).

## UC-WF-05

**Rút yêu cầu đang chờ duyệt**

**Triển khai:** P2. **Actor:** Người gửi hoặc quản trị được cấp quyền.
**Mục tiêu:** Yêu cầu ngừng được xử lý; nội dung có thể tiếp tục biên tập.
**Kích hoạt:** Người dùng yêu cầu thực hiện “rút yêu cầu đang chờ duyệt”.
**Tiền điều kiện:** Yêu cầu chưa kết thúc. Có quyền rút yêu cầu.
**Thông tin đầu vào:** Yêu cầu; lý do rút.

**Luồng chính**

1. Chọn yêu cầu đang mở.
2. Xem các quyết định đã có.
3. Xác nhận rút.
4. Quay lại tạo bản nháp mới từ phiên bản đã gửi.

**Luồng thay thế và ngoại lệ**

- E01: Yêu cầu đã hoàn tất trước khi xác nhận: không đổi ngược quyết định; dùng quy trình thay đổi phù hợp.
- E02: Người duyệt đang mở màn hình cũ: quyết định sau khi rút bị từ chối.

**BR-WF-05:** Rút không xóa lịch sử yêu cầu. Các chấp thuận từng bước cũ không áp dụng cho lần gửi mới.
**Kết quả thành công:** Yêu cầu ngừng được xử lý; nội dung có thể tiếp tục biên tập.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-WF-05-01/v1:** Given rút thành công; When người duyệt xác nhận từ màn hình cũ; Then bị từ chối.
- **AC-WF-05-02/v1:** Given gửi lại phiên bản mới; When xem bước duyệt; Then không kế thừa chấp thuận cũ.
- **AC-WF-05-03/v1:** Given mở audit; When tìm yêu cầu rút; Then còn người rút, thời điểm và lý do.

**Use case liên quan / phụ thuộc:** [UC-WF-02](12-workflow.md#uc-wf-02).

## UC-WF-06

**Phân công lại người phê duyệt**

**Triển khai:** P3. **Actor:** Quản trị quy trình hoặc người được quyền phân công.
**Mục tiêu:** Công việc được chuyển cho người có thể xử lý hợp lệ.
**Kích hoạt:** Người dùng yêu cầu thực hiện “phân công lại người phê duyệt”.
**Tiền điều kiện:** Yêu cầu còn mở và có người thay thế đủ điều kiện.
**Thông tin đầu vào:** Yêu cầu; người hoặc nhóm nhận mới; lý do; hạn xử lý nếu có.

**Luồng chính**

1. Mở yêu cầu và bước chưa hoàn tất.
2. Chọn người nhận mới.
3. Kiểm tra quyền, phạm vi và xung đột.
4. Xác nhận và thông báo cho các bên.

**Luồng thay thế và ngoại lệ**

- E01: Người nhận là tác giả của phiên bản: chặn.
- E02: Không còn người đủ điều kiện: giữ trạng thái chưa được phân công, không bỏ qua bước.

**BR-WF-06:** Phân công công việc không tự cấp quyền. Không xóa các quyết định lịch sử của bước đã hoàn tất.
**Kết quả thành công:** Công việc được chuyển cho người có thể xử lý hợp lệ.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-WF-06-01/v1:** Given người nhận chưa có quyền; When phân công; Then bị chặn hoặc yêu cầu cấp quyền trước.
- **AC-WF-06-02/v1:** Given thay người ở bước 2; When xem bước 1; Then quyết định bước 1 còn nguyên.
- **AC-WF-06-03/v1:** Given đổi người nhận thành công; When người cũ không còn quyền xử lý xác nhận; Then bị từ chối.

**Use case liên quan / phụ thuộc:** [UC-WF-02](12-workflow.md#uc-wf-02), [UC-IAM-04](01-identity-access.md#uc-iam-04).

## UC-WF-07

**Thu hồi phê duyệt không còn hợp lệ**

**Triển khai:** P2. **Actor:** Người phê duyệt hoặc người có quyền thu hồi.
**Mục tiêu:** Phê duyệt không còn được sử dụng; ảnh hưởng tới lịch và bản công khai được theo dõi.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thu hồi phê duyệt không còn hợp lệ”.
**Tiền điều kiện:** Có quyết định phê duyệt còn hiệu lực và quyền thu hồi trong phạm vi.
**Thông tin đầu vào:** Quyết định; phiên bản; lý do; các lịch và bản công khai bị ảnh hưởng.

**Luồng chính**

1. Chọn quyết định cần thu hồi.
2. Xem các lịch và bản công khai dựa trên quyết định.
3. Xác nhận thu hồi và tạm dừng phát hành chưa thực thi.
4. Tạo hoặc thực hiện yêu cầu gỡ riêng cho bản đã công khai khi cần.

**Luồng thay thế và ngoại lệ**

- E01: Bản đã công khai: không báo đã gỡ chỉ vì đã thu hồi duyệt.
- E02: Phát hành đang chuyển bản: xác định trạng thái thực tế và xử lý gỡ nếu đã công khai.

**BR-WF-07:** Thu hồi chặn phát hành mới dựa trên quyết định đó. Không xóa lịch sử phê duyệt; thao tác gỡ thuộc Publishing.
**Kết quả thành công:** Phê duyệt không còn được sử dụng; ảnh hưởng tới lịch và bản công khai được theo dõi.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-WF-07-01/v1:** Given thu hồi trước giờ lịch chạy; When đến hạn; Then không xuất bản dựa trên duyệt đã thu hồi.
- **AC-WF-07-02/v1:** Given nội dung đang công khai; When chỉ thu hồi duyệt; Then không báo gỡ hoàn tất.
- **AC-WF-07-03/v1:** Given xem lịch sử; When mở quyết định; Then còn cả việc chấp thuận và thu hồi.

**Use case liên quan / phụ thuộc:** [UC-WF-03](12-workflow.md#uc-wf-03), [UC-GOV-01](03-governance.md#uc-gov-01).
