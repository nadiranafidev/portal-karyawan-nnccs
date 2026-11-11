# Portal Karyawan NNCCS

Aplikasi desktop Windows untuk Portal Karyawan NNCCS, dibangun dengan [Electron](https://www.electronjs.org/).

---

## Fitur Utama

- Membuka portal karyawan
- Sesi login tersimpan (tidak hilang saat aplikasi ditutup)
- Loading screen profesional
- Halaman error/offline dengan tombol Coba Lagi
- Auto-update melalui GitHub Releases
- Penggantian URL website cukup edit `config.json` di GitHub (tanpa build ulang)
- Keamanan Electron terkonfigurasi (contextIsolation, sandbox, dll.)

---

## Struktur Project

```
Portal-Karyawan-NNCCS/
├── main.js          # Proses utama Electron
├── preload.js       # Jembatan IPC yang aman
├── config.js        # Konfigurasi pusat aplikasi
├── loading.html     # Loading screen
├── error.html       # Halaman error/offline
├── package.json     # Dependensi dan konfigurasi build
├── build/
│   ├── icon.ico     # Ikon aplikasi (harus disediakan)
│   └── icon.png     # Sumber ikon (opsional)
└── dist/            # Hasil build (dibuat otomatis)
```

---

## Persiapan Sebelum Build

### 1. Pasang Node.js

Unduh dan pasang Node.js (versi LTS) dari https://nodejs.org/

Verifikasi instalasi:
```powershell
node --version
npm --version
```

### 2. Pasang dependensi project

Buka PowerShell di folder project ini, lalu jalankan:

```powershell
npm install
```

### 3. Siapkan ikon aplikasi

Letakkan file `icon.ico` (multi-size, min 256x256) di folder `build/`.
Lihat panduan lengkap di `build/README-ICON.md`.

### 4. Uji coba di mode development

```powershell
npm start
```

---

## Build Installer

```powershell
npm run build:win
```

Hasil build akan tersimpan di folder `dist/` dengan nama:
```
Portal-Karyawan-NNCCS-Setup.exe
```

---

## Sistem Penggantian URL Website (Tanpa Build Ulang)

File `config.json` di repository GitHub ini mengontrol URL website yang dibuka aplikasi.

**Lokasi file:** `config.json` di root repository GitHub

**Format:**
```json
{
  "url": -
}
```

**Cara mengganti URL:**
1. Buka repository GitHub: https://github.com/nadiranafidev/portal-karyawan-nnccs
2. Edit file `config.json`
3. Ganti nilai `url` dengan alamat website baru
4. Commit perubahan
5. Saat pengguna membuka aplikasi berikutnya, URL baru akan digunakan secara otomatis

> Penggantian URL **tidak** memicu notifikasi update aplikasi kepada pengguna.

---

## Auto-Update Aplikasi

Auto-update bekerja melalui GitHub Releases:

1. Naikkan versi di `package.json` (contoh: `"version": "1.1.0"`)
2. Jalankan `npm run build:win`
3. Buat release baru di GitHub dengan tag versi yang sama (contoh: `v1.1.0`)
4. Upload file berikut ke GitHub Release:
   - `Portal-Karyawan-NNCCS-Setup.exe`
   - `latest.yml` (dari folder `dist/`)
   - Semua file `.blockmap`
5. Saat pengguna menjalankan aplikasi, update akan terdeteksi dan diunduh secara otomatis

---

## Konfigurasi Utama

Semua nilai yang mungkin perlu diubah terpusat di `config.js`:

| Key | Nilai Default | Keterangan |
|-----|---------------|------------|
| `fallbackUrl` | -` | URL cadangan jika GitHub tidak dapat diakses |
| `remoteConfigUrl` | URL raw GitHub | Lokasi `config.json` di GitHub |
| `allowedDomains` | `['-']` | Domain yang diizinkan untuk navigasi |
| `windowWidth` | `1280` | Lebar jendela awal |
| `windowHeight` | `800` | Tinggi jendela awal |

---

## Repository GitHub

- **Owner:** nadiranafidev
- **Repo:** portal-karyawan-nnccs
- **URL:** https://github.com/nadiranafidev/portal-karyawan-nnccs
- **config.json:** https://raw.githubusercontent.com/nadiranafidev/portal-karyawan-nnccs/main/config.json

---

## Shortcut Keyboard

| Shortcut | Fungsi |
|----------|--------|
| F11 | Toggle fullscreen |
| F5 | Refresh halaman |
| Ctrl+R | Refresh halaman |

---

## Catatan Keamanan

- `contextIsolation: true` — isolasi konteks renderer
- `nodeIntegration: false` — tidak ada akses Node.js di website
- `sandbox: true` — renderer berjalan di sandbox
- `webSecurity: true` — keamanan web aktif
- Navigasi dibatasi ke domain yang diizinkan
- Tautan eksternal dibuka di browser default Windows
