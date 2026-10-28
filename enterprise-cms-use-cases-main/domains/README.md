# Giải thích các tài liệu trong `domains`

Thư mục `domains` chứa đặc tả nghiệp vụ của hệ thống quản lý nội dung doanh nghiệp (Enterprise CMS), chia thành 13 domain với tổng cộng 66 use case. **Domain** là một nhóm nghiệp vụ có cùng trách nhiệm, ví dụ phân quyền, biên tập hoặc xuất bản nội dung.

Các file mô tả người dùng cần làm gì, điều kiện thực hiện và kết quả cần đạt. Đây là baseline đề xuất v0.1, chưa phải yêu cầu đã được khách hàng phê duyệt; cũng chưa quy định stack, API hay schema cơ sở dữ liệu.

## Ý nghĩa từng file

| File | Mã domain | Vai trò và nội dung chính |
|---|---|---|
| [01-identity-access.md](01-identity-access.md) | `IAM` | **Danh tính và phân quyền:** xác định ai được vào CMS và được làm gì trong từng phạm vi. Gồm đăng nhập, đăng xuất, tiếp nhận người dùng, cấp/thu hồi quyền, đình chỉ và ủy quyền phê duyệt có thời hạn. |
| [02-administration.md](02-administration.md) | `ADM` | **Quản trị và tích hợp:** thiết lập workspace, nhận thông báo công việc, kết nối hệ thống bên ngoài, giám sát webhook và nhập/xuất dữ liệu theo quyền. |
| [03-governance.md](03-governance.md) | `GOV` | **Quản trị nội dung và kiểm toán:** theo dõi ai đã làm gì, bàn giao chủ sở hữu, thiết lập chính sách lưu giữ, giữ lại dữ liệu và kiểm soát xóa vĩnh viễn. |
| [04-site-delivery.md](04-site-delivery.md) | `SITE` | **Site và phân phối nội dung:** xác định nội dung sẽ xuất hiện ở đâu và được người đọc tiếp cận như thế nào. Gồm site, đích phân phối, mẫu trang, menu, địa chỉ trang, thông tin tìm kiếm và đọc nội dung đã phát hành. |
| [05-localization.md](05-localization.md) | `LOC` | **Ngôn ngữ và bản dịch:** cấu hình ngôn ngữ và quy tắc dự phòng khi thiếu bản dịch; tạo, duyệt, phát hành bản dịch và rà soát khi phiên bản nguồn thay đổi. |
| [06-content-model.md](06-content-model.md) | `MDL` | **Mô hình nội dung:** định nghĩa loại nội dung và các trường dữ liệu. Quản lý việc kích hoạt, thay đổi mô hình đang dùng, chuyển nội dung sang mô hình mới và ngừng sử dụng loại nội dung. |
| [07-taxonomy.md](07-taxonomy.md) | `TAX` | **Phân loại nội dung:** thiết lập danh mục và thẻ, gắn phân loại cho nội dung, hợp nhất mục trùng và ngừng sử dụng mục phân loại. |
| [08-assets.md](08-assets.md) | `AST` | **Tài nguyên số:** quản lý tệp như ảnh và tài liệu đính kèm. Gồm tải lên, mô tả, tổ chức, thay thế phiên bản tệp, kiểm tra nơi sử dụng và lưu trữ tài nguyên không còn dùng mới. |
| [09-content.md](09-content.md) | `CNT` | **Biên tập nội dung:** tạo nháp, chỉnh sửa có kiểm soát xung đột, nhân bản, xem trước, lưu trữ và khôi phục nội dung từ kho lưu trữ. |
| [10-versioning.md](10-versioning.md) | `VER` | **Phiên bản nội dung:** tra cứu lịch sử, so sánh hai phiên bản và khôi phục từ phiên bản cũ để tiếp tục xử lý theo quy trình. |
| [11-search.md](11-search.md) | `SRCH` | **Tìm kiếm:** tìm nội dung phục vụ biên tập, tìm tài nguyên để tái sử dụng, tìm trên kênh người đọc và lưu/chia sẻ bộ điều kiện tìm kiếm. |
| [12-workflow.md](12-workflow.md) | `WF` | **Phê duyệt nội dung:** thiết lập quy trình duyệt, gửi phiên bản để duyệt, phê duyệt, yêu cầu sửa hoặc từ chối, rút yêu cầu, phân công lại và thu hồi phê duyệt. |
| [13-publishing.md](13-publishing.md) | `PUB` | **Xuất bản:** đưa đúng phiên bản đã duyệt tới đích phân phối. Gồm xuất bản ngay, lập/hủy lịch, gỡ nội dung, khôi phục bản công khai, phát hành tới nhiều đích và xử lý lần phát hành thất bại. |

## Cách đọc một file domain

Đầu file có mã domain, giai đoạn bắt đầu và số use case. Mỗi use case thường gồm:

| Thành phần | Ý nghĩa |
|---|---|
| `UC` — Use Case | Tình huống sử dụng cụ thể, có mã để tham chiếu, ví dụ `UC-CNT-01` là tạo nội dung nháp. |
| Triển khai | Giai đoạn dự kiến thực hiện use case, từ P0 đến P4. |
| Actor | Người hoặc vai trò thực hiện nghiệp vụ. |
| Mục tiêu, kích hoạt | Kết quả mong muốn và sự kiện bắt đầu thao tác. |
| Tiền điều kiện, thông tin đầu vào | Những điều phải có và dữ liệu cần cung cấp trước khi thực hiện. |
| Luồng chính | Các bước thực hiện khi mọi điều kiện phù hợp. |
| Luồng thay thế và ngoại lệ | Cách xử lý tình huống khác hoặc lỗi. |
| `BR` — Business Rule | Quy tắc nghiệp vụ cần tuân thủ, ví dụ không tự duyệt phiên bản do mình tạo hoặc sửa. |
| Kết quả thành công, bảo đảm khi thất bại | Trạng thái cần đạt khi hoàn tất và những dữ liệu/trạng thái cần được bảo vệ nếu thất bại. |
| `AC` — Acceptance Criteria | Tiêu chí nghiệm thu theo Given/When/Then: điều kiện ban đầu, hành động và kết quả quan sát được. |
| Use case liên quan / phụ thuộc | Liên kết tới các nghiệp vụ cần đọc cùng hoặc cần có trước. |

## Quan hệ giữa các domain

Ví dụ khi chuẩn bị và phát hành một bài viết: `IAM` kiểm soát quyền; `SITE` và `LOC` xác định site, ngôn ngữ; `MDL` quy định cấu trúc bài; `TAX` cung cấp danh mục/thẻ; `AST` quản lý ảnh đính kèm; `CNT` hỗ trợ soạn thảo; `VER` lưu lịch sử; `WF` xử lý phê duyệt; `PUB` đưa bản được duyệt ra kênh đọc. `SRCH` hỗ trợ tìm kiếm, `GOV` lưu dấu vết và áp dụng chính sách quản trị, còn `ADM` cung cấp cấu hình, thông báo và tích hợp liên quan.

Một số điểm cần phân biệt khi đọc:

- `ADM` quản lý cấu hình và tích hợp hệ thống; `GOV` quản lý trách nhiệm, kiểm toán và vòng đời lưu giữ dữ liệu.
- `MDL` định nghĩa cấu trúc của một loại nội dung; `CNT` xử lý từng nội dung cụ thể theo cấu trúc đó.
- `WF` xác nhận một phiên bản được phép phát hành; `PUB` thực hiện và xác minh việc phát hành. Đã duyệt chưa có nghĩa là đã công khai.
- `VER` khôi phục từ phiên bản nội dung cũ; khôi phục bản đang phục vụ người đọc thuộc `PUB` và phải tuân thủ điều kiện phê duyệt hiện hành.
- `SITE` thiết lập nơi phục vụ nội dung; `PUB` quản lý lần phát hành tới nơi đó.

## Thứ tự đọc và tài liệu liên quan

Nên đọc [lộ trình triển khai](../implementation-roadmap.md), [quy tắc chung](../common-rules.md) và [mô hình trạng thái](../state-model.md) trước khi đi sâu vào từng domain.

Các số `01`–`13` trong tên file thể hiện thứ tự bắt đầu tham khảo theo lộ trình. Mỗi domain có thể chứa use case ở nhiều giai đoạn; không cần hoàn thành toàn bộ một domain mới bắt đầu domain tiếp theo, và mỗi domain không mặc định tương ứng với một microservice.

Các vấn đề cần chốt nằm trong [Open Issues](../open-issues.md); các phương án giải đáp được tập hợp tại [giải đáp Open Issues](../giai_dap_OI/README.md). Xem thêm [README tổng quan](../README.md) và [nguồn, giới hạn tài liệu](../sources.md) để hiểu phạm vi của bộ đặc tả.
