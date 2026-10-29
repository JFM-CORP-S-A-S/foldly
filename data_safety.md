# Data Safety — Foldly (com.paper.folding.foldly)

Nguồn: manifest + gradle audit (xem CONTENT_READY.md). Dùng để điền Play Console > Data safety.

## Khai báo chung
- Thu thập hoặc chia sẻ dữ liệu người dùng bắt buộc: **Yes**
- Mã hoá dữ liệu khi truyền: **Yes** (HTTPS/TLS qua SDK)
- Cho phép người dùng yêu cầu xoá dữ liệu: **Yes** (dữ liệu local: xoá storage/gỡ app; dữ liệu SDK: liên hệ christensenviviana605730@gmail.com; không có tài khoản)
- Tài khoản: **Không có** — không cần Account deletion URL (app không tạo tài khoản)
- Mua hàng trong app: **Không** (v1 frictionless; BILLING chỉ là hạ tầng thư viện, không có SKU bán)
- Nhắm trẻ em: **Không** (không phải app Families; target age 13+)

## Bảng dữ liệu

| Loại dữ liệu (Play) | Thu thập | Chia sẻ | Bắt buộc/tuỳ chọn | Mục đích | Nguồn |
|---|---|---|---|---|---|
| Device or other IDs — Advertising ID | Có | Có (AdMob, mediation, attribution) | Bắt buộc (với app có ads) | Advertising or marketing; Analytics; Fraud prevention | AD_ID permission, module-ads, Adjust/Meta/TikTok SDK |
| App activity — App interactions | Có | Có (Google/Firebase, ad/attribution) | Bắt buộc | Analytics; Advertising or marketing; App functionality | Firebase Analytics (Telemetry.kt), ad SDK |
| App info and performance — Crash logs | Có | Có (Google/Firebase) | Bắt buộc | Analytics (diagnostics) | Firebase Crashlytics |
| App info and performance — Diagnostics | Có | Có (Google/Firebase, ad SDK) | Bắt buộc | Analytics; App functionality | Firebase Crashlytics / Performance của ads SDK |
| Device or other IDs — Firebase installation ID / push token | Có | Có (Google) | Bắt buộc | Analytics; App functionality (push) | Firebase Analytics, FCM |
| Location — Approximate location | Có (suy ra từ IP bởi SDK; app KHÔNG xin quyền vị trí) | Có (ad SDK) | Bắt buộc | Advertising or marketing; Analytics | Ad SDK / Firebase (IP-derived) |
| Photos and videos | **Không** thu thập (ảnh chụp camera lưu local, không rời máy; chỉ chia sẻ khi người dùng chủ động share) | Không | — | — | CameraX, filesDir/creations |
| Personal info (tên, email, số điện thoại, địa chỉ) | **Không** | Không | — | — | Không có login/tài khoản |
| Financial info / Purchases | **Không** | Không | — | — | Không IAP |
| Contacts, Messages, Audio, Files, Health, Web browsing | **Không** | Không | — | — | — |

## Ghi chú xử lý data
- Dữ liệu chỉ ở máy (không phải "collected" theo định nghĩa Play): favourites, progress, completed, wallet(coin), game, creations (ảnh), reminder, settings (DataStore + filesDir).
- Ảnh camera/gallery: xử lý trên máy (cacheDir/filesDir). Không upload.
- Nội dung origami tải từ CDN (CDN host của nội dung): app chỉ gửi request GET tiêu chuẩn (IP bị CDN nhìn thấy).
- UMP consent (Google) chạy trước quảng cáo cho EEA/UK.
- Phần "chia sẻ" Advertising ID: Play coi chuyển sang service provider xử lý thay mặt (analytics/crash) là không "share", nhưng ads/attribution là "share" — khai là Có để an toàn.

## Quyền (manifest) → ghi chú Play
- AD_ID: khai "Yes" ở mục Advertising ID trong App content.
- CAMERA, POST_NOTIFICATIONS: runtime, tuỳ chọn.
- RECEIVE_BOOT_COMPLETED, VIBRATE, BILLING, ACCESS_ADSERVICES_*: hạ tầng.
