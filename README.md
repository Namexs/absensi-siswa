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
- Admin **menerbitkan token pendaftaran** dari tab **Token Daftar** di panel
  admin (lihat bagian *Token pendaftaran* di bawah) — tanpa token yang sah,
  tidak ada akun guru/siswa baru yang dapat dibuat.
- Admin dapat **mengatur ulang kata sandi** akun guru/siswa mana pun dari
  panel admin (tombol **Atur Ulang Kata Sandi**): admin menetapkan kata sandi
  baru, kata sandi lama langsung berhenti berlaku. Kata sandi pengguna tidak
  pernah dapat dilihat siapa pun — yang tersimpan hanya hash-nya.
- Admin dapat **menghapus data siswa** dari panel admin (tombol **Hapus** /
  **Hapus Siswa** pada setiap catatan siswa): penghapusan selalu meminta
  konfirmasi terlebih dahulu, dan catatan kehadiran siswa itu ikut terhapus.
  Admin juga dapat **menghapus akun siswa** (tombol **Hapus Akun**) —
  termasuk akun yang pendaftarannya gagal/belum lengkap dan belum memiliki
  data siswa; bila akun itu memiliki data siswa, data dan kehadirannya ikut
  terhapus.
- **Kelas hanya dibuat dan diatur oleh Admin** (tab **Semua Kelas**):
  membuat kelas baru, mengubah namanya, dan menghapusnya. Setiap kelas
  **wajib memiliki guru pemilik (wali kelas)** yang dipilih Admin saat
  pembuatan — tidak ada kelas khusus Admin. Kelas yang masih berisi siswa
  **tidak dapat dihapus**; siswanya harus dipindahkan dulu.
- **Admin menempatkan setiap siswa ke kelasnya** (tombol **Atur Kelas**
  pada setiap baris siswa): siswa yang mendaftar sendiri tidak memilih
  kelas — setelah identitasnya tersimpan ia berstatus **"Belum diatur
  Admin"** dan tampil di panel admin sebagai antrean penempatan. Begitu
  Admin menempatkannya ke sebuah kelas, siswa itu **sepenuhnya terlihat
  dan terkelola oleh guru wali kelas tersebut** (identitas, dokumen,
  ubahan, absensi, rekap) — data siswa tidak pernah hanya-di-Admin
  setelah ditempatkan.

## Token pendaftaran

Pendaftaran akun memerlukan **token sekali pakai dari Admin**, untuk mencegah
pendaftaran akun ganda/sembarangan:

- **Token Siswa — 5 karakter**, hanya dapat dipakai untuk mendaftar **akun
  Siswa**.
- **Token Guru — 7 karakter**, hanya dapat dipakai untuk mendaftar **akun
  Guru**.
- Setiap token **unik** (diperiksa terhadap semua token yang ada saat dibuat)
  dan **hangus begitu satu akun berhasil dibuat** dengannya.
- Admin mengelola kedua jenis token secara terpisah di tab **Token Daftar**:
  membuat token (1 atau 5 sekaligus), melihat daftar beserta statusnya
  (*belum dipakai* / *sudah dipakai oleh akun mana* / *dicabut*), dan
  **mencabut** token yang belum dipakai.
- Pendaftaran tanpa token, dengan token yang salah, token peran lain, token
  yang sudah dipakai, atau token yang dicabut akan **ditolak** dengan pesan
  yang jelas.

### 👩‍🏫 Guru

- Mode Guru memerlukan **akun guru** yang didaftarkan sendiri dengan
  **email, nomor HP, kata sandi, dan Token Guru (7 karakter)** dari Admin;
  masuk memakai **email + kata sandi**.
- Setiap guru hanya melihat dan mengelola **data miliknya**: kelas yang
  ditugaskan Admin kepadanya berlabel pemilik akunnya (`ownerTeacherId`),
  dan siswa/absensi/rekap yang tampil hanyalah yang berada di kelas-kelas
  itu. Data guru lain di perangkat yang sama tidak tercampur dan tidak
  terlihat.
- **Guru tidak membuat kelas** — kelas dibuat dan diatur oleh Admin, yang
  juga menempatkan siswa pendaftar ke kelasnya masing-masing. Guru
  menambahkan siswa (satu-satu atau banyak sekaligus) ke kelas yang sudah
  ditugaskan Admin kepadanya; bila Admin belum menugaskan kelas, aplikasi
  menampilkan pesan untuk menghubungi Admin.
- Fitur guru: Beranda ringkasan hari ini, absensi harian per kelas
  (Hadir/Sakit/Izin/Alpa + keterangan), **Pindai QR** — guru memindai QR
  siswa untuk mencatat kehadiran siswa tersebut hari ini (QR siswa yang
  belum dikenal diimpor hanya bila kelas pada QR adalah kelas milik guru
  tersebut; bila belum diatur Admin atau milik guru lain, pindaian ditolak
  dengan penjelasan), kelola siswa di kelasnya (lihat Detail identitas &
  dokumen siswa), cetak kartu QR satu kelas, dan rekap bulanan.

### 🎓 Siswa

- Portal siswa memerlukan **akun siswa** yang didaftarkan sendiri dengan
  **email, nomor HP, kata sandi, dan Token Siswa (5 karakter)** dari Admin;
  masuk memakai **email + kata sandi**.
- **Daftar akun dan pengisian identitas adalah dua langkah terpisah.**
  Setelah daftar/masuk, siswa mendarat di beranda portalnya — formulir
  identitas **tidak** disodorkan otomatis. Selama data belum diisi, portal
  menampilkan pemberitahuan ramah dan tombol **Isi Data Siswa**; siswa
  bebas keluar dan masuk lagi kapan pun, dan masuknya tidak pernah
  terhalang oleh identitas yang belum lengkap. Mendaftarkan ulang dengan
  email yang sama ditolak dengan pesan jelas *"Email sudah terdaftar —
  silakan masuk"*.
- Lewat tombol itu pengguna mendaftarkan **satu siswa** (satu akun = satu
  pendaftaran; formulir terkunci setelahnya) dengan **identitas lengkap**:
  nama lengkap, NISN, tempat lahir, tanggal lahir, nama ayah, nama ibu,
  anak ke berapa, alamat, NIK, email, nomor HP, dan jenis kelamin —
  plus unggahan foto **Kartu Keluarga (KK), KTP ayah, dan KTP ibu**.
- **Kelas tidak dipilih oleh siswa.** Formulir identitas tidak memuat
  kolom kelas sama sekali: setelah identitas tersimpan, siswa berstatus
  **"Belum diatur Admin"** sampai Admin menempatkannya ke kelasnya dari
  panel admin. QR pribadi tetap ada begitu identitas lengkap tersimpan
  (gerbang QR adalah kelengkapan identitas, bukan kelas), dan isi QR
  selalu mencerminkan kelas siswa saat ini — penempatan/pemindahan oleh
  Admin langsung mengubah yang tampil pada QR dan hasil pindaiannya.
- **QR pribadi hanya ada setelah identitas lengkap tersimpan.** Selama
  data siswa belum diisi dan lolos validasi, portal siswa tidak
  menampilkan QR apa pun; QR muncul begitu data siswa tersimpan.
- Pembuatan akun bersifat **atomik**: kata sandi di-hash selesai lebih
  dulu, baru akun ditulis sekali jadi — tidak ada akun setengah jadi
  yang tersimpan tanpa kata sandi yang dapat dipakai.
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
  ke data yang ada; catatan yang sudah ada dilewati, tidak ditimpa). Ekspor
  admin mencakup juga token pendaftaran dan kredensial akun dalam bentuk
  **hash kata sandi** (bukan kata sandi aslinya), sehingga akun hasil
  pemulihan tetap masuk dengan kata sandinya.
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
- **Kata sandi pengguna disimpan sebagai hash**, bukan teks biasa: setiap
  akun memiliki garam acak sendiri dan yang disimpan adalah hash
  SHA-256(garam + kata sandi) yang dihitung dengan WebCrypto
  (`crypto.subtle`). Kata sandi asli tidak tersimpan di mana pun dan tidak
  dapat ditampilkan kembali oleh siapa pun, termasuk admin.

Batasan yang tersisa (jujur apa adanya):

- Data tersimpan **tanpa enkripsi** di localStorage perangkat. Siapa pun
  yang memegang perangkat/peramban itu dapat membaca atau mengubah datanya.
- Kata sandi admin tertanam di kode halaman publik = **gerbang ringan**,
  bukan pengaman kuat (lihat bagian Admin). Hash kata sandi pengguna pun
  dihitung **di sisi klien** pada situs statis publik — perlindungannya
  setara gerbang ringan, **bukan** keamanan sekelas server: hash mencegah
  kata sandi terbaca langsung dari data, tetapi tidak menggantikan
  pengamanan server.
- Tidak ada akun yang bisa dipulihkan dari server — lupa kata sandi berarti
  akun di perangkat itu tidak bisa dibuka sampai **Admin mengatur ulang
  kata sandinya** dari Panel Admin (tombol **Atur Ulang Kata Sandi** pada
  setiap akun guru/siswa; admin menetapkan kata sandi baru dan kata sandi
  lama berhenti berlaku). Tautan "Lupa kata sandi?" pada formulir masuk
  guru & siswa menjelaskan hal ini. Tidak ada OTP dan tidak ada pengiriman
  email/SMS di aplikasi ini.

## Akun lama (sebelum ada kata sandi)

Akun yang dibuat pada versi aplikasi sebelumnya (masuk dengan email +
nomor HP, tanpa kata sandi) **tidak dihapus dan datanya tidak hilang**.
Saat pemiliknya masuk lagi, sesi lamanya diakhiri dan ia diarahkan ke satu
langkah **"Buat Kata Sandi"**: verifikasi nomor HP yang terdaftar satu
kali, lalu buat kata sandi. Setelah itu akun tersebut masuk dengan
email + kata sandi seperti akun baru. Admin juga dapat langsung menetapkan
kata sandi pertama akun lama lewat **Atur Ulang Kata Sandi**.

## Cara data disimpan

Semua data (akun, token, kelas, siswa, absensi) tersimpan **di peramban
perangkat yang dipakai** (localStorage) — tidak ada server dan tidak ada
data yang dikirim ke mana pun. Artinya:

- Data di HP guru dan di HP siswa **terpisah**; catatan yang dibuat di satu
  perangkat tidak muncul di perangkat lain.
- Akun juga hanya ada di perangkat tempat ia dibuat. Berganti HP berarti
  membuat akun baru di HP tersebut dengan token baru dari Admin (data guru
  dapat dipindahkan lewat Ekspor & Impor Data).
- Jangan menghapus data peramban/cache aplikasi ini bila tidak ingin
  datanya hilang. Buat ekspor JSON secara berkala sebagai cadangan.
- Aplikasi mulai dari kosong: Admin membuat kelas (dengan wali kelas),
  guru menambahkan siswa ke kelas yang ditugaskan kepadanya, dan siswa
  yang mendaftar sendiri ditempatkan Admin ke kelasnya masing-masing.

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
