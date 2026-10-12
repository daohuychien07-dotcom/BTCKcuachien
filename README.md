# SmartAgri — Season Dashboard

Trang chính của hệ thống **SmartAgri — Trí tuệ cho mùa vàng**, thực hiện cho bài kiểm tra giữa kỳ học phần **Phát triển ứng dụng Web cơ bản (CSE122)**.

## 1. Thông tin chung

| Nội dung | Chi tiết |
| --- | --- |
| Tên đề tài BTL | SmartAgri — Hệ thống hỗ trợ quản lý mùa vụ thông minh cho nông dân |
| Tên trang đã thực hiện | Season Dashboard — Bảng điều khiển mùa vụ (`index.html`) |
| Đối tượng sử dụng | Nông dân đang quản lý một mùa vụ cụ thể (ví dụ: Vụ Lúa Mùa 2026, lô A1) |
| Mục đích của trang | Gom toàn bộ thông tin nông dân cần mỗi ngày về một màn hình: tiến độ mùa vụ, lịch thu hoạch dự kiến, thời tiết khu vực canh tác, nhật ký hoạt động gần đây, kênh ghi nhanh công việc, kênh hỏi chuyên gia và công cụ chẩn đoán bệnh cây sơ bộ bằng ảnh. |

## 2. Bố cục trang

```
┌───────────┬──────────────────────────────────────────────┐
│ Sidebar   │ Topbar: tiêu đề + lời chào + bell + avatar   │
│ (điều     ├───────────────────────────┬──────────────────┤
│  hướng    │ Hero: Vụ lúa + progress   │ Weather card     │
│  chính)   ├─────────────┬─────────────┴──────────────────┤
│           │ AI card     │ 2 nút CTA + bảng hoạt động     │
│  Đăng xuất│             │  (search + filter + table)     │
└───────────┴─────────────┴────────────────────────────────┘
```

## 3. Các chức năng JavaScript

| # | Chức năng | Kiến thức sử dụng | Cách kích hoạt |
| --- | --- | --- | --- |
| 1 | Render bảng "Hoạt động gần đây" từ mảng object | `Array.filter`, `forEach`, `createElement`, `innerHTML` | Trang tải (`init()`) |
| 2 | Tìm kiếm nhật ký theo nội dung | event `input`, `String.toLowerCase`, `includes` | Gõ vào ô tìm kiếm |
| 3 | Lọc theo loại hoạt động | event `change`, `select.value` | Chọn trong dropdown loại |
| 4 | "Xem tất cả" / "Thu gọn" (giới hạn 5 dòng) | biến trạng thái `showAll`, `Array.slice` | Bấm link góc phải card |
| 5 | Modal ghi nhật ký + validation + thêm dữ liệu vào đầu danh sách | `hidden`, `unshift`, `Date`, regex chiều dài | Bấm nút "Ghi nhật ký nhanh" |
| 6 | Modal hỏi chuyên gia + validation 2 trường | `preventDefault`, `classList.toggle` | Bấm nút "Nhờ chuyên gia hỗ trợ" |
| 7 | Tải/ kéo–thả ảnh lá cây, xem trước bằng `FileReader`, chẩn đoán mô phỏng | `FileReader.readAsDataURL`, event `dragover`/`drop`, `setTimeout` | Bấm hoặc kéo ảnh vào vùng nét đứt |
| 8 | Dropdown thông báo, reset badge về 0 khi đọc | event `click`, `stopPropagation`, `closest` | Bấm chuông 🔔 |
| 9 | Dropdown tài khoản người dùng | `aria-expanded`, `hidden` | Bấm avatar góc phải |
| 10 | Thanh tiến độ đếm từ 0% → 62% khi tải trang | `setInterval`, `clearInterval`, CSS transition | Trang tải |
| 11 | Toggle sidebar trên mobile | `classList.toggle` | Bấm nút ☰ (màn hình ≤ 760px) |
| 12 | Toast phản hồi sau mỗi thao tác | `hidden`, `setTimeout` | Sau khi lưu/gửi/chẩn đoán |

**Đóng mọi modal:** phím `Escape` hoặc bấm nền tối hoặc nút ✕.

## 4. Công nghệ sử dụng

- **HTML5** — semantic: `aside`, `nav`, `header`, `main`, `section`, `article`, `figure`, `table`, `form`, `footer`
- **CSS3** — CSS Variables, CSS Grid (layout 12 cột + grid lồng), Flexbox, `:hover`, `@keyframes`, media query
- **JavaScript thuần** — không framework, không backend, không gọi API thật (kết quả AI là dữ liệu mô phỏng)

## 5. Cấu trúc thư mục

```
.
├── index.html
├── css/
│   └── style.css
├── js/
│   └── main.js
├── assets/
│   ├── hero-rice.png
│   ├── avatar-farmer.png
│   ├── logo.svg
│   └── favicon.svg
└── README.md
```

## 6. Responsive

| Màn hình | Cách thể hiện |
| --- | --- |
| > 1180px (Desktop) | Sidebar cố định 244px; hero 8 cột + thời tiết 4 cột; AI 5fr + cột phải 7fr (2 CTA cạnh nhau, dưới là bảng hoạt động) |
| ≤ 1180px (Tablet) | Hero và thời tiết chiếm toàn hàng; AI và cột phải xếp chồng |
| ≤ 760px (Mobile) | Sidebar ẩn, mở bằng nút ☰ (trượt từ trái); mọi card 1 cột; 2 nút CTA xếp dọc; bảng cuộn ngang |

## 7. Chạy thử

```bash
python -m http.server 8000
# truy cập http://localhost:8000
```

Hoặc mở trực tiếp file `index.html` bằng trình duyệt.

## 8. Link bài nộp

| Nội dung | Link |
| --- | --- |
| Link Figma | _Điền link Figma của bạn_ |
| Link video | _Điền link video — tên file: `CSE122-GK-SVxx-TenSinhVien-SeasonDashboard`_ |
| Link GitHub | https://github.com/daohuychien07-dotcom/BTCKcuachien |

## 9. Ghi chú cho phần thuyết minh

- Dữ liệu mùa vụ, thời tiết, thông báo và nhật ký đang là hằng số khai báo ở đầu `js/main.js` (`season`, `activities`, `notifications`), không có database.
- Chức năng "Chẩn đoán sơ bộ" **mô phỏng** kết quả AI bằng `setTimeout` và sinh điểm tin cậy ngẫu nhiên 82–96%. Ảnh người dùng **không** rời khỏi máy: `FileReader` chỉ đọc ảnh thành data URL để hiển thị trong trình duyệt.
- Tên nông dân, mã số và thông tin liên hệ đang là dữ liệu mẫu.
