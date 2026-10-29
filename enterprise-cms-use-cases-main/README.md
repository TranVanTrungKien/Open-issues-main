# Enterprise CMS — Use Case Pack

**Phiên bản:** 0.1 · **Ngày:** 2026-09-20 · **Trạng thái:** Đề xuất để rà soát.
**Quy mô:** 13 domain, 66 use case và 202 acceptance criteria riêng cho use case.

## Phạm vi

CMS quản lý nội dung cho một doanh nghiệp, nhiều đơn vị và nhiều site.
Theo OI-02, dữ liệu được cô lập theo site mặc định; nội dung và tài nguyên có thể được chia sẻ giữa các site bằng quyền rõ ràng, có phạm vi, có thể thu hồi và có audit.
Phiên bản đầu tiên dùng một locale và **website công khai** làm kênh đọc chính, với một đích phân phối thực tế.
Kiến trúc giữ lõi nội dung và quy trình độc lập với cơ chế phân phối để có thể mở rộng sang intranet và Headless API; hai năng lực này chưa thuộc phạm vi phát hành đầu.
Theo [OI-03](open-issues.md#oi-03), giải pháp danh tính đã chốt gồm SSO doanh nghiệp + MFA + RBAC theo phạm vi + đồng bộ nhóm + quản lý phiên và thu hồi quyền + tài khoản khẩn cấp được kiểm soát; triển khai và kiểm chứng tại P0.
Ngoài các quyết định tại OI-01, OI-02 và OI-03, các baseline còn lại chưa phải yêu cầu nội bộ đã được xác nhận hoặc cam kết một sản phẩm đã có đầy đủ tính năng.
Chưa bao gồm SaaS đa tenant, thanh toán hoặc CRM. Theo OI-01, hiện chưa cần Records Management/Legal Records chuyên dụng vì dự án tập trung vào CMS quản lý và xuất bản nội dung.

## Cách đọc

Đọc [Thứ tự triển khai](implementation-roadmap.md) trước.
Sau đó đọc [Quy tắc chung](common-rules.md) và [Mô hình trạng thái](state-model.md).
Các quyết định chưa được xác nhận nằm ở [Open Issues](open-issues.md).
[Nguồn và giới hạn](sources.md) phân biệt đề xuất nghiệp vụ với tham chiếu công khai.

## Mục lục domain

| Thứ tự bắt đầu | Domain | Giai đoạn đầu | UC | Tài liệu |
|---:|---|---|---:|---|
| 1 | Danh tính và phân quyền | P0 | 6 | [IAM](domains/01-identity-access.md) |
| 2 | Quản trị và tích hợp | P0 | 6 | [ADM](domains/02-administration.md) |
| 3 | Quản trị nội dung và kiểm toán | P0 | 5 | [GOV](domains/03-governance.md) |
| 4 | Site và phân phối nội dung | P0 | 5 | [SITE](domains/04-site-delivery.md) |
| 5 | Ngôn ngữ và bản dịch | P0 | 4 | [LOC](domains/05-localization.md) |
| 6 | Mô hình nội dung | P1 | 4 | [MDL](domains/06-content-model.md) |
| 7 | Phân loại nội dung | P1 | 4 | [TAX](domains/07-taxonomy.md) |
| 8 | Tài nguyên số | P1 | 5 | [AST](domains/08-assets.md) |
| 9 | Biên tập nội dung | P1 | 6 | [CNT](domains/09-content.md) |
| 10 | Phiên bản nội dung | P1 | 3 | [VER](domains/10-versioning.md) |
| 11 | Tìm kiếm | P1 | 4 | [SRCH](domains/11-search.md) |
| 12 | Phê duyệt nội dung | P2 | 7 | [WF](domains/12-workflow.md) |
| 13 | Xuất bản | P2 | 7 | [PUB](domains/13-publishing.md) |

## Cấu trúc một use case

Mỗi use case có mục tiêu, actor, kích hoạt, tiền điều kiện, thông tin đầu vào, luồng chính, ngoại lệ, quy tắc nghiệp vụ, kết quả và AC.
Mã UC/BR/AC ổn định để liên kết với thiết kế và test.
AC dùng Given/When/Then để ghi rõ điều kiện, hành động và kết quả quan sát được.
Luồng chính diễn đạt theo góc nhìn người dùng. Cơ chế triển khai thuộc Detail Design.

## Giai đoạn

| Giai đoạn | Số UC | Trọng tâm |
|---|---:|---|
| P0 | 9 | Nền tảng kiểm soát |
| P1 | 14 | Biên tập có phiên bản |
| P2 | 16 | Phê duyệt và phát hành thực tế |
| P3 | 19 | Mở rộng biên tập doanh nghiệp |
| P4 | 8 | Tích hợp và quản trị dữ liệu nâng cao |

P0–P2 tạo lát cắt vận hành đầu tiên với 39 use case đề xuất.
P3–P4 không phải mặc định làm toàn bộ; ưu tiên theo nhu cầu thực tế.
Năng lực tuân thủ, dịch, migration hoặc tích hợp bắt buộc phải được đưa lên trước production.

## Những điểm đã chỉnh so với bản phân domain ban đầu

Audit và phiên bản cơ bản không để sang giai đoạn sau.
Site và locale tối thiểu được xác định trước nội dung.
Tìm kiếm biên tập cơ bản không đợi đến khi làm công cụ tìm kiếm nâng cao.
Phê duyệt, lịch, trạng thái phân phối và bản đang công khai là những khái niệm riêng.
Không coi mỗi domain là một microservice hoặc một gói việc cần hoàn thành toàn bộ trước khi làm domain khác.

## Định dạng và giới hạn

Mọi file Markdown trong bộ tài liệu dưới 400 dòng.
Liên kết dùng Markdown inline. Sơ đồ dùng Mermaid.
Tài liệu chưa chỉ định stack, API, schema cơ sở dữ liệu, số sprint hay thời gian triển khai.
202 AC là baseline hành vi riêng cho use case, chưa phải bộ kiểm thử đầy đủ hoặc bằng chứng đã kiểm thử.
Trước khi lập trình, cần chốt các Open Issues ảnh hưởng use case và bổ sung chi tiết dữ liệu thực tế.
