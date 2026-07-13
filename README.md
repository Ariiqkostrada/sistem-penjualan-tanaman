# CV Purnama Sejahtera — Laravel (Versi 2)

Website toko tanaman yang telah dimigrasikan dan dikembangkan ke Laravel dengan fitur lengkap.

## Fitur Lengkap

### Pelanggan
- Register, Login, Logout
- **Lupa Password** (reset via token)
- Riwayat pesanan + lacak status pesanan
- **Rating & ulasan** produk setelah pesanan selesai
- Keranjang belanja (session-based)
- Beli sekarang (langsung checkout)
- Checkout dengan simpan data ke database

### Halaman Publik
- Hero section + kategori tanaman
- **Bestseller** (diambil dari data produk terlaris di database)
- Produk per kategori (Hias, Obat, Bibit) dengan deskripsi & rating
- Search realtime produk
- Tombol "Beli Sekarang" dan "Tambah ke Keranjang"

### Admin Panel
- Login admin (credentials dari .env)
- Dashboard dengan:
  - 4 stat card (Produk, Stok, Pesanan, Pelanggan)
  - 2 omset card (Total & Bulan Ini)
  - Grafik pendapatan 6 bulan (bar + line)
  - Grafik penjualan per kategori (donut)
  - Grafik qty produk terlaris (horizontal bar)
  - Top 5 produk terlaris
- CRUD Produk (nama, deskripsi, harga, stok, kategori, rating, **upload foto**)
- Kelola Pesanan (update status +  lihat detail)

---

## Instalasi

### 1. Buat project Laravel dan copy file
```bash
composer create-project laravel/laravel kp-purnama-sejahtera
cd kp-purnama-sejahtera
# Ekstrak zip ini lalu copy semua folder ke dalam project Laravel
```

### 2. Setup .env
```env
APP_NAME="CV Purnama Sejahtera"
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=kp_purnama_sejahtera
DB_USERNAME=root
DB_PASSWORD=

ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin123
```

### 3. Generate key & setup database
```bash
php artisan key:generate
mysql -u root -p -e "CREATE DATABASE kp_purnama_sejahtera;"
php artisan migrate
php artisan db:seed
```

### 4. Copy gambar produk
```bash
# Copy folder assets/img dari website lama ke:
# public/assets/img/
```

### 5. Buat folder storage untuk upload gambar
```bash
mkdir -p public/assets/img
```

### 6. Jalankan
```bash
php artisan serve
```
Buka: **http://localhost:8000**

Admin: **http://localhost:8000/admin/login**

---

## Struktur File Penting

```
app/
  Models/
    Produk.php          ← + relasi ratings, method recalculateStar()
    Pesanan.php         ← + relasi ratings, method sudahDirating()
    Pelanggan.php
    PesananItem.php
    Rating.php          ← ✨ BARU: model rating pelanggan
  Http/Controllers/
    PelangganAuthController.php  ← + lupa password & reset password
    AkunController.php           ← + fitur rating produk
    Admin/DashboardController.php ← + grafik kategori & qty

database/migrations/
  ...000003_create_ratings_table.php          ← ✨ BARU
  ...000004_create_password_reset_tokens_table.php ← ✨ BARU

resources/views/
  auth/
    login.blade.php          ← + link "Lupa password"
    register.blade.php
    lupa-password.blade.php  ← ✨ BARU
    reset-password.blade.php ← ✨ BARU
  akun/
    riwayat.blade.php        ← + tombol & modal rating per produk
    lacak.blade.php
  home.blade.php             ← + section bestseller yang diperbarui
  produk/_card.blade.php     ← + deskripsi, rating, tombol Beli Sekarang
  admin/dashboard.blade.php  ← + 3 grafik Chart.js
  admin/produk.blade.php     ← + upload foto, deskripsi di form & table
```
