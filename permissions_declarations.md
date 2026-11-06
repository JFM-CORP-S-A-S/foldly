# Permission declarations — Foldly · nguồn: compliance_census.md §4

**Kết luận: KHÔNG có permission/foreground-service nào cần form khai báo Play riêng ⇒ no Play Console declaration form required, no permission video required.** Không khai khống quyền thừa. (Grep merged manifest: 0 `foregroundServiceType`; 0 `READ_CONTACTS/LOCATION/READ_MEDIA_*/SMS/CALL_LOG/SYSTEM_ALERT_WINDOW/PACKAGE_USAGE_STATS/QUERY_ALL_PACKAGES/BIND_ACCESSIBILITY_SERVICE/REQUEST_INSTALL_PACKAGES/MANAGE_EXTERNAL_STORAGE`.)

## 1. `android.permission.CAMERA` (runtime, tiêu chuẩn)
- Feature người-dùng-thấy: màn Congrats → **Take photo** → `features/camera/CameraScreen.kt` (CameraX Preview + ImageCapture, có màn rationale rồi `RequestPermission(CAMERA)`); xem trước `PhotoPreviewScreen.kt`; lưu local vào Creations.
- Tuỳ chọn: `uses-feature android.hardware.camera required="false"`; từ chối quyền vẫn dùng **From gallery**.
- Câu trong listing (full_description): *"Finished a model? Take a photo of it with the camera (optional - or pick one from your gallery). Your photos are saved on your device in Creations…"* và *"The camera and notification permissions are optional … Photos you take stay on your device."*
- Declaration: **no Play Console declaration form required, no permission video required** (permission runtime chuẩn, không thuộc nhóm hạn chế).
- Data Safety: ảnh xử lý on-device, không upload ⇒ không khai Photos.

## 2. `android.permission.POST_NOTIFICATIONS` (runtime, tiêu chuẩn) + `RECEIVE_BOOT_COMPLETED`
- Feature: nhắc gấp giấy hằng ngày (bật/tắt trong Profile · "Daily reminder"; `core/ReminderScheduler.kt`, `ReminderReceiver`, `BootReceiver` dựng lại lịch sau khi khởi động lại) + FCM (`OrigamiMessagingService`). Android 13+ hỏi quyền runtime, có thể từ chối.
- Câu trong listing: *"Optional daily folding reminder"* + *"The camera and notification permissions are optional…"*
- Declaration: **no Play Console declaration form required, no permission video required** (Play không đòi video cho POST_NOTIFICATIONS; `RECEIVE_BOOT_COMPLETED` là normal permission).

## 3. Các quyền còn lại — normal/không form
INTERNET, ACCESS_NETWORK_STATE, ACCESS_WIFI_STATE, VIBRATE (haptic khi gấp), WAKE_LOCK, FOREGROUND_SERVICE (không có type ⇒ không form FGS), READ_BASIC_PHONE_STATE, `com.google.android.gms.permission.AD_ID` + `ACCESS_ADSERVICES_*` (khai trong Data Safety/Ads ID declaration — Console hỏi "Does your app use advertising ID?" ⇒ **Yes**, mục đích Advertising/Analytics), `com.android.vending.BILLING` (hạ tầng thư viện ads, KHÔNG có màn/nút mua — census §1; không khai IAP).

## TODO người dùng
- Console → App content → **Advertising ID**: chọn Yes (Advertising + Analytics + App functionality).
- Không cần quay video quyền. Nếu Console vẫn đòi form cho quyền nào, báo lại để bổ sung.
