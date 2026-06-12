# PRD: EstateOS — Real Estate Management
**Project:** EstateOS
**Aesthetic:** Property-tech modern, card-centric, photo-rich, map-integrated, financial dashboard

## 1. Core Layout
- Sidebar kiri: navigasi (Dashboard, Properties, Tenants, Leases, Payments, Reports)
- Header: search properti, notifications, user menu
- Main area: property gallery cards, table view, calendar for lease end
- Panel detail (slide): properti lengkap dengan foto, penyewa, kontrak
- Map overlay: lokasi properti di peta

## 2. Data Structure (Mock Only) — TypeScript interfaces

```typescript
interface Property {
  id: string;
  kode: string;
  nama: string;
  tipe: 'apartemen' | 'rumah-tinggal' | 'ruko' | 'kantor' | 'gudang' | 'tanah';
  alamat: string;
  kota: string;
  provinsi: string;
  luasTanah: number;
  luasBangunan: number;
  kamarTidur: number;
  kamarMandi: number;
  fasilitas: string[];
  hargaSewa: number;
  periodeSewa: 'bulanan' | 'tahunan' | 'harian';
  status: 'tersedia' | 'disewa' | 'dalam-perawatan' | 'terjual';
  fotoUrl: string[];
  pemilik: string;
  latitude: number;
  longitude: number;
}

interface Tenant {
  id: string;
  nama: string;
  nik: string;
  telp: string;
  email: string;
  alamatKtp: string;
  pekerjaan: string;
  status: 'aktif' | 'tidak-aktif';
  ratingPembayaran: number; // 1-5
  documents: TenantDocument[];
  createdAt: string;
}

interface Lease {
  id: string;
  propertyId: string;
  tenantId: string;
  nomorKontrak: string;
  tanggalMulai: string;
  tanggalSelesai: string;
  biayaSewa: number;
  biayaService: number;
  deposit: number;
  dendaTelat: number; // per hari
  status: 'aktif' | 'akan-berakhir' | 'berakhir' | 'diperpanjang' | 'dibatalkan';
  pembayaran: PaymentSchedule[];
  catatan: string;
}

interface PaymentSchedule {
  id: string;
  leaseId: string;
  bulan: number;
  tahun: number;
  jumlah: number;
  status: 'lunas' | 'pending' | 'telat' | 'gagal';
  tanggalJatuhTempo: string;
  tanggalBayar?: string;
  metodeBayar: 'transfer' | 'tunai' | 'qris' | 'auto-debet';
  buktiBayar?: string;
}
```

## 3. Implementation Details — Vite + React + TypeScript + Tailwind CSS
- Vite + React 18 + TypeScript 5
- Zustand untuk filter state + selected property
- React Query untuk data properti dan penyewa
- React Router v6 dengan routes per properti
- react-leaflet/Mapbox untuk peta properti
- Recharts untuk grafik pendapatan sewa
- React Calendar untuk jadwal kontrak berakhir

## 4. Styling & Theming
- CSS Variables: --est-primary: #0891b2; --est-secondary: #06b6d4; --est-accent: #f59e0b
- Status badges: Tersedia (hijau), Disewa (biru), Dalam Perawatan (kuning), Terjual (abu)
- Property card: foto hero, overlay harga, badge status, icon fasilitas
- Payment status: Lunas (hijau), Pending (kuning), Telat (merah), Gagal (abu)
- Map pins: warna sesuai status properti
- Dark mode default untuk dashboard malam

## 5. UI Components Specification — ASCII layout

```
+-----------------------------------------------------+
| EstateOS        [Search properti...]  [🔔][👤]      |
+------+----------------------------------------------+
|      |  [Gallery View] [List View] [Map View]         |
| Prop |  +--------+ +--------+ +--------+             |
| Tenn |  | 📸     | | 📸     | | 📸     |             |
| Leas |  | Apt Cem| | Ruko   | | Rumah  |             |
| Pay  |  | paka   | | Meruya | | Bogor  |             |
| Rep  |  | Rp 5jt | | Rp 12jt| | Rp 3jt |             |
|      |  +--------+ +--------+ +--------+             |
|      |                                                |
|      |  === Quick Stats ===                          |
|      |  [Total Unit] [Terisi] [Pendapatan] [Piutang] |
|      |     25          20      Rp 87jt      Rp 4jt   |
|      |                                                |
|      |  === Kontrak Akan Berakhir ===                |
|      |  Penyewa     | Unit       | Selesai | Status  |
|      |  Budi Santoso| Apt 12     | 30 Jun  | 🔴      |
|      |  Siti Rahma  | Ruko 5     | 15 Jul  | 🟡      |
+------+----------------------------------------------+
```

- **PropertyCard**: Card dengan foto, harga, alamat singkat, status badge, jumlah kamar, icon fasilitas
- **PropertyGallery**: Grid masonry untuk foto properti, bisa fullscreen
- **PropertyMap**: Map dengan marker properti, cluster, popup info
- **TenantCard**: Foto profil, nama, rating, kontak, status
- **LeaseTimeline**: Horizontal timeline untuk durasi kontrak, milestone pembayaran
- **PaymentTable**: Table tagihan, status, jatuh tempo, aksi bayar
- **IncomeChart**: Bar chart pendapatan per bulan, per properti
- **TenantDetailPanel**: Slide panel penyewa: data pribadi, dokumen, riwayat sewa, rating

## 6. Interactions & States — loading, empty, error, edge cases
- **Loading**: Skeleton card property (image placeholder + text shimmer)
- **Empty**: "Belum ada properti terdaftar. Tambahkan properti pertama."
- **Empty tenants**: "Belum ada penyewa untuk properti ini"
- **Error**: "Gagal memuat data properti" + retry button
- **Lease expiring < 30 days**: Card properti border orange, notifikasi dashboard
- **Payment overdue**: Alert merah "Pembayaran {{tenant}} terlambat {{hari}} hari"
- **Property under maintenance**: Disable dari daftar available, label "Dalam Perawatan"
- **Search no result**: "Tidak ditemukan properti dengan kata kunci {{query}}"
- **Map load error**: Fallback ke list view, toast "Peta tidak dapat dimuat"
- **Duplicate NIK**: Validasi saat input tenant

## 7. Language & Localization — Bahasa Indonesia
- Full Bahasa Indonesia
- Status: Tersedia, Disewa, Dalam Perawatan, Terjual
- "Pendapatan bulan ini: Rp {{amount}} dari {{count}} unit"
- "{{count}} kontrak akan berakhir bulan depan"
- Format: "Rp 5.000.000/bulan", "12 Juni 2025"

## 8. Example Mock Data — data realistis nama Indonesia

```typescript
const mockProperties: Property[] = [
  {
    id: "PRP-001",
    kode: "APT/CMK/001",
    nama: "Apartemen Cempaka Putih Tower B-1203",
    tipe: "apartemen",
    alamat: "Jl. Cempaka Putih Raya No. 15",
    kota: "Jakarta Pusat",
    provinsi: "DKI Jakarta",
    luasTanah: 0,
    luasBangunan: 45,
    kamarTidur: 2,
    kamarMandi: 1,
    fasilitas: ["AC", "Furnished", "CCTV", "Kolam Renang", "Gym"],
    hargaSewa: 5000000,
    periodeSewa: "bulanan",
    status: "disewa",
    fotoUrl: ["/mock/photos/apt-cempaka-1.jpg"],
    pemilik: "PT Properti Nusantara",
    latitude: -6.1856,
    longitude: 106.8711
  },
  {
    id: "PRP-002",
    kode: "RKO/MRY/001",
    nama: "Ruko Meruya Boulevard Blok A5",
    tipe: "ruko",
    alamat: "Jl. Meruya Utara, Blok A No. 5",
    kota: "Jakarta Barat",
    provinsi: "DKI Jakarta",
    luasTanah: 120,
    luasBangunan: 250,
    kamarTidur: 0,
    kamarMandi: 2,
    fasilitas: ["Listrik 5500W", "AC Central", "Parkir 2 Mobil", "Shophouse"],
    hargaSewa: 12000000,
    periodeSewa: "tahunan",
    status: "tersedia",
    fotoUrl: ["/mock/photos/ruko-meruya-1.jpg"],
    pemilik: "Hendra Gunawan",
    latitude: -6.1956,
    longitude: 106.7589
  },
  {
    id: "PRP-003",
    kode: "RMH/BGR/001",
    nama: "Rumah Tinggal Bogor Nirwana Residence",
    tipe: "rumah-tinggal",
    alamat: "Perumahan BNR Blok C-12, Cimanggu",
    kota: "Bogor",
    provinsi: "Jawa Barat",
    luasTanah: 150,
    luasBangunan: 100,
    kamarTidur: 3,
    kamarMandi: 2,
    fasilitas: ["AC 2 Unit", "Garasi 1 Mobil", "Taman", "Air Sumur", "Listrik 2200W"],
    hargaSewa: 3500000,
    periodeSewa: "bulanan",
    status: "disewa",
    fotoUrl: ["/mock/photos/rumah-bogor-1.jpg"],
    pemilik: "Siti Rahmawati",
    latitude: -6.5750,
    longitude: 106.7711
  }
];

const mockTenants: Tenant[] = [
  {
    id: "TNT-001",
    nama: "Budi Santoso",
    nik: "3174012506900001",
    telp: "0812-3456-7890",
    email: "budi.santoso@email.com",
    alamatKtp: "Jl. Melati No. 8, Jakarta Selatan",
    pekerjaan: "Karyawan Swasta",
    status: "aktif",
    ratingPembayaran: 5,
    documents: [
      { tipe: "KTP", url: "/mock/docs/ktp_budi.pdf" },
      { tipe: "Kontrak Kerja", url: "/mock/docs/kontrak_budi.pdf" }
    ],
    createdAt: "2024-06-01"
  },
  {
    id: "TNT-002",
    nama: "Rina Marlina",
    nik: "3274015508900002",
    telp: "0815-6789-0123",
    email: "rina.marlina@email.com",
    alamatKtp: "Jl. Anggrek No. 3, Bandung",
    pekerjaan: "Owner Bisnis Online",
    status: "aktif",
    ratingPembayaran: 4,
    documents: [
      { tipe: "KTP", url: "/mock/docs/ktp_rina.pdf" }
    ],
    createdAt: "2024-08-15"
  }
];

const mockLeases: Lease[] = [
  {
    id: "LSE-001",
    propertyId: "PRP-001",
    tenantId: "TNT-001",
    nomorKontrak: "KTR/2024/06/APT/001",
    tanggalMulai: "2024-06-01",
    tanggalSelesai: "2025-06-30",
    biayaSewa: 5000000,
    biayaService: 500000,
    deposit: 10000000,
    dendaTelat: 50000,
    status: "aktif",
    pembayaran: [
      { id: "PAY-001", leaseId: "LSE-001", bulan: 6, tahun: 2024, jumlah: 5500000, status: "lunas", tanggalJatuhTempo: "2024-06-05", tanggalBayar: "2024-06-03", metodeBayar: "transfer" },
      { id: "PAY-012", leaseId: "LSE-001", bulan: 6, tahun: 2025, jumlah: 5500000, status: "pending", tanggalJatuhTempo: "2025-06-05", metodeBayar: "auto-debet" }
    ],
    catatan: "Perpanjangan kontrak via notaris"
  }
];
```

## 9. Success Criteria
- [ ] Dashboard menampilkan total unit, okupansi, pendapatan, piutang
- [ ] CRUD properti dengan upload foto
- [ ] CRUD penyewa dengan validasi NIK unik
- [ ] Manajemen kontrak: buat, perpanjang, akhiri
- [ ] Tracking pembayaran sewa + status jatuh tempo
- [ ] Map view menampilkan lokasi properti
- [ ] Filter & pencarian properti berdasarkan tipe, kota, harga, status
- [ ] Galeri foto properti dengan lightbox
- [ ] Laporan pendapatan per periode
