# Product Requirement Document (PRD)
## IT Asset Management System (ITAMS)
**Studi Kasus:** PT. Makerindo Prima Solusi  
**Versi Dokumen:** 1.0  
**Tanggal:** 15 Juli 2026  

---

## 1. Latar Belakang & Studi Kasus
**PT. Makerindo Prima Solusi** adalah perusahaan yang bergerak di bidang pengembangan software dan perangkat IoT. Seiring dengan pertumbuhan bisnis, jumlah proyek, dan penambahan karyawan, jumlah aset IT perusahaan juga meningkat pesat. Aset yang dikelola meliputi laptop, server, perangkat jaringan, perangkat IoT, hingga lisensi software.

**Permasalahan Saat Ini:**
Saat ini, perusahaan belum memiliki sistem pengelolaan aset yang terpusat. Pencatatan masih dilakukan secara manual menggunakan spreadsheet atau kertas. Kondisi ini menimbulkan berbagai masalah operasional:
1. **Ketidakjelasan Aset:** Tidak ada sistem yang mencatat secara *real-time* siapa yang sedang memegang atau meminjam aset tertentu.
2. **Pelacakan Buruk:** Sulit melacak status aset yang sedang dalam masa *maintenance*, rusak, atau bahkan hilang.
3. **Administrasi Lemah:** Proses peminjaman dan pengembalian aset tidak terdokumentasi dengan baik, sehingga rawan sengketa atau kehilangan.
4. **Tidak Ada Data Akurat:** Manajemen tidak memiliki laporan yang akurat dan *real-time* untuk mendukung pengambilan keputusan strategis (seperti pengadaan aset baru atau *disposal*).

Berdasarkan permasalahan tersebut, diperlukan pembangunan **IT Asset Management System (ITAMS)** berbasis web untuk menyelesaikan masalah pencatatan, peminjaman, pengembalian, dan *maintenance* aset IT secara terpusat.

---

## 2. Tujuan Produk
1. Membangun sistem pengelolaan aset IT yang terpusat, akurat, dan mudah digunakan oleh seluruh pihak di PT. Makerindo Prima Solusi.
2. Menghilangkan pencatatan manual dan menyediakan *database* aset yang *real-time*.
3. Mempermudah proses peminjaman, pengembalian, dan pelacakan aset menggunakan teknologi QR Code.
4. Menyediakan dashboard dan laporan otomatis (PDF/Excel) untuk membantu manajemen dalam pengambilan keputusan.
5. Menerapkan sistem keamanan berbasis peran (*Role-Based Access Control*) agar setiap pengguna hanya mengakses fitur yang menjadi haknya.

---

## 3. Target Pengguna & Kebutuhan Detail Tiap Role
Sistem ini akan digunakan oleh 5 kategori pengguna (role) dengan kebutuhan dan hak akses yang sangat spesifik. Berikut adalah rincian detail kebutuhan masing-masing role:

### 3.1. Admin
**Deskripsi:** Pengelola utama sistem yang memastikan seluruh infrastruktur aplikasi berjalan lancar dan data master terkelola dengan benar.
**Kebutuhan & Hak Akses Detail:**
* **Manajemen Pengguna & Role:** Membuat, mengedit, menonaktifkan akun karyawan, dan menetapkan role (siapa yang jadi Staff, Teknisi, dll).
* **Manajemen Master Data:** Mengelola data induk sistem seperti Kategori Aset, Brand, Supplier, Lokasi (Gudang, Ruang Server, dll), dan Departemen.
* **Akses Penuh (Superuser):** Dapat melihat dan mengedit seluruh modul, termasuk data yang seharusnya hanya bisa dilihat oleh role lain (sebagai *fallback* jika ada kesalahan input).
* **Audit & Keamanan:** Memastikan sistem berjalan tanpa *error* dan memvalidasi struktur data agar tidak terjadi *duplikasi* atau *invalid data*.

### 3.2. Manajemen (Management)
**Deskripsi:** Pengambil keputusan strategis (seperti CEO, CTO, atau Manajer Operasional) yang tidak terlibat dalam operasional harian, namun butuh visibilitas penuh terhadap aset perusahaan.
**Kebutuhan & Hak Akses Detail:**
* **Dashboard Eksekutif:** Melihat ringkasan visual (grafik) total nilai aset, aset per kategori, persebaran aset per lokasi, dan tren peminjaman.
* **Laporan & Export:** Mengunduh laporan komprehensif dalam format PDF atau Excel untuk keperluan rapat direksi atau audit eksternal.
* **Sistem Persetujuan (Approval Workflow):** Memberikan persetujuan (approve/reject) untuk peminjaman aset bernilai tinggi atau pengajuan pengadaan (barang masuk) aset baru dalam jumlah besar.
* **Read-Only Access:** Hanya bisa melihat data (view) tanpa bisa mengedit data mentah untuk mencegah kesalahan input yang merusak database.

### 3.3. Staff Inventaris
**Deskripsi:** Ujung tombak operasional harian yang bertanggung jawab atas fisik dan pencatatan data aset di gudang/perusahaan.
**Kebutuhan & Hak Akses Detail:**
* **Input Data Aset (Master Asset):** Menambahkan aset baru secara detail (Kode Aset, Nama, Kategori, Brand, Serial Number, Tanggal Beli, Garansi, dll) dan mengunggah foto aset.
* **Generate & Cetak QR Code:** Membuat QR Code unik untuk setiap aset agar bisa di-*scan* saat opname atau peminjaman.
* **Transaksi Barang Masuk (Incoming):** Mencatat aset baru yang datang dari supplier (input invoice, jumlah, harga, dan PIC penerima).
* **Transaksi Barang Keluar (Outgoing):** Mencatat aset yang dikeluarkan dari inventaris karena alasan dijual, dihibahkan, atau dibuang (*disposal*).
* **Manajemen Peminjaman:** Memproses request peminjaman dari karyawan, menyiapkan fisik aset, dan mencatat status aset menjadi *Borrowed*.
* **Manajemen Supplier:** Mendata kontak dan riwayat transaksi dengan vendor/supplier penyedia aset.

### 3.4. Teknisi (IT Support)
**Deskripsi:** Tim yang bertanggung jawab atas kondisi teknis, perbaikan, dan verifikasi kelayakan aset IT.
**Kebutuhan & Hak Akses Detail:**
* **Pencatatan Maintenance:** Menerima laporan aset rusak, mencatat detail kerusakan, vendor perbaikan, estimasi biaya, dan status perbaikan.
* **Verifikasi Pengembalian Aset:** Saat karyawan mengembalikan aset, teknisi wajib memverifikasi kondisi fisik dan fungsi aset sebelum diterima kembali ke gudang (memastikan tidak ada kerusakan baru).
* **Update Status Aset:** Mengubah status aset dari *Available* menjadi *Maintenance*, *Broken*, atau kembali ke *Available* setelah selesai diperbaiki.
* **Riwayat Perbaikan:** Melihat history maintenance sebuah aset berdasarkan Serial Number/Kode Aset untuk memutuskan apakah aset layak dipakai atau harus di-*disposal*.

### 3.5. Karyawan (Employee)
**Deskripsi:** Pengguna akhir yang menggunakan aset IT untuk menunjang pekerjaan sehari-hari.
**Kebutuhan & Hak Akses Detail:**
* **Dashboard Pribadi:** Melihat daftar aset apa saja yang sedang ia pegang/pinjam saat ini beserta tanggal batas pengembaliannya.
* **Request Peminjaman:** Mengajukan permohonan peminjaman aset (misal: butuh laptop tambahan untuk proyek) dengan mengisi form request.
* **Pelaporan Kerusakan:** Melaporkan jika aset yang dipegangnya mengalami kerusakan atau hilang melalui sistem.
* **Riwayat Peminjaman:** Melihat history aset apa saja yang pernah ia pinjam dan kembalikan di masa lalu.
* **Akses Terbatas:** Hanya bisa melihat data miliknya sendiri dan tidak bisa melihat data aset karyawan lain atau data master.

---

## 4. Fitur Utama Sistem
Berdasarkan kebutuhan di atas, sistem ITAMS akan memiliki fitur-fitur utama berikut:
1. **Autentikasi & Otorisasi:** Login/Logout dengan JWT, manajemen role, dan pembatasan akses berbasis *middleware*.
2. **Manajemen Master Data:** CRUD untuk Kategori, Brand, Supplier, Lokasi, Departemen, dan User.
3. **Manajemen Aset (Asset Management):** CRUD aset, upload foto, generate QR Code, dan pelacakan status (*Available, Borrowed, Maintenance, Broken, Lost, Disposed*).
4. **Transaksi Aset:** Pencatatan Barang Masuk (Pengadaan) dan Barang Keluar (Disposal/Hibah).
5. **Siklus Peminjaman (Borrowing & Return):** Alur *Request → Approve → Borrowed → Return → Verified by Teknisi → Done*.
6. **Manajemen Maintenance:** Pencatatan kerusakan, proses perbaikan, biaya vendor, dan verifikasi teknisi.
7. **Dashboard & Statistik:** Visualisasi data aset menggunakan grafik (Chart.js).
8. **Pelaporan (Reporting):** Generate dan unduh laporan dalam format PDF dan Excel.

---

## 5. Batasan Sistem (Out of Scope)
Untuk fase pengembangan awal (MVP - *Minimum Viable Product*), sistem **TIDAK** akan mencakup fitur-fitur berikut:
1. **Integrasi Sistem Keuangan/Akuntansi:** Sistem ini tidak akan terhubung dengan software akuntansi (seperti Jurnal/Mekari) untuk perhitungan depresiasi aset secara otomatis.
2. **Aplikasi Mobile Native:** Tidak ada pembuatan aplikasi Android/iOS native. Sistem fokus pada *Web Responsif* yang bisa diakses via browser di HP/Tablet.
3. **Notifikasi Eksternal:** Sistem belum mendukung pengiriman notifikasi otomatis via Email atau WhatsApp (notifikasi hanya tampil di dalam dashboard/web).
4. **Integrasi IoT Real-time:** Meskipun perusahaan membuat IoT, sistem ini hanya mencatat IoT sebagai "Aset/Barang", bukan memonitoring data sensor dari alat IoT tersebut secara *real-time*.

---

## 6. Asumsi & Ketergantungan
* **Asumsi:** Seluruh pengguna (karyawan hingga manajemen) memiliki akses ke perangkat (laptop/PC/Smartphone) dengan browser modern untuk mengakses sistem berbasis web.
* **Asumsi:** Setiap aset fisik yang masuk ke perusahaan akan diberikan label fisik (QR Code) yang dicetak oleh Staff Inventaris.
* **Ketergantungan:** Ketersediaan server/hosting dan database MySQL yang stabil untuk menjalankan aplikasi.
* **Ketergantungan:** Kedisiplinan pengguna dalam menginput dan memperbarui status aset secara *real-time* saat ada perpindahan fisik.