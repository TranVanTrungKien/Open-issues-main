# Xuất bản

**Domain:** `PUB` · **Bắt đầu:** P2 · **Số use case:** 7
**Trạng thái:** Baseline đề xuất v0.1; các quyết định về kênh tại OI-01 và phạm vi chia sẻ tại OI-02 đã được chốt.

Quản lý phát hành, lịch, thu hồi và trạng thái phân phối. Không thay đổi bản nháp hoặc tự phê duyệt.
**Quyết định OI-01:** Đích bắt buộc của phiên bản đầu là website công khai. Publishing làm việc qua định danh và hợp đồng đích phân phối để có thể bổ sung intranet hoặc Headless API sau này; các đích đó chưa thuộc phạm vi phát hành đầu.
**Quyết định OI-02:** Khi bản phát hành dùng nội dung/tài nguyên từ site khác, quyền chia sẻ cho site đích phải được kiểm tra lại trước lúc chuyển bản. Gỡ hoặc thu hồi ở một site không tự làm thay đổi site khác.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-PUB-01](#uc-pub-01) | Xuất bản ngay một phiên bản được duyệt | P2 |
| [UC-PUB-02](#uc-pub-02) | Lập lịch xuất bản hoặc gỡ nội dung | P2 |
| [UC-PUB-03](#uc-pub-03) | Hủy lịch chưa thực thi | P2 |
| [UC-PUB-04](#uc-pub-04) | Gỡ nội dung khỏi kênh phân phối | P2 |
| [UC-PUB-05](#uc-pub-05) | Khôi phục bản công khai trước đó có kiểm soát | P3 |
| [UC-PUB-06](#uc-pub-06) | Phát hành một đợt tới nhiều đích | P4 |
| [UC-PUB-07](#uc-pub-07) | Theo dõi và xử lý lần phát hành thất bại | P2 |

## UC-PUB-01

**Xuất bản ngay một phiên bản được duyệt**

**Triển khai:** P2. **Actor:** Người xuất bản.
**Mục tiêu:** Đích đã chọn phục vụ đúng bản được duyệt; có hồ sơ phát hành.
**Kích hoạt:** Người dùng yêu cầu thực hiện “xuất bản ngay một phiên bản được duyệt”.
**Tiền điều kiện:** Phiên bản đã duyệt cho đúng phạm vi. Đích phân phối sẵn sàng. Người thao tác có quyền hiện hành.
**Thông tin đầu vào:** Nội dung; phiên bản; site; locale; kênh; tài nguyên và quan hệ cần phát hành.

**Luồng chính**

1. Chọn phiên bản đã duyệt và đích.
2. Xem bản xem trước, phụ thuộc và ảnh hưởng đến bản đang công khai.
3. Xác nhận phát hành.
4. Theo dõi đến khi đích phục vụ đúng bản hoặc có lỗi rõ ràng.

**Luồng thay thế và ngoại lệ**

- E01: Thiếu duyệt, sai phạm vi, quyền chia sẻ chéo site không còn hiệu lực, tài nguyên hết quyền hoặc tham chiếu không công khai được: chặn.
- E02: Đích phân phối lỗi: giữ bản công khai trước đó khi chưa chuyển bản; hiển thị thất bại hoặc chưa hoàn tất.

**BR-PUB-01:** Phê duyệt, nội dung và phụ thuộc phải khớp trước lúc chuyển bản; mọi chia sẻ chéo site phải còn hiệu lực cho site đích. Lặp cùng yêu cầu không tạo phát hành trùng. Không báo thành công chỉ vì đã nhận yêu cầu. Luồng phát hành tham chiếu đích bằng định danh/hợp đồng phân phối, không gọi trực tiếp logic dựng website từ lõi nghiệp vụ.
**Kết quả thành công:** Đích đã chọn phục vụ đúng bản được duyệt; có hồ sơ phát hành.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-PUB-01-01/v1:** Given v2 được duyệt và v3 là nháp; When xuất bản v2; Then chỉ v2 được công khai.
- **AC-PUB-01-02/v1:** Given gửi lại cùng yêu cầu; When hoàn tất; Then chỉ có một phát hành nghiệp vụ cho yêu cầu đó.
- **AC-PUB-01-03/v1:** Given đích chưa phục vụ bản mới; When xem trạng thái; Then chưa báo hoàn tất.
- **AC-PUB-01-04/v1:** Given người xuất bản mất quyền sau khi mở màn hình xác nhận; When phát hành; Then hành động bị từ chối.
- **AC-PUB-01-05/v1:** Given bản đã duyệt tham chiếu tệp v1; When có tệp v2 được tải thay; Then phát hành không tự dùng v2.

**Use case liên quan / phụ thuộc:** [UC-WF-03](12-workflow.md#uc-wf-03), [UC-SITE-01](04-site-delivery.md#uc-site-01), [UC-AST-04](08-assets.md#uc-ast-04), [UC-CNT-04](09-content.md#uc-cnt-04).

## UC-PUB-02

**Lập lịch xuất bản hoặc gỡ nội dung**

**Triển khai:** P2. **Actor:** Người xuất bản.
**Mục tiêu:** Có lịch có thể kiểm tra; đến hạn chỉ thực thi khi điều kiện vẫn đạt.
**Kích hoạt:** Người dùng yêu cầu thực hiện “lập lịch xuất bản hoặc gỡ nội dung”.
**Tiền điều kiện:** Có quyền lập lịch. Bản cần xuất bản đã được duyệt; đích đã sẵn sàng.
**Thông tin đầu vào:** Phiên bản; đích; hành động; thời điểm; múi giờ; thời điểm hết hiệu lực nếu có.

**Luồng chính**

1. Chọn phiên bản và hành động.
2. Nhập thời điểm, múi giờ và kiểm tra thời điểm quy đổi.
3. Xem lịch xung đột và xác nhận.
4. Theo dõi kết quả khi đến hạn.

**Luồng thay thế và ngoại lệ**

- E01: Thời điểm không hợp lệ, đã qua hoặc xung đột lịch: chặn.
- E02: Đến hạn nhưng người lập mất quyền, tài nguyên không hợp lệ hoặc duyệt bị thu hồi: dừng và nêu lý do.

**BR-PUB-02:** Lịch gắn với phiên bản cố định, không tự lấy bản mới nhất. Baseline yêu cầu quyền người lập còn hiệu lực khi thực thi; lịch cấp tổ chức cần chính sách riêng.
**Kết quả thành công:** Có lịch có thể kiểm tra; đến hạn chỉ thực thi khi điều kiện vẫn đạt.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-PUB-02-01/v1:** Given đặt lịch v2 rồi tạo nháp v3; When đến hạn; Then không tự xuất bản v3.
- **AC-PUB-02-02/v1:** Given người lập mất quyền trước giờ chạy; When đến hạn; Then lịch bị dừng.
- **AC-PUB-02-03/v1:** Given chọn thời gian địa phương không tồn tại do đổi giờ mùa; When xác nhận; Then phải chọn thời điểm hợp lệ.

**Use case liên quan / phụ thuộc:** [UC-PUB-01](13-publishing.md#uc-pub-01), [UC-IAM-05](01-identity-access.md#uc-iam-05).

## UC-PUB-03

**Hủy lịch chưa thực thi**

**Triển khai:** P2. **Actor:** Người xuất bản có quyền với lịch.
**Mục tiêu:** Lịch được hủy không còn được thực thi về sau.
**Kích hoạt:** Người dùng yêu cầu thực hiện “hủy lịch chưa thực thi”.
**Tiền điều kiện:** Lịch chưa hoàn tất. Có quyền hủy trong phạm vi.
**Thông tin đầu vào:** Lịch; lý do.

**Luồng chính**

1. Mở lịch và trạng thái hiện hành.
2. Chọn hủy.
3. Xác nhận lý do.
4. Nhận kết quả hủy hoặc trạng thái đã bắt đầu thực thi.

**Luồng thay thế và ngoại lệ**

- E01: Lịch đã thực thi: không hiển thị như đã ngăn xuất bản; hướng sang gỡ.
- E02: Hủy đồng thời với thực thi: xác định rõ hành động nào thắng, không báo hủy thành công rồi vẫn phát hành.

**BR-PUB-03:** Hủy lịch không thu hồi nội dung đã công khai. Kết quả hủy phải được kiểm tra cả tại thời điểm chuyển bản.
**Kết quả thành công:** Lịch được hủy không còn được thực thi về sau.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-PUB-03-01/v1:** Given lịch đã hủy thành công; When đến hạn; Then không xuất bản.
- **AC-PUB-03-02/v1:** Given lịch đã hoàn tất; When hủy; Then không báo đã ngăn xuất bản.
- **AC-PUB-03-03/v1:** Given hai người cùng hủy; When xử lý; Then có một trạng thái cuối nhất quán và không tạo lỗi dữ liệu.

**Use case liên quan / phụ thuộc:** [UC-PUB-02](13-publishing.md#uc-pub-02).

## UC-PUB-04

**Gỡ nội dung khỏi kênh phân phối**

**Triển khai:** P2. **Actor:** Người xuất bản.
**Mục tiêu:** Nội dung ngừng được phục vụ ở phạm vi đã chọn và kết quả được xác minh.
**Kích hoạt:** Người dùng yêu cầu thực hiện “gỡ nội dung khỏi kênh phân phối”.
**Tiền điều kiện:** Có quyền gỡ đối với bản đang công khai hoặc phát hành đang xử lý.
**Thông tin đầu vào:** Nội dung; đích; locale; lý do; cách xử lý liên kết và lịch liên quan.

**Luồng chính**

1. Chọn đúng phạm vi cần gỡ.
2. Xem ảnh hưởng đến trang, menu, tham chiếu và lịch tương lai.
3. Xác nhận gỡ cùng phương án xử lý xung đột.
4. Theo dõi đến khi nội dung không còn được phân phối trong giới hạn đã chốt.

**Luồng thay thế và ngoại lệ**

- E01: Đích hoặc cache chưa cập nhật: giữ trạng thái đang gỡ hoặc lỗi.
- E02: Còn lịch có thể phát hành lại: yêu cầu hủy hoặc xác nhận kế hoạch, không gỡ âm thầm rồi tự tái xuất bản.

**BR-PUB-04:** Gỡ không xóa lịch sử. Phạm vi gỡ và nội dung/tài nguyên dùng chung phải được phân biệt; không xóa đối tượng nguồn còn được site hoặc nội dung khác sử dụng. Thu hồi chia sẻ chặn sử dụng mới nhưng không được làm hỏng bản đang công khai âm thầm.
**Kết quả thành công:** Nội dung ngừng được phục vụ ở phạm vi đã chọn và kết quả được xác minh.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-PUB-04-01/v1:** Given chỉ gỡ site A; When hoàn tất; Then site B không tự bị gỡ.
- **AC-PUB-04-02/v1:** Given cache vẫn phục vụ bản cũ ngoài giới hạn; When kiểm tra; Then chưa báo hoàn tất gỡ.
- **AC-PUB-04-03/v1:** Given tài nguyên dùng chung cho bài hoặc site khác; When gỡ bài này; Then nơi dùng khác không mất tài nguyên.

**Use case liên quan / phụ thuộc:** [UC-PUB-01](13-publishing.md#uc-pub-01), [UC-PUB-03](13-publishing.md#uc-pub-03), [UC-AST-04](08-assets.md#uc-ast-04).

## UC-PUB-05

**Khôi phục bản công khai trước đó có kiểm soát**

**Triển khai:** P3. **Actor:** Người xuất bản và người phê duyệt.
**Mục tiêu:** Kênh đọc nhận nội dung khôi phục đã được kiểm tra theo điều kiện hiện hành.
**Kích hoạt:** Người dùng yêu cầu thực hiện “khôi phục bản công khai trước đó có kiểm soát”.
**Tiền điều kiện:** Có hồ sơ phát hành cũ và quyền khôi phục. Phiên bản nguồn còn được lưu giữ.
**Thông tin đầu vào:** Bản phát hành nguồn; phạm vi; lý do; mức độ khẩn.

**Luồng chính**

1. Chọn bản công khai trước đó và xem khác biệt.
2. Tạo bản nháp khôi phục qua Versioning.
3. Kiểm tra tài nguyên, quy tắc hiện hành và duyệt lại.
4. Phát hành bản khôi phục qua quy trình xuất bản.

**Luồng thay thế và ngoại lệ**

- E01: Tài nguyên nguồn không còn được phép dùng: yêu cầu thay.
- E02: Có lịch tương lai xung đột: hủy hoặc điều chỉnh trước khi phát hành.

**BR-PUB-05:** Baseline không có nút rollback bỏ qua duyệt. Cơ chế khẩn cấp rút ngắn bước cần chính sách riêng và chưa nằm trong đặc tả này.
**Kết quả thành công:** Kênh đọc nhận nội dung khôi phục đã được kiểm tra theo điều kiện hiện hành.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-PUB-05-01/v1:** Given chọn bản từng được duyệt; When tạo bản khôi phục; Then vẫn cần duyệt bản mới.
- **AC-PUB-05-02/v1:** Given tài nguyên cũ hết quyền; When phát hành; Then bị chặn.
- **AC-PUB-05-03/v1:** Given khôi phục hoàn tất; When mở lịch sử; Then cả phát hành lỗi và phát hành khôi phục đều còn.

**Use case liên quan / phụ thuộc:** [UC-VER-03](10-versioning.md#uc-ver-03), [UC-WF-03](12-workflow.md#uc-wf-03), [UC-PUB-01](13-publishing.md#uc-pub-01).

## UC-PUB-06

**Phát hành một đợt tới nhiều đích**

**Triển khai:** P4. **Actor:** Người quản lý phát hành.
**Mục tiêu:** Kết quả rõ theo từng nội dung và đích; có thể khắc phục có kiểm soát.
**Kích hoạt:** Người dùng yêu cầu thực hiện “phát hành một đợt tới nhiều đích”.
**Tiền điều kiện:** Mọi đối tượng có phiên bản và phê duyệt phù hợp với từng đích.
**Thông tin đầu vào:** Danh sách nội dung; phiên bản; locale; đích; thứ tự phụ thuộc; chính sách khi lỗi.

**Luồng chính**

1. Tạo đợt phát hành với tập phiên bản cố định.
2. Kiểm tra từng đích và phụ thuộc.
3. Xác nhận thực hiện theo chính sách đã chọn.
4. Theo dõi ma trận thành công, thất bại và phần chưa thực hiện.

**Luồng thay thế và ngoại lệ**

- E01: Một đích thất bại: hiển thị hoàn tất một phần, không báo thành công toàn bộ.
- E02: Yêu cầu thành công đồng thời tuyệt đối trên mọi đích: chưa cho chạy nếu nền tảng không đáp ứng được.

**BR-PUB-06:** Baseline không cam kết giao dịch nguyên tử xuyên mọi hệ thống. Thử lại chỉ áp dụng phần thất bại, không tự tạo lại phát hành thành công.
**Kết quả thành công:** Kết quả rõ theo từng nội dung và đích; có thể khắc phục có kiểm soát.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-PUB-06-01/v1:** Given A thành công và B thất bại; When xem đợt; Then hiển thị hoàn tất một phần.
- **AC-PUB-06-02/v1:** Given thử lại B; When hoàn tất; Then không phát hành trùng A.
- **AC-PUB-06-03/v1:** Given một phiên bản thiếu duyệt ở site đích; When kiểm tra trước; Then phát hành phần đó bị chặn.

**Use case liên quan / phụ thuộc:** [UC-PUB-01](13-publishing.md#uc-pub-01), [UC-PUB-07](13-publishing.md#uc-pub-07).

## UC-PUB-07

**Theo dõi và xử lý lần phát hành thất bại**

**Triển khai:** P2. **Actor:** Người xuất bản hoặc vận hành nội dung.
**Mục tiêu:** Lần thất bại được xử lý mà không phát hành trùng hoặc hồi sinh bản đã bị gỡ.
**Kích hoạt:** Người dùng yêu cầu thực hiện “theo dõi và xử lý lần phát hành thất bại”.
**Tiền điều kiện:** Có lần phát hành, lịch hoặc gỡ đang xử lý hay thất bại.
**Thông tin đầu vào:** Mã lần thực hiện; đích; trạng thái; lựa chọn thử lại hoặc hủy.

**Luồng chính**

1. Mở danh sách lần thực hiện.
2. Xem bước đã xong, bản thực tế đang phục vụ và lỗi.
3. Chọn hành động khắc phục trong phạm vi quyền.
4. Kiểm tra lại điều kiện và theo dõi trạng thái cuối.

**Luồng thay thế và ngoại lệ**

- E01: Quyền hoặc phê duyệt đã thay đổi: không tự thử lại bằng điều kiện cũ.
- E02: Tác vụ mới đã gỡ hoặc thay bản: tác vụ cũ không được ghi đè kết quả mới.

**BR-PUB-07:** Thử lại không đổi phiên bản mục tiêu. Phải kiểm tra cả quyền hiện hành, tính mới của thao tác và điều kiện tài nguyên trước khi chuyển bản.
**Kết quả thành công:** Lần thất bại được xử lý mà không phát hành trùng hoặc hồi sinh bản đã bị gỡ.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-PUB-07-01/v1:** Given phát hành v2 lỗi rồi nội dung đã được gỡ; When tác vụ cũ thử lại; Then không làm v2 xuất hiện lại.
- **AC-PUB-07-02/v1:** Given phê duyệt bị thu hồi; When thử lại; Then bị chặn.
- **AC-PUB-07-03/v1:** Given hệ thống đích đã nhận nhưng phản hồi bị mất; When thử lại cùng mã; Then không tạo bản phát hành nghiệp vụ trùng.

**Use case liên quan / phụ thuộc:** [UC-PUB-01](13-publishing.md#uc-pub-01), [UC-GOV-01](03-governance.md#uc-gov-01).
