# Ghi chú thuật ngữ và chữ viết tắt — Enterprise CMS

Tài liệu tra cứu các chữ viết tắt, mã quy ước và thuật ngữ thường gặp trong bộ đặc tả CMS. Mỗi giải thích được đặt trong ngữ cảnh dự án để thuận tiện khi đọc, họp và rà soát yêu cầu.

**Căn cứ:** [README](README.md), [Open Issues](open-issues.md), [Giải pháp chi tiết](open-issues-giai-phap-chi-tiet.md), [Giải đáp OI](giai_dap_OI/README.md), [Lộ trình](implementation-roadmap.md), [Quy tắc chung](common-rules.md) và các tài liệu domain.

Các định nghĩa giúp đọc tài liệu; chúng không xác nhận phương án đề xuất hoặc bổ sung cam kết triển khai. Mã domain là quy ước của bộ tài liệu, không nhất thiết là chữ viết tắt chuẩn ngành.

## 1. Sản phẩm, vai trò và thiết kế

| Viết tắt | Tên đầy đủ | Nghĩa và cách hiểu trong dự án |
|---|---|---|
| CMS | Content Management System | Hệ thống quản lý nội dung: tạo, sửa, quản lý phiên bản, duyệt và xuất bản nội dung. |
| SaaS | Software as a Service | Phần mềm cung cấp dưới dạng dịch vụ. SaaS đa tenant trong OI-02 là mô hình phục vụ nhiều khách hàng với ranh giới dữ liệu riêng; chưa thuộc baseline hiện tại. |
| CRM | Customer Relationship Management | Hệ thống quản lý quan hệ khách hàng, ví dụ thông tin khách hàng và hoạt động chăm sóc; nằm ngoài phạm vi CMS hiện tại. |
| BA | Business Analyst | Chuyên viên phân tích nghiệp vụ; làm rõ loại nội dung, trường dữ liệu, quy tắc và tiêu chí nghiệm thu cùng chủ nghiệp vụ. |
| UI | User Interface | Giao diện người dùng, ví dụ màn hình soạn bài, nút gửi duyệt và biểu mẫu nhập liệu. |
| UX | User Experience | Trải nghiệm người dùng: mức dễ hiểu, dễ thao tác và hiệu quả của luồng công việc. UI/UX thường chỉ chung công việc thiết kế giao diện và trải nghiệm. |
| SDD | Thường là Software Design Description hoặc Software Design Document | Tài liệu thiết kế phần mềm. Trong bộ tài liệu, đây là bước nhận bàn giao yêu cầu để thiết kế chi tiết; chưa quy định chính thức cách mở rộng tên SDD. |
| Ops | Operations | Bộ phận vận hành: theo dõi hệ thống, xử lý sự cố, sao lưu và khôi phục. Đây là cách viết rút gọn. |
| Prod | Production | Cách viết rút gọn của môi trường vận hành chính thức, nơi phục vụ người dùng và dữ liệu thật. |

## 2. Yêu cầu, kiểm thử và mã tham chiếu

| Viết tắt / mã | Tên đầy đủ / ý nghĩa | Cách dùng trong bộ tài liệu |
|---|---|---|
| OI | Open Issue | Vấn đề còn mở, cần người có trách nhiệm xác nhận. Ví dụ `OI-03` là vấn đề về SSO, MFA và quản lý phiên. `OI3.md` là tên file giải đáp tương ứng. |
| UC | Use Case | Ca sử dụng: mô tả mục tiêu của người dùng, điều kiện, luồng xử lý và kết quả. Ví dụ `UC-IAM-01` là ca đăng nhập bằng tài khoản doanh nghiệp. |
| BR | Business Rule | Quy tắc nghiệp vụ phải tuân thủ khi xử lý. Ví dụ `BR-COM-02` tách xác thực doanh nghiệp khỏi quyền CMS. |
| AC | Acceptance Criterion / Acceptance Criteria | Tiêu chí nghiệm thu, dùng để kiểm chứng hành vi. Ví dụ `AC-IAM-01-01` là tiêu chí thứ 01 của `UC-IAM-01`. |
| COM | Common | Phần quy tắc dùng chung nhiều domain; xuất hiện trong mã `BR-COM-xx` tại `common-rules.md`. |
| E2E | End-to-End | Xuyên suốt từ đầu đến cuối; ví dụ người soạn → người duyệt → người xuất bản → người đọc thực tế. `E2E-01` là mã kịch bản kiểm chứng xuyên suốt. |
| UAT | User Acceptance Testing | Kiểm thử chấp nhận bởi người dùng hoặc đại diện nghiệp vụ. Ma trận UAT xác định các thiết bị, trình duyệt và tình huống cần kiểm chứng. |
| SRC | Source | Tiền tố mã nguồn tham chiếu trong `sources.md`, ví dụ `SRC-01`; không phải mã chức năng. |

**Cách đọc mã:** `UC-WF-03` = ca sử dụng số 03 thuộc domain WF; `BR-WF-03` = quy tắc nghiệp vụ mang mã đó trong domain WF; `AC-WF-03-01` = tiêu chí nghiệm thu số 01 của `UC-WF-03`. Khi lộ trình viết `WF-03`, đó là cách gọi ngắn của use case. `LOC-02/03/04` là ba mã `LOC-02`, `LOC-03`, `LOC-04`.

## 3. Mã 13 nhóm nghiệp vụ (domain)

| Mã | Tên tiếng Anh tương ứng | Nội dung phụ trách | Tài liệu |
|---|---|---|---|
| IAM | Identity and Access Management | Danh tính, đăng nhập, cấp và thu hồi quyền theo phạm vi. | [Danh tính và phân quyền](domains/01-identity-access.md) |
| ADM | Administration | Quản trị workspace, thông báo và tích hợp/nhập dữ liệu. | [Quản trị và tích hợp](domains/02-administration.md) |
| GOV | Governance | Kiểm toán, chủ sở hữu nội dung, lưu giữ và xóa dữ liệu. | [Quản trị nội dung và kiểm toán](domains/03-governance.md) |
| SITE | Site and Content Delivery | Site, cấu trúc trang và kênh phục vụ nội dung cho người đọc. | [Site và phân phối](domains/04-site-delivery.md) |
| LOC | Localization | Ngôn ngữ, locale, bản dịch và quy tắc dự phòng. | [Ngôn ngữ và bản dịch](domains/05-localization.md) |
| MDL | Content Model | Mô hình nội dung: loại nội dung, trường và quan hệ dữ liệu. | [Mô hình nội dung](domains/06-content-model.md) |
| TAX | Taxonomy | Hệ thống phân loại nội dung bằng danh mục và thẻ. | [Phân loại nội dung](domains/07-taxonomy.md) |
| AST | Assets | Tài nguyên số như ảnh, video, tài liệu và quyền sử dụng tệp. | [Tài nguyên số](domains/08-assets.md) |
| CNT | Content | Biên tập, chỉnh sửa và xem trước nội dung. | [Biên tập nội dung](domains/09-content.md) |
| VER | Versioning | Lịch sử phiên bản, so sánh và khôi phục phiên bản. | [Phiên bản nội dung](domains/10-versioning.md) |
| SRCH | Search | Tìm kiếm nội dung và tài nguyên trong phạm vi được phép. | [Tìm kiếm](domains/11-search.md) |
| WF | Workflow | Quy trình gửi duyệt, phê duyệt, trả lại và thu hồi duyệt. | [Phê duyệt nội dung](domains/12-workflow.md) |
| PUB | Publishing | Xuất bản, lập lịch, gỡ nội dung và theo dõi kết quả phân phối. | [Xuất bản](domains/13-publishing.md) |

## 4. Danh tính, tích hợp và định dạng

| Viết tắt | Tên đầy đủ | Nghĩa và ví dụ trong dự án |
|---|---|---|
| SSO | Single Sign-On | Đăng nhập một lần qua hệ thống danh tính doanh nghiệp. Đăng nhập thành công vẫn cần quyền CMS phù hợp để sửa, duyệt hoặc xuất bản. |
| MFA | Multi-Factor Authentication | Xác thực đa yếu tố, ví dụ mật khẩu kết hợp ứng dụng xác thực. Không đồng nghĩa với việc chỉ nhập hai mật khẩu. |
| IdP | Identity Provider | Nhà cung cấp danh tính, thực hiện xác thực người dùng cho CMS. Cần chốt cả cơ chế nhận biết thay đổi/thu hồi quyền từ IdP. |
| API | Application Programming Interface | Giao diện để các phần mềm trao đổi dữ liệu hoặc gọi chức năng, ví dụ kênh đọc lấy nội dung từ CMS. |
| REST | Representational State Transfer | Kiểu kiến trúc thường dùng để thiết kế API theo tài nguyên. Tài liệu có nhắc REST API như một phương án, chưa chốt công nghệ. |
| URL | Uniform Resource Locator | Địa chỉ truy cập tài nguyên trên web, ví dụ đường dẫn một bài viết. Slug thường là phần tên dễ đọc trong URL. |
| MIME | Multipurpose Internet Mail Extensions | Cơ sở của hệ thống kiểu nội dung như `image/png`, `application/pdf`. Khi tải tệp, cần kiểm tra nội dung thực tế thay vì chỉ tin kiểu MIME do client khai báo. |
| JPEG | Joint Photographic Experts Group | Tên nhóm đặt chuẩn, thường dùng để gọi định dạng ảnh JPEG, phổ biến với ảnh chụp. `.jpg` và `.jpeg` là các phần mở rộng thường gặp. |
| PNG | Portable Network Graphics | Định dạng ảnh nén không mất dữ liệu, hỗ trợ nền trong suốt. |
| PDF | Portable Document Format | Định dạng tài liệu giữ bố cục; việc cho phép tải lên còn phụ thuộc chính sách tệp được xác nhận. |
| SEO | Search Engine Optimization | Tối ưu khả năng được công cụ tìm kiếm thu thập và hiển thị, ví dụ tiêu đề và mô tả trang. |
| OWASP | Open Worldwide Application Security Project | Dự án/cộng đồng bảo mật ứng dụng được bộ tài liệu dẫn làm nguồn cho phân quyền, tải tệp và ghi nhật ký. |
| GDPR | General Data Protection Regulation | Tên quy định bảo vệ dữ liệu của Liên minh châu Âu, được nhắc trong phần lưu giữ dữ liệu. Việc nhắc tên chưa xác định quy định này áp dụng cho dự án. |

## 5. Chỉ tiêu vận hành và phục hồi

| Viết tắt | Tên đầy đủ | Ý nghĩa | Ví dụ minh họa, không phải cam kết |
|---|---|---|---|
| SLA | Service Level Agreement | Thỏa thuận mức dịch vụ giữa các bên, có thể bao gồm độ sẵn sàng, thời gian phản hồi hoặc độ trễ phân phối. Cần thống nhất điều kiện và cách đo. | Nếu thỏa thuận xuất bản hoàn tất trong một khoảng thời gian, phải xác định thời điểm bắt đầu và bằng chứng đích đã phục vụ đúng bản. |
| RPO | Recovery Point Objective | Mục tiêu về điểm phục hồi: khoảng mất dữ liệu tối đa chấp nhận được, biểu diễn bằng thời gian. | RPO 15 phút nghĩa là mục tiêu giới hạn mất dữ liệu trong khoảng 15 phút trước sự cố. |
| RTO | Recovery Time Objective | Mục tiêu thời gian khôi phục dịch vụ, tính theo mốc bắt đầu/kết thúc được thống nhất. | RTO 2 giờ nghĩa là mục tiêu khôi phục dịch vụ trong 2 giờ theo phạm vi và mốc đo đã chốt. |

**Dễ nhầm:** RPO trả lời “có thể mất dữ liệu của khoảng thời gian bao lâu?”, RTO trả lời “cần phục hồi dịch vụ trong bao lâu?”. Có backup chưa chứng minh đạt hai mục tiêu; cần diễn tập và đo kết quả theo [OI-13](giai_dap_OI/OI13.md).

## 6. Mã giai đoạn P0–P4

P0–P4 là mã **giai đoạn triển khai** trong dự án, không phải số sprint, thời lượng hoặc mức độ nghiêm trọng của lỗi.

| Mã | Tên giai đoạn | Kết quả chính |
|---|---|---|
| P0 | Nền tảng kiểm soát | Có phạm vi làm việc, đăng nhập, phân quyền, site/locale tối thiểu và audit. |
| P1 | Biên tập có phiên bản | Tạo, sửa, tìm kiếm, xem trước và lưu lịch sử nội dung. |
| P2 | Phê duyệt và phát hành thực tế | Hoàn thành luồng từ soạn bài đến người đọc, có kiểm chứng phân phối và phục hồi. |
| P3 | Mở rộng biên tập doanh nghiệp | Mở rộng ngôn ngữ, quy trình và trải nghiệm website theo nhu cầu. |
| P4 | Tích hợp và quản trị dữ liệu nâng cao | Tích hợp mở rộng, migration, phát hành nhiều đích và quản lý lưu giữ nâng cao. |

Ví dụ `GOV-P4` chỉ nhóm năng lực GOV dự kiến ở P4, không phải mã một use case. Các năng lực có thể chuyển sớm nếu bắt buộc cho lần vận hành đầu tiên; đối chiếu [Lộ trình](implementation-roadmap.md).

## 7. Thuật ngữ tiếng Anh thường gặp (không phải chữ viết tắt)

| Thuật ngữ | Giải thích trong ngữ cảnh CMS |
|---|---|
| Baseline | Phương án cơ sở dùng để phân tích và đối chiếu; baseline đề xuất chưa phải quyết định đã phê duyệt. |
| Domain | Nhóm nghiệp vụ có liên quan, ví dụ phân quyền hoặc xuất bản; không mặc định tương ứng một microservice. |
| Workspace / Site / Tenant | Workspace là không gian làm việc; site là website/kênh nội dung được quản lý; tenant là ranh giới khách hàng/tổ chức trong mô hình đa tenant. Nhiều site chưa đồng nghĩa nhiều tenant. |
| Headless CMS | CMS cung cấp nội dung qua giao diện tích hợp, tách phần quản trị nội dung khỏi phần hiển thị cho người đọc. |
| Locale / Localization | Locale xác định ngôn ngữ và có thể cả vùng, ví dụ `vi-VN`; localization là việc điều chỉnh nội dung cho ngôn ngữ/vùng tương ứng. |
| Fallback | Dùng nội dung thay thế khi locale yêu cầu thiếu nội dung; trong baseline không được lấy bản nháp để hiển thị công khai. |
| Maker-checker | Phân tách người tạo/sửa và người duyệt; người tham gia soạn phiên bản gửi duyệt không tự duyệt phiên bản đó. |
| Authentication / Authorization | Xác thực trả lời “bạn là ai?”; phân quyền trả lời “bạn được làm gì, trong phạm vi nào?”. |
| Audit / Audit trail | Nhật ký/dấu vết kiểm toán ghi nhận ai đã làm gì, với đối tượng nào và khi nào. |
| Cache / Indexing / Latency | Cache là bộ nhớ đệm; indexing là tạo/cập nhật chỉ mục tìm kiếm; latency là độ trễ. Các bước này có thể khiến người đọc chưa thấy bản mới ngay. |
| Backup / Restore / Restore drill | Sao lưu / khôi phục / diễn tập khôi phục để chứng minh bản sao có thể sử dụng và đạt mục tiêu đã chốt. |
| Retention / Legal hold | Retention là chính sách lưu giữ; legal hold là yêu cầu giữ lại dữ liệu, ngăn xóa trong phạm vi bị giữ. |
| Migration | Di chuyển dữ liệu từ hệ thống/nguồn cũ sang CMS, có đối chiếu và xử lý lỗi; baseline nhập thành bản nháp. |
| Go-live / Production | Go-live là thời điểm đưa vào vận hành chính thức; production là môi trường vận hành chính thức. |
| Completion gate | Điều kiện phải đạt để kết thúc một giai đoạn triển khai. |
| Given / When / Then | Cấu trúc viết AC: điều kiện ban đầu / hành động xảy ra / kết quả cần quan sát. |
| Detail Design | Thiết kế chi tiết: cụ thể hóa yêu cầu thành cấu trúc dữ liệu, hợp đồng API và cơ chế xử lý. |
| Webhook | Cơ chế gửi thông báo sự kiện tới địa chỉ tích hợp đã cấu hình, thường qua yêu cầu HTTP. |
| LR / TD trong Mermaid | Ký hiệu hướng sơ đồ: `LR` = Left to Right (trái sang phải); `TD` = Top Down (trên xuống dưới). Không phải thuật ngữ nghiệp vụ. |
