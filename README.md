# SmartAgri — Season Dashboard

Trang chính của hệ thống **SmartAgri — Trí tuệ cho mùa vàng**, thực hiện cho bài kiểm tra giữa kỳ học phần **Phát triển ứng dụng Web cơ bản (CSE122)**.

## 1. Thông tin chung

| Nội dung | Chi tiết |
| --- | --- |
| Tên đề tài BTL | SmartAgri — Hệ thống hỗ trợ quản lý mùa vụ thông minh cho nông dân |
| Tên trang đã thực hiện | Season Dashboard — Bảng điều khiển mùa vụ (`index.html`) |
| Trang phụ trợ | `login.html` (Đăng nhập), `register.html` (Đăng ký), `diary.html` (Nhật ký) và `support.html` (Yêu cầu hỗ trợ kỹ thuật) |
| Luồng sử dụng | Đăng ký → Đăng nhập → Season Dashboard → Nhật ký / Hỗ trợ kỹ thuật (liên kết qua lại ở sidebar) |
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

## 3. Các chức năng JavaScript — Season Dashboard (`index.html`)

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

## 4. Các chức năng JavaScript — Đăng nhập (`login.html`)

| # | Chức năng | Kiến thức sử dụng | Cách kích hoạt |
| --- | --- | --- | --- |
| 1 | Hiện / ẩn mật khẩu, đổi icon và cập nhật `aria-label` | `input.type`, `classList.toggle`, `innerHTML`, `setAttribute` | Bấm biểu tượng con mắt trong ô mật khẩu |
| 2 | Validation: bắt buộc nhập, độ dài tối thiểu (user ≥ 4, pass ≥ 6), thông báo lỗi từng trường | `if/else`, `trim`, `length`, `textContent`, `classList` | Bấm "Đăng nhập" khi dữ liệu chưa hợp lệ |
| 3 | Xoá lỗi ngay khi người dùng gõ lại | event `input` | Gõ vào ô đang báo lỗi |
| 4 | Đăng nhập giả lập: nút chuyển trạng thái "Đang đăng nhập..." có spinner, rồi kiểm tra tài khoản trong mảng | `setTimeout`, vòng `for`, `disabled` | Bấm "Đăng nhập" với dữ liệu hợp lệ |
| 5 | Sai tài khoản → báo lỗi + rung nút + toast đỏ | `classList.add/remove`, `@keyframes shake` | Nhập sai `nongdan / 123456` |
| 6 | Đúng tài khoản → toast chào theo vai trò rồi chuyển sang `index.html` | `window.location.href`, `setTimeout` | Nhập `nongdan / 123456` (hoặc `chuyen_gia / agri2026`) |

Tài khoản demo khai báo bằng hằng số `accounts` ở đầu `js/login.js`.

## 5. Các chức năng JavaScript — Đăng ký (`register.html`)

| # | Chức năng | Kiến thức sử dụng | Cách kích hoạt |
| --- | --- | --- | --- |
| 1 | Validation 4 trường: SĐT theo regex `^(\+?84|0)\d{9,10}$`, họ tên chỉ chữ, năm sinh đủ 16 tuổi, địa chỉ ≥ 5 ký tự | `RegExp.test`, `if/else`, `trim`, `Number.isInteger` | Bấm "Đăng Ký" |
| 2 | Chuẩn hoá số điện thoại (chấp nhận `0987.654.321`, khoảng trắng, gạch ngang) | `String.replace` với regex | Gõ SĐT có dấu phân cách |
| 3 | Tự động hiển thị tuổi ngay khi gõ năm sinh | event `input`, `new Date().getFullYear()` | Gõ ô "Năm sinh" |
| 4 | Chống trùng số điện thoại đã đăng ký | `localStorage.getItem/setItem`, `JSON.parse`, `Array.some` | Đăng ký lại một SĐT đã có |
| 5 | Nút chuyển trạng thái "Đang gửi..." có spinner rồi lưu hồ sơ | `setTimeout`, `disabled`, `Array.push` | Bấm "Đăng Ký" hợp lệ |
| 6 | Thành công → toast xanh rồi quay về `login.html`; thất bại → toast đỏ + rung nút | `window.location.href`, `classList`, `@keyframes shake` | Sau khi gửi |

## 6. Các chức năng JavaScript — Nhật ký (`diary.html`)

| # | Chức năng | Kiến thức sử dụng | Cách kích hoạt |
| --- | --- | --- | --- |
| 1 | Render timeline từ mảng object (màu tag theo loại, dot theo trạng thái) | `Array.forEach`, `createElement`, `classList`, `textContent` | Trang tải (`init()`) |
| 2 | Tìm kiếm theo mô tả hoặc loại hoạt động | event `input`, `toLowerCase`, `indexOf` | Gõ vào ô tìm kiếm |
| 3 | Ba bộ lọc giai đoạn / loại / trạng thái kết hợp | `Array.filter`, `select.value`, `Array.sort`, `localeCompare` | Chọn ở thanh công cụ |
| 4 | Nút "Làm mới" xoá toàn bộ bộ lọc | gán lại giá trị mặc định + `renderEntries()` | Bấm ⟳ Làm mới |
| 5 | Thêm nhật ký mới (validation 3 trường) vào đầu danh sách | `preventDefault`, `Array.unshift`, `new Date()` | Điền form rồi bấm "Lưu nhật ký" |
| 6 | Sửa: bơm dữ liệu vào form, đổi nút thành "Cập nhật nhật ký", highlight bản ghi | `Array.find`, biến trạng thái `editingId`, `scrollIntoView` | Bấm ✏️ Sửa trên bản ghi |
| 7 | Xoá có xác nhận qua modal | event delegation `closest('[data-action]')`, `Array.filter`, `hidden` | Bấm 🗑️ Xóa → xác nhận |
| 8 | Kéo–thả / chọn nhiều ảnh, xem trước, loại từng ảnh | `FileReader.readAsDataURL`, event `drop`/`dragover`, `Array.splice` | Thả ảnh vào vùng nét đứt |
| 9 | Chẩn đoán AI mô phỏng (ngẫu nhiên 1 trong 3 kết quả), Xác nhận gắn vào mô tả, Chỉnh sửa bật `contenteditable` | `setTimeout`, `Math.random`, `getAttribute`/`setAttribute` | Bấm "Chẩn đoán bằng AI" sau khi có ảnh |
| 10 | Dropdown thông báo, toggle sidebar mobile, toast | `stopPropagation`, `classList.toggle`, `setTimeout` | Bấm 🔔 /  / sau mỗi thao tác |

## 7. Các chức năng JavaScript — Yêu cầu hỗ trợ kỹ thuật (`support.html`)

| # | Chức năng | Kiến thức sử dụng | Cách kích hoạt |
| --- | --- | --- | --- |
| 1 | Render danh sách "Yêu cầu đã gửi" từ mảng object, mỗi yêu cầu có mã `#HS-2026-00NN`, pill trạng thái và màu riêng | `Array.forEach`, `createElement`, `textContent`, `classList` | Trang tải (`init()`) |
| 2 | Chọn một yêu cầu → panel "Phản hồi từ chuyên gia" cập nhật tiêu đề, ngày, nội dung tư vấn, các bước xử lý và tài liệu PDF | event `click` + `keydown` (Enter/Space), `Array.find`, event delegation | Bấm vào một dòng trong danh sách |
| 3 | Nút "Xem thêm 7 ⌄" / "Thu gọn ⌃" giới hạn 5 bản ghi | biến trạng thái `showAll`, `Array.slice`, `hidden` | Bấm nút cuối card danh sách |
| 4 | Ba thẻ mức độ khẩn cấp bằng `radio` thật, tô màu theo mức đã chọn | `input[type=radio]`, `classList`, `name` group | Bấm "Bình thường / Khẩn cấp / Báo động" |
| 5 | Đếm ký tự thời gian thực của ô mô tả (`maxlength="300"`) | event `input`, `String.length`, `textContent` | Gõ vào ô mô tả |
| 6 | Validation: tiêu đề ≥ 5 ký tự, mô tả ≥ 20 ký tự, bắt buộc tick đồng ý bảo mật | `if/else`, `trim`, `checked`, `classList.add('is-invalid')` | Bấm "Gửi yêu cầu" khi dữ liệu chưa hợp lệ |
| 7 | Gửi yêu cầu mới → tạo mã `HS-2026-00NN` tăng dần, thêm vào đầu danh sách, mở panel ở trạng thái "đang chờ tiếp nhận" | `Array.unshift`, `String.padStart`, `new Date()`, `form.reset()` | Điền form hợp lệ rồi bấm "Gửi yêu cầu" |
| 8 | Kéo–thả / chọn ảnh đính kèm, xem trước và loại từng ảnh | `FileReader.readAsDataURL`, event `drop`/`dragover`, `Array.splice` | Thả ảnh vào vùng nét đứt |
| 9 | "Đánh dấu đã đọc" đổi pill sang "Đã đọc" và khoá nút | `Array.find`, `disabled`, `textContent` | Bấm nút trong panel chuyên gia |
| 10 | "Đóng" panel → lưới chuyển 3 cột xuống 2 cột; bấm dòng khác mở lại | `classList.add/remove`, CSS Grid | Bấm ✕ hoặc "Đóng" |
| 11 | Dropdown thông báo, toggle sidebar mobile, toast phản hồi | `stopPropagation`, `classList.toggle`, `setTimeout` | Bấm 🔔 / ☰ / sau mỗi thao tác |

## 8. Công nghệ sử dụng

- **HTML5** — semantic: `aside`, `nav`, `header`, `main`, `section`, `article`, `figure`, `table`, `form`, `label`, `input`, `footer`
- **CSS3** — CSS Variables, CSS Grid, Flexbox, `:hover`, `@keyframes`, `border-radius` tạo khối ảnh dạng chiếc lá, SVG inline (icon, sóng, cành lá, bông lúa), media query
- **JavaScript thuần** — không framework, không backend; đăng nhập/đăng ký chỉ mô phỏng, dữ liệu hồ sơ để trong `localStorage`

## 9. Cấu trúc thư mục

```
.
├── index.html          # Trang chính: Season Dashboard
├── diary.html          # Nhật ký (thêm/sửa/xoá, tìm kiếm, lọc, AI)
├── support.html        # Yêu cầu hỗ trợ kỹ thuật (form + danh sách + panel chuyên gia)
├── login.html          # Đăng nhập cho nông dân
├── register.html       # Đăng ký tài khoản nông dân
├── css/
│   ├── layout.css      # Sidebar + khung trang dùng chung cho 3 trang bên trong app
│   ├── style.css       # Style Season Dashboard
│   ├── diary.css       # Style trang nhật ký
│   ├── support.css     # Style trang hỗ trợ kỹ thuật
│   ├── login.css       # Style trang đăng nhập
│   └── register.css    # Style trang đăng ký
├── js/
│   ├── main.js         # Logic Season Dashboard
│   ├── diary.js        # Logic trang nhật ký
│   ├── support.js      # Logic trang hỗ trợ kỹ thuật
│   ├── login.js        # Logic trang đăng nhập
│   └── register.js     # Logic trang đăng ký
├── assets/
│   ├── hero-rice.png
│   ├── login-farmer.png
│   ├── register-field.png
│   ├── avatar-farmer.png
│   ├── logo.svg
│   └── favicon.svg
└── README.md
```

### Sidebar dùng chung (`css/layout.css`)

Ba trang bên trong app (`index.html`, `diary.html`, `support.html`) dùng **một file sidebar chung** là `css/layout.css`, được `<link>` **trước** file CSS riêng của từng trang:

- Nội dung: khung `.app` (lưới sidebar + nội dung), `.sidebar`, `.brand`, `.side-nav`, `.side-link`, `.btn-logout`, nút ☰ và đoạn `@media (max-width: 760px)` đẩy sidebar ra ngoài màn hình.
- `position: sticky; top: 0; height: 100vh` + `.side-nav { flex: 1 }` ⇒ nút **Đăng xuất** luôn nằm sát đáy sidebar và đứng yên khi cuộn trang.
- Markup sidebar ở 3 trang giống hệt nhau, chỉ khác 2 điểm: link đang ở trang nào thì mang `is-active` + `aria-current="page"`, và trên `index.html` các link trỏ `#section-...` (cuộn trong trang) thay vì `index.html#section-...`.
- Trang đăng nhập/đăng ký không có sidebar nên không dùng file này.

## 10. Responsive

| Màn hình | Season Dashboard | Trang đăng nhập / đăng ký |
| --- | --- | --- |
| > 1180px (Desktop) | Sidebar cố định 244px; hero 8 cột + thời tiết 4 cột; AI 5fr + cột phải 7fr | 2 cột: ảnh dạng chiếc lá 42% + cột form; nét viền cong ôm cạnh phải ảnh |
| ≤ 1180 / 980px (Tablet) | Hero và thời tiết chiếm toàn hàng; AI và cột phải xếp chồng | 1 cột: ảnh thành banner bo góc 520px, form bên dưới, bỏ nét viền cong |
| ≤ 760 / 560px (Mobile) | Sidebar ẩn, mở bằng nút ☰; mọi card 1 cột; 2 CTA xếp dọc; bảng cuộn ngang | Ảnh 220px, card giảm padding, nút đăng ký chiếm 100% bề rộng, ẩn hình vẽ phụ |

Riêng `diary.html`: > 1300px mỗi dòng nhật ký có khối trạng thái + nút Sửa/Xóa ở cạnh phải; ≤ 1300px khối đó xuống dòng dưới để phần mô tả không bị ép hẹp; ≤ 1120px form và danh sách xếp chồng; ≤ 760px sidebar thành drawer, thumbnail chiếm toàn chiều ngang card.

Riêng `support.html`: > 1240px lưới 3 cột (form 1.25fr + danh sách 0.85fr + panel chuyên gia 1fr); ≤ 1240px panel chiếm nguyên hàng dưới form và danh sách (2 cột); ≤ 1024px mọi khối xếp thành 1 cột; ≤ 760px sidebar thành drawer mở bằng ☰, thẻ mức độ khẩn cấp xếp dọc.

## 11. Chạy thử

```bash
python -m http.server 8000
# Đăng ký:     http://localhost:8000/register.html
# Đăng nhập:   http://localhost:8000/login.html     (nongdan / 123456)
# Trang chủ:   http://localhost:8000/index.html
# Nhật ký:     http://localhost:8000/diary.html     (hoặc bấm "Nhật ký" ở sidebar)
# Hỗ trợ TK:    http://localhost:8000/support.html   (hoặc bấm "Hỗ trợ" ở sidebar)
```

Hoặc mở trực tiếp các file `.html` bằng trình duyệt.

## 12. Link bài nộp

| Nội dung | Link |
| --- | --- |
| Link Figma | _Điền link Figma của bạn_ |
| Link video | _Điền link video — tên file: `CSE122-GK-SVxx-TenSinhVien-SeasonDashboard`_ |
| Link GitHub | https://github.com/daohuychien07-dotcom/BTCKcuachien |

## 13. Ghi chú cho phần thuyết minh

- Dữ liệu mùa vụ, thời tiết, thông báo và nhật ký là hằng số khai báo ở đầu `js/main.js` (`season`, `activities`, `notifications`), không có database.
- Chức năng "Chẩn đoán sơ bộ" **mô phỏng** kết quả AI bằng `setTimeout` và sinh điểm tin cậy ngẫu nhiên 82–96%. Ảnh người dùng **không** rời khỏi máy: `FileReader` chỉ đọc ảnh thành data URL để hiển thị trong trình duyệt.
- Trang đăng nhập **không có xác thực thật**: so sánh chuỗi với mảng `accounts` rồi chuyển hướng bằng `window.location.href`.
- Trang đăng ký lưu hồ sơ vào `localStorage` (khoá `smartagri_accounts`) chỉ để demo việc chống trùng số điện thoại; dữ liệu nằm trên máy người xem, không gửi đi đâu. Khi thuyết trình cần nói rõ đây là luồng giao diện, chưa phải bảo mật.
- Trang hỗ trợ kỹ thuật (`support.html`) giữ toàn bộ yêu cầu trong mảng `requests` ở đầu `js/support.js`; 7 yêu cầu mẫu và mọi phản hồi của "chuyên gia" đều là dữ liệu dựng sẵn để demo giao diện. Gửi yêu cầu mới chỉ thêm phần tử vào mảng trong phiên hiện tại, **không** lưu và **không** gửi đi đâu — tải lại trang sẽ mất.
- Tên nông dân, mã số và thông tin liên hệ đang là dữ liệu mẫu.
