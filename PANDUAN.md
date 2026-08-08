# Panduan: Deploy Stream ke Railway + Bungkus jadi APK Android

## TAHAP 1 — Deploy backend "Stream" ke Railway

1. Upload folder `Stream/` (hasil ekstrak Stream.zip) ke repo GitHub (bisa privat).
2. Buka https://railway.app → login → **New Project** → **Deploy from GitHub repo**.
3. Pilih repo Stream kamu. Railway akan otomatis mendeteksi `Dockerfile` dan build.
   - Tidak ada environment variable wajib (cek ulang kalau kamu menambah sendiri).
4. Setelah build sukses, buka tab **Settings → Networking** pada service tersebut →
   klik **Generate Domain**.
5. Kamu akan dapat URL publik, contoh:
   `https://stream-production-xxxx.up.railway.app`
6. Buka URL itu di browser, pastikan halaman `/app` (downloader) bisa dipakai normal
   (test ekstrak + download 1 video) sebelum lanjut ke tahap 2.

> Catatan: Railway free tier punya batas jam & bisa sleep/limit resource. Kalau
> traffic besar / dipakai serius, pertimbangkan paket berbayar Railway.

## TAHAP 2 — Bungkus jadi APK Android (via GitHub Actions, dari HP)

Tidak perlu Android Studio / komputer. Semua build APK dilakukan otomatis di
server GitHub.

1. Extract `stream-android-shell.zip` ini di HP (pakai app file manager / ZIP
   extractor).
2. Edit file `capacitor.config.json` — ganti `url` dengan URL Railway kamu dari
   Tahap 1. Bisa edit langsung lewat app GitHub (buka file → pensil edit) setelah
   diupload, jadi tidak wajib edit sebelum upload.
3. Buat repo baru di GitHub (bisa lewat app GitHub atau browser HP), lalu upload
   semua isi folder `stream-android-shell/` ke repo itu (termasuk folder
   tersembunyi `.github/`).
   - Kalau lewat browser: gunakan fitur "Add file → Upload files", drag semua
     isi folder sekaligus.
4. Setelah file ter-upload ke branch `main`, buka tab **Actions** di repo →
   akan muncul workflow **"Build Android APK"** yang otomatis berjalan.
   - Kalau tidak otomatis jalan, klik workflow itu → **Run workflow**.
5. Tunggu sampai selesai (icon centang hijau, biasanya 5–10 menit).
6. Klik run yang sudah selesai → scroll ke bagian **Artifacts** → download
   `stream-app-debug-apk` (berupa file .zip berisi APK).
7. Extract, dapat file `app-debug.apk` → pindahkan ke HP Android kamu → install
   (perlu izin "Install from unknown sources" di setting HP).

> APK debug ini bisa langsung dipakai untuk testing/pribadi. Kalau nanti mau
> publish ke Play Store, perlu APK/AAB yang di-sign dengan release key — ini
> bisa ditambahkan ke workflow kalau sudah sampai tahap itu.

## Cara kerja app ini

APK yang dihasilkan pada dasarnya adalah WebView yang langsung memuat URL Railway
kamu (`server.url` di config) — jadi APK ini butuh koneksi internet dan backend
Railway tetap harus online. Semua proses berat (ekstrak, FFmpeg, download) tetap
jalan di server Railway, bukan di HP.

Kalau nanti mau tambah fitur native (misal notifikasi push, simpan file ke
storage HP secara native, dsb), itu bisa ditambah lewat Capacitor plugin
tambahan — kabari saja kalau sudah sampai situ.
