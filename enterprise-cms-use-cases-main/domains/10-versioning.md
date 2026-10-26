# Phiên bản nội dung

**Domain:** `VER` · **Bắt đầu:** P1 · **Số use case:** 3
**Trạng thái:** Baseline đề xuất v0.1 — chưa phải yêu cầu đã được khách hàng phê duyệt.

Quản lý bản lưu bất biến, so sánh và khôi phục thành bản nháp. Là năng lực gắn với Content, không bắt buộc là dịch vụ riêng.

[Mục lục](../README.md) · [Thứ tự triển khai](../implementation-roadmap.md) · [Quy tắc chung](../common-rules.md) · [Trạng thái](../state-model.md) · [Điểm chưa chốt](../open-issues.md)

Mọi use case dùng điều kiện quyền và quy tắc chung. Luồng lỗi không được báo thành công giả.
AC liên quan năng lực triển khai sau được nghiệm thu khi năng lực đó xuất hiện; không bỏ kiểm tra ở giai đoạn tích hợp.

| Use case | Mục tiêu | Giai đoạn |
|---|---|---|
| [UC-VER-01](#uc-ver-01) | Tra cứu lịch sử phiên bản | P1 |
| [UC-VER-02](#uc-ver-02) | So sánh hai phiên bản | P1 |
| [UC-VER-03](#uc-ver-03) | Khôi phục từ một phiên bản cũ | P2 |

## UC-VER-01

**Tra cứu lịch sử phiên bản**

**Triển khai:** P1. **Actor:** Biên tập viên, người duyệt hoặc kiểm toán viên có quyền.
**Mục tiêu:** Xác định được lịch sử và trạng thái của đúng phiên bản nội dung.
**Kích hoạt:** Người dùng yêu cầu thực hiện “tra cứu lịch sử phiên bản”.
**Tiền điều kiện:** Nội dung đã có ít nhất một bản lưu.
**Thông tin đầu vào:** Nội dung; locale; bộ lọc người sửa hoặc thời gian.

**Luồng chính**

1. Mở lịch sử nội dung.
2. Xem người sửa, thời gian và lý do từng bản.
3. Phân biệt bản nháp, bản đã duyệt và bản đang công khai theo đích.
4. Mở bản được phép đọc.

**Luồng thay thế và ngoại lệ**

- E01: Thiếu quyền đọc phiên bản cũ: không hiển thị dữ liệu của bản đó.
- E02: Phiên bản đã được xử lý theo chính sách lưu giữ: hiển thị trạng thái tương ứng, không dựng lại từ suy đoán.

**BR-VER-01:** Bản lưu bất biến bắt đầu từ lần lưu đầu tiên của Content. Không đợi tới lúc xây màn hình lịch sử mới ghi phiên bản.
**Kết quả thành công:** Xác định được lịch sử và trạng thái của đúng phiên bản nội dung.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-VER-01-01/v1:** Given lưu thành công v1 rồi v2; When mở lịch sử; Then thấy hai bản với người và thời điểm tương ứng.
- **AC-VER-01-02/v1:** Given v1 công khai, v2 đang duyệt; When mở lịch sử; Then hai trạng thái được phân biệt.
- **AC-VER-01-03/v1:** Given sửa v2; When mở lại v1; Then v1 không bị đổi.

**Use case liên quan / phụ thuộc:** [UC-CNT-01](09-content.md#uc-cnt-01).

## UC-VER-02

**So sánh hai phiên bản**

**Triển khai:** P1. **Actor:** Biên tập viên hoặc người duyệt.
**Mục tiêu:** Người xem biết phạm vi thay đổi thực tế.
**Kích hoạt:** Người dùng yêu cầu thực hiện “so sánh hai phiên bản”.
**Tiền điều kiện:** Có quyền đọc cả hai phiên bản.
**Thông tin đầu vào:** Hai phiên bản của cùng nội dung và locale.

**Luồng chính**

1. Chọn bản trước và bản sau.
2. Xem trường được thêm, sửa hoặc bỏ.
3. Kiểm tra thay đổi tài nguyên, phân loại và quan hệ.
4. Quay về biên tập hoặc quyết định duyệt.

**Luồng thay thế và ngoại lệ**

- E01: Mô hình thay đổi giữa hai bản: nêu rõ trường không còn tương ứng.
- E02: Chỉ có quyền một bản: không cho dùng so sánh để đọc bản còn lại.

**BR-VER-02:** So sánh không sửa dữ liệu. So sánh tài nguyên phải phân biệt phiên bản tệp, không chỉ tên tệp.
**Kết quả thành công:** Người xem biết phạm vi thay đổi thực tế.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-VER-02-01/v1:** Given chỉ đổi tài nguyên từ tệp v1 sang v2 cùng tên; When so sánh; Then vẫn nhận ra thay đổi.
- **AC-VER-02-02/v1:** Given thiếu quyền một bản; When so sánh; Then không lộ nội dung bản đó.
- **AC-VER-02-03/v1:** Given hai bản giống nhau; When so sánh; Then báo không có khác biệt nội dung.

**Use case liên quan / phụ thuộc:** [UC-VER-01](10-versioning.md#uc-ver-01).

## UC-VER-03

**Khôi phục từ một phiên bản cũ**

**Triển khai:** P2. **Actor:** Biên tập viên có quyền khôi phục.
**Mục tiêu:** Có bản nháp mới để kiểm tra, duyệt và xuất bản khi cần.
**Kích hoạt:** Người dùng yêu cầu thực hiện “khôi phục từ một phiên bản cũ”.
**Tiền điều kiện:** Phiên bản nguồn còn tồn tại. Có quyền tạo bản làm việc mới.
**Thông tin đầu vào:** Nội dung; phiên bản nguồn; lý do khôi phục.

**Luồng chính**

1. Chọn bản cũ và xem khác biệt với bản hiện tại.
2. Kiểm tra mô hình cùng tài nguyên còn hợp lệ.
3. Xác nhận khôi phục.
4. Nhận một bản nháp mới có tham chiếu nguồn khôi phục.

**Luồng thay thế và ngoại lệ**

- E01: Có người lưu bản mới trong lúc xác nhận: báo xung đột.
- E02: Tài nguyên cũ không còn dùng được: chỉ rõ lỗi, không tự phát hành.

**BR-VER-03:** Khôi phục tạo phiên bản mới, không viết lại lịch sử. Không tự chuyển bản công khai về bản cũ.
**Kết quả thành công:** Có bản nháp mới để kiểm tra, duyệt và xuất bản khi cần.
**Bảo đảm khi thất bại:** Không ghi nhận hoàn tất khi chưa đạt kết quả; giữ dữ liệu đã xác nhận trước đó. Tác vụ nhiều bước phải chỉ rõ phần đã thay đổi và phần chưa thực hiện.

**Acceptance Criteria**

- **AC-VER-03-01/v1:** Given v5 hiện tại và chọn v2; When khôi phục; Then tạo v6 từ v2, vẫn giữ v3-v5.
- **AC-VER-03-02/v1:** Given v5 đang công khai; When khôi phục; Then v5 vẫn công khai.
- **AC-VER-03-03/v1:** Given bản nguồn từng được duyệt; When khôi phục thành bản mới; Then bản mới cần duyệt lại.

**Use case liên quan / phụ thuộc:** [UC-VER-01](10-versioning.md#uc-ver-01), [UC-CNT-02](09-content.md#uc-cnt-02).
