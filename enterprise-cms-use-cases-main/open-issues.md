# Open Issues và quyết định cần chốt

**Trạng thái:** OI-01, OI-02 và OI-03 đã được chốt; các vấn đề còn lại chưa được khách hàng xác nhận.
**Nguyên tắc:** Dùng baseline có đánh dấu để phân tích; không dùng giá trị chưa chốt làm căn cứ nghiệm thu production.

[Mục lục](README.md) · [Lộ trình](implementation-roadmap.md) · [Quy tắc chung](common-rules.md)

## OI-01

**Trạng thái:** Đã chốt ngày 2026-10-22.
**Quyết định về kênh:** Chọn **website công khai** làm kênh chính ở phiên bản đầu tiên.
**Ràng buộc kiến trúc:** Tách quản lý nội dung/quy trình khỏi cơ chế phân phối; mô hình hóa kênh và đích phân phối bằng cấu hình hoặc hợp đồng tích hợp. Không gắn cứng mô hình người đọc ẩn danh hay cách dựng trang vào lõi để có thể bổ sung **intranet** với kiểm soát quyền đọc và **Headless API** có phiên bản sau này.
**Ngoài phạm vi phiên bản đầu:** Vận hành kênh intranet và cung cấp Headless API cho ứng dụng tiêu thụ nội dung. Hai năng lực này cần được xác định phạm vi và nghiệm thu riêng khi triển khai.
**Quyết định về hồ sơ pháp lý:** Hiện chưa cần chức năng **Records Management/Legal Records** chuyên dụng. Phạm vi dự án tập trung vào CMS quản lý và xuất bản nội dung; chỉ bổ sung năng lực quản lý hồ sơ chuyên dụng khi có yêu cầu nghiệp vụ rõ ràng và phạm vi riêng được phê duyệt.
**Chủ quyết định:** Chủ sản phẩm.
**Hạn chốt:** Đã hoàn tất.

## OI-02

**Trạng thái:** Đã chốt ngày 2026-10-28.
**Quyết định:** Một workspace phục vụ một doanh nghiệp, nhiều đơn vị và nhiều site; không phải SaaS đa tenant.
**Ranh giới dữ liệu:** Dữ liệu được cô lập theo site mặc định. Nội dung và tài nguyên chỉ được chia sẻ sang site đích bằng cấp quyền chia sẻ rõ ràng, có phạm vi, có thể thu hồi và có audit; chia sẻ không mặc nhiên cấp quyền sửa, duyệt, xuất bản hoặc quản trị đối tượng nguồn.
**Bảo toàn khi thu hồi:** Ngừng chia sẻ phải chặn việc sử dụng mới và nêu các phụ thuộc hiện có. Không làm hỏng hoặc gỡ âm thầm bản đang công khai; các site chịu ảnh hưởng phải xử lý qua quy trình cập nhật hoặc gỡ có kiểm soát.
**Chủ quyết định:** Chủ sản phẩm và bảo mật.
**Hạn chốt:** Đã hoàn tất. SaaS đa tenant trong tương lai là thay đổi phạm vi kiến trúc, không chỉ thêm bộ lọc.

## OI-03

**Trạng thái:** Đã chốt ngày 2026-10-28 về giải pháp.
**Quyết định:** SSO doanh nghiệp + MFA + RBAC theo phạm vi + đồng bộ nhóm + quản lý phiên và thu hồi quyền + tài khoản khẩn cấp được kiểm soát.
**Xác thực và phân quyền:** Người dùng CMS đăng nhập qua SSO doanh nghiệp và phải đáp ứng MFA. Xác thực thành công không tự cấp quyền nghiệp vụ. RBAC giới hạn theo hành động, đơn vị/nhóm, site, loại nội dung và locale; không có quyền rõ ràng thì từ chối, tuân thủ ranh giới chia sẻ tại OI-02.
**Đồng bộ nhóm:** Đồng bộ thành viên từ nguồn danh tính doanh nghiệp được chỉ định; chỉ ánh xạ sang vai trò và phạm vi CMS đã được quản lý. Thay đổi nhóm phải tính lại quyền hiệu lực và có audit; không tự mở rộng quyền khi đồng bộ lỗi.
**Phiên và thu hồi:** Quản lý thời hạn phiên, đăng xuất và vô hiệu hóa phiên. Quyền bị thu hồi không dùng được ở hành động tiếp theo, kể cả với phiên đã mở; đình chỉ tài khoản phải vô hiệu hóa các phiên CMS liên quan, không chờ bàn giao nội dung.
**Tài khoản khẩn cấp:** Có đường khôi phục quản trị khi SSO không khả dụng; tài khoản được bảo vệ riêng, giới hạn người được sử dụng, ghi lý do, giới hạn thời gian sử dụng, cảnh báo và audit, rà soát sau sử dụng. Không dùng cho công việc thường ngày hoặc tự động bỏ qua quy tắc duyệt/xuất bản.
**Chi tiết triển khai P0 cần xác nhận:** Nhà cung cấp và cấu hình SSO/MFA, thời hạn phiên cụ thể, nguồn/cơ chế/tần suất đồng bộ nhóm, ánh xạ vai trò–phạm vi và quy trình vận hành tài khoản khẩn cấp. Đây là cấu hình để hiện thực hóa giải pháp đã chốt; không tự coi các giá trị chưa xác nhận là căn cứ nghiệm thu.
**Chủ quyết định:** Bộ phận quản trị danh tính và bảo mật.
**Hạn chốt:** Đã hoàn tất quyết định giải pháp; hoàn thiện cấu hình và kiểm chứng trước Completion Gate P0. Trạng thái này không xác nhận đã triển khai hoặc nghiệm thu.

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
**Baseline:** Một đích website công khai hoạt động trong P2; không báo hoàn tất khi chỉ mới nhận yêu cầu.
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
**Baseline:** SSO và đích website công khai là tích hợp nền tảng, không đợi P4. Headless API chưa thuộc go-live đầu tiên; API tích hợp mở rộng và webhook được xem xét ở P4.
**Chủ quyết định:** Chủ sản phẩm và kiến trúc.
**Hạn chốt:** P0/P2 cho kết nối bắt buộc. Yêu cầu nguyên tử xuyên hệ thống cần đánh giá riêng, không mặc định hỗ trợ.
