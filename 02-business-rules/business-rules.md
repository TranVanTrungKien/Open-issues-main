# Business Rules

**Nguyên tắc:** Mỗi rule cần có ID ổn định, phát biểu rõ ràng, nguồn và trạng thái xác nhận.

| ID | Business Rule | Domain | Lý do/nguồn gốc | Trạng thái | Người xác nhận | Nguồn |
|---|---|---|---|---|---|---|
| BR-XXX | [Phát biểu quy tắc] | [Domain] | [Cần xác nhận] | [Blind] | [Chưa rõ] | [Liên kết] |

## Tiêu chí ghi nhận

- Viết quy tắc theo cách có thể kiểm tra.
- Tách rule khỏi giải pháp UI/technical implementation khi có thể.
- Ghi rõ ngoại lệ và điều kiện áp dụng.
- Không chuyển giả định thành rule đã xác nhận nếu chưa có bằng chứng.
