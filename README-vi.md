# Công Cụ Theo Dõi Hiệu Suất Bài Viết Facebook
### Hướng dẫn cài đặt và sử dụng

---

Đây là công cụ mình dùng để theo dõi từng bài viết trên trang From Vietnam to Free, tính tỷ lệ chia sẻ, so sánh với các bài cũ, và biết chính xác bài đang chạy tốt hay cần chú ý, chỉ bằng cách chụp màn hình và gõ một lệnh.

Mình chia sẻ lại để cả nhà dùng cho trang của mình.

---

## Bạn cần gì

- Tài khoản **Claude Pro** (claude.ai) $20/tháng
- **Claude Code** cài trên máy tính (Windows hoặc Mac)
- Hai file mình đính kèm trong bài: `SKILL.md` và `post-benchmarks.json`

---

## Bước 1: Cài Claude Code

Vào **claude.ai/code** và làm theo hướng dẫn cài đặt cho hệ điều hành của bạn (Windows hoặc Mac).

Sau khi cài xong, mở Terminal (Windows: nhấn phím Windows, gõ "cmd", nhấn Enter) và gõ thử:

```
claude --version
```

Nếu hiện số phiên bản là cài thành công.

---

## Bước 2: Tạo thư mục dự án

Tạo một thư mục mới ở bất kỳ đâu trên máy tính. Đặt tên tuỳ ý, ví dụ: `theo-doi-trang`.

Trong Terminal, điều hướng vào thư mục đó:

```
cd đường-dẫn-đến-thư-mục
```

Ví dụ trên Windows:
```
cd C:\Users\TenBan\Documents\theo-doi-trang
```

---

## Bước 3: Tạo cấu trúc thư mục

Tạo các thư mục con theo đúng cấu trúc này (bạn có thể tạo thủ công hoặc dùng Terminal):

```
theo-doi-trang/
├── .claude/
│   └── skills/
│       └── performance-review/
│           └── SKILL.md
└── data/
    └── content/
        └── post-benchmarks.json
```

**Cách tạo nhanh trên Windows (copy paste vào Terminal):**
```
mkdir .claude\skills\performance-review
mkdir data\content
```

---

## Bước 4: Lưu file SKILL.md

Copy nội dung file `SKILL.md` mình đính kèm và lưu vào:
```
.claude/skills/performance-review/SKILL.md
```

Không cần chỉnh sửa gì, file này chứa toàn bộ logic phân tích.

---

## Bước 5: Tạo file dữ liệu bài viết

Copy nội dung file `post-benchmarks.json` mình đính kèm và lưu vào:
```
data/content/post-benchmarks.json
```

Mở file đó và đổi dòng đầu tiên thành tên trang của bạn:

```json
"page": "TÊN TRANG CỦA BẠN",
```

---

## Bước 6: Nhập dữ liệu bài viết cũ (quan trọng)

Đây là bước giúp Claude hiểu baseline của trang bạn. Không có bước này, công cụ không có gì để so sánh.

Với mỗi bài viết bạn đã đăng:

1. Vào trang Facebook của bạn
2. Mở bài viết đó, nhấn **"Xem thông tin chi tiết"** (Post insights)
3. Chụp màn hình
4. Mở Terminal, vào thư mục dự án, gõ `claude` để khởi động
5. Nhập lệnh sau và đính kèm ảnh chụp màn hình:

```
Đọc ảnh chụp màn hình này và thêm bài viết vào file data/content/post-benchmarks.json
```

Claude sẽ tự đọc ảnh, tính tỷ lệ và cập nhật file. Lặp lại cho từng bài viết cũ.

---

## Bước 7: Sử dụng hàng ngày

Mỗi khi muốn theo dõi một bài viết đang chạy:

1. Vào Facebook, mở thông tin chi tiết bài viết, chụp màn hình
2. Trong Claude Code, gõ:

```
/performance-review
```

3. Đính kèm ảnh chụp màn hình, nhấn Enter

Claude sẽ trả về:
- Tỷ lệ chia sẻ, tỷ lệ theo dõi mới
- So sánh với các bài viết cũ của bạn
- Nhận định: bài đang chạy tốt hay cần chú ý
- Một hành động cụ thể cần làm ngay

---

## Bước 8: Khoá bài khi bài viết kết thúc

Khi một bài viết đã ngừng tăng (thường sau 3–5 ngày), chụp màn hình lần cuối và nhập:

```
bài này xong rồi
```

Claude sẽ tự cập nhật file, chuyển bài sang trạng thái "final" và bài đó trở thành baseline để so sánh với các bài sau.

---

## Thời điểm nên chụp màn hình theo dõi

Dựa trên thực nghiệm, bài viết thường có hai đợt tăng lượt xem:
- Đợt 1: giờ 1–9 (khán giả buổi sáng/chiều múi giờ chính)
- Đợt 2: giờ 9–21 tính từ lúc đăng (khán giả múi giờ thứ hai)

Thời điểm nên chụp: **3 giờ, 6 giờ, 9 giờ, 11 giờ, 21 giờ, 25 giờ, 36 giờ** sau khi đăng bài.

Nếu thấy lượt xem chậm lại khoảng giờ thứ 6–8, đừng lo. Đó không phải bài đang chết mà là khoảng trống giữa hai đợt.

---

## Câu hỏi thường gặp

**Tôi chưa có bài viết nào thì sao?**
Bỏ qua Bước 6. Đăng bài đầu tiên, rồi dùng `/performance-review` theo dõi ngay từ đầu. Sau khi bài kết thúc, đó sẽ là baseline đầu tiên của bạn.

**File SKILL.md có cần chỉnh sửa không?**
Không cần. Toàn bộ thông tin riêng của bạn nằm trong `post-benchmarks.json`.

**Lệnh /performance-review không hoạt động?**
Kiểm tra lại đường dẫn file SKILL.md, phải đúng là `.claude/skills/performance-review/SKILL.md`. Đảm bảo bạn đang chạy `claude` từ bên trong thư mục dự án.

---

*Công cụ này do mình xây dựng cho trang From Vietnam to Free. Nếu bạn dùng và thấy có ích, chia sẻ lại kết quả trong nhóm nhé, mình muốn thấy trang của cả nhà lớn lên.*
