# Absensi Siswa

Aplikasi web (PWA) untuk absensi siswa di kelas, seluruhnya dalam Bahasa Indonesia.
Bisa dipasang di HP seperti aplikasi biasa dan tetap bisa dibuka tanpa internet
setelah kunjungan pertama.

## Tiga peran

### 🛡 Admin (pemilik aplikasi)

- Admin adalah **satu-satunya akun bawaan** aplikasi. Masuk lewat tombol
  **Admin** di kanan atas dengan nama pengguna dan kata sandi admin.
- **Catatan keamanan:** pada versi mandiri ini kredensial admin **tertanam di
  kode halaman** (`index.html`). Karena situs ini publik, itu hanya gerbang
  akses ringan — **bukan keamanan yang kuat**: siapa pun yang membuka kode
  sumber halaman dapat menemukannya. Perlakukan panel admin sebagai kemudahan
  pengelolaan di perangkat tepercaya, bukan sebagai pengaman data rahasia.
- Admin memegang **seluruh data di perangkat**: melihat dan mengubah semua
  akun guru, semua siswa (identitas lengkap + foto dokumen KK/KTP ayah/KTP
  ibu), semua kelas, semua absensi, dan rekap gabungan.
- Admin dapat **membuka wilayah akun guru mana pun dan data siswa mana pun**
  dari panel admin — termasuk melihat QR siswa dan mengubah datanya — tanpa
  perlu masuk sebagai mereka. Admin tidak terikat aturan satu-pendaftaran
  maupun pemisahan data antar-guru.
- Admin **tidak membuatkan akun untuk siapa pun**. Semua akun guru dan siswa
  dibuat sendiri oleh penggunanya lewat formulir pendaftaran di masing-masing
  mode. (Guru menambahkan catatan siswa ke daftar kelasnya adalah data kelas,
  bukan akun masuk.)

### 👩‍🏫 Guru

- Mode Guru memerlukan **akun guru** yang didaftarkan sendiri dengan
  **email + nomor HP**; masuk memakai pasangan yang sama.
- Setiap guru hanya melihat dan mengelola **data miliknya**: kelas yang ia
  buat berlabel pemilik akunnya (`ownerTeacherId`), dan siswa/absensi/rekap
  yang tampil hanyalah yang berada di kelas-kelas itu. Data guru lain di
  perangkat yang sama tidak tercampur dan tidak terlihat.
- Fitur guru: Beranda ringkasan hari ini, absensi harian per kelas
  (Hadir/Sakit/Izin/Alpa + keterangan), **Pindai QR** — guru memindai QR
  siswa untuk mencatat kehadiran siswa tersebut hari ini (QR siswa yang
  belum dikenal otomatis diimpor ke kelas guru), kelola kelas & siswa
  (tambah satu-satu atau banyak sekaligus, lihat Detail identitas &
  dokumen siswa), cetak kartu QR satu kelas, dan rekap bulanan.

### 🎓 Siswa

- Portal siswa memerlukan **akun siswa** yang didaftarkan sendiri dengan
  **email + nomor HP**.
- Setelah masuk, pengguna mendaftarkan **satu siswa** (satu akun = satu
  pendaftaran; formulir terkunci setelahnya) dengan **identitas lengkap**:
  nama lengkap, NISN, tempat lahir, tanggal lahir, nama ayah, nama ibu,
  anak ke berapa, alamat, NIK, email, nomor HP, kelas, dan jenis kelamin —
  plus unggahan foto **Kartu Keluarga (KK), KTP ayah, dan KTP ibu**.
- Kesalahan ketik dapat diperbaiki lewat menu **Edit Identitas** oleh
  pemilik akun; guru juga dapat mengubah data dari mode Guru, dan admin
  dapat mengubah data siswa mana pun dari panel admin.
- **QR pribadi hanya tampil untuk akun pemilik pendaftaran itu.** Mode
  Siswa tidak menampilkan daftar, pilihan, QR, atau catatan siswa lain.

## Aturan anti-duplikat

Pendaftaran/ubahan data siswa hanya ditolak bila **seluruh data identitasnya
persis sama** dengan siswa yang sudah terdaftar (nama, NISN, nama ayah, nama
ibu, anak ke berapa, alamat, NIK, email, nomor HP, tempat/tanggal lahir,
jenis kelamin, dan kelas — semuanya sama). Bila hanya sebagian yang sama —
misalnya nama sama, atau NISN sama saja — data **diterima** seperti biasa.
Aturan ini berlaku di pendaftaran siswa, Edit Identitas, tambah/edit siswa
oleh guru, tambah banyak, impor QR, dan ubahan oleh admin.

## Dokumen & penyimpanan foto

Foto KK/KTP diperkecil dulu di peramban (sisi terpanjang maks. 1000 piksel,
JPEG kualitas 0,8) lalu disimpan sebagai data URL di localStorage perangkat.
Bila kuota penyimpanan peramban terlampaui, aplikasi menampilkan pesan
kesalahan yang jelas dan data tidak disimpan setengah-setengah.

## Ekspor & Impor Data (cadangan)

Karena versi mandiri ini menyimpan data **hanya di perangkat**, berkas
ekspor adalah jalur cadangannya:

- **Admin** dapat mengekspor **seluruh data perangkat** menjadi satu berkas
  JSON dan mengimpor berkas semacam itu kembali (isi berkas **digabungkan**
  ke data yang ada; catatan yang sudah ada dilewati, tidak ditimpa).
- **Guru** dapat mengekspor/mengimpor **data miliknya sendiri** dengan cara
  yang sama (menu Ekspor & Impor Data di tab Siswa mode Guru).
- **Rekap CSV**: rekap kehadiran bulanan dapat diunduh sebagai berkas CSV
  (di tab Rekap mode Guru untuk data sendiri; di panel Admin untuk semua
  siswa).

Simpan berkas ekspor di tempat yang aman — berkas itu berisi data pribadi
siswa termasuk foto dokumen.

## Keamanan & batasannya

Penguatan yang diterapkan pada aplikasi ini:

- **Anti-XSS:** semua teks dari pengguna (nama, NISN, alamat, keterangan,
  nama kelas, email, dll.) di-escape saat ditampilkan; tidak ada teks
  pengguna yang disisipkan mentah ke halaman. Nilai foto dokumen yang bukan
  data URL gambar dibuang, dan berkas impor disanitasi (ID hanya boleh
  huruf/angka/`-`/`_`, status & tanggal absensi harus sah).
- **Content-Security-Policy:** halaman memasang CSP ketat — semua sumber
  hanya dari asal yang sama (`'self'`), gambar boleh `data:`/`blob:`
  (untuk foto dokumen & kamera), satu-satunya permintaan keluar adalah
  Google Fonts. Karena aplikasi memakai penangan klik inline, `script-src`
  masih memerlukan `'unsafe-inline'`; pertahanan utama terhadap XSS adalah
  escaping di atas.
- **Unggahan dokumen divalidasi dari isi berkas** (magic bytes JPG/PNG/
  WebP/GIF/BMP + batas 10 MB) dan harus lolos dekode gambar sebelum
  dikompresi & disimpan; berkas bukan gambar ditolak dengan pesan jelas.
- **Validasi formulir:** NISN/NIK/nomor HP harus berpola angka (NIK tepat
  16 angka bila diisi), email harus berformat benar, tanggal lahir tidak
  boleh di masa depan, dan semua teks dibatasi panjangnya.
- **Lingkup data:** mode Siswa hanya pernah mengakses catatan milik akun
  yang sedang masuk; tidak ada pegangan debug/global yang membuka seluruh
  penyimpanan data ke `window`.

Batasan yang tersisa (jujur apa adanya):

- Data tersimpan **tanpa enkripsi** di localStorage perangkat. Siapa pun
  yang memegang perangkat/peramban itu dapat membaca atau mengubah datanya.
- Kata sandi admin tertanam di kode halaman publik = **gerbang ringan**,
  bukan pengaman kuat (lihat bagian Admin).
- Tidak ada akun yang bisa dipulihkan dari server — lupa pasangan
  email+nomor HP berarti data di perangkat itu tidak bisa dibuka lagi
  lewat akun tersebut (admin masih dapat melihat datanya di perangkat
  yang sama).

## Cara data disimpan

Semua data (akun, kelas, siswa, absensi) tersimpan **di peramban perangkat
yang dipakai** (localStorage) — tidak ada server dan tidak ada data yang
dikirim ke mana pun. Artinya:

- Data di HP guru dan di HP siswa **terpisah**; catatan yang dibuat di satu
  perangkat tidak muncul di perangkat lain.
- Akun juga hanya ada di perangkat tempat ia dibuat. Berganti HP berarti
  membuat akun baru di HP tersebut (data guru dapat dipindahkan lewat
  Ekspor & Impor Data).
- Jangan menghapus data peramban/cache aplikasi ini bila tidak ingin
  datanya hilang. Buat ekspor JSON secara berkala sebagai cadangan.
- Aplikasi mulai dari kosong: guru mengisi kelas dan siswanya sendiri.

## Memasang di HP

1. Buka alamat aplikasi ini di Chrome (Android) atau Safari (iPhone).
2. Android/Chrome: ketuk menu ⋮ lalu **"Tambahkan ke Layar Utama"** /
   **"Instal aplikasi"**.
3. iPhone/Safari: ketuk tombol Bagikan lalu **"Tambah ke Layar Utama"**.
4. Ikon "Absensi" akan muncul di layar utama dan aplikasi terbuka layar penuh
   seperti aplikasi biasa, termasuk saat tidak ada internet.

## Berkas

- `index.html` — seluruh aplikasi (tampilan + logika).
- `qrcode.js` — pustaka pembuat QR (Kazuhiko Arase, lisensi MIT).
- `jsQR.js` — pustaka pembaca QR dari kamera (lisensi Apache-2.0).
- `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png` — keperluan
  PWA (bisa dipasang & luring).
