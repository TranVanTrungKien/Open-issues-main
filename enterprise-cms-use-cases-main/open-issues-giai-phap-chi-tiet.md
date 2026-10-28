# ĐẶC TẢ VẤN ĐỀ MỞ (OPEN ISSUES) & PHƯƠNG ÁN GIẢI QUYẾT

**Dự án:** Enterprise CMS  
**Tài liệu hợp nhất:** Giải thích chi tiết câu hỏi và Hướng giải quyết cho 16 Open Issues (OI-01 đến OI-16)  
**Căn cứ đối chiếu:** `README.md`, `common-rules.md`, `implementation-roadmap.md`, `open-issues.md`

---

## I. TỔNG QUAN & NGUYÊN TẮC CHUNG

Tài liệu này tổng hợp toàn bộ **16 vấn đề mở (Open Issues - OI-01 đến OI-16)** cần chính thức phê duyệt trước khi đi vào nghiệm thu Production. Các vấn đề này bao phủ từ mô hình sản phẩm, quản trị danh tính, quy trình phê duyệt, mô hình nội dung cho đến hạ tầng phân phối, backup, lưu giữ dữ liệu và tích hợp hệ thống.

**Nguyên tắc áp dụng:**
1. **Dùng Baseline có đánh dấu:** Khi chưa có quyết định cuối cùng từ chủ quản chuyên trách, hệ thống bắt buộc sử dụng phương án Baseline được đề xuất để làm căn cứ thiết kế và phát triển.
2. **Không nghiệm thu trên giả định:** Mọi giá trị chưa chốt không được dùng làm căn cứ nghiệm thu Production.
3. **Tuân thủ Thứ tự Triển khai (Implementation Roadmap):** Các quyết định ảnh hưởng đến P0/P1/P2 phải được chốt theo đúng Completion Gates của từng mốc.

---

## II. CHI TIẾT 16 VẤN ĐỀ MỞ (OI-01 ĐẾN OI-16)



### 2. OI-02: Phạm vi tổ chức & Chia sẻ dữ liệu
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Mô hình vận hành là một doanh nghiệp gồm nhiều đơn vị/site hay là SaaS đa tenant (nhiều khách hàng dùng chung hệ thống)? Dữ liệu giữa các site có được phép chia sẻ chéo không?
  * *Giải thích:* Việc hỗ trợ SaaS multi-tenant hoặc chia sẻ chéo dữ liệu làm thay đổi hoàn toàn ranh giới bảo mật và mô hình dữ liệu ở cấp kiến trúc cốt lõi, không thể chỉ dùng bộ lọc (filter) đơn thuần để xử lý sau này.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* Cấu hình **1 Workspace doanh nghiệp dùng chung cho nhiều đơn vị và site**. Áp dụng nguyên tắc cô lập dữ liệu: **không chia sẻ dữ liệu mặc định giữa các site/đơn vị**. Người dùng thuộc site A không thể đọc hoặc chỉnh sửa dữ liệu site B.
  * *Phương án mở rộng:* Nếu có nhu cầu chia sẻ nội dung chéo (Cross-site sharing), bắt buộc phải bổ sung đối tượng "Liên kết nội dung" có phân quyền riêng trước khi thiết kế Schema P0.
* **Chủ quyết định & Hạn chốt:** Chủ sản phẩm & Bộ phận Bảo mật — *Trước mô hình dữ liệu P0*.

---

### 3. OI-03: Tích hợp Quản trị Danh tính (SSO / MFA)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Nhà cung cấp SSO/MFA cụ thể là gì, thời hạn phiên đăng nhập bao lâu, tài khoản khẩn cấp (break-glass) thiết lập ra sao, đồng bộ nhóm người dùng và cơ chế vô hiệu hóa khi có sự cố như thế nào?
  * *Giải thích:* Đây là lớp bảo mật cơ sở (P0). Cần thiết lập nguyên tắc rõ ràng giữa xác thực danh tính (Authentication) và cấp quyền nghiệp vụ (Authorization) trong CMS.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* Tích hợp **xác thực doanh nghiệp (SSO/MFA)**. Áp dụng quy tắc tách biệt quyền (**BR-COM-02**): *Đăng nhập SSO thành công không đồng nghĩa với việc tự động có quyền sửa/duyệt nội dung trong CMS*.
  * *Cơ chế vô hiệu hóa:* Khi quyền của người dùng bị thu hồi trên IAM, hệ thống phải **chặn ngay lập tức ở hành động tiếp theo** (**BR-COM-11**). Thiết lập 1 tài khoản quản trị khẩn cấp (break-glass) được bảo vệ riêng.
* **Chủ quyết định & Hạn chốt:** Bộ phận Quản trị danh tính & Bảo mật — *Giai đoạn P0*.

---

### 4. OI-04: Quy trình Phê duyệt (Maker-Checker & Thu hồi)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Luồng phê duyệt yêu cầu 1 cấp hay nhiều cấp duyệt? Nguyên tắc phân tách trách nhiệm (Maker-Checker) áp dụng thế nào? Ai có thẩm quyền thu hồi phê duyệt?
  * *Giải thích:* Quy định nguyên tắc kiểm soát chất lượng và rủi ro nội dung trước khi xuất bản.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* Áp dụng **quy trình 1 cấp duyệt độc lập** (**BR-COM-06**). Bắt buộc tuân thủ Maker-Checker: *người tạo hoặc chỉnh sửa phiên bản gửi duyệt không được tự phê duyệt chính phiên bản đó*.
  * *Phương án mở rộng:* Nếu chính sách doanh nghiệp bắt buộc phê duyệt nhiều cấp ngay từ đầu, phải **chuyển tính năng phê duyệt nhiều cấp (WF-01/03) từ P3 về P2** để hoàn tất lát cắt vận hành E2E.
* **Chủ quyết định & Hạn chốt:** Chủ nội dung & Người phụ trách kiểm soát — *Trước P2*.

---

### 5. OI-05: Cấu trúc Mô hình Nội dung Đầu tiên
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Danh sách các loại nội dung ban đầu (Article, News, Product,...), danh sách trường bắt buộc, quan hệ đối tượng, kích thước, quy tắc URL và nhãn dữ liệu nhạy cảm là gì?
  * *Giải thích:* Cần cấu trúc dữ liệu cụ thể để triển khai luồng biên tập, tạo phiên bản, phân loại và xem trước ở P1.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* **Chưa áp đặt các mẫu phức tạp làm yêu cầu chính thức**. Lựa chọn **1 loại mẫu nội dung thực tế đơn giản** ở P1 để thử nghiệm lát cắt vận hành đầu tiên (Soạn nháp -> Lưu phiên bản -> Preview -> Phê duyệt).
  * *Phương án chi tiết:* Xác định danh sách trường bắt buộc, quy tắc đặt Slug/URL và chính sách nhãn dữ liệu nhạy cảm cho mẫu đầu tiên trước khi xây dựng UI biên tập.
* **Chủ quyết định & Hạn chốt:** Chủ nội dung & Business Analyst (BA) — *Trước P1*.

---

### 6. OI-06: Đích Phân phối & Giới hạn Thời gian (Latency / Cache / Indexing)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Đích phân phối cụ thể là gì, bằng chứng nào xác nhận kênh đọc đã nhận bản mới, và ngưỡng thời gian trễ tối đa (SLA) cho việc xuất bản, gỡ bài, xóa cache, cập nhật chỉ mục tìm kiếm là bao nhiêu?
  * *Giải thích:* Đảm bảo tính trung thực của việc phát hành. Tránh tình trạng hệ thống báo xuất bản thành công nhưng người dùng ngoài web vẫn nhìn thấy nội dung cũ hoặc chưa cập nhật cache.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* Cấu hình **1 đích phân phối hoạt động trong P2**. Bắt buộc tuân thủ quy tắc **BR-COM-08**: *Chỉ báo hoàn tất khi đích phân phối đã thực sự phục vụ nội dung mới*, tuyệt đối không báo thành công khi mới chỉ dừng ở bước tiếp nhận yêu cầu.
  * *Phương án SLA:* Tiến hành kiểm thử tải thực tế để đo đạc chỉ tiêu số giây cho latency/cache/search trước khi chốt cam kết nghiệm thu.
* **Chủ quyết định & Hạn chốt:** Chủ sản phẩm & Vận hành (Ops) — *Trước nghiệm thu P2*.

---

### 7. OI-07: Lập lịch Xuất bản & Xử lý Sự cố Lịch Quá hạn
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Lịch xuất bản tự động chạy theo quyền cá nhân người lập hay ủy quyền tổ chức? Xử lý thế nào đối với các lịch bị quá hạn do sự cố hệ thống kéo dài?
  * *Giải thích:* Đảm bảo an toàn pháp lý và tránh việc tự động phát hành các nội dung đã lỗi thời hoặc người lập lịch đã bị thu hồi quyền.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:*
    1. **Quyền thi hành:** Kiểm tra quyền hiện hành của người lập lịch ngay tại thời điểm Job chạy (**BR-COM-10**). Nếu người lập lịch bị mất quyền, Job sẽ bị hủy và báo lỗi.
    2. **Xử lý lịch quá hạn:** Nếu hệ thống gặp sự cố làm lỡ lịch xuất bản, **không được tự động phát hành bù**. Toàn bộ lịch quá hạn phải chuyển sang trạng thái chờ rà soát thủ công.
* **Chủ quyết định & Hạn chốt:** Chủ nội dung & Vận hành — *Trước triển khai lập lịch P2*.

---

### 8. OI-08: Đa ngôn ngữ, Fallback & Bản dịch Lỗi thời
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Những ngôn ngữ nào bắt buộc hỗ trợ, quy tắc dự phòng (fallback) hiển thị ra sao, trường nào cấm fallback, và khi nào một bản dịch bị coi là lỗi thời do bản nguồn thay đổi?
  * *Giải thích:* Quản lý vòng đời nội dung đa ngôn ngữ độc lập, tránh việc lộ nội dung chưa được duyệt hoặc dịch sai ngữ cảnh.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:*
    1. **Khởi tạo:** P0 bắt đầu với **1 locale mặc định** (**BR-COM-14**).
    2. **Độc lập locale:** Mỗi locale có quy trình nháp, phê duyệt và xuất bản **hoàn toàn độc lập**. **Không dự phòng (fallback) sang bản nháp** của locale khác.
    3. **Bản dịch lỗi thời:** Khi nội dung nguồn có phiên bản mới, bản dịch ở các locale khác sẽ tự động được cắm cờ "Cần rà soát lại (Outdated translation)".
  * *Phương án mở rộng:* Nếu đa ngôn ngữ là yêu cầu bắt buộc khi Go-live, chuyển các use case dịch (LOC-02/03/04) từ P3 lên P2.
* **Chủ quyết định & Hạn chốt:** Chủ nội dung & Nhóm dịch — *Quy tắc ở P0, quy trình dịch trước P3*.

---

### 9. OI-09: Quản lý & Kiểm tra An toàn Tài nguyên (Assets)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Loại tệp nào được phép tải lên, giới hạn dung lượng bao nhiêu, quy trình quét an toàn/độc hại diễn ra thế nào, và tổng dung lượng kho tệp dự kiến là bao nhiêu?
  * *Giải thích:* Bảo vệ hệ thống khỏi tệp mã độc (malware), tấn công upload và đảm bảo tối ưu hóa trải nghiệm người dùng.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* Bắt buộc tuân thủ quy tắc **BR-COM-12**: **Không tin tưởng định dạng tệp do phía client khai báo**. Tệp chưa qua kiểm tra an toàn (Antivirus scan, MIME validation) hoặc chưa phân loại nhãn dữ liệu **bị chặn phát hành**.
  * *Quy trình tệp:* Tệp mới tải lên nằm ở vùng cách ly (quarantine). Tự động tạo thumbnail an toàn và tối ưu hóa dung lượng trước khi duyệt cho phép dùng trong bài viết.
* **Chủ quyết định & Hạn chốt:** Bộ phận Bảo mật & Quản trị tài nguyên — *Trước P1 và xác minh trước Production*.

---

### 10. OI-10: Quyền Sử dụng Tài nguyên (Asset Licensing)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Ai chịu trách nhiệm xác nhận bản quyền tài nguyên số, và xử lý ra sao khi quyền sử dụng tệp hết hạn trong lúc bài viết chứa tệp đó đang công khai?
  * *Giải thích:* Phòng tránh rủi ro vi phạm bản quyền hình ảnh/video gây thiệt hại pháp lý và uy tín doanh nghiệp.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:*
    1. **Chặn phát hành mới:** Khi tài nguyên hết hạn sử dụng, hệ thống **chặn ngay các đợt phát hành mới** chứa tài nguyên đó.
    2. **Xử lý bản đang công khai (Bắt buộc):** Bổ sung quy trình tự động/bán tự động để **gỡ bỏ hoặc thu hồi các bản ghi công khai có chứa tài nguyên hết hạn** (chỉ chặn phát hành mới là không đủ).
* **Chủ quyết định & Hạn chốt:** Chủ nội dung & Người phụ trách bản quyền — *Trước khi phát hành tài nguyên có hạn sử dụng*.

---

### 11. OI-11: Chính sách Lưu giữ, Bán xóa & Giữ lại Dữ liệu (Retention & Hold)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Thời hạn lưu giữ nội dung, nhật ký audit và tệp là bao lâu? Cơ chế đặt lệnh giữ lại dữ liệu (Legal Hold) như thế nào? Phạm vi xóa vĩnh viễn dữ liệu ra sao?
  * *Giải thích:* Đảm bảo tuân thủ các quy định pháp lý về dữ liệu cá nhân (GDPR/Decree 13) và quản lý lưu trữ doanh nghiệp.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* **Baseline chưa bật tính năng xóa vĩnh viễn tự động** (**BR-COM-15**). Dữ liệu bị xóa thông thường chỉ chuyển sang trạng thái lưu trữ/ẩn.
  * *Cơ chế Legal Hold:* Thiết lập cờ "Yêu cầu giữ lại dữ liệu" (Legal Hold) có **ưu tiên tối cao**, đè lên mọi quy tắc hết hạn lưu giữ để chặn toàn bộ hành vi xóa đối tượng.
  * *Phương án nâng cao:* Nếu dữ liệu thật yêu cầu quy định xóa cứng, chuyển các use case GOV-P4 (GOV-03/04/05) lên trước Production.
* **Chủ quyết định & Hạn chốt:** Chủ dữ liệu, Bảo mật & Bộ phận Tuân thủ — *Trước Production*.

---

### 12. OI-12: Chỉ tiêu Hiệu năng & Khối lượng Vận hành
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Số người dùng truy cập đồng thời, số lượng bài viết/site/locale, dung lượng kho tệp và khối lượng lịch lập dự kiến là bao nhiêu?
  * *Giải thích:* Xác định quy mô và thiết kế hạ tầng, cơ sở dữ liệu, bộ nhớ đệm (Cache) phù hợp.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* **Không gán các con số ước lượng giả định thành cam kết hiệu năng chính thức**.
  * *Phương án triển khai:* Thực hiện bài kiểm thử tải (Load Testing & Stress Testing) ở P2 dựa trên kịch bản thực tế để đo đạc và xác nhận khả năng chịu tải trước khi Detail Design.
* **Chủ quyết định & Hạn chốt:** Chủ sản phẩm & Vận hành — *Trước Detail Design và kiểm thử tải P2*.

---

### 13. OI-13: Phương án Backup & Khôi phục sau Sự cố (RPO/RTO)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Mức độ mất dữ liệu tối đa cho phép (RPO), thời gian phục hồi tối đa (RTO) là bao nhiêu, và phạm vi backup gồm những gì?
  * *Giải thích:* Bắt buộc phải có để đảm bảo tính sẵn sàng cao và khả năng phục hồi khi xảy ra thảm họa hạ tầng.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:*
    1. **Phạm vi Backup:** Bắt buộc backup đồng bộ cả 3 thành phần: **Cơ sở dữ liệu, Kho tệp tài nguyên (Assets), và Cấu hình hệ thống**.
    2. **Lịch trình:** Đưa phương án Backup toàn diện vào **Giai đoạn P2** (không hoãn đến P4).
    3. **Diễn tập:** Bắt buộc thực hiện **diễn tập khôi phục (Restore Drill)** thực tế đạt chỉ tiêu RPO/RTO cam kết trước khi lên Production.
* **Chủ quyết định & Hạn chốt:** Vận hành & Chủ sản phẩm — *Giai đoạn P2*.

---

### 14. OI-14: Nhập & Di chuyển Dữ liệu (Migration)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Dữ liệu cũ nhập từ nguồn nào, khóa đối chiếu (mapping key) ra sao, phân quyền di chuyển thế nào, xử lý lỗi từng phần và đối soát số lượng ra sao?
  * *Giải thích:* Đảm bảo tính toàn vẹn dữ liệu từ hệ thống legacy sang CMS mới mà không phá vỡ quy trình kiểm soát chất lượng.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:* Dữ liệu di chuyển từ hệ thống cũ sang CMS mới **bắt buộc đưa về trạng thái Bản nháp (Draft)**, không mặc định giữ nguyên trạng thái "Đã duyệt" hay "Đang công khai" cũ.
  * *Phương án mở rộng:* Nếu phải thay thế CMS cũ ngay lúc Go-live, **chuyển module Migration (ADM-05/06) từ P4 lên P1/P2** để xây dựng công cụ đối soát số lượng và thử nghiệm di chuyển nhiều đợt.
* **Chủ quyết định & Hạn chốt:** Chủ dữ liệu & Nhóm Migration — *Trước P4 (hoặc P1/P2 nếu Go-live ngay)*.

---

### 15. OI-15: Mức độ Thiết kế Trang & Trải nghiệm Biên tập (Template vs. Page Builder)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Trang web được dựng theo mẫu cố định hay dùng công cụ dựng trang linh hoạt (Page Builder), có bao nhiêu mẫu sẵn, và yêu cầu trình duyệt/trợ năng hỗ trợ là gì?
  * *Giải thích:* Cân bằng giữa tốc độ phát triển, kiểm soát chuẩn mực giao diện thương hiệu và tính linh hoạt cho người biên tập.
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:*
    1. **P1/P2:** Sử dụng **mẫu nội dung cố định (Structured Templates/Views)** để tối ưu tiến độ triển khai và kiểm soát chất lượng hiển thị.
    2. **P3:** Mở rộng công cụ dựng trang linh hoạt (**Page Builder - SITE-02**).
  * *Phương án mở rộng:* Nếu cần khả năng dựng trang tùy biến ngay khi Go-live, chuyển SITE-02 vào P2 nhưng giới hạn bộ thành phần (component library) được phép sử dụng.
* **Chủ quyết định & Hạn chốt:** Chủ sản phẩm & Thiết kế (UI/UX) — *Mẫu đầu tiên trước P2, Page Builder trước P3*.

---

### 16. OI-16: Tích hợp Bên ngoài & Tính Nguyên tử (Atomic Operations)
* **Bản chất câu hỏi & Lý do cần chốt:**
  * *Câu hỏi:* Những API hoặc hệ thống bên ngoài nào bắt buộc kết nối ngay khi Go-live? Hệ thống có yêu cầu phát hành đồng thời tuyệt đối (Atomic Publishing) xuyên nhiều hệ thống không?
  * *Giải thích:* Xác định phạm vi tích hợp nền tảng và cơ chế xử lý giao dịch đồng thời (Distributed Transactions).
* **Hướng & Phương án giải quyết:**
  * *Baseline khuyến nghị:*
    1. **Tích hợp bắt buộc:** SSO (P0) và 1 Đích phân phối (P2) là các kết nối nền tảng bắt buộc, không hoãn. Các kết nối Webhook, API mở rộng đưa về P4.
    2. **Tính nguyên tử:** Hệ thống **không mặc định hỗ trợ giao dịch nguyên tử (Atomic) xuyên nhiều hệ thống**. Khi xuất bản đa đích, áp dụng cơ chế xử lý bất đồng bộ với trạng thái ghi nhận từng phần và thử lại (retry) an toàn (**BR-COM-08/09**).
* **Chủ quyết định & Hạn chốt:** Chủ sản phẩm & Kiến trúc sư hệ thống — *Giai đoạn P0/P2*.

---

## III. MA TRẬN TỔNG HỢP VẤN ĐỀ MỞ (OPEN ISSUES MATRIX)

|   Mã OI   | Tên Vấn Đề          | Câu Hỏi Cốt Lõi                      | Hướng Giải Quyết Baseline                            | Chủ Quyết Định                 |          Hạn Chốt           |
| :-------: | :------------------ | :----------------------------------- | :--------------------------------------------------- | :----------------------------- | :-------------------------: |
| **OI-01** | Mô hình sản phẩm    | Loại hình hệ thống & Hồ sơ pháp lý?  | CMS doanh nghiệp 1 kênh; chưa làm hồ sơ pháp lý      | Chủ sản phẩm                   |          Trước P0           |
| **OI-02** | Phạm vi tổ chức     | Single org hay SaaS Multi-tenant?    | 1 Workspace doanh nghiệp; không chia sẻ chéo         | Chủ sản phẩm, Bảo mật          |          Trước P0           |
| **OI-03** | Quản trị danh tính  | Nhà cung cấp SSO/MFA & Vô hiệu hóa?  | SSO doanh nghiệp; phân quyền CMS riêng               | Quản trị danh tính, Bảo mật    |             P0              |
| **OI-04** | Quy trình phê duyệt | Số cấp duyệt & Maker-Checker?        | 1 cấp duyệt độc lập; áp dụng Maker-Checker           | Chủ nội dung, Kiểm soát        |          Trước P2           |
| **OI-05** | Mô hình nội dung    | Các mẫu nội dung ban đầu & Trường?   | Dùng 1 mẫu thực tế để thử lát cắt P1                 | Chủ nội dung, BA               |          Trước P1           |
| **OI-06** | Đích phân phối      | SLA phân phối, Cache & Search?       | 1 đích phân phối P2; báo hoàn tất khi kênh nhận      | Chủ sản phẩm, Vận hành         |        Nghiệm thu P2        |
| **OI-07** | Lập lịch xuất bản   | Quyền thi hành & Lịch quá hạn?       | Quyền chạy theo thời điểm hiện hành; không bù        | Chủ nội dung, Vận hành         |      Trước lập lịch P2      |
| **OI-08** | Đa ngôn ngữ         | Fallback & Bản dịch lỗi thời?        | 1 locale P0; các locale độc lập; không fallback nháp | Chủ nội dung, Nhóm dịch        |        P0 / Trước P3        |
| **OI-09** | Quản lý tài nguyên  | Loại tệp, Dung lượng & Quét độc hại? | Bắt buộc quét tệp; cấm tin client khai báo           | Bảo mật, Quản trị tài nguyên   |       Trước P1 / Prod       |
| **OI-10** | Quyền tài nguyên    | Xử lý khi tài nguyên hết hạn?        | Chặn phát hành mới & gỡ bản công khai                | Chủ nội dung, Bản quyền        |       Trước dùng tệp        |
| **OI-11** | Chính sách lưu giữ  | Thời hạn lưu giữ & Legal Hold?       | Mặc định không xóa tự động; Legal Hold ưu tiên 1     | Chủ dữ liệu, Bảo mật, Tuân thủ |         Trước Prod          |
| **OI-12** | Chỉ tiêu hiệu năng  | Số user, Kho tệp & Khối lượng?       | Đo đạc qua kiểm thử tải thực tế tại P2               | Chủ sản phẩm, Vận hành         |   Trước Detail Design P2    |
| **OI-13** | Phương án Backup    | RPO/RTO & Diễn tập khôi phục?        | Backup Database, Assets, Config ở P2; diễn tập       | Vận hành, Chủ sản phẩm         |             P2              |
| **OI-14** | Migration dữ liệu   | Nguồn dữ liệu & Trạng thái nhập?     | Nhập về dạng Bản nháp (Draft); đối soát số lượng     | Chủ dữ liệu, Nhóm Migration    |    Trước P4 (hoặc P1/P2)    |
| **OI-15** | Thiết kế trang      | Template hay Page Builder?           | Mẫu cố định P1/P2; Page Builder mở rộng P3           | Chủ sản phẩm, Thiết kế         | P2 (Template), P3 (Builder) |
| **OI-16** | Tích hợp hệ thống   | Kết nối bắt buộc & Tính nguyên tử?   | SSO (P0) & 1 Đích (P2); không atomic xuyên hệ thống  | Chủ sản phẩm, Kiến trúc        |           P0 / P2           |

---
*Tài liệu này được xuất bản tự động dưới dạng file `.md` nhằm làm căn cứ họp và trình phê duyệt cho các cấp quản lý.*
