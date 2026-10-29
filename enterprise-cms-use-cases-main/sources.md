# Nguồn và độ tin cậy

[Mục lục](README.md) · [Quy tắc chung](common-rules.md) · [Open Issues](open-issues.md)

## Căn cứ yêu cầu

Căn cứ trực tiếp là danh sách domain Enterprise CMS và yêu cầu viết use case theo thứ tự triển khai trong cuộc trao đổi.
Tìm kiếm tài liệu khả dụng chưa xác định được đặc tả nghiệp vụ CMS có thẩm quyền để chốt các quyết định chi tiết.
Không dùng tài liệu giao diện hoặc nghiệp vụ của dự án khác để suy ra yêu cầu chính thức cho CMS này.
Bộ use case là baseline đề xuất v0.1, cần rà soát với chủ sản phẩm; riêng các quyết định OI-01, OI-02 và OI-03 đã được xác nhận trong cuộc trao đổi và ghi tại [Open Issues](open-issues.md).
Ngày 2026-10-28, người dùng xác nhận giải pháp OI-03: SSO doanh nghiệp + MFA + RBAC theo phạm vi + đồng bộ nhóm + quản lý phiên và thu hồi quyền + tài khoản khẩn cấp được kiểm soát. Đây là quyết định giải pháp, chưa phải bằng chứng triển khai hoặc xác nhận nhà cung cấp/cấu hình cụ thể.

## Cách dùng nguồn công khai

Các nguồn dưới đây được tra cứu ngày 2026-09-20.
Nguồn bảo mật hỗ trợ nguyên tắc thiết kế. Nguồn CMS minh họa những mô hình nghiệp vụ đã được sử dụng.
Chúng không chứng minh CMS đề xuất đã triển khai hoặc đã đáp ứng tiêu chuẩn.
Không chọn Drupal hay Contentful làm nền tảng triển khai trong tài liệu này.
Giới hạn và giấy phép của một sản phẩm cụ thể không được suy ra thành yêu cầu sản phẩm này.

## SRC-01

**Nguồn:** [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html).
**Tham chiếu:** Kiểm tra quyền ở mỗi yêu cầu, tối thiểu quyền và từ chối khi không có cấp quyền phù hợp.
**Áp dụng đề xuất:** IAM, tìm kiếm, xem trước, xuất dữ liệu và tác vụ hẹn giờ.

## SRC-02

**Nguồn:** [OWASP File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html).
**Tham chiếu:** Kiểm tra định dạng, dung lượng, nội dung tệp, quyền tải và vị trí lưu.
**Áp dụng đề xuất:** Kho tài nguyên có trạng thái chờ kiểm tra; tệp chưa đạt không được phát hành.

## SRC-03

**Nguồn:** [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html).
**Tham chiếu:** Ghi sự kiện ứng dụng, bảo vệ log và tránh ghi bí mật nguyên văn.
**Áp dụng đề xuất:** Audit từ P0, tách dấu vết nghiệp vụ với log kỹ thuật.

## SRC-04

**Nguồn:** [Drupal Content Moderation — Overview](https://www.drupal.org/docs/8/core/modules/content-moderation/overview).
**Tham chiếu:** Có thể tồn tại bản đang công khai và bản làm việc đang được rà soát.
**Áp dụng đề xuất:** Không để sửa bản nháp hoặc trả sửa làm thay đổi bản đang công khai.

## SRC-05

**Nguồn:** [Contentful Locale-based Publishing](https://www.contentful.com/help/localization/locale-based-publishing/).
**Tham chiếu:** Mô hình công khai theo locale và xử lý nội dung dự phòng.
**Áp dụng đề xuất:** Locale có quyết định duyệt và phát hành riêng; không lấy bản nháp làm dự phòng.
**Giới hạn:** Không suy ra rằng tính năng lập lịch theo locale trong baseline này sẵn có trên mọi nền tảng hoặc trên Contentful.
