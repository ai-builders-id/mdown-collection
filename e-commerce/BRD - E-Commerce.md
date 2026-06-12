# BRD: E-Commerce — Business Requirements

**Project:** E-Commerce
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Pertumbuhan e-commerce di Indonesia mencapai Rp 530 triliun pada 2025, dengan semakin banyak UMKM dan brand lokal yang berjualan online. Namun banyak toko online masih mengandalkan platform pihak ketiga (Shopee, Tokopedia) yang membatasi branding, data pelanggan, dan margin keuntungan.

E-Commerce Platform ini hadir sebagai solusi toko online mandiri (Direct-to-Consumer / D2C) yang memberikan kontrol penuh kepada pemilik bisnis — dari katalog produk, keranjang, checkout, hingga manajemen pesanan. Dengan UI modern yang dioptimalkan untuk konversi, toko online bisa meningkatkan penjualan langsung tanpa perantara marketplace.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Meningkatkan penjualan online langsung | 30% penjualan berasal dari website sendiri dalam 6 bulan |
| BRD-002 | Meningkatkan conversion rate | Checkout conversion rate > 3% |
| BRD-003 | Mengurangi cart abandonment | Cart abandonment rate < 60% |
| BRD-004 | Memberikan pengalaman belanja yang mulus | Page load < 2 detik, checkout < 5 langkah |

---

## 3. Nilai (Value Proposition)

Apa nilai utama yang diberikan E-Commerce Platform kepada Pemilik Toko dan Pembeli?

- **Brand ownership:** Kontrol penuh atas tampilan dan branding toko online.
- **Zero commission:** Tidak ada potongan biaya per transaksi seperti di marketplace.
- **Checkout mulus:** Multi-step form yang cepat dan intuitif dengan banyak opsi pembayaran.
- **Data pelanggan:** Akses penuh ke data pelanggan untuk marketing dan retensi.
- **Manajemen lengkap:** Katalog produk, stok, pesanan, laporan penjualan dalam satu platform.

---

## 4. Target Pengguna

### Primary Persona: Pemilik Toko Online (Merchant)

- **Demografis:** 25-45 tahun, pemilik UMKM atau brand lokal, melek teknologi, sudah berjualan di marketplace.
- **Kebutuhan utama:** Memiliki toko online sendiri dengan branding yang kuat tanpa biaya langganan mahal.
- **Pain points:** Biaya komisi marketplace tinggi; tidak punya data pelanggan; branding terbatas.

### Secondary Persona: Pembeli Online

- **Demografis:** 18-45 tahun, terbiasa belanja online, memiliki akun e-wallet/mobile banking.
- **Kebutuhan utama:** Pengalaman belanja yang cepat, aman, dan nyaman — dari cari produk sampai bayar.
- **Pain points:** Checkout terlalu panjang; ongkir tidak transparan; stok tidak akurat.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Katalog Produk dengan Grid | P0 | Tampilan produk dalam grid dengan gambar, harga, rating, badge diskon |
| Detail Produk & Varian | P0 | Halaman detail produk dengan galeri gambar, pilihan varian, spesifikasi |
| Filter & Search Produk | P0 | Filter berdasarkan kategori, harga, rating; search dengan autocomplete |
| Keranjang Belanja | P0 | Drawer keranjang dengan quantity control, subtotal, tombol checkout |
| Checkout Multi-Step | P0 | Flow checkout: alamat > pengiriman > pembayaran > konfirmasi |
| Manajemen Pesanan | P1 | Halaman "Pesanan Saya" dengan status tracking (diproses, dikirim, selesai) |
| Alamat Pengiriman | P1 | Simpan dan kelola beberapa alamat pengiriman |
| Wishlist | P2 | Fitur simpan produk favorit |

---

## 6. Model Distribusi

- Static boilerplate frontend — clone dan jalankan dengan Vite
- Mock data dengan katalog produk e-commerce realistis (elektronik, fashion, makanan)
- Keranjang menggunakan React state (useReducer) — tersimpan selama session
- Siap diintegrasikan dengan backend API dan payment gateway (Midtrans, Xendit)

---

## 7. KPI & Metrik Kesuksesan

- **Adoption:** Jumlah produk yang diunggah di katalog (target: 50+ produk)
- **Conversion Rate:** Persentase pengunjung yang checkout (target: > 3%)
- **Cart Abandonment:** Persentase keranjang yang tidak dilanjutkan ke checkout (target: < 60%)
- **AOV (Average Order Value):** Rata-rata nilai pesanan (target: Rp 150.000+)
- **Repeat Purchase:** Persentase pelanggan yang belanja lagi dalam 30 hari (target: 20%)

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | E-Commerce Advantage |
|------------|-----------|------------|-------------------|
| Shopify | Lengkap, mudah setup, banyak plugin | Mahal ($29/bulan + komisi), data di cloud | Gratis, self-hosted, kontrol penuh data |
| WooCommerce | Open source, fleksibel, ekosistem luas | Butuh WordPress, plugin mahal, performa berat | Modern stack (React), ringan, performa tinggi |
| Tokopedia / Shopee | Trafik besar, pembeli sudah ada | Komisi tinggi (5-15%), branding terbatas | Zero komisi, branding penuh, data milik sendiri |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Tidak ada trafik ke toko | Tinggi — penjualan nol | Integrasikan dengan Google Ads, SEO, media sosial |
| Pembeli ragu checkout karena keamanan | Tinggi — cart abandonment tinggi | Tampilkan badge aman, SSL, metode pembayaran terpercaya |
| Stok tidak sinkron antara toko dan gudang | Sedang — pesanan gagal | Notifikasi stok menipis, batasi quantity per checkout |

---

## 10. Rencana Implementasi

1. Fase 1 — Prototipe statis dengan mock data (3 minggu): Katalog, keranjang, checkout flow
2. Fase 2 — Validasi dengan pemilik toko (2 minggu): User testing dengan 5 merchant, refine UX
3. Fase 3 — Integrasi backend / API (6 minggu): Product API, checkout engine, payment gateway, order management
4. Fase 4 — Deployment & scaling (2 minggu): Hosting, onboarding merchant, campaign peluncuran
