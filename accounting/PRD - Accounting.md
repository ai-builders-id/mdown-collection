# PRD: Aplikasi Akuntansi Double-Entry — Sistem Pembukuan Digital untuk UKM Indonesia

**Project:** BukuWarung Akuntansi
**Aesthetic:** Modern, professional, clean SaaS dengan nuansa hijau tua (#0D5C3D) sebagai warna utama

## 1. Core Layout

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] BukuWarung Akuntansi                            [Profil] [⚙] │
├────────┬────────────────────────────────────────────────────────────┤
│        │  Dashboard / Jurnal / Buku Besar / Laporan / Pengaturan    │
│ Sidebar│────────────────────────────────────────────────────────────│
│  🏠    │  [Kartu Saldo] [Jurnal Terbaru] [Peringatan]              │
│  📒    │                                                           │
│  📊    │  ┌─────┬──────┬──────┐  ┌──────────────────────────┐    │
│  📄    │  │Aset  │Utang │Modal │  │Grafik Laba Rugi          │    │
│  🗂️    │  ├─────┼──────┼──────┤  │  [Bar Chart]             │    │
│  ⚙️    │  │Rp X │Rp Y  │Rp Z  │  │                          │    │
│        │  └─────┴──────┴──────┘  └──────────────────────────┘    │
│        │                                                           │
│        │  [Tabel Jurnal Umum — 5 entri terbaru]                  │
│        │  ┌──────┬────────┬──────┬────────┬──────┐               │
│        │  │Tgl   │No.Bukti│Akun  │Debit   │Kredit│               │
│        │  ├──────┼────────┼──────┼────────┼──────┤               │
│        │  │...   │...     │...   │...     │...   │               │
│        │  └──────┴────────┴──────┴────────┴──────┘               │
│        │                                                           │
│        │  [Footer: Total Debit | Total Kredit | Selisih]         │
├────────┴────────────────────────────────────────────────────────────┤
│ © 2025 BukuWarung Akuntansi — v1.0.0                               │
└─────────────────────────────────────────────────────────────────────┘
```

- **Sidebar Kiri:** Navigasi utama — Dashboard, Jurnal, Buku Besar, Neraca Lajur, Laporan Laba Rugi, Neraca, Arus Kas, Pengaturan
- **Panel Utama:** Konten dinamis berdasarkan menu aktif
- **Top Bar:** Breadcrumb, pencarian, notifikasi, profil pengguna
- **Bottom Bar:** Status koneksi, periode akuntansi aktif, versi

## 2. Data Structure (Mock Only)

```typescript
// === Chart of Account ===
interface Account {
  id: string; // e.g. "1-1000"
  code: string; // e.g. "1-1000"
  name: string; // e.g. "Kas Besar"
  type: AccountType; // 'asset' | 'liability' | 'equity' | 'revenue' | 'expense'
  category: string; // e.g. "Kas & Bank"
  normalBalance: 'debit' | 'credit';
  isActive: boolean;
  parentId?: string; // untuk sub-akun
  balance: number; // saldo terkini
  createdAt: string; // ISO date
}

// === Journal Entry ===
interface JournalEntry {
  id: string; // e.g. "JNL-2025-03-001"
  transactionNumber: string; // e.g. "BKM-2025-03-0001"
  date: string; // ISO date — tanggal transaksi
  description: string; // e.g. "Penerimaan kas dari penjualan tunai"
  lines: JournalLine[];
  status: 'draft' | 'posted' | 'reversed';
  createdBy: string; // user ID
  createdAt: string;
  approvedBy?: string;
  approvedAt?: string;
  attachment?: string; // URL file bukti transaksi
}

interface JournalLine {
  id: string;
  accountId: string; // refer ke Account.id
  accountName: string; // denormalized untuk performa
  debit: number;
  credit: number;
  description?: string; // deskripsi per baris
}

// === General Ledger ===
interface GeneralLedger {
  accountId: string;
  accountCode: string;
  accountName: string;
  period: string; // e.g. "2025-03"
  openingBalance: number;
  entries: LedgerEntry[];
  closingBalance: number;
}

interface LedgerEntry {
  journalEntryId: string;
  date: string;
  description: string;
  debit: number;
  credit: number;
  balance: number; // saldo berjalan
}

// === Financial Report ===
interface FinancialReport {
  id: string;
  type: 'balance-sheet' | 'income-statement' | 'cash-flow' | 'trial-balance';
  period: { start: string; end: string };
  generatedAt: string;
  currency: string; // default 'IDR'
  sections: ReportSection[];
  totalDebit: number;
  totalCredit: number;
}

interface ReportSection {
  title: string; // e.g. "ASET LANCAR"
  lines: ReportLine[];
  subtotal: number;
}

interface ReportLine {
  accountCode: string;
  accountName: string;
  value: number;
  indentLevel: number; // 0=header, 1=group, 2=detail
  isBold?: boolean;
}

// === Fiscal Period ===
interface FiscalPeriod {
  id: string;
  name: string; // e.g. "Januari 2025"
  month: number;
  year: number;
  startDate: string;
  endDate: string;
  isOpen: boolean;
  isActive: boolean;
  previousPeriodId?: string;
}
```

## 3. Implementation Details

**Stack:** Vite + React 18 + TypeScript + Tailwind CSS 3 + Lucide React + shadcn/ui + Framer Motion + Recharts

- **Routing:** React Router v6 dengan lazy loading per modul
- **State Management:** Zustand untuk global state (periode aktif, user), React Query untuk server state
- **Forms:** React Hook Form + Zod validasi
- **Data Table:** @tanstack/react-table untuk Buku Besar dan Jurnal
- **PDF Export:** jsPDF + jspdf-autotable untuk cetak laporan
- **Date Handling:** date-fns dengan locale id
- **Number Formatting:** Intl.NumberFormat untuk format IDR

## 4. Styling & Theming

**Palette:**
- Primary: `#0D5C3D` (hijau tua) — mewakili pertumbuhan dan keuangan
- Primary Light: `#1A8C5E`
- Primary Dark: `#083A26`
- Accent: `#F59E0B` (kuning emas) — untuk aksen peringatan dan sorotan
- Background: `#F8FAFC` (slate 50)
- Surface: `#FFFFFF`
- Text Primary: `#1E293B` (slate 800)
- Text Secondary: `#64748B` (slate 500)
- Success: `#10B981`
- Error: `#EF4444`
- Warning: `#F59E0B`
- Border: `#E2E8F0`

**Typography:**
- Font: Inter (sans-serif) — weights 400, 500, 600, 700
- Headings: text-2xl (24px) hingga text-4xl (36px)
- Body: text-sm (14px) untuk tabel, text-base (16px) untuk konten umum
- Monospace: JetBrains Mono untuk kode akun dan nominal

**Spacing:**
- Sidebar: 280px (w-72)
- Container max-width: 1440px
- Card padding: 24px (p-6)
- Gap grid: 24px (gap-6)

## 5. UI Components Specification

### Sidebar Navigation
```
┌─────────────────┐
│ [Logo] BukuW    │ 48px — logo + app name
├─────────────────┤
│ 🏠 Dashboard    │
│ 📒 Jurnal       │
│ 📊 Buku Besar   │
│ 📋 Neraca Lajur │
│ 📄 Laba Rugi    │ — active state
│ 📑 Neraca       │
│ 💰 Arus Kas     │
│ 📁 Laporan Lain │
│ ⚙️ Pengaturan   │
├─────────────────┤
│ 📅 Periode: Mar │ — dropdown periode
│ 🏢 PT Maju Jaya │ — entity selector
└─────────────────┘
```

- Item aktif: bg-primary/10 + text-primary + border-left 3px primary
- Hover: bg-slate-100
- Icon 20px, gap-3 dengan label
- Submenu collapse untuk "Laporan Lain"

### Jurnal Entry Form
```
┌──────────────────────────────────────────────────────────────┐
│ ✏️ Entri Jurnal Baru                                  [Simpan]│
├──────────────────────────────────────────────────────────────┤
│ Tanggal     [📅 15 Maret 2025      ]                         │
│ No. Bukti   [BKM-2025-03-0015       ]                         │
│ Deskripsi   [Penerimaan pembayaran dari PT ABC               ]│
│                                                              │
│ ┌────────────┬──────────────┬────────────┬──────────┐       │
│ │ Kode Akun  │ Nama Akun    │ Debit (Rp) │ Kredit   │       │
│ ├────────────┼──────────────┼────────────┼──────────┤       │
│ │ [1-1010  ▾]│ Kas Besar    │ 15.000.000 │ —        │       │
│ │ [4-1010  ▾]│ Pendapatan   │ —          │15.000.000│       │
│ │ [...]      │ [...]        │ [...]      │ [...]    │       │
│ ├────────────┴──────────────┼────────────┼──────────┤       │
│ │ TOTAL                     │15.000.000  │15.000.000│       │
│ └───────────────────────────┴────────────┴──────────┘       │
│ [+ Tambah Baris]          [Batal] [Simpan sebagai Draft]    │
│                                                              │
│ ⚠️ Total Debit dan Kredit harus sama                         │
└──────────────────────────────────────────────────────────────┘
```

### Kartu Saldo (Dashboard)
```
┌──────────────────┬──────────────────┬──────────────────┬──────────────────┐
│ 💰 Total Aset    │ 📋 Total Utang   │ 🏦 Total Modal   │ 📈 Laba Bruto    │
│ Rp 850.000.000   │ Rp 320.000.000   │ Rp 530.000.000   │ Rp 45.000.000    │
│ ▲ 12.5%          │ ▼ 3.2%           │ ▲ 8.1%           │ ▲ 15.3%          │
│ dari bulan lalu  │ dari bulan lalu  │ dari bulan lalu  │ dari bulan lalu  │
└──────────────────┴──────────────────┴──────────────────┴──────────────────┘
```

### Tabel Jurnal Umum
```
┌──────┬───────────┬────────────────────────┬────────────┬────────────┐
│ Tgl  │ No. Bukti │ Keterangan             │ Debit      │ Kredit     │
├──────┼───────────┼────────────────────────┼────────────┼────────────┤
│15/03 │ BKM-0015  │ Kas Besar              │15.000.000  │            │
│      │           │ Pendapatan Jasa        │            │15.000.000  │
│      │           │ ▶ Pembayaran PT ABC    │            │            │
├──────┼───────────┼────────────────────────┼────────────┼────────────┤
│14/03 │ BKK-0008  │ Biaya Sewa             │ 5.000.000  │            │
│      │           │ Kas Besar              │            │ 5.000.000  │
│      │           │ ▶ Pembayaran sewa April│            │            │
└──────┴───────────┴────────────────────────┴────────────┴────────────┘
```

## 6. Interactions & States

### Loading States
- **Skeleton loader:** Card saldo menampilkan pulse animation (3 baris abu-abu)
- **Spinner:** Saat memproses submit jurnal (overlay dengan spinner + "Menyimpan...")
- **Progress bar:** Saat generate laporan tahunan
- **Shimmer:** Tabel loading dengan 5 baris shimmer animation

### Empty States
- **Belum ada jurnal:** Ilustrasi buku kosong + "Belum ada transaksi. Mulai catat jurnal pertama Anda!"
- **Belum ada akun:** "Chart of Account masih kosong. Buat akun pertama Anda."
- **Tidak ada hasil pencarian:** "Tidak ditemukan jurnal dengan kata kunci '{{KEYWORD}}'"
- **Belum ada laporan:** "Pilih periode untuk generate laporan"

### Error States
- **Network Error:** Banner merah "Gagal memuat data. Periksa koneksi internet." + tombol "Muat Ulang"
- **Validation Error:** Field error merah dengan pesan spesifik (e.g. "Total debit dan kredit tidak sama")
- **Server Error:** Toast "Terjadi kesalahan server. Kode: {{ERROR_CODE}}"
- **Session Expired:** Modal "Sesi berakhir. Silakan login kembali."
- **Data Conflict:** "Data sudah diubah oleh pengguna lain. Muat ulang halaman."

### Edge Cases
- **Double-click submit:** Button disabled setelah klik pertama + "Menyimpan..."
- **Periode tertutup:** Warning saat mencoba entri di periode yang sudah closed
- **Saldo tidak balance:** Alert saat total debit ≠ total kredit
- **Akun non-aktif:** Tidak muncul di dropdown pemilihan akun
- **Nominal negatif:** Validasi tidak mengizinkan nilai negatif di field debit/kredit
- **Hari libur:** Warning jika tanggal transaksi jatuh di hari Minggu/libur nasional

## 7. Language & Localization

Seluruh UI dalam Bahasa Indonesia:
- Labels: "Tanggal", "No. Bukti", "Keterangan", "Debit", "Kredit", "Saldo"
- Actions: "Simpan", "Batal", "Edit", "Hapus", "Cetak", "Export PDF"
- Messages: "Data berhasil disimpan", "Yakin ingin menghapus jurnal ini?"
- Periods: "Januari", "Februari", "Maret",...
- Account Types: "Aset", "Utang", "Modal", "Pendapatan", "Beban"
- Report Titles: "Neraca", "Laporan Laba Rugi", "Laporan Arus Kas", "Neraca Lajur"
- Currency: "Rp" prefix dengan format IDR (1.000.000,00)
- Date: "dd MMMM yyyy" (15 Maret 2025)

## 8. Example Mock Data

```typescript
// Chart of Accounts
const mockAccounts: Account[] = [
  {
    id: "1-1000", code: "1-1000", name: "Kas Besar",
    type: "asset", category: "Kas & Bank", normalBalance: "debit",
    isActive: true, balance: 125_000_000, createdAt: "2024-01-01T00:00:00Z"
  },
  {
    id: "1-1100", code: "1-1100", name: "Bank BCA 123456",
    type: "asset", category: "Kas & Bank", normalBalance: "debit",
    isActive: true, balance: 450_000_000, createdAt: "2024-01-01T00:00:00Z"
  },
  {
    id: "1-2000", code: "1-2000", name: "Piutang Usaha",
    type: "asset", category: "Piutang", normalBalance: "debit",
    isActive: true, balance: 85_000_000, createdAt: "2024-01-01T00:00:00Z"
  },
  {
    id: "2-1000", code: "2-1000", name: "Utang Usaha",
    type: "liability", category: "Utang Lancar", normalBalance: "credit",
    isActive: true, balance: 120_000_000, createdAt: "2024-01-01T00:00:00Z"
  },
  {
    id: "3-1000", code: "3-1000", name: "Modal Pemilik",
    type: "equity", category: "Modal", normalBalance: "credit",
    isActive: true, balance: 500_000_000, createdAt: "2024-01-01T00:00:00Z"
  },
  {
    id: "4-1010", code: "4-1010", name: "Pendapatan Jasa",
    type: "revenue", category: "Pendapatan", normalBalance: "credit",
    isActive: true, balance: 0, createdAt: "2024-01-01T00:00:00Z"
  },
  {
    id: "5-1010", code: "5-1010", name: "Beban Gaji",
    type: "expense", category: "Beban Operasional", normalBalance: "debit",
    isActive: true, balance: 0, createdAt: "2024-01-01T00:00:00Z"
  },
  {
    id: "5-1020", code: "5-1020", name: "Beban Sewa",
    type: "expense", category: "Beban Operasional", normalBalance: "debit",
    isActive: true, balance: 0, createdAt: "2024-01-01T00:00:00Z"
  },
  {
    id: "5-1030", code: "5-1030", name: "Beban Listrik & Air",
    type: "expense", category: "Beban Operasional", normalBalance: "debit",
    isActive: true, balance: 0, createdAt: "2024-01-01T00:00:00Z"
  }
];

// Journal Entry
const mockJournalEntry: JournalEntry = {
  id: "JNL-2025-03-001",
  transactionNumber: "BKM-2025-03-0001",
  date: "2025-03-15T10:00:00Z",
  description: "Penerimaan pembayaran jasa konsultasi dari PT Maju Sejahtera",
  lines: [
    {
      id: "line-1",
      accountId: "1-1000",
      accountName: "Kas Besar",
      debit: 25_000_000,
      credit: 0,
      description: "Penerimaan tunai"
    },
    {
      id: "line-2",
      accountId: "4-1010",
      accountName: "Pendapatan Jasa",
      debit: 0,
      credit: 25_000_000,
      description: "Pendapatan jasa konsultasi"
    }
  ],
  status: "posted",
  createdBy: "user-001",
  createdAt: "2025-03-15T10:05:00Z",
  approvedBy: "user-002",
  approvedAt: "2025-03-15T11:00:00Z"
};

// Laporan Laba Rugi
const mockIncomeStatement: FinancialReport = {
  id: "RPT-2025-03-001",
  type: "income-statement",
  period: { start: "2025-03-01", end: "2025-03-31" },
  generatedAt: "2025-03-31T23:59:00Z",
  currency: "IDR",
  sections: [
    {
      title: "PENDAPATAN",
      lines: [
        { accountCode: "4-1010", accountName: "Pendapatan Jasa", value: 150_000_000, indentLevel: 0 },
        { accountCode: "4-1020", accountName: "Pendapatan Bunga", value: 2_500_000, indentLevel: 0 },
      ],
      subtotal: 152_500_000
    },
    {
      title: "BEBAN OPERASIONAL",
      lines: [
        { accountCode: "5-1010", accountName: "Beban Gaji", value: 65_000_000, indentLevel: 0 },
        { accountCode: "5-1020", accountName: "Beban Sewa", value: 15_000_000, indentLevel: 0 },
        { accountCode: "5-1030", accountName: "Beban Listrik & Air", value: 3_200_000, indentLevel: 0 },
        { accountCode: "5-1040", accountName: "Beban ATK", value: 850_000, indentLevel: 0 },
      ],
      subtotal: 84_050_000
    }
  ],
  totalDebit: 84_050_000,
  totalCredit: 152_500_000
};
```

## 9. Success Criteria

- [ ] CRUD penuh untuk Chart of Account, Jurnal, dan Buku Besar
- [ ] Validasi debit = kredit di setiap entri jurnal
- [ ] Generate otomatis Buku Besar dari jurnal yang sudah diposting
- [ ] Laporan Laba Rugi dan Neraca bisa digenerate per periode
- [ ] Perubahan saldo real-time saat entri jurnal diposting
- [ ] Search dan filter jurnal berdasarkan tanggal, akun, keyword
- [ ] Export laporan ke PDF dengan format profesional
- [ ] Multi-periode: bisa buka/tutup periode fiskal
- [ ] Role-based access: admin, akuntan, viewer
- [ ] Performance: render tabel 10.000 baris jurnal dalam < 2 detik
