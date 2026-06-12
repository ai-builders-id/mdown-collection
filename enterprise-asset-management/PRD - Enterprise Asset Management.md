# PRD: EAM Pro — Enterprise Asset Management
**Project:** EAM Pro
**Aesthetic:** Corporate clean, status-driven, asset lifecycle timeline, maintenance ops dashboard

## 1. Core Layout
- Sidebar kiri: navigasi (Dashboard, Assets, Maintenance, Locations, Vendors, Reports)
- Header: breadcrumb, pencarian aset, user menu
- Main area: tabel aset, timeline maintenance, kalender jadwal
- Panel detail (slide): informasi aset + riwayat maintenance

## 2. Data Structure (Mock Only) — TypeScript interfaces

```typescript
interface Asset {
  id: string;
  kodeAset: string;
  nama: string;
  kategori: 'elektronik' | 'mekanik' | 'kendaraan' | 'furniture' | 'tanah-bangunan' | 'it';
  merek: string;
  model: string;
  nomorSeri: string;
  lokasi: string;
  departemen: string;
  pic: string;
  tanggalPerolehan: string;
  nilaiPerolehan: number;
  nilaiSaatIni: number;
  masaManfaatTahun: number;
  status: 'aktif' | 'dipakai' | 'maintenance' | 'rusak' | 'dihapuskan';
  kondisi: 'baik' | 'kurang-baik' | 'rusak-ringan' | 'rusak-berat';
}

interface MaintenanceRecord {
  id: string;
  assetId: string;
  tipe: 'preventif' | 'korektif' | 'prediktif';
  jadwal: string;
  aktual?: string;
  deskripsi: string;
  teknisi: string;
  biaya: number;
  status: 'dijadwalkan' | 'sedang-dikerjakan' | 'selesai' | 'ditunda';
  catatan: string;
}

interface Location {
  id: string;
  kode: string;
  nama: string;
  alamat: string;
  gedung: string;
  lantai: number;
  ruangan: string;
  pic: string;
}

interface DepreciationSchedule {
  id: string;
  assetId: string;
  tahun: number;
  nilaiAwal: number;
  penyusutan: number;
  nilaiAkhir: number;
  metode: 'garis-lurus' | 'saldo-menurun' | 'unit-produksi';
}
```

## 3. Implementation Details — Vite + React + TypeScript + Tailwind CSS
- Vite + React 18 + TypeScript 5
- Zustand untuk state management filtering + selected asset
- React Query untuk data aset + maintenance
- Recharts untuk grafik depresiasi dan biaya maintenance
- React Calendar/Tailwind untuk jadwal maintenance
- react-to-print untuk export PDF label aset

## 4. Styling & Theming
- CSS Variables: --eam-primary: #0f766e; --eam-secondary: #0d9488; --eam-warning: #d97706
- Status badges: Aktif (hijau), Dipakai (biru), Maintenance (kuning), Rusak (merah), Dihapuskan (abu)
- Layout: card-based untuk dashboard, tabel dense untuk list aset
- Timeline: horizontal dengan milestones

## 5. UI Components Specification — ASCII layout

```
+------------------------------------------+
| EAM Pro                      [🔍][🔔][👤] |
+------+-----------------------------------+
|      |  [Overview Cards]                  |
| Aset |  Total Aset | Nilai Total | Rusak  |
| Mtn  |  [         ] [          ] [      ] |
| Loc  |                                    |
| Ven  |  === Daftar Aset ===              |
| Rep  |  [Cari...] [Filter Kategori] [Tambah]|
|      |  +--------------------------------+ |
|      |  | Kode | Nama    | Lokasi |Status | |
|      |  | AST1 | Laptop  | R.201  | Aktif | |
|      |  | AST2 | AC      | R.105  | Maint | |
|      |  +--------------------------------+ |
|      |                                    |
|      |  === Kalender Maintenance ===     |
|      |  [Jun 2025                        |
|      |   Mo Tu We Th Fr Sa Su            |
|      |        1  2  3  4  5  6           |
+------+-----------------------------------+
```

- **AssetTable**: Dense table dengan kolom kode, nama, kategori, lokasi, status, kondisi, aksi
- **AssetDetailPanel**: Slide panel berisi foto, spesifikasi, riwayat, depresiasi, dokumen
- **MaintenanceCalendar**: Kalender dengan dot indikator jadwal per hari, klik lihat detail
- **DepreciationChart**: Line chart nilai aset dari waktu ke waktu, breakdown per tahun
- **QRCodeLabel**: Generate QR/barcode untuk label fisik aset

## 6. Interactions & States — loading, empty, error, edge cases
- **Loading**: Skeleton card untuk overview, skeleton rows untuk tabel
- **Empty**: "Belum ada aset terdaftar. Tambahkan aset pertama." dengan ilustrasi
- **Error**: Alert "Gagal memuat data aset" + tombol reload
- **Asset disposed/retired**: Konfirmasi ganda "Yakin hapus aset {{kode}}?" dengan alasan
- **Duplicate serial number**: Validasi error "Nomor seri sudah terdaftar"
- **Maintenance overdue**: Badge merah di kalender + notifikasi
- **Search no result**: "Tidak ditemukan aset dengan kata kunci {{query}}"

## 7. Language & Localization — Bahasa Indonesia
- Full Bahasa Indonesia
- Status: Aktif, Dipakai, Maintenance, Rusak, Dihapuskan
- Format rupiah: Rp 15.000.000
- Format tanggal: "12 Mei 2025"
- "Terdapat {{count}} aset dengan maintenance terjadwal hari ini"

## 8. Example Mock Data — data realistis nama Indonesia

```typescript
const mockAssets: Asset[] = [
  {
    id: "AST-001",
    kodeAset: "EAM/IT/2025/001",
    nama: "Laptop Dell Latitude 5440",
    kategori: "elektronik",
    merek: "Dell",
    model: "Latitude 5440",
    nomorSeri: "SN-DELL-2025-001",
    lokasi: "LOK-001",
    departemen: "IT",
    pic: "Bambang Supriyadi",
    tanggalPerolehan: "2025-01-15",
    nilaiPerolehan: 18500000,
    nilaiSaatIni: 16800000,
    masaManfaatTahun: 4,
    status: "dipakai",
    kondisi: "baik"
  },
  {
    id: "AST-002",
    kodeAset: "EAM/MEK/2024/012",
    nama: "Mesin Compressor Kaeser SM15",
    kategori: "mekanik",
    merek: "Kaeser",
    model: "SM15",
    nomorSeri: "KS-SM15-2024-012",
    lokasi: "LOK-002",
    departemen: "Produksi",
    pic: "Herman Gunawan",
    tanggalPerolehan: "2024-06-20",
    nilaiPerolehan: 375000000,
    nilaiSaatIni: 295000000,
    masaManfaatTahun: 10,
    status: "maintenance",
    kondisi: "kurang-baik"
  },
  {
    id: "AST-003",
    kodeAset: "EAM/FUR/2023/045",
    nama: "Meja Rapat Oval 8 Kursi",
    kategori: "furniture",
    merek: "Indachi",
    model: "MR-8000",
    nomorSeri: "FUR-IND-2023-045",
    lokasi: "LOK-003",
    departemen: "HRD",
    pic: "Dewi Sartika",
    tanggalPerolehan: "2023-11-01",
    nilaiPerolehan: 12500000,
    nilaiSaatIni: 8500000,
    masaManfaatTahun: 5,
    status: "aktif",
    kondisi: "baik"
  }
];

const mockMaintenance: MaintenanceRecord[] = [
  {
    id: "MT-001",
    assetId: "AST-002",
    tipe: "preventif",
    jadwal: "2025-05-20",
    aktual: "2025-05-20",
    deskripsi: "Ganti oli compressor dan filter udara",
    teknisi: "Rudi Hartono",
    biaya: 2500000,
    status: "selesai",
    catatan: "Kondisi mesin masih baik, filter perlu diganti 3 bulan lagi"
  },
  {
    id: "MT-002",
    assetId: "AST-001",
    tipe: "korektif",
    jadwal: "2025-05-22",
    deskripsi: "Keyboard laptop beberapa tombol macet",
    teknisi: "Service Center Dell",
    biaya: 450000,
    status: "dijadwalkan",
    catatan: ""
  }
];
```

## 9. Success Criteria
- [ ] Dashboard menampilkan ringkasan total aset, nilai, dan kondisi
- [ ] CRUD aset lengkap dengan validasi duplikasi nomor seri
- [ ] Kalender menampilkan jadwal maintenance dengan akurat
- [ ] Timeline depresiasi aset tampil dengan grafik yang jelas
- [ ] Filter & pencarian aset berespon < 200ms
- [ ] Generate QR/barcode untuk label aset
- [ ] Riwayat maintenance tercatat dengan detail biaya dan teknisi
