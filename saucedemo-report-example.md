# Laporan Testing — SauceDemo

## 1. Overview

| | |
|---|---|
| **Nama Aplikasi** | SauceDemo |
| **URL** | https://www.saucedemo.com |
| **Tanggal Testing** | 23 Oktober 2026 |
| **Jenis Testing** | Manual Functional Testing (Positive & Negative Case) |
| **Tools** | Browser (Chrome), Excel/Markdown untuk dokumentasi |
| **Tujuan** | Latihan functional testing pada flow e-commerce: login, katalog produk, keranjang, dan checkout |

## 2. Scope Testing

Fitur yang diuji:
- Login (berbagai jenis user: standard, locked out, problem, performance glitch)
- Daftar produk (sorting, tampilan)
- Tambah/hapus produk dari keranjang
- Proses checkout (isi data, review, konfirmasi)
- Logout

**Di luar scope:** testing performa, testing keamanan (SQL injection, XSS, dsb.), testing API langsung.

## 3. Test Environment

| | |
|---|---|
| **Browser** | Google Chrome versi 130 |
| **OS** | Windows 11 |
| **Resolusi** | 1920x1080 |
| **Device** | Desktop |

## 4. Test Case

### 4.1 Login

| ID | Skenario | Jenis | Langkah | Expected Result | Actual Result | Status |
|----|----------|-------|---------|------------------|----------------|--------|
| TC-001 | Login dengan username & password valid (standard_user) | Positive | 1. Buka halaman login<br>2. Isi username: `standard_user`<br>3. Isi password: `secret_sauce`<br>4. Klik Login | Berhasil masuk ke halaman produk (Inventory) | Sesuai | Pass |
| TC-002 | Login dengan akun locked_out_user | Negative | 1. Isi username: `locked_out_user`<br>2. Isi password: `secret_sauce`<br>3. Klik Login | Muncul pesan error "Sorry, this user has been locked out" | Sesuai | Pass |
| TC-003 | Login dengan password salah | Negative | 1. Isi username valid<br>2. Isi password salah<br>3. Klik Login | Muncul pesan error username/password tidak cocok | Sesuai | Pass |
| TC-004 | Login dengan field kosong | Negative | 1. Kosongkan username & password<br>2. Klik Login | Muncul pesan error "Username is required" | Sesuai | Pass |
| TC-005 | Login dengan problem_user | Positive (edge case) | 1. Login dengan `problem_user` | Masuk ke Inventory, cek apakah ada bug tampilan gambar produk | Gambar produk tertukar/sama semua | **Fail** (lihat Bug Report BUG-001) |

### 4.2 Keranjang & Checkout

| ID | Skenario | Jenis | Langkah | Expected Result | Actual Result | Status |
|----|----------|-------|---------|------------------|----------------|--------|
| TC-006 | Tambah 1 produk ke keranjang | Positive | 1. Klik "Add to cart" pada salah satu produk | Badge keranjang menunjukkan angka 1 | Sesuai | Pass |
| TC-007 | Checkout dengan data lengkap | Positive | 1. Isi First Name, Last Name, Zip Code<br>2. Klik Continue<br>3. Klik Finish | Muncul halaman "Thank you for your order" | Sesuai | Pass |
| TC-008 | Checkout dengan field kosong | Negative | 1. Kosongkan First Name<br>2. Klik Continue | Muncul pesan error "First Name is required" | Sesuai | Pass |

*(dan seterusnya sesuai jumlah test case yang kamu buat)*

## 5. Bug Report

| ID | Judul | Severity | Steps to Reproduce | Expected | Actual | Screenshot |
|----|-------|----------|---------------------|----------|--------|------------|
| BUG-001 | Gambar produk tertukar saat login sebagai problem_user | Low (data demo, bukan bug kritikal) | 1. Login dengan `problem_user`<br>2. Lihat halaman Inventory | Setiap produk menampilkan gambar yang sesuai | Semua produk menampilkan gambar yang sama (anjing) | `assets/screenshots/saucedemo/bug-001.png` |

## 6. Summary

- **Total test case**: 8
- **Pass**: 7
- **Fail**: 1
- **Bug ditemukan**: 1 (severity rendah, memang sengaja ada di akun demo `problem_user` untuk simulasi bug)
- **Kesimpulan**: Flow utama (login, keranjang, checkout) pada SauceDemo berjalan sesuai ekspektasi untuk user standar. Satu bug ditemukan pada akun simulasi `problem_user`, sesuai dengan tujuan akun tersebut sebagai demo bug intentional.

---
*Catatan: SauceDemo adalah situs demo publik yang memang disediakan untuk latihan automation & manual testing, sehingga semua temuan di atas aman untuk didokumentasikan secara terbuka.*
