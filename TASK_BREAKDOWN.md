# Task Breakdown – IT Asset Management System (ITAMS)

Dokumen ini menjelaskan pembagian tugas, peran, serta tanggung jawab setiap anggota tim dalam pengembangan **IT Asset Management System (ITAMS)** di **PT. Makerindo Prima Solusi**. Proses pengembangan sistem dibagi menjadi dua area utama, yaitu **Backend Development** dan **Frontend Development**, dengan pembagian tanggung jawab yang disesuaikan dengan kompetensi masing-masing anggota tim.

---

# 1. Profil dan Peran Tim Pengembangan

## Backend Developer

**Nama:** Fajar Bahari Supriatna

**Tanggung Jawab:**
- Merancang dan mengelola struktur basis data menggunakan MySQL.
- Mengembangkan RESTful API menggunakan Express.js.
- Mengimplementasikan autentikasi dan otorisasi menggunakan JSON Web Token (JWT).
- Melakukan validasi data pada setiap endpoint API.
- Mengembangkan fitur pembuatan QR Code untuk identifikasi aset.
- Mengimplementasikan mekanisme unggah (upload) berkas.
- Mengembangkan fitur ekspor dokumen dan laporan.

## Frontend Developer

**Nama:** Wulan Sugiarti

**Tanggung Jawab:**
- Merancang antarmuka pengguna (UI) dan pengalaman pengguna (UX) menggunakan Figma atau Google Stitch.
- Mengembangkan aplikasi web berbasis Single Page Application (SPA) menggunakan React (Vite).
- Mengelola state aplikasi menggunakan React Hooks.
- Mengimplementasikan sistem navigasi (routing) pada aplikasi.
- Melakukan integrasi RESTful API menggunakan Axios.
- Memastikan antarmuka aplikasi bersifat responsif, konsisten, dan mudah digunakan.

---

# 2. Matriks Pembagian Modul

Pembagian modul utama pada proyek ITAMS disusun berdasarkan ruang lingkup pekerjaan dan tanggung jawab masing-masing anggota tim.

| Nama Modul / Fitur | Backend Developer | Frontend Developer | Status Modul |
| :----------------- | :---------------- | :----------------- | :----------- |
| Modul 1: Authentication & JWT | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |
| Modul 2: User & Role Management | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |
| Modul 3: Master Data & Lokasi | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |
| Modul 4: Asset Management & QR Code | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |
| Modul 5: Transaksi Barang Masuk & Keluar | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |
| Modul 6: Peminjaman & Pengembalian Aset | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |
| Modul 7: Maintenance Tracking | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |
| Modul 8: Dashboard & Statistik | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |
| Modul 9: Report & Document Export | Fajar Bahari Supriatna | Wulan Sugiarti | Tahap Perencanaan |

---

# 3. Rencana Alur Kerja Kolaborasi

Untuk mendukung proses pengembangan sistem menggunakan arsitektur modern berbasis **React** dan **Express.js**, alur kerja kolaborasi antara Backend Developer dan Frontend Developer dirancang sebagai berikut:

1. **Penyusunan API Contract**

   Sebelum proses implementasi dimulai, Backend Developer dan Frontend Developer menyepakati spesifikasi komunikasi data (API Contract), meliputi endpoint, parameter, struktur request, response, serta format JSON yang akan didokumentasikan pada dokumen **Software Design Document (SDD)** menggunakan Swagger/OpenAPI. Kesepakatan ini memungkinkan Frontend Developer mengembangkan antarmuka menggunakan *mock data* tanpa menunggu implementasi API selesai.

2. **Pengembangan Secara Paralel**

   Setelah API Contract disetujui, masing-masing anggota tim melaksanakan pengembangan secara mandiri. Backend Developer berfokus pada implementasi basis data, logika bisnis, dan RESTful API, sedangkan Frontend Developer berfokus pada pengembangan antarmuka pengguna menggunakan React, Bootstrap 5, dan implementasi navigasi aplikasi.

3. **Tahap Integrasi Sistem**

   Setelah pengembangan backend dan frontend selesai, dilakukan proses integrasi dengan menghubungkan aplikasi React ke RESTful API menggunakan Axios. Tahap ini juga mencakup pengujian fungsionalitas, validasi komunikasi data, serta penyempurnaan sistem apabila ditemukan kendala selama proses integrasi.

---

# 4. Rincian Pembagian Tugas per Fitur

Rincian teknis setiap modul, termasuk daftar endpoint RESTful API, struktur request dan response, pembagian komponen antarmuka, serta spesifikasi implementasi masing-masing fitur akan dijabarkan secara lebih rinci pada **Tahap 6** setelah dokumen **Software Requirements Specification (SRS)** dan **Software Design Document (SDD)** memperoleh persetujuan dari **Kak Daffa** selaku pembimbing proyek.