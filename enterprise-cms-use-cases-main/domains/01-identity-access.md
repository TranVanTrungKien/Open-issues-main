# Danh tính và phân quyền

**Domain:** `IAM` · **Bắt đầu:** P0 · **Số use case:** 6
**Trạng thái:** Baseline đề xuất v0.1; ranh giới quyền giữa các site tại OI-02 và giải pháp danh tính tại OI-03 đã được chốt.

Quản lý danh tính, vai trò, nhóm và quyền theo phạm vi. Không quyết định nội dung được duyệt hay được xuất bản.
**Quyết định OI-02:** Cô lập quyền theo site là mặc định. Quyền sử dụng nội dung/tài nguyên được chia sẻ không tự cấp quyền sửa, duyệt, xuất bản hoặc quản trị đối tượng nguồn.
**Quyết định OI-03:** SSO doanh nghiệp + MFA + RBAC theo phạm vi + đồng bộ nhóm + quản lý phiên và thu hồi quyền + tài khoản khẩn cấp được kiểm soát. Hoàn thiện cấu hình và kiểm chứng tại P0 theo [OI-03](../open-issues.md#oi-03) và BR-COM-19; chưa xác nhận đã triển khai.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-IAM-01](#uc-iam-01) | Đăng nhập bằng tài khoản doanh nghiệp | P0 |
| [UC-IAM-02](#uc-iam-02) | Đăng xuất và kết thúc phiên CMS | P0 |
| [UC-IAM-03](#uc-iam-03) | Tiếp nhận người dùng vào CMS | P0 |
| [UC-IAM-04](#uc-iam-04) | Thiết lập và cấp quyền theo phạm vi | P0 |
| [UC-IAM-05](#uc-iam-05) | Thu hồi quyền hoặc đình chỉ người dùng | P0 |
| [UC-IAM-06](#uc-iam-06) | Ủy quyền phê duyệt có thời hạn | P3 |

## UC-IAM-01

**Đăng nhập bằng tài khoản doanh nghiệp**

**Triển khai:** P0. **Actor:** Nhân viên CMS.
**Mục tiêu:** Người dùng mở được phiên CMS với đúng quyền hiện hành.
**Kích hoạt:** Người dùng yêu cầu thực hiện “đăng nhập bằng tài khoản doanh nghiệp”.
**Tiền điều kiện:** Tài khoản đang hoạt động. Nhà cung cấp danh tính đã được cấu hình.
**Thông tin đầu vào:** Danh tính doanh nghiệp; xác thực SSO và bằng chứng đáp ứng MFA theo chính sách được cấu hình.

**Luồng chính**

1. Chọn đăng nhập doanh nghiệp.
2. Hoàn tất xác thực SSO và đáp ứng MFA tại hệ thống danh tính doanh nghiệp.
3. Xem danh tính và các phạm vi được cấp.
4. Mở phạm vi làm việc được phép.

**Luồng thay thế và ngoại lệ**

- E01: Tài khoản bị khóa: không mở phiên.
- E02: Xác thực thành công nhưng chưa có quyền CMS: hiển thị cách liên hệ quản trị, không tự cấp vai trò.
- E03: Chưa đáp ứng MFA: không mở phiên CMS sử dụng được.
- E04: SSO không khả dụng: không mở phiên thông thường; người được chỉ định dùng quy trình tài khoản khẩn cấp theo BR-COM-19 để khôi phục quản trị, có lý do, giới hạn thời gian, cảnh báo, audit và rà soát sau sử dụng.

**BR-IAM-01:** SSO doanh nghiệp và MFA là yêu cầu đã chốt tại OI-03; danh tính hợp lệ không tự tạo quyền truy cập. Thời hạn phiên và cấu hình xác thực phải được xác nhận trước gate P0. Tài khoản khẩn cấp được bảo vệ riêng, truy vết người sử dụng thực tế và không tự bỏ qua kiểm soát nội dung theo BR-COM-19.
**Kết quả thành công:** Người dùng mở được phiên CMS với đúng quyền hiện hành.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-IAM-01-01/v1:** Given tài khoản có quyền site A; When đăng nhập; Then chỉ vào được phạm vi đã cấp.
- **AC-IAM-01-02/v1:** Given tài khoản đã khóa; When đăng nhập; Then không có phiên CMS sử dụng được.
- **AC-IAM-01-03/v1:** Given xác thực hợp lệ nhưng chưa có vai trò; When vào CMS; Then không được xem nội dung nội bộ.

## UC-IAM-02

**Đăng xuất và kết thúc phiên CMS**

**Triển khai:** P0. **Actor:** Nhân viên CMS.
**Mục tiêu:** Phiên CMS cũ không còn được sử dụng.
**Kích hoạt:** Người dùng yêu cầu thực hiện “đăng xuất và kết thúc phiên cms”.
**Tiền điều kiện:** Có phiên CMS đang hoạt động.
**Thông tin đầu vào:** Phiên hiện tại.

**Luồng chính**

1. Chọn đăng xuất.
2. Nhận cảnh báo khi còn thay đổi chưa lưu.
3. Xác nhận kết thúc phiên.
4. Trở về trang đăng nhập.

**Luồng thay thế và ngoại lệ**

- E01: Hủy xác nhận: tiếp tục phiên khi phiên còn hiệu lực.
- E02: Phiên đã hết hạn: vẫn hiển thị trạng thái đã đăng xuất, không báo thao tác thất bại.

**BR-IAM-02:** Phiên CMS có thời hạn và hỗ trợ vô hiệu hóa. Phiên hết hạn, đã đăng xuất hoặc bị vô hiệu hóa không được tiếp tục lấy dữ liệu hay thực hiện hành động. Đăng xuất CMS không mặc định đăng xuất mọi ứng dụng SSO; không giữ quyền cũ chỉ vì phiên đang mở.
**Kết quả thành công:** Phiên CMS cũ không còn được sử dụng.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-IAM-02-01/v1:** Given đã đăng xuất; When dùng lại phiên cũ để mở bài nội bộ; Then truy cập bị từ chối.
- **AC-IAM-02-02/v1:** Given có nội dung chưa lưu; When hủy đăng xuất; Then nội dung đang nhập còn trên màn hình.
- **AC-IAM-02-03/v1:** Given phiên đã hết hạn; When đăng xuất; Then người dùng ở trạng thái chưa đăng nhập.

**Use case liên quan / phụ thuộc:** [UC-IAM-01](01-identity-access.md#uc-iam-01).

## UC-IAM-03

**Tiếp nhận người dùng vào CMS**

**Triển khai:** P0. **Actor:** Quản trị danh tính.
**Mục tiêu:** Có hồ sơ người dùng thuộc đúng đơn vị, truy vết được người tiếp nhận.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tiếp nhận người dùng vào cms”.
**Tiền điều kiện:** Có quyền quản lý người dùng trong tổ chức.
**Thông tin đầu vào:** Danh tính; đơn vị; nhóm từ nguồn danh tính doanh nghiệp được chỉ định; ngày hiệu lực; người phụ trách; ánh xạ nhóm–vai trò–phạm vi được quản lý.

**Luồng chính**

1. Tìm danh tính trong nguồn doanh nghiệp hoặc tạo lời mời theo chính sách.
2. Đối chiếu đơn vị và nhóm đồng bộ từ nguồn doanh nghiệp; kiểm tra ánh xạ sang vai trò/phạm vi CMS được quản lý.
3. Kiểm tra trùng danh tính và phạm vi quản lý.
4. Xác nhận tiếp nhận và xem trạng thái quyền ban đầu.

**Luồng thay thế và ngoại lệ**

- E01: Danh tính đã tồn tại: mở hồ sơ hiện có, không tạo bản sao.
- E02: Đơn vị ngoài phạm vi: từ chối tiếp nhận vào đơn vị đó.
- E03: Đồng bộ nhóm lỗi hoặc chưa có ánh xạ: hiển thị trạng thái cần xử lý, không tự cấp thêm quyền hoặc coi việc đồng bộ đã hoàn tất.

**BR-IAM-03:** Không dùng tên hiển thị làm khóa danh tính. Thành viên nhóm được đồng bộ từ nguồn doanh nghiệp được chỉ định; quyền chỉ phát sinh qua cấp quyền rõ ràng hoặc ánh xạ nhóm–vai trò–phạm vi đã được quản lý. Thay đổi nhóm phải tính lại quyền hiệu lực và ghi audit; không yêu cầu đăng nhập lại mới thu hồi quyền cũ. Lỗi đồng bộ không được tự mở rộng quyền.
**Kết quả thành công:** Có hồ sơ người dùng thuộc đúng đơn vị, truy vết được người tiếp nhận.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-IAM-03-01/v1:** Given danh tính đã có; When tiếp nhận lại; Then không có người dùng trùng.
- **AC-IAM-03-02/v1:** Given người mới chưa có vai trò; When mở nội dung; Then bị từ chối.
- **AC-IAM-03-03/v1:** Given quản trị chỉ quản lý đơn vị A; When thêm người vào B; Then thao tác bị chặn.

**Use case liên quan / phụ thuộc:** [UC-IAM-01](01-identity-access.md#uc-iam-01).

## UC-IAM-04

**Thiết lập và cấp quyền theo phạm vi**

**Triển khai:** P0. **Actor:** Quản trị phân quyền hoặc người được cấp quyền quản lý chia sẻ.
**Mục tiêu:** Người được cấp chỉ thực hiện được hành động trong phạm vi cho phép.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thiết lập và cấp quyền theo phạm vi”.
**Tiền điều kiện:** Người dùng/nhóm cần cấp quyền hoặc nội dung/tài nguyên cần chia sẻ tồn tại. Site và phạm vi nguồn, đích đã được khai báo.
**Thông tin đầu vào:** Người dùng/nhóm hoặc đối tượng chia sẻ; vai trò; hành động; đơn vị; site nguồn; site đích; loại nội dung; ngôn ngữ; thời hạn.

**Luồng chính**

1. Chọn người dùng/nhóm cần cấp quyền hoặc đối tượng cùng site đích cần chia sẻ.
2. Chọn vai trò mẫu hoặc hành động sử dụng được phép.
3. Giới hạn phạm vi và thời hạn.
4. Xem quyền hiệu lực và xác nhận.

**Luồng thay thế và ngoại lệ**

- E01: Không đủ quyền cấp một hành động: chặn cấp quyền đó.
- E02: Thay đổi làm mất quản trị viên cuối cùng: chặn hoặc yêu cầu phương án thay thế.

**BR-IAM-04:** Áp dụng RBAC: vai trò xác định hành động, giới hạn theo đơn vị/nhóm, site, loại nội dung và locale. Nhóm và đơn vị thuộc IAM; cấu hình workspace không tự cấp quyền. Ánh xạ nhóm đồng bộ sang vai trò/phạm vi phải được quản lý, ghi audit và tính lại quyền khi thay đổi. Quyền soạn, duyệt và xuất bản tách biệt. Quyền chia sẻ phải nêu site nguồn, site đích và hành động được phép; quyền dùng ở site đích không suy rộng thành quyền quản trị đối tượng nguồn. Không tự duyệt phiên bản do mình tạo hoặc sửa.
**Kết quả thành công:** Người được cấp chỉ thực hiện được hành động trong phạm vi cho phép.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-IAM-04-01/v1:** Given biên tập viên site A chỉ được chia sẻ quyền sử dụng bài từ site B; When sửa bài nguồn ở site B; Then bị từ chối.
- **AC-IAM-04-02/v1:** Given chỉ có quyền duyệt; When xuất bản; Then bị từ chối.
- **AC-IAM-04-03/v1:** Given quyền đã hết hạn; When thực hiện hành động; Then quyền cũ không còn hiệu lực.

**Use case liên quan / phụ thuộc:** [UC-IAM-03](01-identity-access.md#uc-iam-03), [UC-SITE-01](04-site-delivery.md#uc-site-01), [UC-LOC-01](05-localization.md#uc-loc-01).

## UC-IAM-05

**Thu hồi quyền hoặc đình chỉ người dùng**

**Triển khai:** P0. **Actor:** Quản trị danh tính.
**Mục tiêu:** Quyền bị thu hồi không dùng được; dấu vết và trách nhiệm lịch sử vẫn còn.
**Kích hoạt:** Người dùng yêu cầu thực hiện “thu hồi quyền hoặc đình chỉ người dùng”.
**Tiền điều kiện:** Có quyền quản trị đối tượng. Không phụ thuộc việc bàn giao đã hoàn tất.
**Thông tin đầu vào:** Người dùng; phạm vi thu hồi; thời điểm; lý do.

**Luồng chính**

1. Chọn thu hồi một quyền hoặc đình chỉ toàn tài khoản.
2. Xem nội dung, yêu cầu duyệt và lịch đang gắn với người đó.
3. Xác nhận thu hồi.
4. Theo dõi danh sách công việc cần bàn giao.

**Luồng thay thế và ngoại lệ**

- E01: Người bị đình chỉ là chủ sở hữu duy nhất: chuyển việc chưa xử lý cho quản trị, không trì hoãn đình chỉ.
- E02: Cố đình chỉ quản trị viên cuối cùng: yêu cầu phương án khôi phục quản trị.

**BR-IAM-05:** Thu hồi có hiệu lực khi kiểm tra hành động tiếp theo, kể cả với phiên đang mở và quyền bị mất do thay đổi nhóm/ánh xạ. Đình chỉ tài khoản phải vô hiệu hóa các phiên CMS liên quan và ghi audit, không đợi bàn giao. Lịch cá nhân chưa chạy bị tạm dừng để tái ủy quyền; nội dung đã xuất bản không tự bị gỡ.
**Kết quả thành công:** Quyền bị thu hồi không dùng được; dấu vết và trách nhiệm lịch sử vẫn còn.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-IAM-05-01/v1:** Given đã đình chỉ; When dùng phiên cũ để lưu; Then thao tác bị từ chối.
- **AC-IAM-05-02/v1:** Given có lịch chưa chạy của người bị đình chỉ; When đến lịch; Then không tự xuất bản và hiện lý do tạm dừng.
- **AC-IAM-05-03/v1:** Given có bài đã xuất bản; When đình chỉ tác giả; Then bài không tự bị gỡ.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04).

## UC-IAM-06

**Ủy quyền phê duyệt có thời hạn**

**Triển khai:** P3. **Actor:** Người phê duyệt và quản trị được ủy quyền.
**Mục tiêu:** Người nhận xử lý được công việc trong đúng thời hạn và phạm vi.
**Kích hoạt:** Người dùng yêu cầu thực hiện “ủy quyền phê duyệt có thời hạn”.
**Tiền điều kiện:** Người nhận đang hoạt động và đủ điều kiện cho phạm vi được ủy quyền.
**Thông tin đầu vào:** Người nhận; phạm vi; ngày bắt đầu/kết thúc; lý do.

**Luồng chính**

1. Chọn công việc hoặc phạm vi được phép ủy quyền.
2. Chọn người nhận và thời hạn.
3. Kiểm tra xung đột trách nhiệm.
4. Xác nhận và theo dõi hiệu lực.

**Luồng thay thế và ngoại lệ**

- E01: Người nhận là tác giả hoặc đã sửa phiên bản cần duyệt: không được tự duyệt.
- E02: Ủy quyền đã hết hạn: chuyển công việc chưa xử lý về hàng đợi đủ quyền.

**BR-IAM-06:** Không cấp quyền rộng hơn người ủy quyền. Quyết định ghi cả người thao tác và căn cứ ủy quyền.
**Kết quả thành công:** Người nhận xử lý được công việc trong đúng thời hạn và phạm vi.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-IAM-06-01/v1:** Given ủy quyền chỉ cho site A; When duyệt site B; Then bị từ chối.
- **AC-IAM-06-02/v1:** Given ủy quyền hết hạn; When duyệt; Then bị từ chối.
- **AC-IAM-06-03/v1:** Given người nhận đã sửa phiên bản; When duyệt chính phiên bản đó; Then bị chặn.

**Use case liên quan / phụ thuộc:** [UC-IAM-04](01-identity-access.md#uc-iam-04), [UC-WF-03](12-workflow.md#uc-wf-03).
