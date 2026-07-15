# Software Requirements Specification (SRS)
## IT Asset Management System (ITAMS)
**Studi Kasus:** PT. Makerindo Prima Solusi  
**Versi Dokumen:** 1.0  
**Tanggal:** 15 Juli 2026  
**Referensi:** PRD.md v1.0, TASK_BREAKDOWN.md v1.0, Project Guideline ITAMS MKR 2026

---

## 1. Pendahuluan

### 1.1 Tujuan Dokumen
Dokumen SRS (Software Requirements Specification) ini bertujuan untuk mendefinisikan secara rinci kebutuhan fungsional dan non-fungsional dari sistem IT Asset Management System (ITAMS). Dokumen ini menjadi acuan utama bagi tim Backend Developer (Fajar Bahari Supriatna), Frontend Developer (Wulan Sugiarti), dan pembimbing proyek (Kak Daffa) dalam proses perancangan sistem (SDD), pengembangan, hingga pengujian (QC).

### 1.2 Ruang Lingkup Sistem
ITAMS adalah sistem berbasis web yang memungkinkan PT. Makerindo Prima Solusi untuk:
* Mengelola data master aset IT (hardware, software, IoT) secara terpusat.
* Melakukan pencatatan transaksi barang masuk (pengadaan) dan barang keluar (disposal/hibah).
* Mengelola siklus peminjaman dan pengembalian aset internal.
* Melacak kondisi aset melalui pencatatan maintenance/perbaikan.
* Menyajikan dashboard dan laporan (PDF/Excel) untuk pengambilan keputusan.
* Menerapkan sistem keamanan berbasis role (Role-Based Access Control).

### 1.3 Definisi, Akronim, dan Singkatan

| Istilah | Definisi |
| --- | --- |
| ITAMS | IT Asset Management System — sistem yang sedang dibangun |
| Aset | Barang IT milik perusahaan (laptop, server, IoT, lisensi, dll) |
| QR Code | Kode 2D unik yang ditempel pada aset untuk identifikasi cepat |
| JWT | JSON Web Token — standar autentikasi berbasis token |
| SPA | Single Page Application — arsitektur frontend React |
| RBAC | Role-Based Access Control — pembatasan akses berdasarkan role |
| CRUD | Create, Read, Update, Delete — operasi dasar data |
| Master Data | Data induk (kategori, brand, supplier, lokasi, departemen) |
| MoSCoW | Metode prioritas: Must, Should, Could, Won't |
| MVP | Minimum Viable Product — versi minimal yang layak dirilis |

### 1.4 Pengguna Sistem (Stakeholder)

1. **Admin** — Pengelola utama sistem.
2. **Manajemen** — Pengambil keputusan strategis.
3. **Staff Inventaris** — Pengelola operasional aset harian.
4. **Teknisi (IT Support)** — Penanggung jawab perbaikan aset.
5. **Karyawan (Employee)** — Pengguna akhir aset.

---

## 2. Deskripsi Umum Sistem

### 2.1 Perspektif Produk
ITAMS adalah aplikasi web baru yang berdiri sendiri (stand-alone), tidak terintegrasi dengan sistem akuntansi/keuangan yang sudah ada. Sistem ini dibangun dengan arsitektur Client-Server terpisah (Backend Express.js + Frontend React) yang berkomunikasi melalui RESTful API dengan format JSON.

### 2.2 Fungsi Utama Sistem
* Autentikasi dan otorisasi berbasis JWT.
* Manajemen master data aset.
* Pencatatan transaksi masuk/keluar aset.
* Siklus peminjaman → pengembalian → verifikasi teknisi.
* Pencatatan maintenance aset.
* Dashboard analitik dan laporan (PDF/Excel).
* Generate dan scan QR Code untuk identifikasi aset.

### 2.3 Karakteristik Pengguna
Seluruh pengguna diasumsikan memiliki kemampuan dasar menggunakan aplikasi web melalui browser modern (Chrome, Edge, Firefox, Safari) pada perangkat desktop, laptop, tablet, atau smartphone.

### 2.4 Asumsi dan Ketergantungan
* Server dan database MySQL tersedia dan stabil.
* Seluruh pengguna memiliki akses ke perangkat dengan browser modern.
* Setiap aset fisik akan diberi label QR Code oleh Staff Inventaris.
* Kedisiplinan pengguna dalam mengupdate status aset secara real-time.
* Dokumen PRD dan TASK_BREAKDOWN telah disetujui sebelum SRS ini disusun.

### 2.5 Batasan Sistem (Out of Scope)
Untuk fase MVP, sistem **TIDAK** mencakup:
* Integrasi dengan sistem akuntansi/keuangan.
* Aplikasi mobile native (Android/iOS).
* Notifikasi otomatis via Email/WhatsApp.
* Monitoring data sensor IoT secara real-time.

---

## 3. Kebutuhan Fungsional (Functional Requirements)

Kebutuhan fungsional dibagi per modul sesuai TASK_BREAKDOWN.md. Setiap kebutuhan diberi ID unik (FR-XXX) untuk memudahkan penelusuran ke tahap desain (SDD) dan testing. Prioritas menggunakan metode MoSCoW: **Must** (wajib di MVP), **Should** (penting tapi bisa ditunda), **Could** (nice to have).

### 3.1 Modul 1: Authentication & JWT

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-101 | Sistem harus menyediakan halaman login dengan input email/username dan password. | Semua Role | Must |
| FR-102 | Sistem harus memvalidasi kredensial pengguna terhadap database. | Sistem | Must |
| FR-103 | Sistem harus menghasilkan JWT (Access Token & Refresh Token) setelah login berhasil. | Sistem | Must |
| FR-104 | Sistem harus menyimpan token di sisi client (localStorage/cookie) dengan aman. | Sistem | Must |
| FR-105 | Sistem harus menyediakan fitur logout yang menghapus token dari client. | Semua Role | Must |
| FR-106 | Sistem harus menyediakan mekanisme refresh token agar sesi tidak terputus tiba-tiba. | Sistem | Should |
| FR-107 | Sistem harus memblokir akses ke endpoint yang dilindungi jika token tidak valid/kadaluarsa. | Sistem | Must |
| FR-108 | Sistem harus mencatat riwayat login (IP, waktu, device) untuk keperluan audit. | Sistem | Should |
| FR-109 | Sistem harus mengunci akun sementara setelah 5x gagal login berturut-turut. | Sistem | Should |
| FR-110 | Sistem harus menyediakan endpoint untuk validasi status token aktif. | Sistem | Must |

### 3.2 Modul 2: User & Role Management

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-201 | Admin dapat menambahkan pengguna baru dengan data: nama, email, password, role, departemen. | Admin | Must |
| FR-202 | Admin dapat mengedit data pengguna (kecuali email yang sudah terdaftar). | Admin | Must |
| FR-203 | Admin dapat menonaktifkan (soft delete) akun pengguna tanpa menghapus data historis. | Admin | Must |
| FR-204 | Admin dapat mengubah role seorang pengguna. | Admin | Must |
| FR-205 | Sistem harus memvalidasi keunikan email dan username. | Sistem | Must |
| FR-206 | Sistem harus menyediakan daftar role: Admin, Manajemen, Staff Inventaris, Teknisi, Karyawan. | Sistem | Must |
| FR-207 | Admin dapat melihat daftar seluruh pengguna dengan filter berdasarkan role/departemen. | Admin | Must |
| FR-208 | Admin dapat mereset password pengguna lain. | Admin | Should |
| FR-209 | Pengguna dapat mengubah password miliknya sendiri setelah login. | Semua Role | Must |
| FR-210 | Admin dapat menghapus pengguna secara permanen (hard delete) hanya jika tidak ada data historis terkait. | Admin | Could |
| FR-211 | Sistem harus menyediakan fitur pencarian pengguna berdasarkan nama/email. | Admin | Must |
| FR-212 | Sistem harus membatasi akses menu berdasarkan role (menu dinamis). | Sistem | Must |

### 3.3 Modul 3: Master Data & Lokasi

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-301 | Admin/Staff Inventaris dapat mengelola data **Kategori Aset** (Laptop, Server, IoT, Network, Software). | Admin, Staff | Must |
| FR-302 | Admin/Staff Inventaris dapat mengelola data **Brand** (Dell, HP, Lenovo, Cisco, dll). | Admin, Staff | Must |
| FR-303 | Admin/Staff Inventaris dapat mengelola data **Supplier** (nama, kontak, alamat, email). | Admin, Staff | Must |
| FR-304 | Admin/Staff Inventaris dapat mengelola data **Lokasi** (Gudang, Ruang Server, Lantai 1, dll). | Admin, Staff | Must |
| FR-305 | Admin/Staff Inventaris dapat mengelola data **Departemen** (Engineering, HR, Finance, dll). | Admin, Staff | Must |
| FR-306 | Sistem harus mencegah penghapusan master data yang sedang direferensikan oleh aset/transaction. | Sistem | Must |
| FR-307 | Sistem harus menyediakan fitur pencarian dan filter pada setiap tabel master data. | Admin, Staff | Should |
| FR-308 | Sistem harus memvalidasi keunikan nama pada setiap master data. | Sistem | Must |
| FR-309 | Admin/Staff dapat mengedit data master yang sudah ada. | Admin, Staff | Must |
| FR-310 | Sistem harus menyediakan pagination pada tabel master data (10/25/50 data per halaman). | Admin, Staff | Should |

### 3.4 Modul 4: Asset Management & QR Code

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-401 | Staff Inventaris dapat menambahkan aset baru dengan field: Kode Aset, Nama, Kategori, Brand, Model, Serial Number, Purchase Date, Warranty, Supplier, Lokasi, Status, Foto. | Staff | Must |
| FR-402 | Sistem harus meng-generate **Kode Aset** secara otomatis dengan format unik (contoh: AST-2026-0001). | Sistem | Must |
| FR-403 | Sistem harus meng-generate **QR Code** otomatis untuk setiap aset berdasarkan Kode Aset. | Sistem | Must |
| FR-404 | Staff Inventaris dapat mengunggah foto aset (format JPG/PNG, maks 2MB). | Staff | Must |
| FR-405 | Admin/Staff dapat mengedit data aset (kecuali Kode Aset yang sudah final). | Admin, Staff | Must |
| FR-406 | Sistem harus mencatat status aset: Available, Borrowed, Maintenance, Broken, Lost, Disposed. | Sistem | Must |
| FR-407 | Sistem harus menyediakan halaman daftar aset dengan filter (kategori, lokasi, status, brand) dan pencarian. | Admin, Staff, Manajemen | Must |
| FR-408 | Sistem harus menyediakan halaman detail aset yang menampilkan seluruh informasi + riwayat peminjaman + riwayat maintenance. | Semua Role (terbatas) | Must |
| FR-409 | Staff Inventaris dapat mencetak label QR Code (single/batch) untuk ditempel pada aset fisik. | Staff | Must |
| FR-410 | Sistem harus mencegah penghapusan aset yang sedang dipinjam atau dalam maintenance. | Sistem | Must |
| FR-411 | Karyawan dapat melihat detail aset yang sedang ia pinjam. | Karyawan | Must |
| FR-412 | Manajemen dapat melihat seluruh data aset secara read-only. | Manajemen | Must |
| FR-413 | Staff Inventaris dapat mengunggah beberapa foto aset (maks 5 foto per aset). | Staff | Should |
| FR-414 | Sistem harus menyediakan fitur bulk import aset via file Excel/CSV. | Staff | Could |
| FR-415 | Sistem harus menampilkan indikator garansi (hijau/kuning/merah) berdasarkan tanggal expired. | Staff, Manajemen | Should |

### 3.5 Modul 5: Transaksi Barang Masuk & Keluar

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-501 | Staff Inventaris dapat mencatat **Barang Masuk** (pengadaan) dengan data: No. Invoice, Tanggal, Supplier, PIC Penerima, Catatan. | Staff | Must |
| FR-502 | Transaksi Barang Masuk harus memiliki detail item (aset apa saja yang masuk, jumlah, harga satuan). | Staff | Must |
| FR-503 | Sistem harus otomatis menambah stok/mencatat aset baru berdasarkan transaksi Barang Masuk. | Sistem | Must |
| FR-504 | Staff Inventaris dapat mencatat **Barang Keluar** dengan alasan: Disposal (dibuang), Hibah, Dijual, Dipindahkan. | Staff | Must |
| FR-505 | Transaksi Barang Keluar harus mencantumkan daftar aset yang keluar, alasan, dan PIC penanggung jawab. | Staff | Must |
| FR-506 | Sistem harus otomatis mengubah status aset menjadi "Disposed" saat tercatat di Barang Keluar. | Sistem | Must |
| FR-507 | Manajemen dapat melihat laporan Barang Masuk & Keluar secara read-only. | Manajemen | Must |
| FR-508 | Sistem harus menyediakan riwayat transaksi masuk/keluar dengan filter tanggal. | Admin, Staff, Manajemen | Must |
| FR-509 | Sistem harus menghitung total nilai transaksi masuk/keluar per periode. | Manajemen, Staff | Should |
| FR-510 | Staff dapat membatalkan transaksi Barang Masuk/Keluar yang salah (dengan catatan alasan). | Staff | Should |
| FR-511 | Sistem harus mencegah penghapusan transaksi yang sudah terlanjur mengubah status aset. | Sistem | Must |
| FR-512 | Untuk pengadaan bernilai besar, sistem harus meneruskan ke Manajemen untuk Approval. | Sistem | Should |

### 3.6 Modul 6: Peminjaman & Pengembalian Aset

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-601 | Karyawan dapat mengajukan **Request Peminjaman** aset dengan data: aset yang diminta, tujuan peminjaman, estimasi durasi. | Karyawan | Must |
| FR-602 | Sistem hanya mengizinkan request untuk aset dengan status "Available". | Sistem | Must |
| FR-603 | Staff Inventaris/Admin dapat melihat daftar request peminjaman yang masuk. | Staff, Admin | Must |
| FR-604 | Untuk peminjaman bernilai tinggi/jumlah banyak, sistem harus meneruskan ke **Manajemen untuk Approval**. | Sistem | Should |
| FR-605 | Manajemen dapat **Approve/Reject** request peminjaman. | Manajemen | Must |
| FR-606 | Setelah di-approve, Staff Inventaris memproses serah terima fisik dan sistem mengubah status aset menjadi "Borrowed". | Staff | Must |
| FR-607 | Sistem harus mencatat tanggal pinjam, tanggal jatuh tempo, dan peminjam. | Sistem | Must |
| FR-608 | Karyawan dapat mengajukan **Pengembalian** aset yang ia pinjam. | Karyawan | Must |
| FR-609 | Teknisi wajib **memverifikasi kondisi fisik** aset saat pengembalian sebelum diterima kembali ke gudang. | Teknisi | Must |
| FR-610 | Setelah verifikasi, status aset kembali menjadi "Available" (atau "Maintenance"/"Broken" jika ada kerusakan baru). | Sistem | Must |
| FR-611 | Sistem harus menampilkan notifikasi/daftar aset yang sudah melewati tanggal jatuh tempo (overdue). | Staff, Admin, Karyawan | Should |
| FR-612 | Karyawan dapat melihat riwayat peminjaman miliknya sendiri. | Karyawan | Must |
| FR-613 | Sistem harus membatasi 1 request aktif per karyawan untuk aset yang sama. | Sistem | Must |
| FR-614 | Staff dapat memperpanjang durasi peminjaman atas request karyawan. | Staff | Should |
| FR-615 | Sistem harus mencatat kondisi aset saat serah terima (baik/rusak ringan/rusak berat). | Teknisi, Staff | Should |

### 3.7 Modul 7: Maintenance Tracking

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-701 | Karyawan/Teknisi dapat **melaporkan kerusakan** aset dengan data: aset, deskripsi kerusakan, tanggal laporan, foto (opsional). | Karyawan, Teknisi | Must |
| FR-702 | Sistem otomatis mengubah status aset menjadi "Maintenance" atau "Broken" saat laporan masuk. | Sistem | Must |
| FR-703 | Teknisi dapat mencatat detail perbaikan: vendor perbaikan, estimasi biaya, tanggal mulai, estimasi selesai. | Teknisi | Must |
| FR-704 | Teknisi dapat mengubah status perbaikan: In Progress, Completed, Cancelled. | Teknisi | Must |
| FR-705 | Setelah perbaikan selesai, Teknisi mencatat biaya aktual dan catatan hasil perbaikan. | Teknisi | Must |
| FR-706 | Sistem harus menyediakan riwayat maintenance per aset (untuk analisis kelayakan). | Teknisi, Staff, Manajemen | Must |
| FR-707 | Manajemen dapat melihat laporan total biaya maintenance per periode. | Manajemen | Must |
| FR-708 | Teknisi dapat mengunggah foto kondisi aset sebelum/sesudah perbaikan. | Teknisi | Should |
| FR-709 | Sistem harus memberikan rekomendasi disposal jika biaya maintenance > 50% harga aset. | Sistem | Could |
| FR-710 | Karyawan dapat melihat status perbaikan aset yang ia laporkan. | Karyawan | Must |
| FR-711 | Teknisi dapat menugaskan vendor eksternal untuk perbaikan tertentu. | Teknisi | Should |

### 3.8 Modul 8: Dashboard & Statistik

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-801 | Sistem harus menyediakan **Dashboard Eksekutif** untuk Manajemen: total aset, total nilai aset, aset per kategori, aset per lokasi, tren peminjaman bulanan. | Manajemen | Must |
| FR-802 | Sistem harus menyediakan **Dashboard Operasional** untuk Staff Inventaris: aset tersedia, aset dipinjam, aset dalam maintenance, request pending. | Staff | Must |
| FR-803 | Sistem harus menyediakan **Dashboard Teknisi**: daftar aset rusak, perbaikan in-progress, selesai bulan ini. | Teknisi | Must |
| FR-804 | Sistem harus menyediakan **Dashboard Pribadi** untuk Karyawan: aset yang sedang dipinjam, request status, riwayat. | Karyawan | Must |
| FR-805 | Dashboard harus menampilkan visualisasi grafik (pie, bar, line) menggunakan Chart.js. | Sistem | Must |
| FR-806 | Data dashboard harus di-agregasi secara real-time dari database. | Sistem | Should |
| FR-807 | Dashboard Admin menampilkan ringkasan seluruh aktivitas sistem (user aktif, transaksi harian, error log). | Admin | Should |
| FR-808 | Dashboard harus mendukung filter periode (hari ini, minggu ini, bulan ini, custom range). | Manajemen, Staff | Should |
| FR-809 | Sistem harus menampilkan widget "Aset Overdue" di dashboard Staff. | Staff | Must |
| FR-810 | Sistem harus menampilkan widget "Aset即将 Expired Warranty" di dashboard Staff. | Staff | Could |

### 3.9 Modul 9: Report & Document Export

| ID | Kebutuhan Fungsional | Aktor | Prioritas |
| --- | --- | --- | --- |
| FR-901 | Sistem harus dapat meng-ekspor **Daftar Aset** ke format PDF. | Admin, Staff, Manajemen | Must |
| FR-902 | Sistem harus dapat meng-ekspor **Daftar Aset** ke format Excel (.xlsx). | Admin, Staff, Manajemen | Must |
| FR-903 | Sistem harus dapat meng-ekspor **Laporan Peminjaman** (periode tertentu) ke PDF/Excel. | Admin, Staff, Manajemen | Must |
| FR-904 | Sistem harus dapat meng-ekspor **Laporan Barang Masuk/Keluar** ke PDF/Excel. | Admin, Staff, Manajemen | Must |
| FR-905 | Sistem harus dapat meng-ekspor **Laporan Maintenance** (biaya & riwayat) ke PDF/Excel. | Admin, Staff, Manajemen | Must |
| FR-906 | Laporan yang di-ekspor harus memiliki header perusahaan (logo, nama PT, tanggal cetak). | Sistem | Should |
| FR-907 | Sistem harus mendukung filter laporan berdasarkan rentang tanggal, kategori, lokasi, departemen. | Admin, Staff, Manajemen | Must |
| FR-908 | Sistem harus dapat meng-ekspor **Laporan Pengguna Aktif** ke PDF/Excel. | Admin | Should |
| FR-909 | Laporan PDF harus memiliki format yang rapi dan profesional (layak untuk presentasi manajemen). | Sistem | Should |
| FR-910 | Sistem harus menyediakan template laporan yang dapat di-customize oleh Admin. | Admin | Could |

---

## 4. Kebutuhan Non-Fungsional (Non-Functional Requirements)

| ID | Kategori | Kebutuhan Non-Fungsional |
| --- | --- | --- |
| NFR-001 | **Performa** | Setiap endpoint API harus merespons dalam waktu maksimal **2 detik** untuk operasi standar (CRUD) dan **5 detik** untuk operasi kompleks (export laporan, agregasi dashboard). |
| NFR-002 | **Performa** | Sistem harus mampu menangani minimal **50 pengguna konkuren** tanpa penurunan performa signifikan. |
| NFR-003 | **Keamanan** | Password pengguna harus di-hash menggunakan **bcrypt** (minimal 10 salt rounds). |
| NFR-004 | **Keamanan** | Seluruh komunikasi API harus menggunakan **HTTPS** pada environment production. |
| NFR-005 | **Keamanan** | Sistem harus menerapkan **rate limiting** untuk mencegah brute-force attack pada endpoint login. |
| NFR-006 | **Keamanan** | Seluruh endpoint yang dilindungi wajib divalidasi dengan **JWT middleware**. |
| NFR-007 | **Keamanan** | Sistem harus menerapkan **Role-Based Access Control (RBAC)** di sisi backend (bukan hanya di frontend). |
| NFR-008 | **Keamanan** | Seluruh input dari client wajib divalidasi di backend menggunakan **express-validator/Joi**. |
| NFR-009 | **Keamanan** | Kredensial sensitif (DB password, JWT secret) harus disimpan di file **.env** dan tidak boleh di-commit ke Git. |
| NFR-010 | **Usability** | Antarmuka harus **responsif** (desktop, tablet, mobile) menggunakan Bootstrap 5. |
| NFR-011 | **Usability** | Sistem harus memiliki navigasi yang konsisten dan menu dinamis sesuai role pengguna. |
| NFR-012 | **Usability** | Sistem harus memberikan feedback yang jelas (loading state, success/error toast) pada setiap aksi pengguna. |
| NFR-013 | **Reliabilitas** | Sistem harus memiliki error handling terpusat (centralized error handler) dengan response JSON yang konsisten. |
| NFR-014 | **Maintainability** | Kode backend harus mengikuti struktur modular (controllers, models, routes, middlewares, utils). |
| NFR-015 | **Maintainability** | Kode frontend harus mengikuti prinsip satu komponen satu file dengan penamaan PascalCase. |
| NFR-016 | **Portabilitas** | Sistem harus dapat dijalankan di environment Linux, macOS, dan Windows. |
| NFR-017 | **Dokumentasi** | Seluruh endpoint API wajib didokumentasikan menggunakan **Swagger/OpenAPI**. |
| NFR-018 | **Kompatibilitas** | Sistem harus berjalan normal di browser modern (Chrome, Firefox, Edge, Safari versi terbaru). |
| NFR-019 | **Skalabilitas** | Struktur database harus dinormalisasi (minimal 3NF) untuk mendukung pengembangan fitur lanjutan. |
| NFR-020 | **Audit** | Sistem harus mencatat log aktivitas penting (login, create/update/delete data kritis) untuk keperluan audit. |
| NFR-021 | **Availability** | Sistem harus memiliki uptime minimal 99% pada environment production. |
| NFR-022 | **Backup** | Database harus di-backup otomatis setiap hari (daily backup). |
| NFR-023 | **Logging** | Sistem harus mencatat log error dan aktivitas ke file log terpisah (bukan console.log saja). |
| NFR-024 | **Testing** | Setiap endpoint API wajib memiliki minimal 1 test case di Postman Collection. |
| NFR-025 | **Code Quality** | Kode harus mengikuti standar ESLint/Prettier untuk konsistensi format. |

---

## 5. Kebutuhan Antarmuka (Interface Requirements)

### 5.1 Antarmuka Pengguna (User Interface)
* Aplikasi frontend dibangun sebagai **Single Page Application (SPA)** menggunakan React + Vite.
* Styling menggunakan **CSS3 + Bootstrap 5** (atau React-Bootstrap).
* Routing menggunakan **React Router** dengan proteksi route berbasis role.
* State management menggunakan **React Hooks** (useState, useContext).
* Komunikasi ke backend menggunakan **Axios** dengan interceptor untuk JWT.
* Layout aplikasi harus konsisten: Sidebar (menu dinamis per role) + Header (info user + logout) + Main Content.
* Setiap halaman harus memiliki breadcrumb untuk navigasi yang jelas.
* Form input harus memiliki validasi real-time (client-side) sebelum submit ke backend.

### 5.2 Antarmuka Perangkat Keras (Hardware Interface)
* Sistem tidak berinteraksi langsung dengan hardware khusus, namun mendukung:
  * **Printer** untuk mencetak label QR Code aset.
  * **Webcam/Scanner** (opsional) untuk scan QR Code via browser (menggunakan library jsQR atau html5-qrcode).

### 5.3 Antarmuka Perangkat Lunak (Software Interface)
* **Database:** MySQL 8.x (via mysql2 atau Sequelize).
* **Runtime:** Node.js 18+.
* **Framework Backend:** Express.js 4.x.
* **Library Pendukung Backend:**
  * JWT (jsonwebtoken)
  * Validasi (express-validator / Joi)
  * Upload file (multer)
  * QR Code (qrcode)
  * Export PDF (pdfkit / puppeteer)
  * Export Excel (exceljs)
  * API Docs (swagger-ui-express, swagger-jsdoc)
  * Password hashing (bcrypt)
  * Environment variables (dotenv)
  * CORS (cors)
  * Logging (morgan, winston)
* **Library Pendukung Frontend:**
  * React 18+
  * React Router
  * Axios
  * Chart.js (react-chartjs-2)
  * Bootstrap 5
  * React Icons
  * React Toastify (untuk notifikasi)
  * React Hook Form (untuk form handling)

### 5.4 Antarmuka Komunikasi (Communication Interface)
* Protokol: **HTTP/HTTPS**
* Format data: **JSON**
* Pola URL RESTful: `/api/v1/{resource}` (kata benda jamak)
* HTTP Method: GET, POST, PUT/PATCH, DELETE sesuai fungsi.
* Response format standar:
```json
{
  "success": true,
  "message": "Deskripsi pesan",
  "data": { ... }
}

