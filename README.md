# Panda Printer POS Agent

Aplikasi desktop untuk menghubungkan dan mengontrol printer thermal USB untuk sistem Point of Sale (POS). Agent ini berjalan sebagai layanan background di komputer Anda dan memungkinkan aplikasi web mengirim perintah cetak ke printer thermal struk melalui HTTP API.

## Apa itu aplikasi ini?

Ini adalah **aplikasi desktop** yang bertindak sebagai jembatan antara aplikasi web POS Anda dengan printer thermal USB. Aplikasi ini:
- Mendeteksi printer thermal USB yang terhubung ke komputer Anda
- Menyediakan antarmuka sederhana untuk memilih dan mengkonfigurasi printer
- Menjalankan web server lokal (API) yang menerima perintah cetak
- Memformat dan mengirim perintah cetak ke printer thermal Anda
- Mendukung berbagai perintah cetak (teks, perataan, tabel, QR code, barcode, dll.)

## Fitur

- ✅ Dukungan printer thermal USB
- ✅ Deteksi otomatis printer yang terhubung
- ✅ HTTP REST API untuk cetak jarak jauh
- ✅ Integrasi system tray
- ✅ Fungsi update otomatis
- ✅ Cross-platform (macOS, Windows, Linux)
- ✅ Cetak struk, tiket, label, dan lainnya
- ✅ Dukungan QR code dan barcode
- ✅ Format tabel kustom

---

## Persyaratan (Yang Anda Butuhkan Terlebih Dahulu)

### 1. Install Node.js

Node.js diperlukan untuk menjalankan aplikasi ini. Jangan khawatir jika Anda belum pernah menggunakannya sebelumnya!

#### Untuk macOS:
1. Kunjungi [https://nodejs.org/](https://nodejs.org/)
2. Download **versi LTS** (direkomendasikan untuk kebanyakan pengguna)
3. Buka file `.pkg` yang sudah diunduh dan ikuti panduan instalasi
4. Verifikasi instalasi dengan membuka **Terminal** dan ketik:
   ```bash
   node --version
   npm --version
   ```
   Anda seharusnya melihat nomor versi yang ditampilkan

#### Untuk Windows:
1. Kunjungi [https://nodejs.org/](https://nodejs.org/)
2. Download **versi LTS** (Windows Installer)
3. Jalankan installer `.msi` dan ikuti panduan setup
4. Verifikasi instalasi dengan membuka **Command Prompt** dan ketik:
   ```cmd
   node --version
   npm --version
   ```
   Anda seharusnya melihat nomor versi yang ditampilkan

#### Untuk Linux (Ubuntu/Debian):
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Verifikasi instalasi:
```bash
node --version
npm --version
```

### 2. Install Git (Opsional tetapi Direkomendasikan)

Git membantu Anda mengunduh dan mengelola kode proyek.

#### Untuk macOS:
```bash
# Install Homebrew terlebih dahulu jika Anda belum memilikinya
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Kemudian install Git
brew install git
```

#### Untuk Windows:
1. Download dari [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Jalankan installer dan ikuti panduan setup

#### Untuk Linux:
```bash
sudo apt-get install git
```

---

## Langkah Instalasi

### Langkah 1: Unduh Proyek

#### Opsi A: Menggunakan Git (Direkomendasikan)
Buka terminal/command prompt Anda dan jalankan:

```bash
# Navigasi ke lokasi dimana Anda ingin menyimpan proyek
cd ~/Documents  # macOS/Linux
# ATAU
cd C:\Users\YourUsername\Documents  # Windows

# Clone proyek
git clone https://github.com/yourusername/panda-printer-pos-agent.git

# Masuk ke folder proyek
cd panda-printer-pos-agent
```

#### Opsi B: Download ZIP
1. Buka halaman repository GitHub
2. Klik tombol hijau "Code"
3. Klik "Download ZIP"
4. Ekstrak file ZIP ke lokasi yang Anda inginkan
5. Buka terminal/command prompt dan navigasi ke folder tersebut

### Langkah 2: Install Dependensi

Langkah ini mengunduh semua library dan tools yang dibutuhkan untuk menjalankan aplikasi.

```bash
# Pastikan Anda berada di folder proyek
# Kemudian jalankan:
npm install
```

Proses ini mungkin memakan waktu 5-15 menit tergantung koneksi internet Anda. Anda akan melihat banyak teks bergulir - itu normal!

**Masalah Umum:**
- Jika Anda melihat error tentang permissions di macOS/Linux, coba: `sudo npm install`
- Jika instalasi gagal, coba hapus folder `node_modules` dan file `package-lock.json`, kemudian jalankan `npm install` lagi

### Langkah 3: Hubungkan Printer USB Anda

1. Colokkan printer thermal USB Anda ke komputer
2. Pastikan printer dalam keadaan menyala
3. Install driver printer jika diperlukan oleh produsen

---

## Menjalankan Aplikasi

### Mode Development (Untuk Testing)

```bash
npm start
```

Ini akan:
1. Membuka jendela aplikasi
2. Menjalankan web server pada port 3800 (default)
3. Memungkinkan Anda melihat informasi debug

### Production Build (Untuk Penggunaan Harian)

```bash
npm run package
```

Ini akan membuat file aplikasi standalone:
- **macOS**: Lihat di folder `release/build/mac/` untuk file `.dmg`
- **Windows**: Lihat di folder `release/build/win/` untuk installer `.exe`
- **Linux**: Lihat di folder `release/build/linux/` untuk package

Double-click installer untuk menginstall aplikasi di sistem Anda.

---

## Menggunakan Aplikasi

### 1. Pilih Printer Anda

1. Jalankan aplikasi
2. Anda akan melihat daftar printer USB yang terdeteksi
3. Klik pada printer Anda untuk memilihnya
4. Pilihan akan tersimpan secara otomatis

### 2. Menggunakan HTTP API

Setelah aplikasi berjalan, ia akan menjalankan web server di `http://localhost:3800`

#### Endpoint: POST `/print`

Kirim perintah cetak sebagai JSON:

```javascript
// Contoh menggunakan JavaScript fetch
fetch('http://localhost:3800/print', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify([
    { CMD: 'ALIGN', ARGS: 'CT' },
    { CMD: 'STYLE', ARGS: 'B' },
    { CMD: 'TEXT', ARGS: 'WELCOME TO OUR STORE\n' },
    { CMD: 'STYLE', ARGS: 'NORMAL' },
    { CMD: 'TEXT', ARGS: '------------------------\n' },
    { CMD: 'FEED', ARGS: 2 }
  ])
})
```

#### Perintah yang Tersedia:

| Perintah | Deskripsi | Contoh Args |
|---------|-------------|--------------|
| `TEXT` | Cetak teks | `"Hello World\n"` |
| `ALIGN` | Atur perataan | `"CT"` (tengah), `"LT"` (kiri), `"RT"` (kanan) |
| `FEED` | Umpan kertas | `2` (jumlah baris) |
| `FONT` | Atur font | `"A"`, `"B"`, `"C"` |
| `STYLE` | Gaya teks | `"B"` (tebal), `"U"` (garis bawah), `"NORMAL"` |
| `LINE_SPACE` | Spasi baris | `50` (piksel) |
| `TABLE` | Cetak tabel | `[{text: "Item", align: "LEFT", width: 0.5}, ...]` |
| `QRCODE` | Cetak QR code | `{data: "https://example.com", size: 6}` |
| `BARCODE` | Cetak barcode | `{data: "123456789", type: "EAN13"}` |
| `IMAGE` | Cetak gambar | Data gambar terenkode Base64 |
| `CUT` | Potong kertas | (tidak perlu args) |

#### Contoh: Cetak Struk

```javascript
const receipt = [
  { CMD: 'ALIGN', ARGS: 'CT' },
  { CMD: 'STYLE', ARGS: 'B' },
  { CMD: 'TEXT', ARGS: 'MY STORE\n' },
  { CMD: 'STYLE', ARGS: 'NORMAL' },
  { CMD: 'TEXT', ARGS: '123 Main Street\n' },
  { CMD: 'TEXT', ARGS: 'Phone: 555-1234\n' },
  { CMD: 'TEXT', ARGS: '================================\n' },
  { CMD: 'ALIGN', ARGS: 'LT' },
  { 
    CMD: 'TABLE', 
    ARGS: [
      [
        { text: 'Item', align: 'LEFT', width: 0.6 },
        { text: 'Qty', align: 'CENTER', width: 0.2 },
        { text: 'Price', align: 'RIGHT', width: 0.2 }
      ],
      [
        { text: 'Coffee', align: 'LEFT', width: 0.6 },
        { text: '2', align: 'CENTER', width: 0.2 },
        { text: '$6.00', align: 'RIGHT', width: 0.2 }
      ]
    ]
  },
  { CMD: 'TEXT', ARGS: '================================\n' },
  { CMD: 'ALIGN', ARGS: 'RT' },
  { CMD: 'TEXT', ARGS: 'Total: $6.00\n' },
  { CMD: 'ALIGN', ARGS: 'CT' },
  { CMD: 'TEXT', ARGS: 'Thank you!\n' },
  { CMD: 'FEED', ARGS: 3 },
  { CMD: 'CUT', ARGS: null }
];

fetch('http://localhost:3800/print', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(receipt)
});
```

---

## Troubleshooting

### Printer Tidak Terdeteksi
- Pastikan printer dalam keadaan menyala dan terhubung via USB
- Coba cabut dan colokkan kembali printer
- Restart aplikasi
- Periksa apakah driver printer sudah terinstall

### Port Sudah Digunakan
Jika port 3800 sudah digunakan, Anda bisa mengubahnya di source code:
- Edit file `src/main/main.ts`
- Cari `const PORT = 3800`
- Ubah ke nomor port yang berbeda

### Aplikasi Tidak Mau Dijalankan
- Pastikan Node.js terinstall dengan benar
- Coba hapus folder `node_modules` dan jalankan `npm install` lagi
- Periksa output console untuk pesan error

### Masalah Permission (Linux)
Anda mungkin perlu menambahkan user Anda ke grup `lp` dan `dialout`:
```bash
sudo usermod -a -G lp,dialout $USER
```
Kemudian log out dan log in kembali.

---

## Struktur Proyek

```
panda-printer-pos-agent/
├── src/
│   ├── main/          # Electron main process (backend)
│   │   ├── main.ts    # Titik masuk aplikasi
│   │   ├── menu.ts    # Menu aplikasi
│   │   └── proto/     # Protocol buffers
│   ├── renderer/      # React UI (frontend)
│   │   ├── App.tsx    # Komponen React utama
│   │   └── index.tsx  # Titik masuk React
│   └── types/         # Definisi tipe TypeScript
├── assets/            # Ikon dan resource
├── release/           # Aplikasi yang sudah di-build
└── package.json       # Dependensi proyek dan script
```

---

## Development

### Script yang Tersedia

```bash
# Mulai mode development
npm start

# Build untuk production
npm run build

# Package untuk distribusi
npm run package

# Jalankan test
npm test

# Lint code
npm run lint
```

### Tech Stack

- **Electron** - Desktop application framework
- **React** - UI library
- **TypeScript** - Programming language
- **Express** - HTTP server
- **@node-escpos** - Thermal printer library
- **Webpack** - Module bundler
- **TailwindCSS** - CSS framework

---

## Build untuk Distribusi

### macOS
```bash
npm run package
```
Membuat installer `.dmg` di folder `release/build/mac/`

### Windows
```bash
npm run package
```
Membuat installer `.exe` di folder `release/build/win/`

### Linux
```bash
npm run package
```
Membuat package di folder `release/build/linux/`

---

## Catatan Keamanan API

Secara default, HTTP API hanya menerima koneksi dari localhost. Jika Anda perlu menerima koneksi dari perangkat lain di jaringan Anda, pastikan untuk mengimplementasikan autentikasi yang tepat dan gunakan HTTPS.

---

## Kontribusi

Kontribusi sangat diterima! Silakan submit Pull Request.

---

## Lisensi

Proyek ini dilisensikan di bawah MIT License - lihat file [LICENSE](LICENSE) untuk detail.

---

## Dukungan

Jika Anda mengalami masalah:
1. Periksa bagian [Troubleshooting](#troubleshooting)
2. Tinjau issue GitHub yang ada
3. Buat issue baru dengan detail tentang masalah Anda

---

## Changelog

Lihat [CHANGELOG.md](CHANGELOG.md) untuk riwayat versi dan update.
