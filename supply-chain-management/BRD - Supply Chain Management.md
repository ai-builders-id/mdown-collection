# BRD: SCM Pro — Business Requirements

## 1. Latar Belakang & Konteks Bisnis
Supply chain management di Indonesia memiliki tantangan fragmentasi data antara gudang, supplier, dan tim logistik. Banyak UMKM dan perusahaan menengah masih menggunakan spreadsheet terpisah untuk inventory, pengiriman, dan pelacakan. SCM Pro hadir sebagai platform terpadu untuk mengelola rantai pasok dari hulu ke hilir, dengan fokus pada visibilitas stok real-time dan otomatisasi alert reorder.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target |
|--------|--------|--------|
| Mengurangi stockout | % produk dengan stok < minimum | < 5% |
| Mempercepat waktu pengiriman | Rata-rata lead time pengiriman | Turun 20% |
| Meningkatkan akurasi inventory | Selisih stok fisik vs sistem | < 2% |
| Digitalisasi tracking pengiriman | % pengiriman terlacak real-time | 100% |
| Efisiensi operasional gudang | Waktu siklus penerimaan barang | < 4 jam |

## 3. Value Proposition
- **Visibilitas End-to-End**: Lihat status inventory dan pengiriman dari satu dashboard
- **Otomasi Reorder**: Alert otomatis saat stok mendekati minimum, plus rekomendasi jumlah order
- **Tracking Real-Time**: Lacak posisi pengiriman dengan integrasi GPS/logistik pihak ketiga
- **Laporan Siap Pakai**: 10+ template laporan untuk audit, evaluasi supplier, dan analisis tren

## 4. Target Pengguna

| Persona | Peran | Kebutuhan Utama |
|---------|-------|-----------------|
| Budi Santoso | Manajer Gudang (35 th) | Melihat stok real-time, reorder point, lokasi barang |
| Siti Rahmawati | Supervisor Logistik (29 th) | Tracking pengiriman, jadwal armada, dokumen PO |
| Andi Pratama | Purchasing Manager (42 th) | Evaluasi supplier, negosiasi harga, lead time |
| Ratna Dewi | CEO / Owner (50 th) | Laporan executive, biaya supply chain, profit margin |

## 5. Fitur Prioritas MVP

| Prioritas | Fitur |
|-----------|-------|
| P0 | Dashboard inventory real-time, CRUD produk, alert stok kritis |
| P0 | Tracking pengiriman + status kanban |
| P1 | Manajemen supplier + purchase order |
| P1 | Modul gudang multi-lokasi + zona |
| P1 | Laporan dasar (stok, pengiriman, supplier) |
| P2 | Integrasi dengan ekspedisi (JNE, Sicepat, dll) |
| P2 | Forecast demand berbasis historis |
| P2 | Barcode/QR scanning untuk penerimaan barang |

## 6. Model Distribusi
- **Web App** (primary): Akses via browser desktop/tablet
- **Mobile Responsive**: PWA untuk scanning dan tracking di lapangan
- **Deployment**: On-premise untuk enterprise, cloud untuk UKM (SaaS)
- **Pricing Tier**:
  - Free: 1 gudang, 100 produk, 3 user
  - Pro: Unlimited gudang, 5000 produk, 10 user, laporan
  - Enterprise: Custom, API access, dedicated support

## 7. KPI & Metrik
- Inventory Turnover Ratio: Target > 6x/tahun
- Order Fulfillment Rate: Target > 95%
- Supplier On-Time Delivery Rate: Target > 90%
- Average Search Time: User menemukan produk < 3 detik
- User Adoption: 80% tim operasional aktif dalam 1 bulan

## 8. Analisis Kompetitor
| Kompetitor | Kelebihan | Kekurangan |
|------------|-----------|------------|
| **Jurnal (Inventory)** | Sudah ada ekosistem akuntansi | Inventory terbatas, tidak ada tracking shipment |
| **Waresix** | Fokus logistik gudang | Terlalu besar untuk UKM, harga mahal |
| **Paper.id** | Manajemen invoice + PO | Fitur inventory masih dasar |
| **Spreadsheet** | Gratis, familiar | Rawan error, tidak real-time, tidak scalable |

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Data migrasi dari spreadsheet corrupt | Tinggi | Tool import template + validasi otomatis |
| Resistensi user terhadap sistem baru | Sedang | Pelatihan bertahap, onboarding gamifikasi |
| Integrasi dengan ekspedisi gagal | Sedang | API fallback + input manual |
| Koneksi internet tidak stabil di gudang | Rendah | Mode offline sementara + sync queue |

## 10. Rencana Implementasi
- **Phase 1 (Minggu 1-4)**: Foundation — Auth, CRUD produk dan supplier, dashboard dasar
- **Phase 2 (Minggu 5-8)**: Tracking — Modul pengiriman, kanban status, upload dokumen
- **Phase 3 (Minggu 9-12)**: Gudang — Multi-lokasi, zona, transfer antar gudang
- **Phase 4 (Minggu 13-16)**: Laporan — Template laporan ekspor PDF/Excel, export data
