# XanhGO
# PhuongSM — Hướng dẫn triển khai & giới hạn thật của trình duyệt

## Cách chạy ngay
Mở `index.html` — mọi tính năng tính cước, lưu lịch sử, hoá đơn, xuất PDF/Excel/Word, in, QR, chia sẻ đều chạy thật ngay trong file này.

## Để cài đặt như một ứng dụng thật (installable PWA) trên điện thoại
Trình duyệt chỉ cho phép "Cài đặt ứng dụng / Thêm vào màn hình chính" khi 3 file dưới đây được **host trên một tên miền HTTPS thật** (không thể làm điều này ngay trong khung xem trước của Claude vì đó là iframe):
1. `index.html`
2. `manifest.json`
3. `sw.js` (service worker, giúp dùng ngoại tuyến)

Cách làm nhanh (miễn phí):
- Kéo cả 3 file vào [Netlify Drop](https://app.netlify.com/drop) hoặc GitHub Pages / Vercel.
- Sau khi có link HTTPS, mở link đó trên điện thoại → trình duyệt sẽ tự hiện nút "Cài đặt ứng dụng" (nút ⇩ trong app cũng sẽ hoạt động thật lúc đó).
- Thêm 2 icon `icon-192.png` và `icon-512.png` vào cùng thư mục để hiển thị icon đẹp khi cài.

## Những gì trình duyệt KHÔNG cho phép làm thật (nói thẳng, không giả lập)
- **Tự động bật độ sáng màn hình tối đa**: không có API web nào cho phép JavaScript chỉnh độ sáng phần cứng của thiết bị. App dùng **Wake Lock API thật** để giữ màn hình luôn bật sáng (không tắt/khoá) khi mở mã QR — đây là điều gần nhất trình duyệt cho phép.
- **Thông báo nền (push) khi tắt trình duyệt**: cần máy chủ đẩy (push server) thật. App đã bật **Notification API thật** — bạn sẽ nhận thông báo thật khi lưu chuyến đi trong lúc tab đang mở. Để có thông báo nền khi đóng app, cần triển khai backend riêng.
- **Bản đồ chỉ vẽ được khi thiết bị/mạng cho phép gọi OpenStreetMap/Nominatim** — nếu mạng công ty/thiết bị chặn, ô nhập quãng đường (km) vẫn hoạt động 100% để tính giá thủ công.

## Dữ liệu được lưu ở đâu, có mất không?
Lịch sử chuyến đi và số lượt xem dùng `window.storage` (lưu trữ bền vững của Claude) với cơ chế dự phòng `localStorage`. Số lượt xem dùng khoá **dùng chung (shared)** nên mọi người mở link đều thấy cùng một con số cộng dồn, không bị đặt lại về 0 khi tải lại trang.
