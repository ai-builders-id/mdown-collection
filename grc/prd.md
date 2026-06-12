# PRD: GRC — Aplikasi Governance Risk Compliance untuk Risk & Compliance Officer

**Project:** GRC (Governance Risk Compliance)
**Aesthetic:** Professional, serious, security-grade

---

## 1. Core Layout

Aplikasi GRC menggunakan layout dua panel dengan dashboard sentral:

- **Sidebar Kiri (260px):** Navigasi — Dashboard, Risk Register, Compliance Tracker, Audit, Kebijakan, Laporan.
- **Panel Utama (flex):** Konten dinamis — tergantung modul aktif. Kanban board untuk risk register, tabel compliance, form audit.
- **Top Bar (56px):** Breadcrumb, search global, notifikasi, user avatar.

Tidak ada panel kanan tetap — konten detail dibuka sebagai modal/overlay.

---

## 2. Data Structure (Mock Only)

```typescript
export interface RiskItem {
  id: string;
  kode: string;
  nama: string;
  deskripsi: string;
  kategori: 'strategis' | 'operasional' | 'keuangan' | 'kepatuhan' | 'reputasi';
  dampak: 'sangat_rendah' | 'rendah' | 'sedang' | 'tinggi' | 'sangat_tinggi';
  probabilitas: 'sangat_rendah' | 'rendah' | 'sedang' | 'tinggi' | 'sangat_tinggi';
  skor: number; // dampak x probabilitas
  status: 'teridentifikasi' | 'dinilai' | 'dimitigasi' | 'dimonitor' | 'ditutup';
  pemilik: string;
  departemen: string;
  mitigasi: string;
  tenggat: string;
  createdAt: string;
  updatedAt: string;
}

export interface ComplianceItem {
  id: string;
  kode: string;
  nama: string;
  regulasi: string;
  kategori: 'internal' | 'eksternal';
  tingkatKepatuhan: 'patuh' | 'sebagian' | 'tidak_patuh' | 'tidak_terkait';
  frekuensi: 'bulanan' | 'kuartalan' | 'tahunan' | 'satu_kali';
  tenggat: string;
  pemilik: string;
  departemen: string;
  catatan: string;
  status: 'terjadwal' | 'terlaksana' | 'terlewat' | 'ditunda';
  createdAt: string;
  updatedAt: string;
}

export interface AuditTemuan {
  id: string;
  kode: string;
  judul: string;
  deskripsi: string;
  severity: 'rendah' | 'sedang' | 'tinggi' | 'kritis';
  area: string;
  auditor: string;
  auditee: string;
  tanggalDitemukan: string;
  status: 'terbuka' | 'ditindaklanjuti' | 'selesai' | 'ditutup';
  tindakanPerbaikan: string;
  tenggatPerbaikan: string;
  createdAt: string;
  updatedAt: string;
}

export interface Kebijakan {
  id: string;
  judul: string;
  nomorDokumen: string;
  versi: number;
  kategori: string;
  departemenPemilik: string;
  status: 'draft' | 'aktif' | 'revisi' | 'kadaluarsa';
  tanggalTerbit: string;
  tanggalRevisi: string;
  tanggalKadaluarsa: string;
  createdAt: string;
  updatedAt: string;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS / CSS Modules
- **Icons:** Lucide React
- **Animation:** Framer Motion
- **Chart:** Recharts (untuk heatmap risk, grafik kepatuhan)
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary #78716C (Stone/Neutral), dengan aksen abu-abu alami dan merah untuk alert/risk tinggi. Nuansa profesional, serius.
- **Typography:** Font system-native dengan bobot regular dan medium, hindari bold berlebihan.
- **Spacing:** Konsisten, rapat, gunakan utility classes Tailwind.

---

## 5. UI Components Specification

```text
+------------------+----------------------------------------------------+
|                   |                                                    |
|    SIDEBAR        |     RISK REGISTER (KANBAN VIEW)                    |
|    Dashboard      |                                                    |
|    Risk Register  | +-------------+ +-----------+ +------------+       |
|    Compliance     | | Identified  | | Assessed  | | Mitigated  |       |
|    Audit          | | Risk A     | | Risk B    | | Risk C     |       |
|    Kebijakan      | | Risk D     | | Risk E    | | Risk F     |       |
|    Laporan        | +-------------+ +-----------+ +------------+       |
|                   |                                                    |
|    Filter Risiko: | [Departemen v] [Kategori v] [Skor >= 10]          |
|                   |                                                    |
|    Ringkasan:     | Total: 25 | Kritis: 3 | Tinggi: 7 | Sedang: 15   |
+------------------+----------------------------------------------------+
```

Komponen utama:
- **RiskKanbanBoard:** Tampilan kanban 5 kolom untuk pipeline status risiko.
- **RiskCard:** Kartu risiko dalam kanban — kode, nama, skor, status, severity indicator warna.
- **RiskHeatmap:** Matriks 5x5 dampak vs probabilitas, warna dari hijau (rendah) ke merah (kritis).
- **ComplianceTable:** Tabel kepatuhan dengan status badge (hijau=patuh, kuning=sebagian, merah=tidak patuh).
- **AuditTemuanCard:** Kartu temuan audit dengan severity badge.
- **KebijakanList:** Daftar kebijakan dengan status dan versi.
- **StatCard:** Kartu statistik untuk dashboard.
- **SeverityBadge:** Badge level severity (rendah=hijau, sedang=kuning, tinggi=oranye, kritis=merah).

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton untuk kanban board, shimmer untuk tabel compliance.
- **Empty (risk register kosong):** "Belum ada risiko yang teridentifikasi. Mulai dengan menambahkan risiko baru." + tombol "Tambah Risiko".
- **Error:** Alert "Gagal memuat data. Silakan refresh halaman."
- **Edge cases:** Skor risiko 0 — tidak ditampilkan di heatmap. Compliance tanpa data — "Belum ada assessment compliance untuk periode ini."

**Flow Interaksi utama:**
1. User buka Risk Register → kanban board dengan 5 kolom status.
2. User klik "Tambah Risiko" → modal form risk assessment (nama, deskripsi, kategori, dampak, probabilitas, mitigasi).
3. User isi form → skor otomatis terhitung (dampak x probabilitas), tampil di heatmap.
4. User drag risk card antar kolom → status berubah, history tercatat.
5. User buka Compliance Tracker → tabel dengan filter per departemen, frekuensi, status.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Semua UI labels, tombol, notifikasi, placeholder dalam Bahasa Indonesia.
- Istilah teknis (risk register, compliance, audit) tetap dalam bahasa Inggris baku.

---

## 8. Example Mock Data

```typescript
export const mockRisks: RiskItem[] = [
  {
    id: 'RSK-001',
    kode: 'RISK-IT-001',
    nama: 'Kebocoran Data Pelanggan',
    deskripsi: 'Risiko kebocoran data pelanggan akibat kerentanan sistem keamanan database',
    kategori: 'operasional',
    dampak: 'sangat_tinggi',
    probabilitas: 'sedang',
    skor: 20,
    status: 'diminitigasi',
    pemilik: 'Budi Santoso',
    departemen: 'Teknologi Informasi',
    mitigasi: 'Implementasi enkripsi end-to-end, audit keamanan berkala, patch management',
    tenggat: '2024-09-30',
    createdAt: '2024-01-15T08:00:00Z',
    updatedAt: '2024-06-01T10:30:00Z',
  },
  {
    id: 'RSK-002',
    kode: 'RISK-FIN-001',
    nama: 'Keterlambatan Pelaporan Pajak',
    deskripsi: 'Risiko denda akibat keterlambatan pelaporan dan pembayaran pajak bulanan',
    kategori: 'kepatuhan',
    dampak: 'sedang',
    probabilitas: 'rendah',
    skor: 6,
    status: 'dimonitor',
    pemilik: 'Siti Rahmawati',
    departemen: 'Keuangan',
    mitigasi: 'Sistem reminder otomatis H-7, kalender pajak, review bulanan',
    tenggat: '2024-07-15',
    createdAt: '2024-02-01T09:00:00Z',
    updatedAt: '2024-06-10T14:00:00Z',
  },
  {
    id: 'RSK-003',
    kode: 'RISK-HR-001',
    nama: 'Kehilangan Karyawan Kunci',
    deskripsi: 'Risiko kehilangan talent kunci di divisi engineering tanpa successor plan',
    kategori: 'strategis',
    dampak: 'tinggi',
    probabilitas: 'tinggi',
    skor: 16,
    status: 'dinilai',
    pemilik: 'Agus Wijaya',
    departemen: 'Sumber Daya Manusia',
    mitigasi: 'Succession planning, retention program, knowledge management system',
    tenggat: '2024-08-31',
    createdAt: '2024-03-10T10:00:00Z',
    updatedAt: '2024-06-12T11:00:00Z',
  },
];

export const mockCompliance: ComplianceItem[] = [
  {
    id: 'COM-001',
    kode: 'COMP-UU-IT-001',
    nama: 'UU ITE Pasal 27-29',
    regulasi: 'Undang-Undang Informasi dan Transaksi Elektronik',
    kategori: 'eksternal',
    tingkatKepatuhan: 'sebagian',
    frekuensi: 'tahunan',
    tenggat: '2024-12-31',
    pemilik: 'Budi Santoso',
    departemen: 'Teknologi Informasi',
    catatan: 'Perlu update kebijakan privasi data sesuai amanat UU',
    status: 'terjadwal',
    createdAt: '2024-01-01T08:00:00Z',
    updatedAt: '2024-06-01T09:00:00Z',
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] Risk Kanban board dengan drag & drop antar kolom berfungsi
- [ ] Risk Heatmap menampilkan matriks dampak x probabilitas
- [ ] Compliance table dengan filter dan status badge berfungsi
- [ ] CRUD operations untuk risk, compliance, audit temuan
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Responsive design (desktop & mobile)
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
