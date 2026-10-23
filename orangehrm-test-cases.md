# Laporan Testing — OrangeHRM Demo

## 1. Overview

| | |
|---|---|
| **Nama Aplikasi** | OrangeHRM Demo (Open Source HR Management System) |
| **URL** | https://opensource-demo.orangehrmlive.com |
| **Tanggal Testing** | [isi tanggal kamu testing] |
| **Jenis Testing** | Manual Functional Testing — Risk-Based (Boundary Value Analysis & Equivalence Partitioning) |
| **Tools** | Browser (Chrome), Markdown untuk dokumentasi |
| **Tujuan** | Menguji fungsi utama HR system dari sisi kenyamanan user: apakah fitur inti berjalan semestinya dan menangani input tidak valid dengan baik |

## 2. Strategi Pemilihan Test Case

Daripada menguji semua kombinasi input, test case dipilih dengan prioritas berikut:

1. **Happy Path / Critical Path** — alur utama yang paling sering dipakai user (login → dashboard → kelola data karyawan → logout).
2. **Boundary Value Analysis** — fokus di titik batas (field angka, panjang karakter), karena bug paling sering muncul di sini.
3. **Equivalence Partitioning** — input dikelompokkan jadi kelas valid/tidak valid, cukup 1-2 perwakilan per kelas, tidak semua kombinasi dicoba.
4. **Representative Negative Case** — hanya skenario error yang realistis terjadi di dunia nyata (field kosong, format salah), bukan semua variasi karakter aneh.

## 3. Scope Testing

Fitur yang diuji: Login, Dashboard, PIM (Personnel Information Management — tambah & cari karyawan), Leave (ajukan cuti), Logout.

**Di luar scope:** modul Payroll, Recruitment, Performance, Admin (role management), testing API, testing performa.

## 4. Test Environment

| | |
|---|---|
| **Browser** | Google Chrome versi 130 |
| **OS** | Windows 11 |
| **Akun** | Admin (username: Admin / password: admin123 — kredensial demo publik) |

## 5. Test Case

### 5.1 Login (5 test case)

| ID | Skenario | Teknik | Langkah | Expected Result | Status |
|----|----------|--------|---------|------------------|--------|
| TC-01 | Login dengan username & password valid | Happy Path | 1. Buka halaman login<br>2. Isi username `Admin`, password `admin123`<br>3. Klik Login | Berhasil masuk ke Dashboard | |
| TC-02 | Login dengan password salah | Negative | 1. Isi username valid, password salah<br>2. Klik Login | Muncul pesan "Invalid credentials" | |
| TC-03 | Login dengan field kosong | Negative (Equivalence) | 1. Kosongkan username & password<br>2. Klik Login | Muncul pesan "Required" pada kedua field | |
| TC-04 | Login dengan username mengandung spasi di awal/akhir | Boundary | 1. Isi username ` Admin ` (ada spasi)<br>2. Isi password valid<br>3. Klik Login | Sistem trim spasi dan tetap berhasil login, atau muncul error yang jelas | |
| TC-05 | Login lalu klik Logout, coba back ke dashboard | Edge Case | 1. Login berhasil<br>2. Logout<br>3. Klik tombol Back browser | Tidak bisa akses dashboard, diarahkan ke halaman login | |

### 5.2 Dashboard (2 test case)

| ID | Skenario | Teknik | Langkah | Expected Result | Status |
|----|----------|--------|---------|------------------|--------|
| TC-06 | Dashboard menampilkan widget utama setelah login | Happy Path | 1. Login<br>2. Amati halaman Dashboard | Widget (Time at Work, My Actions, dll) tampil tanpa error | |
| TC-07 | Navigasi antar menu sidebar (Admin, PIM, Leave, dll) | Happy Path | 1. Klik tiap menu di sidebar satu per satu | Setiap menu terbuka tanpa error/blank page | |

### 5.3 PIM — Tambah Karyawan (8 test case)

| ID | Skenario | Teknik | Langkah | Expected Result | Status |
|----|----------|--------|---------|------------------|--------|
| TC-08 | Tambah karyawan dengan data lengkap & valid | Happy Path | 1. Masuk PIM → Add Employee<br>2. Isi First Name, Last Name<br>3. Klik Save | Karyawan baru tersimpan dan muncul di Employee List | |
| TC-09 | Tambah karyawan dengan First Name kosong | Negative (Equivalence) | 1. Kosongkan First Name, isi Last Name<br>2. Klik Save | Muncul pesan "Required" | |
| TC-10 | First Name 1 karakter (batas bawah) | Boundary | 1. Isi First Name dengan 1 huruf, misal "A" | Diterima (jika memang field tidak punya minimal panjang) atau muncul validasi jelas | |
| TC-11 | First Name tepat di batas maksimal karakter (biasanya 30/40, cek dulu di form) | Boundary | 1. Isi First Name sepanjang batas maksimal yang ditentukan | Diterima dan tersimpan utuh | |
| TC-12 | First Name 1 karakter melebihi batas maksimal | Boundary | 1. Isi First Name 1 karakter lebih panjang dari maksimal | Ditolak, atau karakter terpotong otomatis (expected result tergantung spesifikasi) | |
| TC-13 | Isi First Name dengan angka/simbol | Negative (Equivalence) | 1. Isi First Name "12345" atau "@@@" | Idealnya muncul validasi format nama, atau catat sebagai bug jika diterima begitu saja | |
| TC-14 | Upload foto profil karyawan | Happy Path | 1. Pilih employee → upload foto (format jpg/png) | Foto berhasil terupload dan tampil | |
| TC-15 | Cari karyawan dengan nama yang tidak ada di sistem | Negative | 1. Masuk Employee List<br>2. Cari nama random yang tidak terdaftar | Muncul pesan "No Records Found", bukan error/crash | |

### 5.4 Leave — Ajukan Cuti (7 test case)

| ID | Skenario | Teknik | Langkah | Expected Result | Status |
|----|----------|--------|---------|------------------|--------|
| TC-16 | Ajukan cuti dengan tanggal valid (hari ini s/d besok) | Happy Path | 1. Masuk Leave → Apply<br>2. Pilih leave type, tanggal mulai & akhir<br>3. Klik Apply | Pengajuan cuti berhasil tersimpan dan muncul di Leave List | |
| TC-17 | Ajukan cuti dengan tanggal mulai setelah tanggal akhir | Negative | 1. Isi tanggal mulai lebih besar dari tanggal akhir<br>2. Klik Apply | Muncul pesan error validasi tanggal | |
| TC-18 | Ajukan cuti tanpa memilih Leave Type | Negative (Equivalence) | 1. Kosongkan Leave Type, isi tanggal<br>2. Klik Apply | Muncul pesan "Required" | |
| TC-19 | Ajukan cuti dengan durasi 1 hari (batas bawah) | Boundary | 1. Pilih tanggal mulai = tanggal akhir (1 hari) | Berhasil, durasi tercatat 1 hari | |
| TC-20 | Ajukan cuti dengan tanggal di masa lalu | Negative/Edge Case | 1. Pilih tanggal mulai sebelum hari ini | Idealnya ditolak atau muncul warning; catat sebagai bug jika diterima tanpa validasi | |
| TC-21 | Batalkan pengajuan cuti yang sudah dibuat | Happy Path | 1. Masuk Leave List<br>2. Pilih cuti yang baru diajukan<br>3. Klik Cancel | Status cuti berubah jadi "Cancelled" | |
| TC-22 | Ajukan 2 cuti dengan tanggal yang overlap | Edge Case | 1. Ajukan cuti tanggal 1-3<br>2. Ajukan cuti lagi tanggal 2-4 (overlap) | Idealnya muncul warning/error; catat sebagai bug jika sistem mengizinkan tanpa validasi | |

### 5.5 Logout & Session (2 test case)

| ID | Skenario | Teknik | Langkah | Expected Result | Status |
|----|----------|--------|---------|------------------|--------|
| TC-23 | Logout dari sistem | Happy Path | 1. Klik ikon profil di kanan atas<br>2. Klik Logout | Berhasil keluar, diarahkan ke halaman login | |
| TC-24 | Buka dua tab, logout di salah satu tab | Edge Case | 1. Buka 2 tab dengan sesi sama<br>2. Logout di tab 1<br>3. Coba aksi di tab 2 | Tab 2 idealnya ikut ter-logout atau diminta login ulang | |

## 6. Bug Report

*(Isi di sini kalau ada temuan selama eksekusi, format sama seperti laporan sebelumnya: ID, Judul, Severity, Steps to Reproduce, Expected, Actual, Screenshot)*

## 7. Summary

- **Total test case**: 24
- **Distribusi teknik**: Happy Path (9), Negative/Equivalence (8), Boundary (5), Edge Case (4)
- **Alasan cakupan**: Test case difokuskan pada alur kritikal (login, kelola data karyawan, pengajuan cuti) dan titik rawan bug (batas input, validasi tanggal), bukan menguji semua kombinasi input yang mungkin.
- **Pass / Fail**: [isi setelah eksekusi]
- **Kesimpulan**: [isi setelah eksekusi]

---
*Catatan: OrangeHRM Demo adalah instance open source publik yang memang disediakan untuk keperluan demo dan latihan, sehingga aman untuk didokumentasikan secara terbuka di portofolio.*
