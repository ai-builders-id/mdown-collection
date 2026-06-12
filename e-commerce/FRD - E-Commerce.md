# FRD: E-Commerce — Functional Requirements

**Project:** E-Commerce
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk E-Commerce — aplikasi E-Commerce (Produk, Keranjang, Checkout, Pesanan) untuk Pemilik Toko Online.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Katalog & Produk

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Menampilkan halaman utama dengan hero banner dan grid produk unggulan | P0 |
| FRD-002 | Menampilkan grid produk dengan kartu (gambar, nama, harga, rating, badge diskon) | P0 |
| FRD-003 | Menampilkan halaman detail produk (galeri gambar, deskripsi, spesifikasi, varian, review) | P0 |
| FRD-004 | Memilih varian produk (warna, ukuran) dengan harga dan stok yang sesuai | P0 |
| FRD-005 | Menampilkan breadcrumb navigasi (Home > Kategori > Produk) | P1 |
| FRD-006 | Menampilkan produk terkait di halaman detail | P1 |
| FRD-007 | Menampilkan review dan rating produk dari pembeli | P1 |

### 2.2 Modul Filter & Pencarian

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-008 | Filter produk berdasarkan kategori (checkbox) | P0 |
| FRD-009 | Filter produk berdasarkan rentang harga (range slider) | P0 |
| FRD-010 | Filter produk berdasarkan rating minimal | P0 |
| FRD-011 | Pencarian produk dengan input text (debounce 300ms, autocomplete) | P0 |
| FRD-012 | Urutkan produk (Terbaru, Termurah, Termahal, Terlaris, Rating Tertinggi) | P1 |
| FRD-013 | Pagination hasil pencarian/filter | P1 |

### 2.3 Modul Keranjang

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-014 | Menambahkan produk ke keranjang (dengan varian terpilih) | P0 |
| FRD-015 | Menampilkan keranjang sebagai slide-over dari kanan | P0 |
| FRD-016 | Mengubah quantity item di keranjang (+ / -) | P0 |
| FRD-017 | Menghapus item dari keranjang | P0 |
| FRD-018 | Menampilkan subtotal, ongkir, diskon, dan total di keranjang | P0 |
| FRD-019 | Badge jumlah item di ikon keranjang header | P0 |
| FRD-020 | Animasi notifikasi saat produk ditambahkan ke keranjang | P2 |

### 2.4 Modul Checkout

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-021 | Multi-step checkout: Alamat > Pengiriman > Pembayaran > Konfirmasi | P0 |
| FRD-022 | Memilih alamat pengiriman (dari daftar alamat tersimpan) | P0 |
| FRD-023 | Menambahkan alamat pengiriman baru | P0 |
| FRD-024 | Memilih kurir pengiriman (JNE, J&T, SiCepat, Gosend) | P0 |
| FRD-025 | Memilih metode pembayaran (Transfer Bank, E-Wallet, COD) | P0 |
| FRD-026 | Menampilkan ringkasan pesanan sebelum konfirmasi | P0 |
| FRD-027 | Menampilkan halaman sukses pesanan setelah checkout | P0 |

### 2.5 Modul Manajemen Pesanan

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-028 | Menampilkan daftar pesanan user dengan status (diproses, dikirim, selesai) | P1 |
| FRD-029 | Menampilkan detail pesanan (item, alamat, pengiriman, pembayaran, tracking) | P1 |
| FRD-030 | Membatalkan pesanan (jika status masih pending) | P1 |

### 2.6 Modul Wishlist

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-031 | Menambahkan produk ke wishlist (toggle heart icon) | P2 |
| FRD-032 | Menampilkan daftar wishlist | P2 |
| FRD-033 | Memindahkan item dari wishlist ke keranjang | P2 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Belanja dan Checkout

- **Aktor:** Pembeli Online
- **Pre-condition:** Produk tersedia di katalog
- **Post-condition:** Pesanan berhasil dibuat
- **Alur Normal:**
  1. Pembeli membuka halaman utama dan mencari produk melalui search atau kategori
  2. Pembeli mengklik produk — melihat detail, varian, dan review
  3. Pembeli memilih varian (warna/ukuran) dan klik "Tambah ke Keranjang"
  4. Notifikasi muncul — badge keranjang bertambah
  5. Pembeli membuka keranjang (slide dari kanan), review item, klik "Checkout"
  6. Step 1: Pilih alamat pengiriman (atau tambah baru)
  7. Step 2: Pilih kurir
  8. Step 3: Pilih metode pembayaran
  9. Step 4: Review ringkasan pesanan dan klik "Buat Pesanan"
  10. Halaman sukses muncul dengan nomor pesanan
- **Alur Alternatif:**
  - A1: Pembeli menambah produk lain dari keranjang dengan klik "Lanjutkan Belanja"
  - A2: Pembeli mengubah quantity di keranjang sebelum checkout
- **Exception:**
  - E1: Stok habis saat checkout — notifikasi "Stok tidak mencukupi"
  - E2: Varian yang dipilih sudah habis — tombol disabled "Stok Habis"

### UC-02: Melacak Pesanan

- **Aktor:** Pembeli Online
- **Pre-condition:** Pesanan sudah dibuat dan sedang diproses
- **Post-condition:** Pembeli mengetahui status terkini pesanan
- **Alur Normal:**
  1. Pembeli membuka halaman "Pesanan Saya"
  2. Daftar pesanan ditampilkan dengan status masing-masing
  3. Pembeli mengklik salah satu pesanan
  4. Halaman detail pesanan menampilkan: item, alamat, kurir, tracking number, status timeline
- **Alur Alternatif:**
  - A1: Pembeli membatalkan pesanan yang masih "Menunggu Pembayaran"
- **Exception:**
  - E1: Pesanan sudah dikirim — tombol batalkan tidak tersedia

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | Stok validation | Quantity di keranjang tidak boleh melebihi stok tersedia |
| BR-002 | Minimum order | Minimum transaksi Rp 10.000 (belum termasuk ongkir) |
| BR-003 | Pembatalan | Pesanan hanya bisa dibatalkan jika status masih "Menunggu Pembayaran" |
| BR-004 | Ongkir | Ongkir dihitung berdasarkan kota dan kurir (mock: Rp 10.000 - Rp 50.000) |
| BR-005 | Diskon | Kode voucher (mock) memberikan diskon persentase atau nominal tetap |

---

## 5. Integrasi & Data Flow

```text
[Header/Search] --> [Product Grid] --> [Product Detail]
       |                                    |
       v                                    v
[Category/Filter]                     [Cart Drawer]
                                            |
                                            v
                                     [Checkout Steps]
                                            |
                                            v
                                     [Order Success]
                                            |
                                            v
                                     [My Orders Page]
```

---

## 6. Non-Functional Requirements (dari FRD)

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa loading | < 2 detik |
| NFR-002 | Responsive | Desktop + Tablet + Mobile |
| NFR-003 | Aksesibilitas | WCAG 2.1 AA |
| NFR-004 | Bahasa | Bahasa Indonesia (id) |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| Katalog Produk & Grid | High | Medium | P0 |
| Detail Produk & Varian | High | Medium | P0 |
| Keranjang Belanja | High | Medium | P0 |
| Checkout Multi-Step | High | High | P0 |
| Search & Filter Produk | High | Medium | P0 |
| Manajemen Pesanan | High | Medium | P1 |
| Wishlist | Low | Low | P2 |
| Review Produk | Medium | Medium | P1 |
