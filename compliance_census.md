# COMPLIANCE CENSUS — Foldly (appCode 05f · com.paper.folding.foldly)

Run ASO job 492 · store a019 JFM CORP S.A.S · 2026-10-29.
Nguồn bằng chứng: `app/src/main/AndroidManifest.xml`, merged manifest `app/build/intermediates/merged_manifests/prodDebug/…/AndroidManifest.xml`, `app/build.gradle.kts`, `gradle/libs.versions.toml`, source `app/src/main/java/com/paper/folding/origamilab/**`. Mỗi trục dán grep thật — census KHÔNG suy từ trí nhớ.

App = hướng dẫn gấp giấy (origami · kirigami · 3D origami) từng bước có ảnh + clip video cắt đúng bước, mini-game gấp giấy, camera chụp thành phẩm, gallery sáng tạo, coin economy (coin KIẾM trong app). Bản v1 FREE, mọi mẫu mở sẵn, không IAP tiền thật, không login. targetSdk=36, minSdk=24, applicationId=com.paper.folding.foldly (namespace base = origamilab).

---

## 1. IAP / billing — KHÔNG có bán hàng thật ⇒ KHÔNG trang refund, KHÔNG khai IAP

**Bằng chứng:**
- Manifest: `com.android.vending.BILLING` CÓ (comment nêu rõ "hạ tầng module-ads/IAP của code_base… App v1 KHÔNG bán gì: không paywall, không nút mua").
- Billing chỉ đến từ lib ads: `GlobalApp.kt:12 import com.bbl.ads.modules.billing.AppPurchase`, `GlobalApp.kt:121 initBilling()`.
- `ads/AdsLoader.kt:57 isPurchased() = AppPurchase.getInstance().isAdFree.value || SettingManager.isPurchased.value` → billing CHỈ dùng cho cờ ad-free.
- KHÔNG màn/nút MUA tiền thật: grep `launchBillingFlow` / `.purchase(` trong UI = 0 (chỉ có telemetry helper `Telemetry.kt:iapPurchaseStart`, KHÔNG được gọi).
- `features/store/CoinStoreScreen.kt` = coin economy ẢO: coin KIẾM bằng chơi game / rate (`EarnRow`, `NotEnoughCoinsDialog.onPlay → openTab(GAME)`), tiêu coin mở theme cosmetic (`PlayerRepo.buyTheme(id, price)`, price tính bằng coin so với `wallet.coins`). KHÔNG tiền thật.

→ Quyết định: KHÔNG trang refund, KHÔNG khai IAP. GIỮ dep lib ads (ngoại lệ hạ tầng FRICTIONLESS_ACCESS §3). Listing KHÔNG dùng "free" gây nhầm là không ads.

## 2. Nội-dung-user — local-only ⇒ KHÔNG trang DMCA

**Bằng chứng:**
- Nội dung mẫu = manifest BBL CDN (read-only, `survey/api/manifest_api.md`; 159 mẫu) — app TIÊU THỤ nội dung, user KHÔNG upload lên server.
- Ảnh thành phẩm user tự chụp lưu cục bộ (`features/camera/*`, `features/creations/*`), share qua `FileProvider` do user chủ động — KHÔNG rời máy lên server app (app KHÔNG backend). Grep endpoint upload/host = 0.

→ Quyết định: Local-only. Disclosure trong description. Mục Copyright/IP trong Terms. KHÔNG trang DMCA.

## 3. Data & privacy — có SDK thu thập, KHÔNG account/login ⇒ KHÔNG delete-account

**Bằng chứng:**
- SDK (`build.gradle.kts` + `libs.versions.toml`): firebase-analytics, firebase-crashlytics, firebase-config (RC), firebase-messaging (FCM), adjust-android 5.7.0, facebook-android-sdk 18.3.0, TikTok Business SDK 1.5.0 (attribution — GlobalApp.kt:79 setTiktokAppId, init thật), user-messaging-platform 4.0.0 (UMP consent), AdMob Next-Gen + AppLovin + Facebook mediation.
- KHÔNG login/account: grep `signIn|FirebaseAuth|GoogleSignIn|signUp|createUser|oauth` = 0. Không SDK auth.
- Advertising ID: `com.google.android.gms.permission.AD_ID` + `ACCESS_ADSERVICES_*`.

→ Quyết định: Privacy đúng hành vi (analytics/crash + Advertising ID). KHÔNG delete-account. Data Safety khai analytics/crash/diagnostics + Advertising ID (Shared/Advertising), KHÔNG user-id/email/account. Khớp FRICTIONLESS_ACCESS.

## 4. API nhạy cảm — CAMERA + POST_NOTIFICATIONS runtime chuẩn ⇒ KHÔNG cần Play declaration form

**Bằng chứng (merged prod uses-permission):**
INTERNET, ACCESS_NETWORK_STATE, ACCESS_WIFI_STATE, CAMERA, POST_NOTIFICATIONS, RECEIVE_BOOT_COMPLETED, VIBRATE, WAKE_LOCK, FOREGROUND_SERVICE, READ_BASIC_PHONE_STATE, AD_ID, ACCESS_ADSERVICES_*, BILLING, CHECK_LICENSE, c2dm RECEIVE, BIND_GET_INSTALL_REFERRER, + perm mediation (amazon/samsung/applovin).

- CAMERA — DÙNG THẬT: `features/camera/CameraScreen.kt` (CameraX Preview + ImageCapture, rationale → RequestPermission(CAMERA)), `PhotoPreviewScreen.kt`. Chụp ảnh thành phẩm ở màn Congrats. `uses-feature camera required=false` (fallback "Choose from Gallery"). Listing ĐƯỢC nêu "chụp ảnh thành phẩm".
- POST_NOTIFICATIONS + RECEIVE_BOOT_COMPLETED: nhắc gấp giấy hằng ngày (`core/ReminderScheduler.kt`, `ReminderReceiver`, `BootReceiver`) + FCM (`features/notification/OrigamiMessagingService`). Hợp lệ.
- KHÔNG perm đòi declaration form: grep merged = 0 với READ_CONTACTS, ACCESS_FINE/COARSE_LOCATION, READ_MEDIA_*, SMS, READ_CALL_LOG, SYSTEM_ALERT_WINDOW, PACKAGE_USAGE_STATS, QUERY_ALL_PACKAGES, BIND_ACCESSIBILITY_SERVICE, REQUEST_INSTALL_PACKAGES. KHÔNG foregroundServiceType.

→ Quyết định: KHÔNG cần permission-declaration form + KHÔNG video demo quyền. `permissions_declarations.md` ghi CAMERA + POST_NOTIFICATIONS là standard-runtime (justification + câu listing + "no Console form required").

## 5. Data Safety — nháp khai theo SDK

Bản đồ SDK→data-type: Firebase Analytics + Adjust + Facebook → App activity, App info/performance; Crashlytics → Crash logs, Diagnostics; Ads → Advertising ID (Device/other IDs), Shared/Advertising; Camera → ảnh on-device, user chủ động share, app KHÔNG thu thập/gửi (no backend) ⇒ KHÔNG khai Photos "collected". KHÔNG personal info/location/contacts.

→ Quyết định: `data_safety.csv` khai App activity, App info/performance (diagnostics/crash), Device/other IDs (Advertising ID — Shared/Advertising). KHÔNG over-declare.

## 6. Ads — CÓ ⇒ "Contains ads"

Bằng chứng: module-ads `com.bbl.ads` + AdMob APPLICATION_ID (manifest meta) + mediation AppLovin/Facebook + attribution Adjust/Facebook/TikTok. → "Contains ads" = Yes.

## 7. Đối tượng — general audience ⇒ Everyone, KHÔNG child-safety

Bằng chứng: `product_foundation.md` segment = người gấp giấy thư giãn + cha mẹ gấp cùng con; nội dung lành mạnh, không UGC/social server. KHÔNG nhắm RIÊNG trẻ em. → Everyone rating. KHÔNG trang child-safety (không Designed for Families, không UGC).

## 8. Appearance variants — ĐÃ CHỐT

order.md `design_variants: 1`. Caller chốt variant thị giác ASO = SKY_STUDIO (teal #0CA6AE + tím #7D2AE7). Không ambiguity ⇒ KHÔNG dừng hỏi; mọi ảnh ASO dùng SKY_STUDIO.

## 9. Call Log/SMS handler-gate & Minimize (16909972)

Merged manifest KHÔNG có call-log/SMS/contacts/location/media. targetSdk=36 (<37). → Không áp; minimize chưa bắt buộc, không perm nào trong diện này.

---

## KẾT LUẬN CENSUS

Trang legal cần dựng: `privacy` + `terms` (kèm mục Copyright/IP). KHÔNG refund/dmca/delete-account/child-safety.

Disclosure trong description: (a) Contains ads; (b) nội dung origami do app cung cấp + ảnh thành phẩm lưu cục bộ; (c) mọi mẫu mở sẵn miễn phí (không ad-gate/coin để mở mẫu) — coin chỉ mở theme cosmetic, kiếm bằng chơi; (d) camera chụp ảnh thành phẩm (tuỳ chọn, fallback gallery).

Mục audit: Privacy Policy PASS (URL live), Data Safety khớp census, KHÔNG claim sai (không IAP, không login, không feature ảo), Contains-ads đúng, Everyone rating.

Submission assets: `data_safety.csv` + `data_safety.md`; `permissions_declarations.md` (CAMERA + POST_NOTIFICATIONS, no-Console-form). KHÔNG video demo quyền.

---

## 10. BỔ SUNG của ASO run 492 (aso-expert) — phát hiện thêm, kèm bằng chứng

**10a. Catalog nội dung có mẫu mang IP bên thứ ba + mẫu vũ khí (rủi ro CONTENT, không phải listing).**
Bằng chứng: `app/src/main/assets/manifest_seed.json` (159 mục; đếm `name`/`category`): tên mẫu `Naruto 3D`, `Naruto Shuriken`, `Rasengan Shuriken`, `Pokeball 3D` (nhãn IP anime/game); category `Gun` (6 mẫu: AK Rifle, Handgun, Shotgun, Double-Barrel Pistol, Scattergun, Toy Double-Barrel Gun), `Weapons` (10 mẫu: Sword, Karambit, Knife, Hatchet, Kunai, Saber…), `Ninja` (9). Tab "3D Origami" trong Discover hiển thị "Naruto 3D" (ảnh chụp emulator).
→ Hệ quả: (1) listing + screenshot ĐÃ tránh: không nêu tên IP, không chụp tab 3D Origami, không nhắc vũ khí; (2) khai Content rating (IARC) PHẢI trung thực về "weapons" — rất có thể KHÔNG ra "Everyone" thuần (kỳ vọng Everyone 10+/tương đương); KHÔNG khẳng định rating trong listing; (3) khuyến nghị cho chủ sản phẩm: ẩn khỏi catalog (CDN manifest `hasVideo/visible`) các mẫu mang tên thương hiệu bên thứ ba (Naruto/Pokeball) trước khi phát hành — tên nhân vật có bản quyền trong nội dung app có thể bị Play xét IP/impersonation dù listing sạch.

**10b. Clip video theo bước KHÔNG có ở mọi mẫu** (kiểm trên seed): `hasVideo=true` + mọi bước có `startTimeMs/endTimeMs` ở 51 mẫu origami + 7 mẫu 3D = ~58 mẫu; kirigami 0/39; 87+ mẫu origami không clip. ⇒ Listing KHÔNG được nói "mỗi bước/mọi mẫu có clip" — dùng "video clips on 50+ models" (đúng tại bundle seed; manifest live trên CDN có thể nhiều hơn, KHÔNG ít hơn). Sửa luôn brief USP cho khớp.

**10c. Data Safety — Approximate location.** Privacy Policy đã công bố "IP address and approximate location derived from it (country/region level)" cho Ad SDK/Firebase/CDN ⇒ Data Safety khai **Approximate location** (Collected + Shared, Analytics/Advertising) để khớp Privacy (Play cross-check).

**10d. Đối tượng.** Privacy Policy §11 nêu app KHÔNG nhắm trẻ <13 ⇒ Target audience trong Play Console phải chọn 13+ (không "Children"/Families), listing KHÔNG dùng từ "kids/children". (product_foundation nêu phụ huynh gấp cùng con — chỉ là persona, không phải đối tượng khai báo.)

**10e. Permission manifest vs Privacy.** Merged manifest còn `READ_BASIC_PHONE_STATE`, `ACCESS_ADSERVICES_TOPICS`, `ACCESS_ADSERVICES_CUSTOM_AUDIENCE`, `WAKE_LOCK`, `FOREGROUND_SERVICE` (không có `foregroundServiceType`) — đều normal/không form khai báo, nhưng bảng "Permissions used" của Privacy chưa liệt kê ⇒ khuyến nghị bổ sung (không chặn).

**10f. Icon launcher legacy.** `mipmap-*/ic_launcher*.webp` (API 24–25) vẫn là hạc cam kem của master Origami Lab; adaptive icon (API 26+) dùng `variant_icon_bg` + `m1_icon` (teal→tím) — icon store 512 khớp adaptive (đa số thiết bị). Khuyến nghị cập nhật mipmap legacy để máy API 24–25 cũng khớp (ngoài phạm vi ghi file app của ASO).
