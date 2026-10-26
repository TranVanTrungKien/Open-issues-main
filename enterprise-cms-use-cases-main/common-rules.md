# Quy tắc chung — Enterprise CMS

**Trạng thái:** Đề xuất v0.1. Cần chủ sản phẩm phê duyệt trước khi trở thành yêu cầu chính thức.

[Mục lục](README.md) · [Thứ tự triển khai](implementation-roadmap.md) · [Mô hình trạng thái](state-model.md) · [Nguồn](sources.md)

## Phạm vi và giả định

CMS phục vụ một doanh nghiệp, nhiều đơn vị và nhiều site.
Baseline có một locale mặc định và một đích phân phối hoạt động ở lần phát hành đầu tiên.
Đa tenant SaaS, thanh toán, CRM, thương mại điện tử và quản lý hồ sơ pháp lý chuyên dụng chưa thuộc phạm vi.
CMS có thể phục vụ website công khai hoặc cổng nội bộ. Kênh nội bộ phải có quyền đọc riêng.
Nguồn công khai chỉ là tham chiếu thiết kế. Không coi tính năng một sản phẩm là yêu cầu bắt buộc cho sản phẩm này.

## Vai trò nghiệp vụ

| Vai trò | Trách nhiệm | Không mặc định có |
|---|---|---|
| Quản trị hệ thống | Cấu hình môi trường và workspace | Quyền duyệt mọi nội dung |
| Quản trị danh tính | Tiếp nhận, cấp và thu hồi quyền | Quyền tự cấp ngoài phạm vi được quản lý |
| Quản trị nội dung | Mô hình, tài nguyên, phân loại, chủ sở hữu | Quyền bỏ qua duyệt |
| Biên tập viên | Soạn, xem trước, gửi duyệt | Quyền công khai trực tiếp |
| Người duyệt | Quyết định theo phiên bản và phạm vi | Quyền tự duyệt bản mình tạo hoặc sửa |
| Người xuất bản | Phát hành và gỡ bản đã đủ điều kiện | Quyền sửa ngầm bản đã duyệt |
| Biên dịch viên | Chuẩn bị nội dung theo locale | Quyền duyệt mọi locale |
| Kiểm toán viên | Đọc dấu vết trong phạm vi | Quyền sửa hoặc xóa lịch sử |
| Người đọc | Đọc nội dung được phân phối cho mình | Quyền xem trước bản nháp |
| Tài khoản tích hợp | Thực hiện tác vụ trong quyền đã cấp | Quyền quản trị toàn hệ thống |

## BR-COM-01 — Kiểm tra quyền ở mọi điểm truy cập

Quyền dựa trên hành động, tổ chức/nhóm, site, loại nội dung, locale và chính sách sở hữu.
Không có quyền rõ ràng thì từ chối.
Kiểm tra quyền hiện hành khi mở, lưu, duyệt, phát hành, tải tệp, xuất dữ liệu và thực thi lịch.
Ẩn nút không thay thế kiểm tra phía xử lý.
Tìm kiếm, số lượng, gợi ý, thumbnail, bản xem trước và thông báo không được làm lộ dữ liệu.
Tham chiếu: [OWASP Authorization](sources.md#src-01).

## BR-COM-02 — Tách quyền quản trị và quyền nghiệp vụ

Không tự cấp quyền nội dung chỉ vì người dùng đăng nhập SSO thành công.
Không coi System Admin đương nhiên có quyền sửa, duyệt và xuất bản mọi nội dung.
Quyền cấp cho nhóm phải có phạm vi được quản lý và được kiểm tra lại khi nhóm thay đổi.
Phải bảo vệ khả năng quản trị cuối cùng và có quy trình khôi phục truy cập an toàn.

## BR-COM-03 — Giữ nguyên bản lưu

Mỗi lần lưu thành công tạo hoặc xác nhận một phiên bản nhận biết được.
Dữ liệu của phiên bản đã lưu không bị sửa lại.
Thay đổi mới tạo phiên bản mới.
Thông tin quyết định và trạng thái được ghi liên kết với phiên bản; không chỉnh lại dữ liệu lịch sử.
Phiên bản hóa là điều kiện của Content từ P1, không phải phần thêm sau Workflow.

## BR-COM-04 — Tách bản làm việc, quyết định duyệt và bản công khai

Một nội dung có thể đang công khai v2 trong khi v3 là nháp hoặc đang duyệt.
Trạng thái công khai xét theo nội dung, site, locale và đích phân phối.
Lưu, xem trước, duyệt, lưu trữ và xuất bản là các hành động khác nhau.
Không dùng một trường trạng thái duy nhất để thay mọi khái niệm này.
Tham chiếu mô hình bản đang công khai và bản làm việc: [Drupal Content Moderation](sources.md#src-04).

## BR-COM-05 — Phê duyệt gắn đúng căn cứ

Quyết định gắn với phiên bản nội dung, mô hình, phụ thuộc và phạm vi cần duyệt.
Quy trình áp dụng có phiên bản.
Sửa nội dung sau duyệt tạo bản mới chưa được duyệt.
Phê duyệt bản cũ không tự biến mất khi có bản nháp mới, nhưng không được dùng cho bản mới.
Thu hồi phê duyệt là quyết định riêng, có lý do và ảnh hưởng tới lịch chưa thực thi.

## BR-COM-06 — Phân tách trách nhiệm

Người tạo hoặc tham gia sửa phiên bản trong lần gửi duyệt không tự phê duyệt phiên bản đó.
Ủy quyền, phân công lại hoặc có nhiều vai trò không bỏ qua quy tắc này.
Baseline P2 có một cấp duyệt độc lập.
Nhiều cấp bắt buộc phải chuyển vào P2 nếu chính sách doanh nghiệp yêu cầu ngay khi vận hành.

## BR-COM-07 — Khóa căn cứ phát hành

Bản phát hành biết chính xác nội dung, mô hình, tài nguyên, phân loại và cấu hình hiển thị được sử dụng.
Thay tệp hoặc nhãn dùng trong bản công khai không được làm đổi bản phát hành ngoài quy trình.
Tham chiếu bắt buộc phải hợp lệ ở đích hoặc nằm trong đợt phát hành có thứ tự hợp lệ.
Không tự phát hành bản nháp được tham chiếu chỉ để làm cho bản chính hợp lệ.

## BR-COM-08 — Chỉ báo hoàn tất khi đích đã đạt kết quả

Nhận yêu cầu, đang xử lý và đích phục vụ bản mới là ba trạng thái khác nhau.
Lỗi ở một đích không được báo thành công ở toàn bộ đích.
Mỗi lần chuyển bản phải bảo toàn tính nhất quán theo khả năng đã cam kết của đích đó.
Không mặc định có giao dịch nguyên tử xuyên nhiều hệ thống.
Giới hạn thời gian cập nhật kênh, cache và tìm kiếm phải chốt trước nghiệm thu production.

## BR-COM-09 — Tác vụ lặp và tác vụ lỗi thời

Thử lại cùng yêu cầu nghiệp vụ không tạo phát hành trùng.
Mã yêu cầu và phiên bản mục tiêu được giữ khi thử lại.
Tác vụ cũ không được ghi đè một lần gỡ hoặc phát hành mới hơn.
Trước khi chuyển bản, kiểm tra lại quyền, quyết định duyệt, tài nguyên và trạng thái hủy.
Kết quả thắng trong tranh chấp hủy/thực thi phải rõ ràng.

## BR-COM-10 — Thời gian và lịch

Người dùng nhìn thấy thời điểm và múi giờ đã chọn.
Lịch lưu được thời điểm tuyệt đối cùng múi giờ dùng khi lập.
Đổi múi giờ hiển thị không làm đổi lịch đã xác nhận.
Thời điểm không tồn tại hoặc có hai cách hiểu do đổi giờ mùa phải được xử lý rõ.
Baseline không tự phát hành bù một lịch đã quá hạn sau sự cố dài; yêu cầu rà soát lại.
Lịch tự gỡ hoặc hết hiệu lực phải có chính sách khôi phục sau sự cố được chốt riêng.

## BR-COM-11 — Thu hồi quyền

Quyền bị thu hồi không còn dùng được ở hành động tiếp theo.
Không trì hoãn đình chỉ vì nội dung chưa được bàn giao.
Nội dung đã công khai không tự bị gỡ chỉ vì tác giả nghỉ việc.
Lịch do cá nhân tạo cần quyền hiện hành của người đó tại thời điểm chạy trong baseline.
Bàn giao chủ sở hữu không tự tái ủy quyền cho các lịch đã dừng.

## BR-COM-12 — Tài nguyên an toàn và có quyền sử dụng

Chỉ tài nguyên đã qua kiểm tra mới được tham gia phát hành.
Tệp nháp, tệp cách ly và thumbnail nhạy cảm không được công khai ngoài ý muốn.
Loại tệp, kích thước và nội dung thực được kiểm tra theo chính sách.
Thông tin quyền sử dụng phải còn hiệu lực ở thời điểm phát hành.
Ảnh hưởng khi quyền sử dụng hết trong lúc đang công khai phải chốt ở [OI-10](open-issues.md#oi-10).
Tham chiếu: [OWASP File Upload](sources.md#src-02).

## BR-COM-13 — Audit từ đầu

Các thao tác nhạy cảm có người thực hiện, đối tượng, thời điểm, phạm vi, kết quả và căn cứ liên quan.
Đăng nhập, cấp quyền, thay cấu hình được ghi từ P0; nội dung, duyệt và phát hành bổ sung ngay khi có năng lực đó.
Không có chức năng sửa audit qua quy trình quản trị thông thường.
Không ghi mật khẩu, token hoặc bí mật tích hợp nguyên văn.
Nếu không thể ghi dấu vết bền vững cho thao tác nhạy cảm, không báo thao tác đã hoàn tất có kiểm toán.
Không coi log kỹ thuật đơn thuần là lịch sử phê duyệt.
Tham chiếu: [OWASP Logging](sources.md#src-03).

## BR-COM-14 — Bản dịch độc lập nhưng truy vết được nguồn

Locale mặc định tồn tại từ P0, kể cả khi chỉ hỗ trợ một ngôn ngữ.
Bản dịch biết phiên bản nguồn được dùng.
Mỗi locale có thể có bản nháp, quyết định duyệt và bản công khai riêng.
Không dùng bản nháp làm fallback.
Không ghi đè bản dịch đang công khai khi bản nguồn thay đổi.
Tham chiếu mô hình phát hành theo locale: [Contentful Locale-based Publishing](sources.md#src-05).
Tài liệu này không giả định các giới hạn sản phẩm Contentful cũng là giới hạn của CMS đề xuất.

## BR-COM-15 — Lưu trữ, giữ lại và xóa

Lưu trữ không đồng nghĩa gỡ công khai hoặc xóa vĩnh viễn.
Nội dung chỉ được xác nhận ngừng công khai khi quy trình gỡ đã đạt điều kiện.
Không bật xóa vĩnh viễn khi thời hạn và phạm vi xóa chưa được chốt.
Yêu cầu giữ lại chặn xóa và có ưu tiên hơn việc hết hạn.
Việc xử lý cache, chỉ mục, bản sao và backup phải có phạm vi rõ; không hứa xóa mọi bản sao tức thì.

## BR-COM-16 — Thất bại phải quan sát được

Lưu xung đột không được ghi đè thay đổi của người khác im lặng.
Thao tác thất bại không được trả kết quả giống thành công.
Tác vụ nhiều bước hiển thị rõ đã xử lý, chưa xử lý và không thể xử lý.
Thông báo lỗi hướng dẫn bước tiếp theo nhưng không lộ bí mật hoặc dữ liệu ngoài quyền.

## BR-COM-17 — Ranh giới tài liệu và triển khai

Domain là nhóm năng lực nghiệp vụ, không mặc định là microservice.
Các use case tham chiếu nhau có thể cần làm trong cùng một lát cắt.
Versioning đi cùng Content; audit đi cùng mọi thay đổi; tìm kiếm cơ bản đi cùng nội dung.
Base Design chốt hành vi quan sát được. Detail Design mới chốt API, dữ liệu, giao dịch và cơ chế phân phối.
Không dùng 202 AC riêng trong các file như một danh sách test hoàn chỉnh; còn cần AC chung, kiểm tra bảo mật, vận hành và các nhánh triển khai cụ thể.

## Điều kiện phi chức năng trước production

| Nhóm | Cần chốt và chứng minh |
|---|---|
| Bảo mật | SSO/MFA, phạm vi quyền, phiên, kiểm soát tệp và nội dung nhập, bí mật tích hợp |
| Phục hồi | Mức mất dữ liệu cho phép, thời gian phục hồi, backup bao gồm tệp và cấu hình, diễn tập restore |
| Hiệu năng | Số người dùng, nội dung, tài nguyên, kích thước tệp, thời gian phản hồi |
| Phân phối | Giới hạn trễ xuất bản, gỡ, cache và tìm kiếm; hành vi khi mất kết nối |
| Quan sát | Cảnh báo lỗi duyệt, lịch, phát hành, quét tệp và gián đoạn audit |
| Khả năng sử dụng | Bàn phím, hỗ trợ người khuyết tật, thông báo lỗi, ngôn ngữ và múi giờ |
| Dữ liệu | Phân loại nhạy cảm, lưu giữ, phạm vi xuất, quy trình xử lý yêu cầu xóa |

Chưa có số liệu được khách hàng xác nhận; các chỉ tiêu định lượng nằm ở [Open Issues](open-issues.md).
