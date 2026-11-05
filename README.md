# MikroTik Hotspot Login Page

Template halaman login **MikroTik Hotspot** dengan antarmuka responsif, tema warna gelap, dan teks antarmuka berbahasa Indonesia. Template ini menggunakan file HTML yang diproses langsung oleh MikroTik RouterOS, bukan aplikasi web yang berdiri sendiri.

## Fitur

- Halaman login voucher/username dan password.
- Dukungan alur login HTTP-CHAP melalui `md5.js` ketika variabel `$(chap-id)` tersedia dari Hotspot.
- Halaman setelah login (`alogin.html`) dan halaman status sesi (`status.html`).
- Halaman logout (`logout.html`) yang menampilkan informasi sesi.
- Halaman error dan pesan error dalam Bahasa Indonesia serta Bahasa Inggris.
- Layout responsif dengan stylesheet lokal.
- Tombol QR Code sudah ditampilkan pada antarmuka, tetapi integrasi pemindai QR belum dikonfigurasi di dalam file proyek ini.

## Struktur proyek

```text
Mikrotik-login-page-main/
├── alogin.html             # Redirect setelah login berhasil
├── error.html              # Halaman error Hotspot
├── errors-en.txt           # Pesan error Bahasa Inggris
├── errors.txt              # Pesan error Bahasa Indonesia
├── favicon.ico
├── github-hotspot.zip      # Arsip salinan file hotspot
├── login.html              # Halaman login utama
├── logout.html             # Informasi setelah logout
├── md5.js                  # Helper MD5 untuk HTTP-CHAP
├── radvert.html            # Halaman redirect iklan Hotspot
├── redirect.html           # Redirect Hotspot
├── rlogin.html             # Redirect/login untuk captive portal
├── status.html             # Status sesi pengguna
├── css/
│   ├── background.css
│   └── mikhmon-ui-light.css
└── img/
    └── favicon.png
```

## Persyaratan

- Router MikroTik dengan fitur Hotspot yang sudah dikonfigurasi.
- Akses administrator ke RouterOS melalui Winbox, WebFig, atau FTP/SFTP yang tersedia pada perangkat.
- Peramban dengan JavaScript aktif untuk alur login CHAP.

Tidak diperlukan Node.js, npm, framework, atau proses build. File harus disajikan oleh layanan Hotspot MikroTik agar variabel seperti `$(link-login-only)`, `$(username)`, dan `$(ip)` diproses oleh RouterOS.

## Instalasi pada MikroTik

1. **Cadangkan halaman Hotspot yang sedang digunakan.**  
   Simpan salinan folder HTML Hotspot bawaan sebelum menggantinya.

2. **Ekstrak arsip proyek** di komputer.  
   Buka folder `Mikrotik-login-page-main`.

3. **Unggah file ke direktori HTML Hotspot.**  
   Unggah isi folder tersebut ke direktori Hotspot pada router. Pertahankan struktur folder `css/` dan `img/`. Jangan hanya mengunggah folder ZIP atau folder proyek sebagai satu file. File `login.html`, `status.html`, `logout.html`, `md5.js`, file error, dan direktori aset harus berada pada lokasi yang dapat diakses relatif oleh halaman.

4. **Atur HTML Directory.**  
   Buka konfigurasi Hotspot Server Profile yang digunakan oleh server Hotspot, lalu atur **HTML Directory** ke direktori tempat file tadi diunggah. Nama menu dapat sedikit berbeda bergantung pada versi RouterOS dan aplikasi administrasi yang digunakan.

5. **Uji koneksi klien.**  
   Hubungkan perangkat ke jaringan Hotspot dan buka halaman login. Uji login menggunakan akun/voucher yang valid, kemudian periksa halaman status dan logout.

6. **Pulihkan dari cadangan bila diperlukan.**  
   Apabila halaman tidak tampil atau autentikasi gagal, kembalikan direktori HTML sebelumnya dan periksa kembali path file serta pengaturan profil Hotspot.

> **Catatan:** Membuka `login.html` langsung dari komputer atau melalui web server biasa bukan pengujian autentikasi yang valid. Placeholder RouterOS seperti `$(link-login-only)` hanya akan diisi ketika halaman disajikan oleh MikroTik Hotspot.

## Kustomisasi

### Logo

`login.html` merujuk ke file `img/logo.png`, tetapi file tersebut **belum tersedia** di arsip ini. Tambahkan logo milik Anda dengan nama `logo.png` ke dalam folder `img/`, atau ubah path gambar pada `login.html`.

### Nama jaringan, teks, dan paket internet

Edit `login.html` untuk mengganti teks contoh seperti:

- `kemangi 41`
- `LOREM IPSUM` dan paragraf contoh
- `PAKET INTERNET`
- `Rp XXX`, masa aktif, dan informasi paket

Ganti seluruh teks placeholder dengan nama jaringan, informasi layanan, dan harga yang benar sebelum digunakan.

### Tampilan

- `css/mikhmon-ui-light.css` berisi komponen dan gaya antarmuka.
- `css/background.css` mengatur latar belakang serta gaya tombol login/QR Code.

Ubah file CSS tersebut sesuai identitas visual jaringan Anda. Pertahankan nama dan path file CSS agar halaman tetap dapat memuat stylesheet.

### Pesan error

- `errors.txt`: pesan error Bahasa Indonesia.
- `errors-en.txt`: pesan error Bahasa Inggris.

Edit teks di sisi kanan tanda `=` sesuai bahasa dan pesan yang ingin ditampilkan. Pertahankan nama kode error dan placeholder RouterOS seperti `$(username)`, `$(ip)`, serta `$(error-orig)`.

## Catatan fitur QR Code

Tombol **QR CODE** saat ini hanya berupa elemen tautan dengan `href` kosong. Proyek ini tidak menyertakan halaman pemindai QR, pustaka kamera, atau kode yang menghubungkan tombol tersebut ke layanan pemindai. Karena itu, tombol tersebut belum dapat dianggap sebagai fitur pemindaian yang siap digunakan.

Walled Garden untuk `XXX.github.io` dan pengaktifan HTTP PAP. Instruksi tersebut merupakan petunjuk integrasi eksternal lama; aturan Walled Garden/HTTP PAP saja tidak mengaktifkan tombol QR pada template ini. Verifikasi layanan pemindai, domain, dan persyaratan autentikasinya sebelum menambahkan aturan pada router.

## Pemecahan masalah

| Masalah | Hal yang perlu diperiksa |
|---|---|
| Gambar logo tidak tampil | Pastikan `img/logo.png` sudah ditambahkan atau path pada `login.html` sudah diubah. |
| CSS tidak termuat | Pastikan folder `css/` tetap berada di samping file HTML dan nama file CSS tidak berubah. |
| Halaman menampilkan teks `$(...)` | Halaman kemungkinan dibuka di luar layanan MikroTik Hotspot atau direktori HTML belum diatur dengan benar. |
| Login CHAP gagal | Pastikan `md5.js` terunggah, JavaScript aktif di peramban, dan konfigurasi metode login Hotspot sesuai dengan metode autentikasi yang digunakan. |
| Tombol QR Code tidak membuka pemindai | Integrasi QR belum disediakan; atur tautan dan implementasi pemindai yang sesuai terlebih dahulu. |
| Halaman setelah login/status tidak ditemukan | Pastikan `alogin.html`, `status.html`, dan file Hotspot terkait telah diunggah ke direktori HTML yang sama. |

## LIVE DEMO

LIVE DEMO BISA DIAKSES
https://bahrizalmulyawan.github.io/demo2/login.html
