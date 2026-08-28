# MyDrama Ku — Android APK (WebView Wrapper)

WebView wrapper untuk https://drama-tau-virid.vercel.app, dibuat dari template ZeruSoft-APK-Builder.

## Cara build APK

1. Push folder ini ke repo GitHub baru (atau repo yang sudah ada).
2. Buka tab **Actions** di repo → pilih workflow **🚀 Build APK** → **Run workflow**.
   - Isi `version_name` (mis. `1.0`) dan `version_code` (mis. `1`), lalu jalankan.
3. Setelah selesai, unduh artifact **MyDramaKu-APK** dari halaman run tersebut.
   - Isinya 2 file: `-debug-` (bisa langsung diinstal untuk testing) dan `-release-`
     (perlu keystore, lihat di bawah).

## Supaya APK release bisa "update install" konsisten (opsional tapi disarankan)

Tanpa keystore release, APK release akan otomatis fallback ke debug key —
tetap bisa diinstal, tapi tiap kali build ulang APK dianggap "aplikasi
berbeda" oleh Android (harus uninstall dulu sebelum instal versi baru).

Untuk keystore permanen:

```bash
keytool -genkey -v -keystore mydramaku-release.jks \
  -keyalg RSA -keysize 2048 -validity 10000 -alias mydramaku
```

Lalu tambahkan 4 secrets ini di **Settings → Secrets and variables → Actions**:

| Secret | Isi |
|---|---|
| `MYDRAMAKU_KEYSTORE_B64` | hasil `base64 -w0 mydramaku-release.jks` |
| `MYDRAMAKU_KEYSTORE_PASSWORD` | password keystore |
| `MYDRAMAKU_KEY_ALIAS` | `mydramaku` (atau alias yang kamu pakai) |
| `MYDRAMAKU_KEY_PASSWORD` | password key |

**Simpan file `.jks` ini baik-baik di luar repo** — kalau hilang, semua
update APK berikutnya nggak akan bisa menimpa instalasi lama.

## Yang sudah disesuaikan dari template ZeruSoft

- Package: `com.zerusoft` → `com.mydramaku.app`
- URL yang dibuka: `https://drama-tau-virid.vercel.app`
- Nama app: `MyDrama Ku`
- Warna status bar / tema: `#050506` (bg) + `#FFC727` (gold), disamakan dengan tema situs
- Icon launcher: dari logo segitiga emas yang kamu kirim, sudah di-generate ke semua ukuran mipmap (mdpi–xxxhdpi) + versi bulat (`ic_launcher_round`)
- Nama secrets GitHub & prefix nama file APK: `ZERUSOFT_*` → `MYDRAMAKU_*`, `ZeruSoft-v...` → `MyDramaKu-v...`
