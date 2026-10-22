# Bio Discord — Cloudflare Workers

Website là HTML/CSS/JavaScript tĩnh. Nội dung được đặt trong `public/`; không có framework, build step, database hay server-side API. `wrangler.jsonc` cấu hình Workers Static Assets và trang 404 trả đúng mã 404. Không bật `run_worker_first`, nên file tĩnh được Cloudflare phục vụ mà không gọi Worker script.

## Chạy thử

```bash
npm install
npm run dev
```

## Kiểm tra và deploy

```bash
npm run check
npx wrangler login
npm run deploy
```

`npm run check` là dry-run, không publish. Sau deploy, mở domain `workers.dev` để thử `/`, `/assets/fog-discord-mark.svg`, `/.well-known/discord`, `/robots.txt`, `/sitemap.xml` và một URL không tồn tại. URL không tồn tại phải hiển thị `public/404.html` và trả HTTP 404.

## Gắn domain riêng

Trong Cloudflare Dashboard, mở **Workers & Pages → bio-discord → Settings → Domains & Routes** rồi thêm `meo-mate.site` làm Custom Domain. Không thay DNS hoặc gỡ hosting hiện tại cho đến khi preview trên `workers.dev` đã qua kiểm tra và đã xác nhận phương án chuyển domain. Bật **Always Use HTTPS** ở Cloudflare để yêu cầu HTTP chuyển sang HTTPS. Không cấu hình `www` trừ khi bản ghi DNS và redirect cho `www.meo-mate.site` đã được tạo.

Nếu triển khai qua Git, giữ file `wrangler.jsonc` ở thư mục gốc. Đây là static-only Worker; không đặt build command hoặc cấu hình entrypoint ứng dụng. Wrangler là công cụ deploy/dev, không được gửi vào thư mục public.

## Lưu ý dịch vụ bên ngoài

Trang vẫn gọi Google Fonts, Discord CDN, Lanyard và YouTube trực tiếp từ trình duyệt. Các request này không đi qua Workers của `meo-mate.site`; lỗi hoặc giới hạn ở các dịch vụ đó không thể xác định từ số liệu request 2xx/3xx/4xx của zone này. Nếu trạng thái Discord lỗi hoặc quá nhiều request, kiểm tra DevTools Network và Lanyard riêng.
