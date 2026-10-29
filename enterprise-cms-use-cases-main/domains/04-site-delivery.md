# Site và phân phối nội dung

**Domain:** `SITE` · **Bắt đầu:** P0 · **Số use case:** 5
**Trạng thái:** Baseline đề xuất v0.1; các quyết định về kênh tại OI-01 và phạm vi site tại OI-02 đã được chốt.

Quản lý website, kênh đọc, cấu trúc trang và điều hướng. Publishing quyết định bản nào được phát hành.
**Quyết định OI-01:** Website công khai là kênh chính của phiên bản đầu. Thiết kế kênh/đích phải mở rộng được sang intranet và Headless API mà không thay vòng đời nội dung; hai kênh mở rộng chưa thuộc phạm vi phát hành đầu.
**Quyết định OI-02:** Dữ liệu được cô lập theo site mặc định. Trang chỉ được dùng nội dung/tài nguyên từ site khác khi có cấp quyền chia sẻ rõ ràng cho site đích.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-SITE-01](#uc-site-01) | Khởi tạo site và đích phân phối | P0 |
| [UC-SITE-02](#uc-site-02) | Soạn cấu trúc trang bằng mẫu được phép | P3 |
| [UC-SITE-03](#uc-site-03) | Quản lý menu và điều hướng | P3 |
| [UC-SITE-04](#uc-site-04) | Quản lý địa chỉ trang và thông tin tìm kiếm | P3 |
| [UC-SITE-05](#uc-site-05) | Đọc nội dung đã được phát hành | P2 |

## UC-SITE-01

**Khởi tạo site và đích phân phối**

**Triển khai:** P0. **Actor:** Quản trị site.
**Mục tiêu:** Có site dùng để cấp quyền và tạo nội dung; trạng thái sẵn sàng được hiển thị.
**Kích hoạt:** Người dùng yêu cầu thực hiện “khởi tạo site và đích phân phối”.
**Tiền điều kiện:** Workspace đã được cấu hình. Có quyền tạo site.
**Thông tin đầu vào:** Tên và mã site; đơn vị sở hữu; locale; loại kênh; môi trường; địa chỉ đích.

**Luồng chính**

1. Tạo site và chọn đơn vị sở hữu.
2. Chọn ngôn ngữ mặc định và cấu hình website công khai làm kênh phân phối đầu tiên.
3. Phân biệt đích xem trước và đích công khai.
4. Lưu cấu hình và kiểm tra khả năng kết nối khi đích sẵn sàng.

**Luồng thay thế và ngoại lệ**

- E01: Mã site trùng: yêu cầu mã khác.
- E02: Đích chưa hoạt động: giữ trạng thái chưa sẵn sàng xuất bản, vẫn cho chuẩn bị nội dung.

**BR-SITE-01:** Tạo site không đồng nghĩa đưa site ra công khai. Một workspace có thể có nhiều site; loại kênh và đích phân phối phải được định danh, cấu hình và có phạm vi rõ ràng. Lõi site không được giả định mọi kênh tương lai đều công khai hoặc đều dựng trang phía máy chủ.
**Kết quả thành công:** Có site dùng để cấp quyền và tạo nội dung; trạng thái sẵn sàng được hiển thị.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SITE-01-01/v1:** Given mã site đã tồn tại; When tạo site cùng mã; Then bị chặn.
- **AC-SITE-01-02/v1:** Given đích website công khai chưa được kiểm tra; When xuất bản; Then bị chặn.
- **AC-SITE-01-03/v1:** Given chỉ tạo site; When truy cập công khai; Then không tự xuất hiện bản nháp.

**Use case liên quan / phụ thuộc:** [UC-ADM-01](02-administration.md#uc-adm-01).

## UC-SITE-02

**Soạn cấu trúc trang bằng mẫu được phép**

**Triển khai:** P3. **Actor:** Biên tập viên site.
**Mục tiêu:** Có phiên bản trang hợp lệ để duyệt, chưa thay đổi trang công khai.
**Kích hoạt:** Người dùng yêu cầu thực hiện “soạn cấu trúc trang bằng mẫu được phép”.
**Tiền điều kiện:** Site tồn tại. Có mẫu và thành phần được doanh nghiệp cho phép.
**Thông tin đầu vào:** Loại trang; mẫu; khối nội dung; thứ tự; vùng hiển thị; tham chiếu nội dung.

**Luồng chính**

1. Tạo bản nháp trang và chọn mẫu.
2. Thêm, xếp thứ tự các khối được phép.
3. Chọn nội dung hoặc tài nguyên cho từng khối.
4. Xem trước và gửi qua quy trình duyệt.

**Luồng thay thế và ngoại lệ**

- E01: Khối thiếu dữ liệu bắt buộc: chỉ rõ vị trí lỗi.
- E02: Nội dung liên kết không đủ điều kiện công khai: chặn phát hành trang.

**BR-SITE-02:** Trang có phiên bản, phê duyệt và xuất bản như nội dung. Nội dung/tài nguyên chéo site chỉ được chọn khi quyền chia sẻ cho site đích còn hiệu lực; quyền dùng không cho phép sửa nguồn. Không cho chèn mã tùy ý để vượt kiểm soát bảo mật.
**Kết quả thành công:** Có phiên bản trang hợp lệ để duyệt, chưa thay đổi trang công khai.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SITE-02-01/v1:** Given sửa bố cục trang đang công khai; When lưu; Then trang công khai giữ nguyên.
- **AC-SITE-02-02/v1:** Given khối bắt buộc thiếu nội dung; When gửi duyệt; Then bị chặn.
- **AC-SITE-02-03/v1:** Given chọn nội dung từ site khác nhưng không được chia sẻ cho site đích; When thêm vào trang; Then bị từ chối.

**Use case liên quan / phụ thuộc:** [UC-CNT-04](09-content.md#uc-cnt-04), [UC-VER-01](10-versioning.md#uc-ver-01), [UC-WF-02](12-workflow.md#uc-wf-02).

## UC-SITE-03

**Quản lý menu và điều hướng**

**Triển khai:** P3. **Actor:** Biên tập viên site.
**Mục tiêu:** Có bản điều hướng hợp lệ, chỉ thay đổi công khai sau phát hành.
**Kích hoạt:** Người dùng yêu cầu thực hiện “quản lý menu và điều hướng”.
**Tiền điều kiện:** Có quyền sửa điều hướng của site.
**Thông tin đầu vào:** Vị trí menu; nhãn; thứ tự; quan hệ cha-con; đích liên kết.

**Luồng chính**

1. Mở bản nháp điều hướng.
2. Thêm hoặc di chuyển mục menu.
3. Kiểm tra cây và các liên kết đích.
4. Xem trước rồi gửi duyệt và xuất bản bằng cơ chế chung.

**Luồng thay thế và ngoại lệ**

- E01: Tạo vòng lặp cha-con: chặn.
- E02: Liên kết tới bản nháp không nằm trong cùng đợt phát hành hợp lệ: chặn công khai.

**BR-SITE-03:** Điều hướng có bản nháp và bản công khai riêng. Không dùng việc ẩn menu thay cho kiểm soát truy cập.
**Kết quả thành công:** Có bản điều hướng hợp lệ, chỉ thay đổi công khai sau phát hành.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SITE-03-01/v1:** Given di chuyển mục A vào con của chính A; When lưu; Then bị chặn.
- **AC-SITE-03-02/v1:** Given chỉnh menu nháp; When người đọc mở site; Then vẫn thấy menu đang công khai.
- **AC-SITE-03-03/v1:** Given mục menu trỏ tới nội dung không được đọc; When kiểm tra phát hành; Then báo lỗi.

**Use case liên quan / phụ thuộc:** [UC-SITE-02](04-site-delivery.md#uc-site-02), [UC-PUB-01](13-publishing.md#uc-pub-01).

## UC-SITE-04

**Quản lý địa chỉ trang và thông tin tìm kiếm**

**Triển khai:** P3. **Actor:** Biên tập viên site.
**Mục tiêu:** Người đọc đi tới đúng trang; thay đổi đường dẫn có kiểm soát.
**Kích hoạt:** Người dùng yêu cầu thực hiện “quản lý địa chỉ trang và thông tin tìm kiếm”.
**Tiền điều kiện:** Có quyền chỉnh đường dẫn và metadata của site.
**Thông tin đầu vào:** Đường dẫn theo locale; tiêu đề tìm kiếm; mô tả; địa chỉ chuẩn; chuyển hướng.

**Luồng chính**

1. Nhập đường dẫn và thông tin hiển thị.
2. Kiểm tra trùng trong site và locale.
3. Khi đổi đường dẫn, xem liên kết cũ và đề xuất chuyển hướng.
4. Duyệt và phát hành thay đổi.

**Luồng thay thế và ngoại lệ**

- E01: Đường dẫn trùng: không ghi đè trang khác.
- E02: Chuyển hướng tạo vòng lặp hoặc trỏ tới đích không được phép: chặn.

**BR-SITE-04:** Quy tắc chuẩn hóa đường dẫn phải thống nhất. Metadata và đường dẫn công khai gắn với bản phát hành.
**Kết quả thành công:** Người đọc đi tới đúng trang; thay đổi đường dẫn có kiểm soát.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SITE-04-01/v1:** Given hai trang cùng đường dẫn trong cùng site và locale; When phát hành; Then xung đột bị chặn.
- **AC-SITE-04-02/v1:** Given A chuyển B và B chuyển A; When xác nhận; Then vòng lặp bị chặn.
- **AC-SITE-04-03/v1:** Given sửa metadata nháp; When xem site công khai; Then metadata cũ chưa bị đổi.

**Use case liên quan / phụ thuộc:** [UC-SITE-01](04-site-delivery.md#uc-site-01), [UC-PUB-01](13-publishing.md#uc-pub-01).

## UC-SITE-05

**Đọc nội dung đã được phát hành**

**Triển khai:** P2. **Actor:** Khách truy cập hoặc nhân viên được quyền đọc.
**Mục tiêu:** Người đọc chỉ nhận nội dung được phép và đúng bản đang công khai.
**Kích hoạt:** Người dùng yêu cầu thực hiện “đọc nội dung đã được phát hành”.
**Tiền điều kiện:** Có bản phát hành đang có hiệu lực tại site, locale và kênh được yêu cầu.
**Thông tin đầu vào:** Địa chỉ hoặc nội dung; locale; danh tính người đọc khi kênh yêu cầu.

**Luồng chính**

1. Mở website hoặc kênh nội dung.
2. Được kiểm tra quyền đọc khi cần.
3. Nhận bản đã phát hành và tài nguyên tương ứng.
4. Mở các liên kết được công khai trong cùng phạm vi.

**Luồng thay thế và ngoại lệ**

- E01: Nội dung chưa phát hành hoặc đã gỡ: hiển thị không khả dụng theo chính sách.
- E02: Thiếu bản dịch: dùng quy tắc fallback đã được chốt, không lấy bản nháp.

**BR-SITE-05:** Bản xem trước không dùng chung quyền truy cập công khai. Quyền đọc tài nguyên và cache phải phù hợp kênh.
**Kết quả thành công:** Người đọc chỉ nhận nội dung được phép và đúng bản đang công khai.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-SITE-05-01/v1:** Given bản v1 đang công khai và v2 đang nháp; When mở trang; Then thấy v1.
- **AC-SITE-05-02/v1:** Given nội dung riêng tư; When người chưa có quyền mở trực tiếp; Then không đọc được cả trang lẫn tài nguyên riêng.
- **AC-SITE-05-03/v1:** Given gỡ đã hoàn tất; When mở lại kênh phân phối; Then không nhận lại bản bị gỡ từ cache vượt giới hạn đã cam kết.

**Use case liên quan / phụ thuộc:** [UC-PUB-01](13-publishing.md#uc-pub-01), [UC-LOC-01](05-localization.md#uc-loc-01), [UC-AST-01](08-assets.md#uc-ast-01).
