# Quản trị và tích hợp

**Domain:** `ADM` · **Bắt đầu:** P0 · **Số use case:** 6
**Trạng thái:** Baseline đề xuất v0.1; phạm vi tổ chức tại OI-02 và giải pháp danh tính nền tảng tại OI-03 đã được chốt.

Quản lý cấu hình, thông báo và kết nối. Không tự vượt quyền nghiệp vụ của các domain khác.
**Quyết định OI-02:** Một workspace phục vụ một doanh nghiệp, nhiều đơn vị và nhiều site; không phải SaaS đa tenant. Dữ liệu được cô lập theo site mặc định và chỉ nội dung/tài nguyên được chia sẻ chéo site có kiểm soát.
**Quyết định OI-03:** Cấu hình SSO/MFA, nguồn đồng bộ nhóm, chính sách phiên và quy trình tài khoản khẩn cấp là nền tảng P0; IAM quản lý ánh xạ vai trò/phạm vi và thu hồi quyền. Không hoãn các kết nối này đến UC-ADM-03 ở P4.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-ADM-01](#uc-adm-01) | Thiết lập không gian làm việc | P0 |
| [UC-ADM-02](#uc-adm-02) | Nhận và xử lý thông báo công việc | P2 |
| [UC-ADM-03](#uc-adm-03) | Kết nối hệ thống bên ngoài | P4 |
| [UC-ADM-04](#uc-adm-04) | Đăng ký và giám sát webhook | P4 |
| [UC-ADM-05](#uc-adm-05) | Nhập nội dung từ nguồn khác | P4 |
| [UC-ADM-06](#uc-adm-06) | Xuất dữ liệu theo phạm vi được phép | P4 |

## UC-ADM-01

**Thiết lập không gian làm việc**

**Triển khai:** P0. **Actor:** Quản trị hệ thống.
**Mục tiêu:** Workspace có cấu hình nền tảng và người chịu trách nhiệm.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thiết lập không gian làm việc”.
**Tiền điều kiện:** Quản trị viên khởi tạo đã được cấp bằng quy trình triển khai an toàn.
**Thông tin đầu vào:** Tên workspace; tổ chức; múi giờ hiển thị; locale mặc định; đầu mối vận hành.

**Luồng chính**

1. Nhập thông tin không gian làm việc.
2. Chọn múi giờ và quy ước thời gian.
3. Kiểm tra cấu hình bắt buộc cùng SSO/MFA, nguồn đồng bộ nhóm, chính sách phiên và quy trình khôi phục quản trị khẩn cấp theo OI-03.
4. Xác nhận để bắt đầu cấu hình site.

**Luồng thay thế và ngoại lệ**

- E01: Thiếu cấu hình bắt buộc: chỉ lưu nháp, chưa cho vận hành.
- E02: Đổi múi giờ sau khi có lịch: hiển thị ảnh hưởng trước khi xác nhận.

**BR-ADM-01:** Workspace phục vụ một doanh nghiệp với nhiều đơn vị và site, không phải SaaS đa tenant. Tạo workspace, đơn vị hoặc site không tự mở quyền truy cập chéo; chia sẻ nội dung/tài nguyên tuân theo BR-COM-18. Cấu hình danh tính theo OI-03 và BR-COM-19 phải được xác nhận, kiểm chứng trước gate P0; không tự gán nhà cung cấp, thời hạn phiên hoặc cơ chế đồng bộ. Đổi múi giờ hiển thị không được đổi thời điểm tuyệt đối của lịch đã lập.
**Kết quả thành công:** Workspace có cấu hình nền tảng và người chịu trách nhiệm.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-ADM-01-01/v1:** Given thiếu locale mặc định; When kích hoạt workspace; Then bị chặn.
- **AC-ADM-01-02/v1:** Given lịch đã định thời điểm; When đổi múi giờ hiển thị; Then thời điểm chạy không đổi.
- **AC-ADM-01-03/v1:** Given cấu hình hợp lệ; When xác nhận; Then site mới dùng được cấu hình nền tảng.

## UC-ADM-02

**Nhận và xử lý thông báo công việc**

**Triển khai:** P2. **Actor:** Biên tập viên, người duyệt, người xuất bản.
**Mục tiêu:** Người nhận biết công việc cần xử lý mà không được cấp thêm quyền.
**Kích hoạt:** Người dùng yêu cầu thực hiện “nhận và xử lý thông báo công việc”.
**Tiền điều kiện:** Có sự kiện nghiệp vụ mà người nhận được quyền biết.
**Thông tin đầu vào:** Loại sự kiện; người nhận; kênh nhận; tùy chọn nhắc việc.

**Luồng chính**

1. Mở hộp thông báo trong CMS.
2. Xem công việc và trạng thái hiện tại.
3. Mở đối tượng để xử lý theo quyền hiện hành.
4. Đánh dấu đã đọc hoặc chỉnh tùy chọn kênh nhận.

**Luồng thay thế và ngoại lệ**

- E01: Mất quyền với đối tượng: không hiển thị nội dung chi tiết.
- E02: Gửi email lỗi: công việc vẫn tồn tại trong CMS và có trạng thái gửi lại.

**BR-ADM-02:** Thông báo không phải nguồn quyết định trạng thái duyệt. Không gửi nội dung nhạy cảm qua kênh chưa được phép.
**Kết quả thành công:** Người nhận biết công việc cần xử lý mà không được cấp thêm quyền.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-ADM-02-01/v1:** Given email gửi lỗi; When mở hộp việc CMS; Then vẫn thấy việc được giao.
- **AC-ADM-02-02/v1:** Given quyền đã thu hồi; When mở thông báo cũ; Then không đọc được đối tượng.
- **AC-ADM-02-03/v1:** Given cùng sự kiện được gửi lại; When xem hộp việc; Then không sinh công việc nghiệp vụ trùng.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04), [UC-WF-02](12-workflow.md#uc-wf-02).

## UC-ADM-03

**Kết nối hệ thống bên ngoài**

**Triển khai:** P4. **Actor:** Quản trị tích hợp.
**Mục tiêu:** Có kết nối hoạt động trong đúng phạm vi đã cấp.
**Kích hoạt:** Người dùng yêu cầu thực hiện “kết nối hệ thống bên ngoài”.
**Tiền điều kiện:** Có phê duyệt sử dụng hệ thống đích và phạm vi dữ liệu trao đổi.
**Thông tin đầu vào:** Loại kết nối; môi trường; địa chỉ đích; thông tin xác thực; phạm vi dữ liệu.

**Luồng chính**

1. Chọn kết nối và môi trường.
2. Khai báo thông tin xác thực bằng cơ chế được bảo vệ.
3. Kiểm tra kết nối với dữ liệu thử được phép.
4. Xác nhận kích hoạt và theo dõi trạng thái.

**Luồng thay thế và ngoại lệ**

- E01: Xác thực không hợp lệ: không kích hoạt.
- E02: Địa chỉ hoặc dữ liệu ngoài danh sách cho phép: chặn gửi thử.

**BR-ADM-03:** Tài khoản tích hợp có quyền riêng, tối thiểu và thu hồi được. Không hiển thị lại bí mật hoặc ghi bí mật vào nhật ký.
**Kết quả thành công:** Có kết nối hoạt động trong đúng phạm vi đã cấp.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-ADM-03-01/v1:** Given thông tin xác thực sai; When thử kết nối; Then báo lỗi và chưa kích hoạt.
- **AC-ADM-03-02/v1:** Given kết nối chỉ được đọc nội dung công khai; When lấy bản nháp; Then bị từ chối.
- **AC-ADM-03-03/v1:** Given mở cấu hình hoặc audit; When xem chi tiết; Then không thấy bí mật nguyên văn.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04), [UC-ADM-01](02-administration.md#uc-adm-01).

## UC-ADM-04

**Đăng ký và giám sát webhook**

**Triển khai:** P4. **Actor:** Quản trị tích hợp.
**Mục tiêu:** Theo dõi được sự kiện đã giao, đang chờ và không thể giao.
**Kích hoạt:** Người dùng yêu cầu thực hiện “đăng ký và giám sát webhook”.
**Tiền điều kiện:** Đích nhận được phê duyệt. Đã xác định phạm vi sự kiện được gửi.
**Thông tin đầu vào:** Địa chỉ nhận; loại sự kiện; phạm vi; chính sách thử lại.

**Luồng chính**

1. Chọn các sự kiện cần nhận.
2. Kiểm tra đích và kích hoạt đăng ký.
3. Xem từng lần giao sự kiện và phản hồi.
4. Yêu cầu gửi lại sự kiện thất bại còn hợp lệ.

**Luồng thay thế và ngoại lệ**

- E01: Đích lỗi hoặc quá hạn: giữ lịch sử thất bại, thử lại có giới hạn.
- E02: Sự kiện có dữ liệu không còn được phép gửi: chặn phát lại hoặc giảm dữ liệu theo chính sách.

**BR-ADM-04:** Một sự kiện có mã ổn định qua các lần gửi. Không cam kết giao đúng một lần; bên nhận cần xử lý trùng. Kết quả nghiệp vụ không bị đảo chỉ vì webhook lỗi.
**Kết quả thành công:** Theo dõi được sự kiện đã giao, đang chờ và không thể giao.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-ADM-04-01/v1:** Given cùng sự kiện được gửi lại; When bên nhận kiểm tra; Then mã sự kiện giữ nguyên.
- **AC-ADM-04-02/v1:** Given webhook thất bại sau xuất bản; When xem bài; Then trạng thái xuất bản không bị đổi thành chưa xuất bản.
- **AC-ADM-04-03/v1:** Given đích chưa được duyệt; When kích hoạt; Then bị chặn.

**Use case liên quan / phụ thuộc:** [UC-ADM-03](02-administration.md#uc-adm-03), [UC-GOV-01](03-governance.md#uc-gov-01).

## UC-ADM-05

**Nhập nội dung từ nguồn khác**

**Triển khai:** P4. **Actor:** Quản trị nội dung hoặc người di chuyển dữ liệu.
**Mục tiêu:** Nội dung hợp lệ được tạo hoặc cập nhật có thể truy vết; lỗi có báo cáo.
**Kích hoạt:** Người dùng yêu cầu thực hiện “nhập nội dung từ nguồn khác”.
**Tiền điều kiện:** Nguồn được phép sử dụng. Loại nội dung, phạm vi và ánh xạ đã được chuẩn bị.
**Thông tin đầu vào:** Gói dữ liệu; ánh xạ trường; khóa đối chiếu; cách xử lý trùng.

**Luồng chính**

1. Chọn dữ liệu và ánh xạ.
2. Chạy kiểm tra trước, xem lỗi theo bản ghi.
3. Xác nhận tập bản ghi hợp lệ và chính sách toàn bộ hoặc từng phần.
4. Nhập vào bản nháp và tải báo cáo kết quả.

**Luồng thay thế và ngoại lệ**

- E01: Quan hệ tham chiếu không hợp lệ: ghi lỗi rõ đối tượng.
- E02: Chạy lại cùng đợt nhập: không tạo trùng khi dùng cùng khóa đối chiếu.

**BR-ADM-05:** Nhập dữ liệu không tự cấp phê duyệt hoặc xuất bản. Dữ liệu chưa được kiểm tra an toàn không được đưa ra kênh công khai.
**Kết quả thành công:** Nội dung hợp lệ được tạo hoặc cập nhật có thể truy vết; lỗi có báo cáo.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-ADM-05-01/v1:** Given một bản ghi sai kiểu trường; When kiểm tra trước; Then nêu rõ bản ghi và trường lỗi.
- **AC-ADM-05-02/v1:** Given nhập thành công; When xem nội dung; Then nội dung ở bản nháp, chưa công khai.
- **AC-ADM-05-03/v1:** Given chạy lại cùng gói và khóa đối chiếu; When hoàn tất; Then không phát sinh bản sao ngoài chính sách đã chọn.

**Use case liên quan / phụ thuộc:** [UC-MDL-01](06-content-model.md#uc-mdl-01), [UC-CNT-01](09-content.md#uc-cnt-01), [UC-AST-01](08-assets.md#uc-ast-01).

## UC-ADM-06

**Xuất dữ liệu theo phạm vi được phép**

**Triển khai:** P4. **Actor:** Quản trị nội dung hoặc người được cấp quyền xuất.
**Mục tiêu:** Người dùng nhận đúng dữ liệu được phép, có dấu vết xuất.
**Kích hoạt:** Người dùng yêu cầu thực hiện “xuất dữ liệu theo phạm vi được phép”.
**Tiền điều kiện:** Có quyền xuất đối với toàn bộ tập dữ liệu được chọn.
**Thông tin đầu vào:** Bộ lọc; trường xuất; định dạng; mục đích; thời hạn tải.

**Luồng chính**

1. Chọn phạm vi và trường dữ liệu.
2. Xem số lượng cùng cảnh báo dữ liệu nhạy cảm.
3. Xác nhận tạo gói xuất.
4. Tải kết quả sau khi kiểm tra lại quyền.

**Luồng thay thế và ngoại lệ**

- E01: Quyền bị thu hồi trong khi chuẩn bị: không cho tải phần dữ liệu mất quyền.
- E02: Tệp quá lớn: xử lý theo đợt và hiển thị tiến độ, không cắt mất dữ liệu im lặng.

**BR-ADM-06:** Quyền xuất riêng với quyền xem. Gói xuất không chứa bí mật tích hợp; đường tải có giới hạn truy cập và hiệu lực.
**Kết quả thành công:** Người dùng nhận đúng dữ liệu được phép, có dấu vết xuất.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-ADM-06-01/v1:** Given chỉ có quyền xem; When xuất; Then bị chặn nếu chưa có quyền xuất.
- **AC-ADM-06-02/v1:** Given mất quyền trước khi tải; When mở kết quả; Then tải bị từ chối.
- **AC-ADM-06-03/v1:** Given xuất thành công; When xem audit; Then có người xuất, phạm vi và thời điểm.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04), [UC-GOV-01](03-governance.md#uc-gov-01).
