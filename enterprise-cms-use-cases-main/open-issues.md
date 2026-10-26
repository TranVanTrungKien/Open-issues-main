# Open Issues và quyết định cần chốt

**Trạng thái:** Chưa được khách hàng xác nhận.
**Nguyên tắc:** Dùng baseline có đánh dấu để phân tích; không dùng giá trị chưa chốt làm căn cứ nghiệm thu production.

[Mục lục](README.md) · [Lộ trình](implementation-roadmap.md) · [Quy tắc chung](common-rules.md)

## OI-01

**Cần chốt:** Website công khai, cổng nội bộ hay headless CMS? Có nhu cầu quản lý hồ sơ pháp lý chuyên dụng không?
**Baseline:** CMS nội dung cho một doanh nghiệp. Một kênh đọc được chọn cho lần phát hành đầu.
**Chủ quyết định:** Chủ sản phẩm.
**Hạn chốt:** Trước P0. Ảnh hưởng Site, Publishing và quyền người đọc.

## OI-02

**Cần chốt:** Một doanh nghiệp nhiều đơn vị hay SaaS đa tenant? Dữ liệu có được chia sẻ giữa các site không?
**Baseline:** Một workspace doanh nghiệp, nhiều đơn vị và site. Không có chia sẻ chéo phạm vi mặc định.
**Chủ quyết định:** Chủ sản phẩm và bảo mật.
**Hạn chốt:** Trước mô hình dữ liệu P0. SaaS đa tenant là thay đổi phạm vi kiến trúc, không chỉ thêm bộ lọc.

## OI-03

**Cần chốt:** Nhà cung cấp SSO, MFA, thời hạn phiên, tài khoản khẩn cấp, nguồn đồng bộ nhóm và cách vô hiệu hóa.
**Baseline:** Xác thực doanh nghiệp; quyền CMS riêng; quyền bị thu hồi không dùng được ở hành động tiếp theo.
**Chủ quyết định:** Bộ phận quản trị danh tính và bảo mật.
**Hạn chốt:** P0.

## OI-04

**Cần chốt:** Một hay nhiều cấp duyệt? Có áp dụng maker-checker cho mọi loại nội dung không? Ai được thu hồi duyệt?
**Baseline:** Một cấp độc lập. Người tham gia tạo/sửa trong lần gửi duyệt không tự duyệt.
**Chủ quyết định:** Chủ nội dung và người phụ trách kiểm soát.
**Hạn chốt:** Trước P2. Nếu nhiều cấp bắt buộc, chuyển năng lực này từ P3 về P2.

## OI-05

**Cần chốt:** Các loại nội dung đầu tiên, trường bắt buộc, quan hệ, kích thước, quy tắc URL và dữ liệu nhạy cảm.
**Baseline:** Chưa áp đặt Article/News/Product là yêu cầu chính thức. Có thể dùng một loại mẫu để thử lát cắt đầu.
**Chủ quyết định:** Chủ nội dung và BA.
**Hạn chốt:** Trước P1.

## OI-06

**Cần chốt:** Đích phân phối, bằng chứng đã phục vụ bản mới, giới hạn trễ xuất bản/gỡ/cache/tìm kiếm.
**Baseline:** Một đích hoạt động trong P2; không báo hoàn tất khi chỉ mới nhận yêu cầu.
**Chủ quyết định:** Chủ sản phẩm và vận hành.
**Hạn chốt:** Trước nghiệm thu P2. Không tự đặt số giây khi chưa có căn cứ.

## OI-07

**Cần chốt:** Lịch theo quyền cá nhân hay ủy quyền cấp tổ chức? Xử lý lịch bị lỡ sau sự cố thế nào?
**Baseline:** Quyền người lập còn hiệu lực lúc chạy. Lịch xuất bản quá hạn cần được rà soát lại, không tự phát hành bù.
**Chủ quyết định:** Chủ nội dung và vận hành.
**Hạn chốt:** Trước triển khai lập lịch P2.

## OI-08

**Cần chốt:** Ngôn ngữ bắt buộc, fallback, trường không được fallback, và thế nào là bản dịch lỗi thời.
**Baseline:** Một locale P0; locale độc lập về duyệt và phát hành; không fallback sang nháp.
**Chủ quyết định:** Chủ nội dung và nhóm dịch.
**Hạn chốt:** Quy tắc mô hình ở P0; quy trình dịch trước P3. Đa ngôn ngữ bắt buộc khi go-live phải chuyển vào P2.

## OI-09

**Cần chốt:** Loại tệp, giới hạn tải, kiểm tra an toàn, ảnh/video cần xử lý và dung lượng dự kiến.
**Baseline:** Tệp chưa đạt kiểm tra không được phát hành; không tin định dạng khai báo từ phía tải lên.
**Chủ quyết định:** Bảo mật và quản trị tài nguyên.
**Hạn chốt:** Trước P1 và xác minh trước production.

## OI-10

**Cần chốt:** Ai xác nhận quyền sử dụng tài nguyên? Làm gì khi quyền hết trong lúc tài nguyên đang công khai?
**Baseline:** Chặn phát hành mới khi hết quyền; cần kế hoạch thu hồi các bản đang công khai chịu ảnh hưởng.
**Chủ quyết định:** Chủ nội dung và người phụ trách quyền sử dụng.
**Hạn chốt:** Trước phát hành tài nguyên có hạn sử dụng.
**Không được ngầm hiểu:** Chặn phát hành mới là đủ để tiếp tục phục vụ mọi bản cũ.

## OI-11

**Cần chốt:** Thời hạn lưu giữ nội dung/audit/tệp, yêu cầu giữ lại, phạm vi xóa, backup và dữ liệu cá nhân.
**Baseline:** Chưa bật xóa vĩnh viễn tự động. Không coi đó là một chính sách lưu giữ đáp ứng mọi yêu cầu.
**Chủ quyết định:** Chủ dữ liệu, bảo mật và bộ phận phụ trách tuân thủ.
**Hạn chốt:** Trước production. Các use case GOV-P4 phải chuyển sớm nếu là điều kiện sử dụng dữ liệu thật.

## OI-12

**Cần chốt:** Số người dùng đồng thời, số nội dung/site/locale, kích thước kho tệp và khối lượng lịch.
**Baseline:** Không gán số giả định thành cam kết hiệu năng.
**Chủ quyết định:** Chủ sản phẩm và vận hành.
**Hạn chốt:** Trước Detail Design và kiểm thử tải P2.

## OI-13

**Cần chốt:** Mức mất dữ liệu cho phép, thời gian phục hồi và phạm vi backup.
**Baseline:** Có backup dữ liệu, tệp và cấu hình; diễn tập khôi phục trước production.
**Chủ quyết định:** Vận hành và chủ sản phẩm.
**Hạn chốt:** P2. Không đợi tới phần tích hợp mở rộng P4.

## OI-14

**Cần chốt:** Nguồn nhập, khóa đối chiếu, quyền di chuyển, xử lý lỗi từng phần và đối soát số lượng.
**Baseline:** Nhập thành nháp; không chuyển mặc định phê duyệt hoặc trạng thái công khai cũ.
**Chủ quyết định:** Chủ dữ liệu và nhóm migration.
**Hạn chốt:** Trước P4; chuyển vào P1/P2 nếu phải thay hệ thống đang dùng ngay lúc go-live.

## OI-15

**Cần chốt:** Mức độ thiết kế trang, mẫu có sẵn, yêu cầu trợ năng và thiết bị trình duyệt hỗ trợ.
**Baseline:** P1/P2 dùng mẫu cố định hoặc trình xem có cấu trúc; page builder mở rộng ở P3.
**Chủ quyết định:** Chủ sản phẩm và thiết kế.
**Hạn chốt:** Mẫu đầu tiên trước P2; page builder trước P3.

## OI-16

**Cần chốt:** API hoặc hệ thống tích hợp nào bắt buộc khi go-live? Có yêu cầu phát hành đồng thời tuyệt đối không?
**Baseline:** SSO và một đích phân phối là tích hợp nền tảng, không đợi P4. Tích hợp mở rộng và webhook ở P4.
**Chủ quyết định:** Chủ sản phẩm và kiến trúc.
**Hạn chốt:** P0/P2 cho kết nối bắt buộc. Yêu cầu nguyên tử xuyên hệ thống cần đánh giá riêng, không mặc định hỗ trợ.
