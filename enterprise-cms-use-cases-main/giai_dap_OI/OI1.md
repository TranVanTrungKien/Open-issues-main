# OI-01: Định hình mô hình sản phẩm & Hồ sơ pháp lý

**Trạng thái:** Đã rà soát nội dung; website công khai vẫn là đề xuất, chưa được khách hàng xác nhận.

[Nguồn issue](../open-issues.md#oi-01) · [Mục lục giải đáp](README.md) · [Quy tắc chung](../common-rules.md)

> Tách hai quyết định: website công khai/cổng nội bộ xác định đối tượng và quyền đọc; headless xác định kiến trúc phân phối. Hai trục có thể kết hợp, không phải ba lựa chọn loại trừ nhau. Ví dụ trong tài liệu không mặc định đều thuộc phiên bản đầu.


**Cần chốt:** Website công khai, cổng nội bộ hay headless CMS? Có nhu cầu quản lý hồ sơ pháp lý chuyên dụng không?
**Baseline:** CMS nội dung cho một doanh nghiệp. Một kênh đọc được chọn cho lần phát hành đầu.
**Chủ quyết định:** Chủ sản phẩm.
**Hạn chốt:** Trước P0. Ảnh hưởng Site, Publishing và quyền người đọc.

## Phân loại hệ thống và định hướng giải pháp 

## 1. Baseline

Hệ thống được xây dựng theo định hướng:

> **CMS nội dung cho một doanh nghiệp, với một kênh đọc được lựa chọn cho lần phát hành đầu tiên.**

Mục tiêu chính là cho phép doanh nghiệp quản lý, cập nhật và xuất bản nội dung thông qua CMS.

---

## 2. Các loại hệ thống có thể lựa chọn

### 2.1. Website công khai

#### Mô tả

Website công khai là hệ thống mà nội dung được cung cấp cho người dùng bên ngoài doanh nghiệp truy cập thông qua Internet.

#### Ví dụ

Website giới thiệu của một công ty:

* Trang chủ
* Giới thiệu doanh nghiệp
* Sản phẩm/Dịch vụ
* Tin tức
* Bài viết
* Liên hệ

Nhân viên sử dụng CMS để quản lý nội dung, trong khi khách hàng truy cập Website để đọc nội dung.

#### Giải pháp 

Sử dụng mô hình:

```text
Admin / Biên tập viên
        |
        v
      CMS
        |
        v
     Website
        |
        v
    Khách hàng
```

CMS có thể cung cấp các chức năng:

* Quản lý bài viết
* Quản lý tin tức
* Quản lý trang
* Quản lý hình ảnh
* Quản lý danh mục
* Quản lý menu
* Quản lý người dùng và quyền

#### Phù hợp khi

Doanh nghiệp chủ yếu muốn **công bố thông tin và nội dung cho khách hàng hoặc người dùng bên ngoài**.

---

### 2.2. Cổng thông tin nội bộ

#### Mô tả

Cổng thông tin nội bộ là hệ thống phục vụ nhân viên hoặc các thành viên bên trong doanh nghiệp.

Người dùng thường phải đăng nhập và được phân quyền trước khi truy cập.

#### Ví dụ

Cổng thông tin nội bộ của doanh nghiệp:

```text
Nhân viên
   |
   v
Đăng nhập
   |
   v
Cổng thông tin nội bộ
   |
   +-- Thông báo
   +-- Quy định
   +-- Tài liệu
   +-- Tin tức nội bộ
   +-- Chính sách công ty
```

#### Giải pháp

Hệ thống cần có:

* Authentication: xác thực người dùng
* Authorization: phân quyền
* CMS quản lý nội dung
* Quản lý tài liệu nội bộ
* Kiểm soát quyền truy cập

Ví dụ phân quyền:

```text
Admin
  |
  +-- Quản lý toàn bộ hệ thống

Biên tập viên
  |
  +-- Tạo / sửa / gửi duyệt nội dung

Nhân viên
  |
  +-- Đọc nội dung được phép truy cập
```

#### Phù hợp khi

Doanh nghiệp muốn xây dựng một hệ thống **chỉ dành cho nhân viên hoặc người dùng nội bộ**.

---

### 2.3. Headless CMS

#### Mô tả

Headless CMS là mô hình tách phần **quản lý nội dung** khỏi phần **giao diện hiển thị**.

CMS chịu trách nhiệm lưu trữ và quản lý nội dung. Nội dung được cung cấp thông qua API cho các kênh khác nhau.

#### Ví dụ

Một nội dung được quản lý một lần nhưng có thể được sử dụng trên nhiều kênh:

```text
                    CMS
                     |
                     v
                    API
              /      |      \
             /       |       \
            v        v        v
        Website   Mobile App  Kiosk
```

Ví dụ Admin tạo một bài viết:

```text
Tiêu đề:
Giới thiệu công ty ABC

Nội dung:
...

Hình ảnh:
...
```

Website và Mobile App có thể gọi API để lấy cùng nội dung này.

#### Giải pháp

Có thể sử dụng kiến trúc:

```text
Frontend
    |
    | REST API / GraphQL
    v
Headless CMS
    |
    v
Database
```

Ưu điểm:

* Có thể sử dụng nội dung trên nhiều kênh
* Frontend và CMS có thể phát triển độc lập
* Dễ mở rộng thêm Mobile App hoặc các kênh khác

Nhược điểm:

* Kiến trúc phức tạp hơn CMS truyền thống
* Cần xây dựng và quản lý API
* Chi phí phát triển ban đầu có thể cao hơn

#### Phù hợp khi

Doanh nghiệp dự kiến nội dung sẽ được sử dụng trên **nhiều kênh khác nhau**.

---

## 3. Quản lý hồ sơ pháp lý chuyên dụng

### 3.1. Không cần hệ thống pháp lý chuyên dụng

Nếu doanh nghiệp chỉ cần quản lý các nội dung thông thường:

```text
Bài viết
Tin tức
Trang giới thiệu
Thông báo
Hình ảnh
Tài liệu thông thường
```

thì CMS thông thường là đủ.

Không cần xây dựng một hệ thống quản lý hồ sơ pháp lý riêng.

---

### 3.2. Có nhu cầu quản lý hồ sơ pháp lý

Nếu doanh nghiệp cần quản lý các loại hồ sơ như:

```text
Hợp đồng
Giấy phép
Chứng nhận
Hồ sơ pháp lý
Văn bản pháp lý
```

và cần các chức năng chuyên biệt như:

* Theo dõi thời hạn
* Quản lý phiên bản
* Phê duyệt hồ sơ
* Lưu lịch sử thay đổi
* Phân quyền truy cập
* Theo dõi người xử lý
* Cảnh báo hồ sơ sắp hết hạn

thì cần khảo sát nghiệp vụ riêng trước khi quyết định xây module chuyên dụng hoặc tích hợp hệ thống ngoài. Phiên bản, phê duyệt và phân quyền cũng là năng lực CMS thông thường; chỉ các chức năng này chưa đủ để kết luận cần module pháp lý. Nếu có yêu cầu riêng về vòng đời hồ sơ, bảo toàn chứng cứ, giữ lại hoặc chữ ký, phải phân tích phạm vi và tiêu chí kiểm chứng riêng.

Ví dụ:

```text
CMS
 |
 +-- Content Management
 |     |
 |     +-- Bài viết
 |     +-- Tin tức
 |     +-- Trang
 |
 +-- Legal Document Management
       |
       +-- Hồ sơ pháp lý
       +-- Hợp đồng
       +-- Giấy phép
       +-- Phiên bản
       +-- Phê duyệt
       +-- Thời hạn
```

---

# 4. So sánh các lựa chọn

| Loại                  | Đối tượng sử dụng                | Ví dụ                           | Giải pháp                       |
| --------------------- | -------------------------------- | ------------------------------- | ------------------------------- |
| Website công khai     | Khách hàng, người dùng bên ngoài | Website công ty                 | CMS + Website                   |
| Cổng thông tin nội bộ | Nhân viên                        | Portal nội bộ                   | CMS + Đăng nhập + Phân quyền    |
| Headless CMS          | Một hoặc nhiều kênh                       | Website + Mobile App + Kiosk    | Headless CMS + API              |
| CMS + quản lý pháp lý | Doanh nghiệp cần quản lý hồ sơ   | Hợp đồng, giấy phép, chứng nhận | CMS + Legal Document Management |

---

# 5. Đề xuất cho phiên bản đầu tiên

Dựa trên baseline:

> **CMS nội dung cho một doanh nghiệp. Một kênh đọc được chọn cho lần phát hành đầu.**

Có thể lựa chọn:

```text
                 CMS
                  |
                  v
           Website công khai
                  |
                  v
              Người dùng
```

### Phạm vi phiên bản đầu

CMS cung cấp:

* Quản lý bài viết
* Quản lý tin tức
* Quản lý trang
* Quản lý danh mục
* Quản lý hình ảnh
* Quản lý người dùng
* Phân quyền
* Xuất bản nội dung

Kênh đọc đầu tiên:

> **Website công khai**

### Hồ sơ pháp lý

Trong phiên bản đầu:

> **Không yêu cầu hệ thống quản lý hồ sơ pháp lý chuyên dụng**, trừ khi nghiệp vụ thực tế của doanh nghiệp yêu cầu quản lý hợp đồng, giấy phép, chứng nhận hoặc các hồ sơ có quy trình pháp lý riêng.

### Khả năng mở rộng

Trong tương lai có thể mở rộng theo hướng:

```text
                       CMS
                        |
                       API
              __________|__________
             |          |          |
             v          v          v
         Website    Mobile App   Kiosk
```

Có thể chọn kiến trúc **Headless CMS** ngay từ đầu hoặc mở rộng API về sau. Headless có thể phục vụ một website công khai hoặc một cổng nội bộ; không bắt buộc nhiều kênh. Lần đầu vẫn chỉ có một kênh hoạt động.

---

# 6. Kết luận

Đề xuất để chủ sản phẩm xác nhận cho phiên bản đầu tiên là:

> **CMS nội dung cho doanh nghiệp + Website công khai là kênh đọc đầu tiên.**

Không cần triển khai hệ thống quản lý hồ sơ pháp lý chuyên dụng nếu CMS chỉ quản lý nội dung thông thường.

Kiến trúc nên được thiết kế có khả năng mở rộng để sau này có thể hỗ trợ **Headless CMS/API và nhiều kênh đọc khác** nếu doanh nghiệp có nhu cầu.


---

## 7. Kết quả rà soát và điều kiện chốt OI-01

### Những điểm đã chỉnh

- Phân biệt kênh đọc với headless; kiến trúc headless có thể chọn từ đầu và chỉ phục vụ một kênh.
- Sửa quyền biên tập viên thành tạo/sửa/gửi duyệt. Quyền duyệt và xuất bản cấp riêng; quản trị hệ thống không mặc định được duyệt mọi nội dung.
- Đánh dấu website công khai và chưa làm hồ sơ pháp lý chuyên dụng là đề xuất chờ xác nhận, không suy ra từ baseline một kênh.
- Phân biệt năng lực CMS thông thường và nhu cầu hồ sơ chuyên dụng. OI11 vẫn phải chốt lưu giữ/giữ lại/xóa dù chưa làm module pháp lý.

### Phạm vi đề xuất và thông tin cần xác nhận

Đề xuất **website công khai** làm kênh đầu để cụ thể hóa lát cắt: người đọc chỉ nhận bản đã phát hành; khu vực quản trị dùng danh tính doanh nghiệp và quyền CMS riêng theo OI3. Nếu chọn cổng nội bộ, bổ sung quyền đọc cho nội dung, tệp, search và cache, không chỉ đặt màn hình đăng nhập trước website.

Chưa chọn headless, REST hay GraphQL chỉ dựa trên kênh công khai. Nhóm kiến trúc xác nhận hợp đồng phân phối và bằng chứng đích phục vụ đúng bản cùng OI6/OI16. Có API không bắt buộc tách dịch vụ.

Bài viết/Tin tức/Trang là ví dụ; OI5 xác nhận loại thực tế. Menu, SEO và page builder mặc định ở P3, chỉ chuyển vào P2 nếu bắt buộc go-live theo OI15. Chưa gồm mobile app/kiosk, SaaS đa tenant hoặc hồ sơ pháp lý chuyên dụng nếu chưa có quyết định mở rộng phạm vi.

Chủ sản phẩm xác nhận trước P0: kênh và đối tượng đọc, site/đích đầu, có nhu cầu hồ sơ chuyên dụng không kèm ví dụ quy trình, các năng lực bắt buộc go-live. Ghi người xác nhận, ngày và phạm vi trước đóng issue; cập nhật UC/BR/AC và lộ trình nếu quyết định khác baseline.

### Tiêu chí kiểm chứng đề xuất

- Người đọc chỉ nhận đúng bản đã phát hành; không mở được nháp, preview riêng hoặc tệp cách ly qua URL trực tiếp.
- Người tạo/sửa không tự duyệt; có quyền CMS không đồng nghĩa có mọi quyền nghiệp vụ.
- Có nháp mới không đổi bản công khai; tạo site không tự công khai site.
- Nếu chọn kênh nội bộ, người mất quyền không lấy mới nội dung/tệp qua phiên hoặc cache cũ trong phạm vi hệ thống kiểm soát.

Tham chiếu: [Site](../domains/04-site-delivery.md), [IAM](../domains/01-identity-access.md), [Publishing](../domains/13-publishing.md), [OI5](OI5.md), [OI6](OI6.md), [OI11](OI11.md), [OI15](OI15.md), [OI16](OI16.md).
