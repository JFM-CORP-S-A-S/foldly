# Review notes / Testing instructions — Foldly (dán vào Play Console · App access & Notes for reviewers)

**Marketing URL:** https://jfm-corp-s-a-s.github.io/foldly/ (landing marketing LIVE, HTTP 200, title "Foldly" — publish 2026-10-29)
**Support / Privacy / Terms URL:** https://jfm-corp-s-a-s.github.io/foldly/privacy/ · https://jfm-corp-s-a-s.github.io/foldly/terms/
**Liên hệ:** JFM CORP S.A.S · christensenviviana605730@gmail.com

## App access
Toàn bộ chức năng dùng được **không cần tài khoản/đăng nhập/mã** — mở app là dùng. Cần internet để tải hướng dẫn (ảnh/video) và quảng cáo.

## Mục đích & đối tượng
Foldly là app hướng dẫn gấp giấy (origami, kirigami, 3D origami) từng bước cho **người mới và người yêu thủ công (13+)**: gấp thư giãn buổi tối/cuối tuần, một tay cầm điện thoại tay kia cầm giấy.

## Pain-point & cách giải
Người gấp hay **bị kẹt ở đúng một bước** vì ảnh tĩnh không đủ, còn video tổng quát phải tua mãi; nhiều app cùng ngành còn chặn mẫu bằng quảng cáo/tiền ảo. Foldly: mỗi bước một màn riêng (mũi tên + nét gấp), nút **"Watch this step"** phát đúng đoạn clip của bước đang kẹt (có ở 50+ mẫu), **mọi hướng dẫn mở sẵn** — không xem ad hay tốn coin để mở mẫu.

## Khác biệt cơ chế (đích danh)
1) Clip cắt đúng từng bước (`startTimeMs/endTimeMs` mỗi bước), một chạm; 2) không ad-gate/coin-gate để mở hướng dẫn; 3) không tài khoản; 4) Mini Fold: gấp tờ giấy để lộ ảnh ẩn; 5) chụp ảnh thành phẩm lưu cục bộ (Creations).

## Hướng dẫn test (không cần dữ liệu seed)
1. Mở app → đi qua các màn mở đầu (ngôn ngữ/giới thiệu) → Home.
2. Home → hero "Whale" → **Start fold** (hoặc Discover → chọn mẫu → Start fold). Màn gấp: bấm **Next step**; bấm **Watch this step · 0:05** để xem clip của bước (cần mạng). Nút "?" trên màn gấp mở bảng "Basic folds" (kiểu gấp cơ bản).
3. Đến bước cuối bấm **Finish & celebrate** → Congrats → **Take photo** (cấp quyền Camera; hoặc **From gallery**) → ảnh vào tab **Creations**.
4. Tab **Game** → Play level → vuốt/bấm mũi tên để gấp, lộ ảnh; hoàn thành level nhận coin (coin ảo, chỉ mở theme trong game).
5. Tab **Profile** → Language (12 ngôn ngữ), Daily reminder (bật/tắt; hỏi quyền Notification trên Android 13+).
6. Quảng cáo: app có quảng cáo (AdMob + mediation); màn đồng ý UMP hiện ở EEA/UK.

## Lưu ý trung thực
Không có mua trong app (v1), không đăng nhập. Billing trong manifest chỉ là hạ tầng thư viện quảng cáo (cờ ad-free), không có màn/nút mua. Camera/Notification là quyền tuỳ chọn.
