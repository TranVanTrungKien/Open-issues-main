# Thứ tự triển khai Enterprise CMS

**Trạng thái:** Baseline đề xuất v0.1; OI-01, OI-02 và OI-03 đã được chốt. **Phạm vi:** 13 domain, 66 use case, 202 AC riêng cho use case.
[Mục lục](README.md) · [Quy tắc chung](common-rules.md) · [Trạng thái](state-model.md) · [Open Issues](open-issues.md)

## Cách đọc thứ tự

P0–P4 là giai đoạn triển khai, không phải thời lượng hoặc số sprint.
Triển khai theo lát cắt nghiệp vụ. Không cần làm toàn bộ một domain rồi mới chuyển sang domain kế tiếp.
Phần nền của Site/Localization/Governance/Administration xuất hiện sớm; năng lực nâng cao của cùng domain xuất hiện sau.
Versioning đi cùng Content. Audit được ghi từ thay đổi đầu tiên. Tìm kiếm nội bộ cơ bản nằm ở P1.
Tài liệu chưa gán ngày hoàn thành vì chưa có đội ngũ, nền tảng, số lượng dữ liệu và cam kết vận hành.
Theo quyết định OI-01, website công khai là kênh chính của phiên bản đầu. P0 phải giữ ranh giới giữa lõi nội dung/quy trình và bộ phân phối; intranet và Headless API là mở rộng sau, không phải tiêu chí nghiệm thu của P0–P2.
Theo quyết định OI-02, P0 thiết lập ranh giới cô lập mặc định theo site và mô hình cấp quyền chia sẻ có audit; P1 áp dụng mô hình đó cho nội dung/tài nguyên, P2 kiểm tra lại quyền chia sẻ khi duyệt và phát hành.
Theo [quyết định OI-03](open-issues.md#oi-03), P0 phải triển khai SSO doanh nghiệp + MFA + RBAC theo phạm vi + đồng bộ nhóm + quản lý phiên và thu hồi quyền + tài khoản khẩn cấp được kiểm soát. Quyết định giải pháp đã chốt không đồng nghĩa hoàn tất triển khai; cấu hình cụ thể và bằng chứng kiểm chứng là điều kiện qua gate P0.

| Giai đoạn | Số UC | Kết quả |
|---|---:|---|
| P0 — Nền tảng kiểm soát | 9 | Tạo được phạm vi làm việc an toàn, phân quyền và ghi audit. |
| P1 — Biên tập có phiên bản | 14 | Tạo, sửa, tìm và xem trước nội dung mà không có rủi ro ghi đè lịch sử. |
| P2 — Phê duyệt và phát hành thực tế | 16 | Hoàn tất lát cắt người soạn → người duyệt → người xuất bản → người đọc. |
| P3 — Mở rộng biên tập doanh nghiệp | 19 | Mở rộng ngôn ngữ, tổ chức biên tập và trải nghiệm website theo nhu cầu đã chốt. |
| P4 — Tích hợp và quản trị dữ liệu nâng cao | 8 | Kết nối hệ thống khác, di chuyển dữ liệu, phát hành nhiều đích và xử lý vòng đời lưu giữ. |

## P0 — Nền tảng kiểm soát

**Mục tiêu:** Tạo được phạm vi làm việc an toàn, phân quyền và ghi audit.
**Thứ tự trong giai đoạn:** ADM-01 → IAM-01/02/03 → SITE-01 + LOC-01 → IAM-04/05. GOV-01 đi cùng từ thay đổi đầu tiên.

| Use case | Mục tiêu |
|---|---|
| [UC-IAM-01](domains/01-identity-access.md#uc-iam-01) | Đăng nhập bằng tài khoản doanh nghiệp |
| [UC-IAM-02](domains/01-identity-access.md#uc-iam-02) | Đăng xuất và kết thúc phiên CMS |
| [UC-IAM-03](domains/01-identity-access.md#uc-iam-03) | Tiếp nhận người dùng vào CMS |
| [UC-IAM-04](domains/01-identity-access.md#uc-iam-04) | Thiết lập và cấp quyền theo phạm vi |
| [UC-IAM-05](domains/01-identity-access.md#uc-iam-05) | Thu hồi quyền hoặc đình chỉ người dùng |
| [UC-ADM-01](domains/02-administration.md#uc-adm-01) | Thiết lập không gian làm việc |
| [UC-GOV-01](domains/03-governance.md#uc-gov-01) | Tra cứu và xuất dấu vết kiểm toán |
| [UC-SITE-01](domains/04-site-delivery.md#uc-site-01) | Khởi tạo site và đích phân phối |
| [UC-LOC-01](domains/05-localization.md#uc-loc-01) | Thiết lập ngôn ngữ và quy tắc dự phòng |

**Completion Gate P0:** Đăng nhập qua SSO và đáp ứng MFA; RBAC theo phạm vi chặn truy cập ngoài quyền; đồng bộ nhóm cập nhật đúng quyền và không tự mở rộng quyền khi lỗi; người site A không đọc/sửa dữ liệu site B nếu không có quyền rõ ràng; quyền chia sẻ nêu site nguồn, site đích và hành động; phiên hết hạn/đăng xuất/vô hiệu hóa không dùng lại được; quyền thu hồi bị chặn ở hành động tiếp theo, kể cả phiên đang mở; khôi phục quản trị bằng tài khoản khẩn cấp có kiểm soát khi SSO không khả dụng; sự kiện nhạy cảm có audit và sử dụng khẩn cấp có cảnh báo, rà soát.
**Cấu hình phải xác nhận trước gate P0:** Nhà cung cấp/cấu hình SSO và MFA, thời hạn phiên, nguồn/cơ chế/tần suất đồng bộ, ánh xạ nhóm–vai trò–phạm vi và quy trình tài khoản khẩn cấp theo OI-03.
**Điểm dừng:** Chưa đạt gate thì ghi rõ lỗi chặn, người xử lý và kiểm tra lại. Không tự thêm chức năng ngoài phạm vi để thay thế tiêu chí chưa đạt.

## P1 — Biên tập có phiên bản

**Mục tiêu:** Tạo, sửa, tìm và xem trước nội dung mà không có rủi ro ghi đè lịch sử.
**Thứ tự trong giai đoạn:** MDL-01 + TAX-01 + AST-01/02 → CNT-01/02/04 cùng VER-01/02 → TAX-02 + AST-05 → SRCH-01/02 + GOV-02.

| Use case | Mục tiêu |
|---|---|
| [UC-GOV-02](domains/03-governance.md#uc-gov-02) | Bàn giao chủ sở hữu nội dung |
| [UC-MDL-01](domains/06-content-model.md#uc-mdl-01) | Định nghĩa và kích hoạt loại nội dung |
| [UC-TAX-01](domains/07-taxonomy.md#uc-tax-01) | Thiết lập danh mục và thẻ |
| [UC-TAX-02](domains/07-taxonomy.md#uc-tax-02) | Phân loại một nội dung |
| [UC-AST-01](domains/08-assets.md#uc-ast-01) | Tải tài nguyên số lên kho |
| [UC-AST-02](domains/08-assets.md#uc-ast-02) | Mô tả và tổ chức tài nguyên |
| [UC-AST-05](domains/08-assets.md#uc-ast-05) | Lưu trữ tài nguyên không còn dùng mới |
| [UC-CNT-01](domains/09-content.md#uc-cnt-01) | Tạo nội dung nháp |
| [UC-CNT-02](domains/09-content.md#uc-cnt-02) | Chỉnh sửa nội dung mà không ghi đè thay đổi khác |
| [UC-CNT-04](domains/09-content.md#uc-cnt-04) | Xem trước một phiên bản nội dung |
| [UC-VER-01](domains/10-versioning.md#uc-ver-01) | Tra cứu lịch sử phiên bản |
| [UC-VER-02](domains/10-versioning.md#uc-ver-02) | So sánh hai phiên bản |
| [UC-SRCH-01](domains/11-search.md#uc-srch-01) | Tìm nội dung để biên tập hoặc xử lý |
| [UC-SRCH-02](domains/11-search.md#uc-srch-02) | Tìm tài nguyên để tái sử dụng |

**Completion Gate P1:** Tạo được một loại nội dung thực tế; gán tài nguyên hợp lệ, gồm tài nguyên chéo site được chia sẻ rõ ràng; đối tượng không được chia sẻ vẫn bị cô lập; mở lại bản đã lưu; phát hiện lưu đồng thời; xem trước không lộ ra công khai.
**Điểm dừng:** Chưa đạt gate thì ghi rõ lỗi chặn, người xử lý và kiểm tra lại. Không tự thêm chức năng ngoài phạm vi để thay thế tiêu chí chưa đạt.

## P2 — Phê duyệt và phát hành thực tế

**Mục tiêu:** Hoàn tất lát cắt người soạn → người duyệt → người xuất bản → người đọc.
**Thứ tự trong giai đoạn:** AST-04 → WF-01/02/03/04/05/07 + ADM-02 → PUB-01 và SITE-05 cùng lát cắt → PUB-02/03/04/07 → VER-03 + CNT-05.

| Use case | Mục tiêu |
|---|---|
| [UC-ADM-02](domains/02-administration.md#uc-adm-02) | Nhận và xử lý thông báo công việc |
| [UC-SITE-05](domains/04-site-delivery.md#uc-site-05) | Đọc nội dung đã được phát hành |
| [UC-AST-04](domains/08-assets.md#uc-ast-04) | Kiểm tra nơi sử dụng tài nguyên |
| [UC-CNT-05](domains/09-content.md#uc-cnt-05) | Lưu trữ nội dung không còn vận hành |
| [UC-VER-03](domains/10-versioning.md#uc-ver-03) | Khôi phục từ một phiên bản cũ |
| [UC-WF-01](domains/12-workflow.md#uc-wf-01) | Thiết lập quy trình phê duyệt nội dung |
| [UC-WF-02](domains/12-workflow.md#uc-wf-02) | Gửi một phiên bản để phê duyệt |
| [UC-WF-03](domains/12-workflow.md#uc-wf-03) | Phê duyệt nội dung được giao |
| [UC-WF-04](domains/12-workflow.md#uc-wf-04) | Yêu cầu sửa lại hoặc từ chối nội dung |
| [UC-WF-05](domains/12-workflow.md#uc-wf-05) | Rút yêu cầu đang chờ duyệt |
| [UC-WF-07](domains/12-workflow.md#uc-wf-07) | Thu hồi phê duyệt không còn hợp lệ |
| [UC-PUB-01](domains/13-publishing.md#uc-pub-01) | Xuất bản ngay một phiên bản được duyệt |
| [UC-PUB-02](domains/13-publishing.md#uc-pub-02) | Lập lịch xuất bản hoặc gỡ nội dung |
| [UC-PUB-03](domains/13-publishing.md#uc-pub-03) | Hủy lịch chưa thực thi |
| [UC-PUB-04](domains/13-publishing.md#uc-pub-04) | Gỡ nội dung khỏi kênh phân phối |
| [UC-PUB-07](domains/13-publishing.md#uc-pub-07) | Theo dõi và xử lý lần phát hành thất bại |

**Completion Gate P2:** Đúng bản được duyệt đến được website công khai; chia sẻ chéo site được kiểm tra lại trước phát hành; thu hồi chia sẻ chặn sử dụng mới mà không làm hỏng bản công khai âm thầm; lịch và hủy an toàn; gỡ được xác minh; thử lại không trùng; diễn tập phục hồi và kiểm thử bảo mật đạt.
**Điểm dừng:** Chưa đạt gate thì ghi rõ lỗi chặn, người xử lý và kiểm tra lại. Không tự thêm chức năng ngoài phạm vi để thay thế tiêu chí chưa đạt.

## P3 — Mở rộng biên tập doanh nghiệp

**Mục tiêu:** Mở rộng ngôn ngữ, tổ chức biên tập và trải nghiệm website theo nhu cầu đã chốt.
**Thứ tự trong giai đoạn:** Ưu tiên theo nhu cầu: thay đổi mô hình và dữ liệu → phân loại/tài nguyên → ủy quyền/phân công → dịch → trang/điều hướng → tìm kiếm → khôi phục bản công khai.

| Use case | Mục tiêu |
|---|---|
| [UC-IAM-06](domains/01-identity-access.md#uc-iam-06) | Ủy quyền phê duyệt có thời hạn |
| [UC-SITE-02](domains/04-site-delivery.md#uc-site-02) | Soạn cấu trúc trang bằng mẫu được phép |
| [UC-SITE-03](domains/04-site-delivery.md#uc-site-03) | Quản lý menu và điều hướng |
| [UC-SITE-04](domains/04-site-delivery.md#uc-site-04) | Quản lý địa chỉ trang và thông tin tìm kiếm |
| [UC-LOC-02](domains/05-localization.md#uc-loc-02) | Tạo bản dịch từ một phiên bản nguồn |
| [UC-LOC-03](domains/05-localization.md#uc-loc-03) | Gửi bản dịch để duyệt và phát hành |
| [UC-LOC-04](domains/05-localization.md#uc-loc-04) | Rà soát bản dịch khi nguồn thay đổi |
| [UC-MDL-02](domains/06-content-model.md#uc-mdl-02) | Thay đổi mô hình đang được sử dụng |
| [UC-MDL-03](domains/06-content-model.md#uc-mdl-03) | Chuyển nội dung sang mô hình mới |
| [UC-MDL-04](domains/06-content-model.md#uc-mdl-04) | Ngừng sử dụng một loại nội dung |
| [UC-TAX-03](domains/07-taxonomy.md#uc-tax-03) | Hợp nhất danh mục hoặc thẻ trùng |
| [UC-TAX-04](domains/07-taxonomy.md#uc-tax-04) | Ngừng sử dụng một mục phân loại |
| [UC-AST-03](domains/08-assets.md#uc-ast-03) | Thay thế tệp bằng phiên bản mới |
| [UC-CNT-03](domains/09-content.md#uc-cnt-03) | Nhân bản nội dung để tái sử dụng |
| [UC-CNT-06](domains/09-content.md#uc-cnt-06) | Khôi phục nội dung từ kho lưu trữ |
| [UC-SRCH-03](domains/11-search.md#uc-srch-03) | Tìm kiếm trên kênh người đọc |
| [UC-SRCH-04](domains/11-search.md#uc-srch-04) | Lưu và chia sẻ bộ điều kiện tìm kiếm |
| [UC-WF-06](domains/12-workflow.md#uc-wf-06) | Phân công lại người phê duyệt |
| [UC-PUB-05](domains/13-publishing.md#uc-pub-05) | Khôi phục bản công khai trước đó có kiểm soát |

**Completion Gate P3:** Không làm mất dữ liệu cũ; không thay bản công khai ngoài quy trình; locale, phân quyền và nội dung dự phòng hoạt động đúng.
**Điểm dừng:** Chưa đạt gate thì ghi rõ lỗi chặn, người xử lý và kiểm tra lại. Không tự thêm chức năng ngoài phạm vi để thay thế tiêu chí chưa đạt.

## P4 — Tích hợp và quản trị dữ liệu nâng cao

**Mục tiêu:** Kết nối hệ thống khác, di chuyển dữ liệu, phát hành nhiều đích và xử lý vòng đời lưu giữ.
**Thứ tự trong giai đoạn:** ADM-03 → ADM-04; MDL/CNT đã ổn định → ADM-05/06; GOV-03 → GOV-04 → GOV-05; PUB-06 sau khi phát hành đơn đích ổn định.
Headless API có thể được xác định phạm vi tại giai đoạn mở rộng này, nhưng không tự động được coi là đã nằm trong P4 nếu chưa có hợp đồng API, mô hình quyền và tiêu chí nghiệm thu được phê duyệt. Kênh intranet cũng cần một phạm vi triển khai riêng.

| Use case | Mục tiêu |
|---|---|
| [UC-ADM-03](domains/02-administration.md#uc-adm-03) | Kết nối hệ thống bên ngoài |
| [UC-ADM-04](domains/02-administration.md#uc-adm-04) | Đăng ký và giám sát webhook |
| [UC-ADM-05](domains/02-administration.md#uc-adm-05) | Nhập nội dung từ nguồn khác |
| [UC-ADM-06](domains/02-administration.md#uc-adm-06) | Xuất dữ liệu theo phạm vi được phép |
| [UC-GOV-03](domains/03-governance.md#uc-gov-03) | Thiết lập chính sách lưu giữ |
| [UC-GOV-04](domains/03-governance.md#uc-gov-04) | Đặt hoặc gỡ yêu cầu giữ lại dữ liệu |
| [UC-GOV-05](domains/03-governance.md#uc-gov-05) | Phê duyệt và thực hiện xóa vĩnh viễn |
| [UC-PUB-06](domains/13-publishing.md#uc-pub-06) | Phát hành một đợt tới nhiều đích |

**Completion Gate P4:** Nhập có đối soát; xuất không vượt quyền; webhook thử lại an toàn; nhiều đích có trạng thái từng phần; hold chặn xóa.
**Điểm dừng:** Chưa đạt gate thì ghi rõ lỗi chặn, người xử lý và kiểm tra lại. Không tự thêm chức năng ngoài phạm vi để thay thế tiêu chí chưa đạt.

## Lát cắt nghiệm thu production đầu tiên

P0 + P1 + P2 tương ứng 39 use case trong baseline hiện tại.
Con số này là phạm vi đề xuất, không phải một chuẩn bắt buộc cho mọi doanh nghiệp.

| Mã kiểm thử E2E | Kịch bản cần chứng minh | Use case chính |
|---|---|---|
| E2E-01 | Biên tập viên tạo bài, người khác duyệt, người xuất bản phát hành, người đọc nhận đúng bản | [UC-CNT-01](domains/09-content.md#uc-cnt-01), [UC-WF-03](domains/12-workflow.md#uc-wf-03), [UC-PUB-01](domains/13-publishing.md#uc-pub-01), [UC-SITE-05](domains/04-site-delivery.md#uc-site-05) |
| E2E-02 | Sửa bản đang công khai không làm đổi bản người đọc đang nhận | [UC-CNT-02](domains/09-content.md#uc-cnt-02), [UC-VER-01](domains/10-versioning.md#uc-ver-01) |
| E2E-03 | Người tạo/sửa không tự duyệt, kể cả có vai trò duyệt | [UC-IAM-04](domains/01-identity-access.md#uc-iam-04), [UC-WF-03](domains/12-workflow.md#uc-wf-03) |
| E2E-04 | Lịch gắn phiên bản, quyền mất hoặc duyệt bị thu hồi thì không chạy | [UC-PUB-02](domains/13-publishing.md#uc-pub-02), [UC-IAM-05](domains/01-identity-access.md#uc-iam-05), [UC-WF-07](domains/12-workflow.md#uc-wf-07) |
| E2E-05 | Hủy cạnh tranh với thực thi có kết quả rõ; tác vụ cũ không hồi sinh bài đã gỡ | [UC-PUB-03](domains/13-publishing.md#uc-pub-03), [UC-PUB-04](domains/13-publishing.md#uc-pub-04), [UC-PUB-07](domains/13-publishing.md#uc-pub-07) |
| E2E-06 | Người ngoài phạm vi và không được chia sẻ không thấy bài, số lượng, thumbnail, preview, tài nguyên riêng | [UC-SRCH-01](domains/11-search.md#uc-srch-01), [UC-SRCH-02](domains/11-search.md#uc-srch-02), [UC-CNT-04](domains/09-content.md#uc-cnt-04) |
| E2E-07 | Nội dung có tài nguyên đang cách ly không được phát hành | [UC-AST-01](domains/08-assets.md#uc-ast-01), [UC-WF-02](domains/12-workflow.md#uc-wf-02), [UC-PUB-01](domains/13-publishing.md#uc-pub-01) |
| E2E-08 | Khôi phục thành nháp, lịch sử cũ còn nguyên, không tự công khai | [UC-VER-03](domains/10-versioning.md#uc-ver-03), [UC-PUB-01](domains/13-publishing.md#uc-pub-01) |
| E2E-09 | Lỗi thông báo không làm mất công việc; lỗi đích không báo thành công giả | [UC-ADM-02](domains/02-administration.md#uc-adm-02), [UC-PUB-07](domains/13-publishing.md#uc-pub-07) |
| E2E-10 | Audit giải thích được quyết định duyệt, phát hành và thu hồi | [UC-GOV-01](domains/03-governance.md#uc-gov-01), [UC-WF-07](domains/12-workflow.md#uc-wf-07) |
| E2E-11 | Site đích dùng được nội dung/tài nguyên được chia sẻ nhưng không sửa nguồn; thu hồi chặn dùng mới và không phá bản đang công khai âm thầm | [UC-IAM-04](domains/01-identity-access.md#uc-iam-04), [UC-AST-04](domains/08-assets.md#uc-ast-04), [UC-CNT-01](domains/09-content.md#uc-cnt-01), [UC-PUB-01](domains/13-publishing.md#uc-pub-01) |
| E2E-12 | Chưa đáp ứng MFA không mở phiên; nhóm đồng bộ chỉ cấp vai trò/phạm vi đã ánh xạ; gỡ nhóm hoặc thu hồi quyền chặn hành động tiếp theo bằng phiên cũ | [UC-IAM-01](domains/01-identity-access.md#uc-iam-01), [UC-IAM-03](domains/01-identity-access.md#uc-iam-03), [UC-IAM-04](domains/01-identity-access.md#uc-iam-04), [UC-IAM-05](domains/01-identity-access.md#uc-iam-05) |
| E2E-13 | Phiên hết hạn/đăng xuất/vô hiệu hóa bị từ chối; khi SSO lỗi, tài khoản khẩn cấp khôi phục quản trị trong phạm vi/thời gian cho phép, có cảnh báo/audit/rà soát và không tự bỏ qua duyệt | [UC-IAM-01](domains/01-identity-access.md#uc-iam-01), [UC-IAM-02](domains/01-identity-access.md#uc-iam-02), [UC-GOV-01](domains/03-governance.md#uc-gov-01) |

## Điều kiện không được đẩy sang giai đoạn sau

| Nhu cầu thực tế | Điều chỉnh thứ tự |
|---|---|
| Có nhiều ngôn ngữ bắt buộc khi go-live | Đưa LOC-02/03/04 vào trước gate P2 |
| Chính sách bắt buộc nhiều cấp duyệt | Hoàn thiện nhánh nhiều cấp của WF-01/03 trong P2 |
| Dữ liệu thật cần retention/hold trước sử dụng | Đưa GOV-03/04/05 vào trước production theo mức cần thiết |
| Phải thay CMS cũ ngay lúc go-live | Đưa ADM-05/06 và diễn tập migration vào P1/P2 |
| Kênh đầu tiên bắt buộc hệ thống ngoài | Kết nối cần thiết là P0/P2, không đợi module quản trị tích hợp P4 |
| Bắt buộc tạo trang tùy biến khi go-live | Đưa SITE-02/03/04 vào P2 và điều chỉnh phạm vi còn lại |

## Phân biệt việc ghi dữ liệu và màn hình quản trị

Ghi audit từ P0; giao diện lọc nâng cao có thể làm sau.
Ghi phiên bản từ lần lưu đầu P1; không đợi UI so sánh mới bắt đầu ghi.
Ghi site và locale từ đầu; không đợi mở rộng đa ngôn ngữ mới thêm quan hệ.
Kiểm tra phụ thuộc tài nguyên tối thiểu từ P1; màn hình phân tích nơi dùng đầy đủ ở P2.
Thời hạn giữ dữ liệu, backup và diễn tập phục hồi được chốt trước production, dù trang quản trị chính sách có thể ra sau.

## Bàn giao sang SDD

Mỗi use case là đơn vị phân tích hành vi, không mặc định là một ticket hoặc một màn hình.
Base Design chốt actor, quy tắc, trạng thái và kết quả nhìn thấy được.
Detail Design liên kết tới UC/BR/AC rồi chốt API, dữ liệu, phân quyền và xử lý đồng thời.
Test plan bao phủ AC riêng, quy tắc chung và các kịch bản E2E ở trên.
Change Request cập nhật use case và AC hiện hành; không sao chép toàn bộ đặc tả thành nhiều nguồn sự thật.
