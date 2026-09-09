# Công Cụ Theo Dõi Hiệu Suất Bài Viết Facebook
### Hướng dẫn cài đặt và sử dụng

---

Đây là công cụ mình dùng để theo dõi từng bài viết trên trang From Vietnam to Free, tính tỷ lệ chia sẻ, so sánh với các bài cũ, và biết chính xác bài đang chạy tốt hay cần chú ý, chỉ bằng cách chụp màn hình và gõ một lệnh.

Mình chia sẻ lại để cả nhà dùng cho trang của mình.

---

## Bạn cần gì

- Claude Desktop (tải miễn phí tại claude.ai/download)
- Claude Pro $20/tháng để dùng tab Code
- Các file trong repo này

---

## Bước 1: Cài Claude Desktop

Vào claude.ai/download, tải về và cài đặt như bất kỳ app nào trên Windows hoặc Mac. Không cần terminal, không cần cài thêm gì khác.

Sau khi cài xong, mở app lên. Bạn sẽ thấy hai tab ở trên cùng: Chat và Code.

Tab Code cần Claude Pro $20/tháng. Tab Chat thì miễn phí và xử lý được 80% nhu cầu hàng ngày. Nhưng để dùng công cụ theo dõi bài viết thì cần tab Code.

---

## Bước 2: Tạo thư mục dự án

Tạo một thư mục mới ở bất kỳ đâu trên máy tính. Đặt tên tuỳ ý, ví dụ: `theo-doi-trang`.

Bên trong thư mục đó, tạo cấu trúc như sau (tạo thủ công bằng cách chuột phải, New Folder):

```
theo-doi-trang/
├── .claude/
│   └── skills/
│       └── performance-review/
│           └── SKILL.md
├── data/
│   └── content/
│       └── post-benchmarks.json
└── CLAUDE.md
```

Lưu ý: thư mục `.claude` bắt đầu bằng dấu chấm. Trên Windows, thư mục này có thể bị ẩn. Bạn vẫn có thể tạo bình thường, chỉ cần đặt tên đúng.

---

## Bước 3: Copy các file vào đúng vị trí

Từ repo này, copy từng file vào đúng đường dẫn:

- `SKILL.md` vào `.claude/skills/performance-review/SKILL.md`
- `post-benchmarks.json` vào `data/content/post-benchmarks.json`
- `CLAUDE.md` vào thư mục gốc (cùng cấp với `.claude` và `data`)

---

## Bước 4: Điền thông tin của bạn vào CLAUDE.md

Mở file `CLAUDE.md` và điền thông tin về trang và business của bạn. File này giúp Claude hiểu bạn là ai và làm việc trong ngữ cảnh của bạn, không phải từ đầu mỗi phiên.

---

## Bước 5: Điền tên trang vào post-benchmarks.json

Mở file `post-benchmarks.json` và đổi dòng:

```json
"page": "TÊN TRANG CỦA BẠN",
```

---

## Bước 6: Nhập dữ liệu bài viết cũ (quan trọng)

Bước này giúp Claude hiểu baseline của trang bạn. Không có bước này, công cụ không có gì để so sánh.

Với mỗi bài viết bạn đã đăng:

1. Vào trang Facebook của bạn
2. Mở bài viết đó, nhấn "Xem thông tin chi tiết" (Post insights)
3. Chụp màn hình
4. Mở Claude Desktop, chọn tab Code, trỏ vào thư mục dự án
5. Nhập lệnh sau và đính kèm ảnh chụp màn hình:

```
Đọc ảnh chụp màn hình này và thêm bài viết vào file data/content/post-benchmarks.json
```

Claude sẽ tự đọc ảnh, tính tỷ lệ và cập nhật file. Lặp lại cho từng bài viết cũ.

---

## Bước 7: Sử dụng hàng ngày

Mỗi khi muốn theo dõi một bài viết đang chạy:

1. Vào Facebook, mở thông tin chi tiết bài viết, chụp màn hình
2. Mở Claude Desktop, chọn tab Code
3. Gõ lệnh và đính kèm ảnh:

```
/performance-review
```

Claude sẽ trả về:
- Tỷ lệ chia sẻ, tỷ lệ theo dõi mới
- Tốc độ tăng trưởng so với đỉnh
- So sánh với các bài viết cũ của bạn
- Dự đoán lượt xem cuối cùng
- Một hành động cụ thể cần làm ngay

---

## Bước 8: Khoá bài khi bài viết kết thúc

Khi một bài viết đã ngừng tăng (thường sau 3-5 ngày), chụp màn hình lần cuối và nhập:

```
bài này xong rồi
```

Claude sẽ tự cập nhật file, chuyển bài sang trạng thái "final" và bài đó trở thành baseline để so sánh với các bài sau.

---

## Thời điểm nên chụp màn hình theo dõi

Dựa trên thực nghiệm, bài viết thường có hai đợt tăng lượt xem:
- Đợt 1: giờ 1-9 (khán giả buổi sáng/chiều múi giờ chính)
- Đợt 2: giờ 9-21 tính từ lúc đăng (khán giả múi giờ thứ hai)

Thời điểm nên chụp: 3 giờ, 6 giờ, 9 giờ, 11 giờ, 21 giờ, 25 giờ, 36 giờ sau khi đăng bài.

Nếu thấy lượt xem chậm lại khoảng giờ thứ 6-8, đừng lo. Đó không phải bài đang chết mà là khoảng trống giữa hai đợt.

---

## Bảo mật dữ liệu

Nếu bạn lo ngại về việc dữ liệu bị dùng để train AI, có thể tắt tùy chọn này trong Claude:

Vào Settings, chọn Privacy, tắt tùy chọn "Improve Claude for everyone" (hoặc tương tự tùy phiên bản). Sau khi tắt, các cuộc hội thoại của bạn sẽ không được dùng để huấn luyện model.

Nếu bạn dùng Claude cho công ty hoặc team, Claude Team và Enterprise mặc định không dùng dữ liệu để train. Nếu dùng qua API thì cũng không dùng để train theo mặc định.

Với business cá nhân như tiệm nail, trang Facebook cá nhân thì dữ liệu đó là của bạn và bạn có toàn quyền quyết định. Nhưng nếu bạn đang làm cho công ty khác thì nên check policy của họ trước khi upload dữ liệu công ty lên bất kỳ AI tool nào.

---

## Câu hỏi thường gặp

Tôi chưa có bài viết nào thì sao?
Bỏ qua Bước 6. Đăng bài đầu tiên, rồi dùng /performance-review theo dõi ngay từ đầu. Sau khi bài kết thúc, đó sẽ là baseline đầu tiên của bạn.

File SKILL.md có cần chỉnh sửa không?
Không cần. Toàn bộ thông tin riêng của bạn nằm trong post-benchmarks.json và CLAUDE.md.

Lệnh /performance-review không hoạt động?
Kiểm tra lại đường dẫn file SKILL.md, phải đúng là .claude/skills/performance-review/SKILL.md. Đảm bảo bạn đang mở tab Code và trỏ vào đúng thư mục dự án.

Tab Code không hiện lên?
Bạn cần Claude Pro $20/tháng. Tab Chat miễn phí nhưng không chạy được skill files.

---

*Công cụ này do mình xây dựng cho trang From Vietnam to Free. Nếu bạn dùng và thấy có ích, chia sẻ lại kết quả trong nhóm nhé, mình muốn thấy trang của cả nhà lớn lên.*
