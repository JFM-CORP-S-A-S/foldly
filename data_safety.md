# Data Safety — Foldly (com.paper.folding.foldly) · nguồn: compliance_census.md §3/§5/§10c · file import: `data_safety.csv`

Cách nạp: Play Console → App content → Data safety → **Import CSV** (`blueprint/aso/data_safety.csv`; chỉ cột *Response value* được điền, ID/thứ tự dòng giữ nguyên template `instructions/aso/data_safety_template.csv`, 782 dòng, 38 dòng có giá trị).

## Câu top-level
| Câu | Khai | Bằng chứng |
|---|---|---|
| Collects/shares required user data types | **true** | SDK Firebase/Adjust/Meta/TikTok/AdMob gửi dữ liệu rời máy (build.gradle.kts + libs.versions.toml) |
| Encrypted in transit | **true** | Firebase/AdMob/CDN đều HTTPS/TLS (Privacy §7) |
| Account creation | **None** (`PSL_ACM_NONE`) | grep `FirebaseAuth\|signIn\|GoogleSignIn` trong app/src = 0 |
| Data deletion by user | **Yes** + URL `https://jfm-corp-s-a-s.github.io/foldly/privacy/` | Privacy §9: yêu cầu xoá qua email + xoá local bằng clear storage. **TODO người dùng:** nếu Console đòi URL trang chuyên biệt thì đổi; hoặc chọn "No" (app không có tài khoản) — hai phương án đều hợp lệ, chọn theo chính sách nội bộ |

## Data type đã khai (mọi mục: user control = **Required**, ephemeral = **No**)
| Data type | Collected | Shared | Purpose (collected) | Purpose (shared) | Bằng chứng / SDK |
|---|---|---|---|---|---|
| App activity → App interactions | ✔ | ✔ | Analytics | Analytics, Advertising | Firebase Analytics (collected); Adjust/Meta/TikTok attribution + ad SDK nhận event (shared) — Privacy §2.1 "App interactions", §4 |
| App info & performance → Crash logs | ✔ | — | App functionality, Analytics | — | Firebase Crashlytics (Google xử lý nhân danh dev ⇒ không "shared") |
| App info & performance → Diagnostics | ✔ | — | App functionality, Analytics | — | Crashlytics + Firebase Performance-type metrics; Privacy §2.1 |
| Device or other IDs | ✔ | ✔ | App functionality, Analytics, Advertising | Analytics, Advertising | Advertising ID (`com.google.android.gms.permission.AD_ID`, `ACCESS_ADSERVICES_AD_ID`), Firebase installation ID + FCM token (functionality), ad mediation/attribution (shared) |
| Location → Approximate location | ✔ | ✔ | Analytics, Advertising | Advertising | IP-derived country/region level (Privacy §2.1 dòng 2) — ad SDK + Firebase |

## KHÔNG khai (có lý do — tránh over-declare)
- Photos/Videos: ảnh camera xử lý + lưu **on-device**, share qua Android share sheet do user chủ động; app KHÔNG có backend upload (grep endpoint upload = 0) ⇒ không "collect".
- Name/Email/Phone/User IDs/Contacts/Financial/Purchase history/Health/Messages/Search history/Installed apps: không có (census §3-§4, manifest không có `QUERY_ALL_PACKAGES`, billing chỉ cờ ad-free).
- Files and docs: chỉ đọc item user chọn từ gallery qua picker, không upload.

## Đối chiếu Data Safety ↔ Privacy Policy ↔ Listing (Play cross-check)
- Privacy §2.1 liệt kê: Advertising ID/IDs ✔ · IP + approx location ✔ · device & app info (⇒ Diagnostics/Other app performance; đã khai Diagnostics) ✔ · app interactions ✔ · crash logs & diagnostics ✔ · Firebase installation ID + push token (Device IDs) ✔ · install/attribution ✔ ⇒ **KHỚP**.
- Privacy §2.2/§2.3: dữ liệu local + ảnh on-device, không upload ⇒ khớp việc không khai Photos ✔.
- Privacy §11 not directed to children <13 ⇒ Target audience = 13+ (KHÔNG Families). Listing không nói "kids" ✔.
- Listing nêu "Foldly contains ads", "No account", "Photos you take stay on your device" ⇒ nhất quán ✔.
- Lệch nhỏ (không chặn, khuyến nghị sửa Privacy): bảng Permissions §3 chưa liệt `READ_BASIC_PHONE_STATE`, `ACCESS_ADSERVICES_TOPICS/CUSTOM_AUDIENCE`, `WAKE_LOCK`, `FOREGROUND_SERVICE` có trong merged manifest.

## Việc người dùng làm trong Console
1. Import CSV, rà lại bảng xem Console hiển thị đúng 5 data type ở trên. 2. Ads declaration = **Contains ads**. 3. Target audience **13+**. 4. Content rating questionnaire — trả lời trung thực mục "weapons" (xem census §10a).
