# PRD: SCM Pro — Supply Chain Management Dashboard
**Project:** SCM Pro
**Aesthetic:** Industrial, data-dense, status-indicator driven, dark-mode ops dashboard

## 1. Core Layout
- Vertical sidebar kiri: navigasi modul (Dashboard, Inventory, Pengiriman, Tracking, Supplier, Laporan)
- Header atas: pencarian global, notifikasi, user avatar
- Main area: kanban-style status board untuk pengiriman, tabel inventory, timeline tracking
- Panel kanan (slide): detail item / shipment

## 2. Data Structure (Mock Only) — TypeScript interfaces

```typescript
interface Product {
  id: string;
  sku: string;
  nama: string;
  kategori: 'bahan-baku' | 'produk-jadi' | 'packaging' | 'consumable';
  unit: 'pcs' | 'kg' | 'liter' | 'box' | 'pallet';
  stockMin: number;
  stockMax: number;
  stockSaatIni: number;
  lokasiGudang: string[];
  supplierUtama: string;
  leadTimeHari: number;
  hargaBeli: number;
}

interface Shipment {
  id: string;
  nomorResi: string;
  purchaseOrder: string;
  supplier: string;
  status: 'dijadwalkan' | 'dalam-perjalanan' | 'diterima-sebagian' | 'selesai' | 'tertunda';
  origin: string;
  destination: string;
  estimasiTiba: string;
  aktualTiba?: string;
  items: ShipmentItem[];
  driver: string;
  kendaraan: string;
}

interface Warehouse {
  id: string;
  kode: string;
  nama: string;
  alamat: string;
  kapasitas: number;
  terpakai: number;
  zona: Zone[];
  pic: string;
  kontak: string;
}

interface Zone {
  id: string;
  kode: string;
  kapasitasRak: number;
  suhu?: number;
  tipe: 'dry' | 'cold' | 'hazardous' | 'bulk';
}
```

## 3. Implementation Details — Vite + React + TypeScript + Tailwind CSS
- Vite + React 18 + TypeScript 5
- Tailwind CSS 3 + daisyUI untuk komponen siap pakai
- Zustand untuk state management inventory + filter
- React Query untuk data fetching + caching
- React Router v6 untuk routing modul
- Recharts untuk grafik supply-demand
- react-beautiful-dnd untuk drag-drop status shipment

## 4. Styling & Theming
- CSS Variables: --scm-primary: #1e3a5f; --scm-warning: #f59e0b; --scm-danger: #dc2626; --scm-success: #16a34a; --scm-surface-card: #f8fafc
- Dark mode toggle via daisyUI
- Status badges warna: Scheduled (blue), In Transit (amber), Partial Received (orange), Complete (green), Delayed (red)
- Layout grid responsif: sidebar collapsible di tablet

## 5. UI Components Specification — ASCII layout

```
+----------------------------------+
| SCM Pro              [🔍][🔔][👤] |
+------+---------------------------+
|      |                           |
| 📦   |  +--------+ +---------+   |
| Inven |  | Stok   | | Nilai   |   |
| 🚚   |  | Kritis | | Total   |   |
| Kirim |  +--------+ +---------+   |
| 📊   |                           |
| Lap   |  === Status Pengiriman === |
| ⚙️   |  [Dijadwalkan][Di Jalan]  |
| Set   |  [Diterima Sebagian]      |
|      |  +--card--+ +--card--+    |
|      |  | PO-001 | | PO-002 |    |
|      |  | Toko A | | Toko B |    |
|      |  +--------+ +--------+    |
+------+---------------------------+
```

- **InventoryTable**: Kolom: SKU, Nama, Kategori, Stock, Min/Max, Status, Aksi. Filter by kategori, search SKU/nama
- **ShipmentKanban**: 4 kolom status, card berisi nomor PO, supplier, estimasi, progress bar
- **WarehouseMap**: Visual grid gudang dengan zona warna, hover detail kapasitas
- **StockAlertBadge**: Badge merah untuk stock di bawah minimum, kuning untuk mendekati minimum
- **SupplierCard**: Nama, kontak, lead time, rating bintang, status kontrak

## 6. Interactions & States — loading, empty, error, edge cases
- **Loading**: Skeleton table 5 baris, pulse animation untuk card
- **Empty**: "Belum ada data inventory. Tambahkan produk pertama Anda." disertai ilustrasi kotak kosong
- **Error**: Banner merah "Gagal memuat data. Coba lagi." dengan tombol Retry
- **Stock under minimum**: Alert merah di dashboard + badge merah di sidebar Inventory
- **Shipment delayed**: Card bergeser ke kolom "Tertunda" dengan indikator waktu keterlambatan
- **Partial receipt**: Form edit untuk menandai jumlah diterima, sisa otomatis masuk backorder
- **Search no result**: "Tidak ditemukan {{keyword}}"

## 7. Language & Localization — Bahasa Indonesia
- Seluruh UI menggunakan Bahasa Indonesia baku
- Notasi: "Anda memiliki {{count}} pengiriman tertunda"
- Format tanggal: "Senin, 12 Mei 2025"
- Format mata uang: Rp {{amount}}
- Status: Dijadwalkan, Dalam Perjalanan, Diterima Sebagian, Selesai, Tertunda

## 8. Example Mock Data — data realistis nama Indonesia

```typescript
const mockProducts: Product[] = [
  {
    id: "PRD-001",
    sku: "BB-TEPUNG-001",
    nama: "Tepung Terigu Cakra Kembar 25kg",
    kategori: "bahan-baku",
    unit: "kg",
    stockMin: 500,
    stockMax: 5000,
    stockSaatIni: 230,
    lokasiGudang: ["GDG-JKT-01", "GDG-BDG-01"],
    supplierUtama: "SUP-001",
    leadTimeHari: 3,
    hargaBeli: 12500
  },
  {
    id: "PRD-002",
    sku: "PJ-ROTI-001",
    nama: "Roti Tawar Sari Roti 600g",
    kategori: "produk-jadi",
    unit: "pcs",
    stockMin: 100,
    stockMax: 2000,
    stockSaatIni: 1450,
    lokasiGudang: ["GDG-JKT-01"],
    supplierUtama: "SUP-002",
    leadTimeHari: 1,
    hargaBeli: 28500
  }
];

const mockShipments: Shipment[] = [
  {
    id: "SHP-001",
    nomorResi: "SCM-2025-05/001",
    purchaseOrder: "PO/2025/05/001",
    supplier: "PT Sembada Makmur",
    status: "dalam-perjalanan",
    origin: "Gudang Cakung, Jakarta Timur",
    destination: "Gudang Batuceper, Tangerang",
    estimasiTiba: "2025-05-15T14:00:00",
    items: [{ productId: "PRD-001", qty: 500, unit: "kg" }],
    driver: "Slamet Riyadi",
    kendaraan: "B 9876 FUS"
  },
  {
    id: "SHP-002",
    nomorResi: "SCM-2025-05/002",
    purchaseOrder: "PO/2025/05/002",
    supplier: "CV Berkat Jaya",
    status: "tertunda",
    origin: "Gudang Waru, Sidoarjo",
    destination: "Gudang Margomulyo, Surabaya",
    estimasiTiba: "2025-05-13T09:00:00",
    items: [{ productId: "PRD-002", qty: 300, unit: "pcs" }],
    driver: "Hasan Basri",
    kendaraan: "L 1234 AB"
  }
];
```

## 9. Success Criteria
- [ ] Dashboard menampilkan status real-time inventory dan pengiriman
- [ ] CRUD produk, supplier, dan gudang berfungsi penuh
- [ ] Drag-drop shipment antar status kanban berhasil menyimpan perubahan
- [ ] Alert stok kritis muncul otomatis saat stok < minimum
- [ ] Filter dan pencarian inventory berespon < 200ms
- [ ] Timeline pengiriman menampilkan estimasi vs aktual
- [ ] Semua state loading/empty/error tertangani
