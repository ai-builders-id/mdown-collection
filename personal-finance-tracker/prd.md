# PRD: CatatanDuit — Aplikasi Pencatatan Keuangan Pribadi

**Project:** CatatanDuit
**Aesthetic:** Modern, colorful, playful, friendly — hijau toska (#14B8A6) sebagai warna utama

## 1. Core Layout

```
┌───────────────────────────────────────────────────────────────────────┐
│ ⬅️ CatatanDuit                           🔍 Rp 12.500.000 [Profil] ▾│
├───────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  💰 Saldo Total: Rp 12.500.000                                       │
│     ┌────────┐ ┌────────┐ ┌────────┐                                │
│     │ 💳     │ │ 🏦     │ │ 💵     │                                │
│     │ BCA    │ │ Mandiri│ │ Tunai  │                                │
│     │ 5.2jt  │ │ 4.3jt  │ │ 3.0jt  │                                │
│     └────────┘ └────────┘ └────────┘ [+ Akun]                       │
│                                                                       │
│  📊 Pengeluaran Bulan Ini: Rp 3.200.000                             │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ [Bar Chart — per kategori] Makanan | Transport | Belanja ... │   │
│  │ ████████████ 1.2jt  ████████ 800rb  ██████ 600rb ...         │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                       │
│  📝 Transaksi Terbaru (Maret 2025)                                   │
│  ┌─────┬──────────────────────┬──────────┬────────┐                 │
│  │ Tgl │ Deskripsi            │ Kategori │ Nominal│                 │
│  ├─────┼──────────────────────┼──────────┼────────┤                 │
│  │ 15  │ 🍜 Makan siang       │ Makanan  │ -45.000│                 │
│  │ 15  │ 🚕 Gojek ke kantor   │ Transport│ -25.000│                 │
│  │ 14  │ 💰 Gaji Maret        │ Gaji     │+8.500.000│               │
│  │ 14  │ 📱 Tagihan Telkom    │ Utilitas │ -350.000│                 │
│  └─────┴──────────────────────┴──────────┴────────┘                 │
│                                                                       │
│  [+ Tambah Transaksi]        [📊 Laporan] [🎯 Anggaran]              │
│                                                                       │
├───────────────────────────────────────────────────────────────────────┤
│ 🏠 ████ 📊 ██ 📋 ██ ⚙️  — Bottom Navigation (Mobile)                │
│    Beranda  Laporan  Anggaran  Pengaturan                            │
└───────────────────────────────────────────────────────────────────────┘
```

- **Mobile-First:** Bottom navigation untuk mobile, sidebar untuk tablet/desktop
- **Dashboard:** Ringkasan saldo, grafik pengeluaran, daftar transaksi terbaru
- **FAB (Floating Action Button):** "+" untuk tambah transaksi cepat
- **Top Bar:** Saldo total, pencarian, profil

## 2. Data Structure (Mock Only)

```typescript
// === Transaction ===
interface Transaction {
  id: string; // e.g. "TRX-2025-03-001"
  type: 'income' | 'expense' | 'transfer';
  amount: number; // always positive
  description: string; // e.g. "Makan siang di Warteg"
  categoryId: string;
  categoryName: string; // denormalized
  accountId: string;
  accountName: string;
  date: string; // ISO date
  time: string; // "HH:mm"
  isRecurring: boolean;
  recurringRule?: RecurringRule;
  tags: string[];
  notes?: string;
  attachment?: string; // URL foto bukti
  location?: { lat: number; lng: number; name: string };
  createdAt: string;
}

interface RecurringRule {
  frequency: 'daily' | 'weekly' | 'monthly' | 'yearly';
  interval: number; // every N days/weeks/months
  endDate?: string;
  count?: number;
}

// === Account / Wallet ===
interface Account {
  id: string; // e.g. "ACC-001"
  name: string; // e.g. "BCA Syariah"
  type: 'cash' | 'bank' | 'ewallet' | 'savings' | 'investment';
  balance: number;
  currency: string; // default "IDR"
  color: string; // hex color for UI
  icon: string; // icon name
  isActive: boolean;
  includeInTotal: boolean;
  order: number; // display order
  createdAt: string;
}

// === Category ===
interface Category {
  id: string; // e.g. "CAT-001"
  name: string; // e.g. "Makanan & Minuman"
  type: 'income' | 'expense';
  icon: string; // emoji or lucide icon name
  color: string;
  parentId?: string; // for subcategories
  budget?: Budget;
  isActive: boolean;
}

// === Budget ===
interface Budget {
  id: string;
  categoryId: string;
  categoryName: string;
  amount: number; // monthly budget limit
  spent: number; // spent so far this month
  period: { month: number; year: number };
  alertAt: number; // percentage to trigger alert (default 80)
  rollover: boolean; // carry over unused to next month
}

// === Financial Goal ===
interface FinancialGoal {
  id: string;
  name: string; // e.g. "Liburan ke Bali"
  targetAmount: number;
  currentAmount: number;
  deadline: string; // ISO date
  icon: string;
  color: string;
  status: 'active' | 'completed' | 'cancelled';
  autoSaveRule?: {
    accountId: string;
    amount: number;
    frequency: 'daily' | 'weekly' | 'monthly';
  };
}

// === Report Summary ===
interface MonthlyReport {
  month: number;
  year: number;
  totalIncome: number;
  totalExpense: number;
  netSavings: number;
  topCategories: { category: string; amount: number; percentage: number }[];
  dailySpending: { date: string; amount: number }[];
  comparisonToLastMonth: { income: number; expense: number };
}
```

## 3. Implementation Details

**Stack:** Vite + React 18 + TypeScript + Tailwind CSS 3 + Lucide React + shadcn/ui + Framer Motion + Recharts

- **Mobile-first responsive design** dengan breakpoint 640px (mobile), 768px (tablet), 1024px (desktop)
- **Offline-first:** IndexedDB via Dexie.js untuk data lokal, sync ke cloud saat online
- **PWA:** Service worker untuk offline access, install prompt
- **Quick entry:** Bottom sheet untuk input transaksi cepat
- **Charts:** Recharts pie chart untuk kategori, bar chart untuk tren

## 4. Styling & Theming

**Palette:**
- Primary: `#14B8A6` (teal / hijau toska) — fresh, finansial, growth
- Primary Light: `#5EEAD4`
- Primary Dark: `#0F766E`
- Accent: `#8B5CF6` (purple) — untuk goals dan fitur premium
- Background: `#F9FAFB`
- Surface: `#FFFFFF`
- Text Primary: `#111827`
- Text Secondary: `#6B7280`
- Income: `#10B981` (green)
- Expense: `#EF4444` (red)
- Warning: `#F59E0B`
- Category Colors: variasi warna per kategori (makanan=orange, transport=biru, dll)

**Typography:**
- Font: Inter (sans-serif) — playful tapi tetap profesional
- Saldo: text-3xl (30px) bold
- Transaksi: text-sm (14px)

**Spacing:**
- Mobile: padding 16px horizontal
- Card: rounded-2xl (16px) soft
- Shadow: subtle, warm shadows

## 5. UI Components Specification

### Floating Action Button (FAB)
```
                    ┌─────────────┐
                    │ 💰 Pemasukan │
           ┌───────┤ 💸 Pengeluaran│
           │       │ 🔄 Transfer  │
    ┌──────┴──────┐│              │
    │     ＋      ││  [Tutup]     │
    └─────────────┘└──────────────┘
```
- Default: FAB circular dengan icon "+"
- Tap: expand 3 opsi (Pemasukan, Pengeluaran, Transfer)
- Animasi: Framer Motion spring

### Transaction Bottom Sheet
```
┌──────────────────────────────────────────────────────────────┐
│ ✏️ Tambah Transaksi                                     [X] │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│ 💸 Pengeluaran      💰 Pemasukan       🔄 Transfer          │
│ [●───────────────○───────────────○──────────]               │
│                                                              │
│ Nominal            [Rp 45.000                    ]           │
│ Kategori           [🍜 Makanan & Minuman        ▾]           │
│ Tanggal            [📅 15 Maret 2025            ▾]           │
│ Dompet             [💳 BCA Syariah              ▾]           │
│ Catatan            [Makan siang di Warteg Budi   ]           │
│                                                              │
│ [ ] Transaksi berulang?                                      │
│                                                              │
│           [Batal]              [Simpan]                      │
└──────────────────────────────────────────────────────────────┘
```

### Budget Ring Indicator
```
      ╭─────────╮
      │   75%   │  ← Circular progress
      │  ▄▀█▀▄  │  Makanan: Rp900.000 / Rp1.200.000
      │  █████  │
      ╰─────────╯
  Sisa: Rp300.000 | Tersisa 16 hari
```

## 6. Interactions & States

### Loading States
- **Dashboard:** Skeleton cards (3 shimmer cards for accounts)
- **Transaction list:** 5 shimmer rows with pulse
- **Charts:** Skeleton circle for pie chart, skeleton bars for bar chart
- **Pull-to-refresh:** On mobile, swipe down to refresh data

### Empty States
- **No transactions:** Ilustrasi dompet kosong + "Belum ada transaksi. Mulai catat pengeluaran pertama Anda!"
- **No accounts:** + "Buat akun pertama untuk mulai mencatat"
- **No budget:** + "Atur anggaran bulanan untuk kontrol keuangan"
- **No goals:** + "Buat tujuan finansial, seperti liburan atau dana darurat"
- **Filter empty:** "Tidak ada transaksi dengan filter ini"

### Error States
- **Sync failed:** Banner "Gagal sinkronisasi. Data tersimpan di lokal." + tombol sync manual
- **Network:** Toast "Tidak ada koneksi internet. Mode offline."
- **Validation:** Field error merah + pesan (e.g., "Nominal harus diisi")
- **Delete confirmation:** Bottom sheet "Yakin hapus transaksi ini?" + [Hapus] [Batal]

### Edge Cases
- **Duplicate entry:** Deteksi transaksi serupa dalam 5 menit → "Transaksi serupa terdeteksi. Tetap simpan?"
- **Over budget:** Notifikasi "Anggaran Makanan sudah 90%! Sisa Rp120.000"
- **Negative balance:** Peringatan jika pengeluaran > saldo akun
- **Future date:** Warning "Tanggal di masa depan. Yakin?"
- **Split transaction:** Satu transaksi bisa di-split ke beberapa kategori
- **Currency conversion:** Jika ada transaksi mata uang asing

## 7. Language & Localization

Bahasa Indonesia penuh:
- Labels: "Pemasukan", "Pengeluaran", "Transfer", "Saldo", "Anggaran", "Tabungan"
- Categories: "Makanan", "Transportasi", "Belanja", "Hiburan", "Kesehatan", "Pendidikan", "Tagihan & Utilitas", "Gaji", "Investasi"
- Actions: "Tambah", "Simpan", "Hapus", "Edit", "Filter", "Cari"
- Messages: "Berhasil disimpan", "Yakin menghapus?", "Koneksi offline"
- Period: "Minggu ini", "Bulan ini", "Tahun ini", "Kustom"
- Currency: Rp dengan format Indonesia
- Date: "15 Mar 2025", "Hari ini", "Kemarin"

## 8. Example Mock Data

```typescript
const mockAccounts: Account[] = [
  {
    id: "ACC-001",
    name: "BCA Syariah",
    type: "bank",
    balance: 5230000,
    currency: "IDR",
    color: "#2563EB",
    icon: "banknote",
    isActive: true,
    includeInTotal: true,
    order: 1,
    createdAt: "2024-06-01T00:00:00Z"
  },
  {
    id: "ACC-002",
    name: "Mandiri",
    type: "bank",
    balance: 4300000,
    currency: "IDR",
    color: "#DC2626",
    icon: "banknote",
    isActive: true,
    includeInTotal: true,
    order: 2,
    createdAt: "2024-06-01T00:00:00Z"
  },
  {
    id: "ACC-003",
    name: "Tunai Dompet",
    type: "cash",
    balance: 1250000,
    currency: "IDR",
    color: "#059669",
    icon: "wallet",
    isActive: true,
    includeInTotal: true,
    order: 3,
    createdAt: "2024-06-01T00:00:00Z"
  },
  {
    id: "ACC-004",
    name: "GoPay",
    type: "ewallet",
    balance: 350000,
    currency: "IDR",
    color: "#00AFF5",
    icon: "smartphone",
    isActive: true,
    includeInTotal: true,
    order: 4,
    createdAt: "2024-09-15T00:00:00Z"
  }
];

const mockTransactions: Transaction[] = [
  {
    id: "TRX-2025-03-001",
    type: "expense",
    amount: 45000,
    description: "Makan siang di Warteg Budi",
    categoryId: "CAT-001",
    categoryName: "Makanan & Minuman",
    accountId: "ACC-003",
    accountName: "Tunai Dompet",
    date: "2025-03-15",
    time: "12:30",
    isRecurring: false,
    tags: ["makan", "siang"],
    location: { lat: -6.2088, lng: 106.8456, name: "Warteg Budi, Jakarta" },
    createdAt: "2025-03-15T12:35:00Z"
  },
  {
    id: "TRX-2025-03-002",
    type: "expense",
    amount: 25000,
    description: "Gojek ke kantor",
    categoryId: "CAT-002",
    categoryName: "Transportasi",
    accountId: "ACC-004",
    accountName: "GoPay",
    date: "2025-03-15",
    time: "08:15",
    isRecurring: false,
    tags: ["gojek", "transport"],
    createdAt: "2025-03-15T08:20:00Z"
  },
  {
    id: "TRX-2025-03-003",
    type: "income",
    amount: 8500000,
    description: "Gaji Bulan Maret 2025",
    categoryId: "CAT-100",
    categoryName: "Gaji",
    accountId: "ACC-001",
    accountName: "BCA Syariah",
    date: "2025-03-14",
    time: "09:00",
    isRecurring: true,
    recurringRule: { frequency: "monthly", interval: 1 },
    tags: ["gaji", "bulanan"],
    createdAt: "2025-03-14T09:05:00Z"
  },
  {
    id: "TRX-2025-03-004",
    type: "expense",
    amount: 350000,
    description: "Tagihan Telkomsel Internet",
    categoryId: "CAT-005",
    categoryName: "Tagihan & Utilitas",
    accountId: "ACC-001",
    accountName: "BCA Syariah",
    date: "2025-03-14",
    time: "10:00",
    isRecurring: true,
    recurringRule: { frequency: "monthly", interval: 1 },
    tags: ["internet", "tagihan"],
    createdAt: "2025-03-14T10:05:00Z"
  },
  {
    id: "TRX-2025-03-005",
    type: "transfer",
    amount: 500000,
    description: "Transfer ke tabungan",
    categoryId: "",
    categoryName: "",
    accountId: "ACC-001",
    accountName: "BCA Syariah",
    date: "2025-03-13",
    time: "15:00",
    isRecurring: false,
    tags: ["tabungan"],
    createdAt: "2025-03-13T15:05:00Z"
  }
];

const mockBudgets: Budget[] = [
  { id: "BGT-001", categoryId: "CAT-001", categoryName: "Makanan & Minuman", amount: 1200000, spent: 900000, period: { month: 3, year: 2025 }, alertAt: 80, rollover: false },
  { id: "BGT-002", categoryId: "CAT-002", categoryName: "Transportasi", amount: 800000, spent: 650000, period: { month: 3, year: 2025 }, alertAt: 80, rollover: false },
  { id: "BGT-003", categoryId: "CAT-003", categoryName: "Belanja", amount: 1000000, spent: 400000, period: { month: 3, year: 2025 }, alertAt: 75, rollover: true },
  { id: "BGT-004", categoryId: "CAT-005", categoryName: "Tagihan & Utilitas", amount: 1500000, spent: 1350000, period: { month: 3, year: 2025 }, alertAt: 80, rollover: false },
];

const mockGoal: FinancialGoal = {
  id: "GOAL-001",
  name: "Liburan ke Bali",
  targetAmount: 5000000,
  currentAmount: 1750000,
  deadline: "2025-08-01",
  icon: "umbrella-beach",
  color: "#8B5CF6",
  status: "active",
  autoSaveRule: {
    accountId: "ACC-001",
    amount: 500000,
    frequency: "monthly"
  }
};
```

## 9. Success Criteria

- [ ] CRUD transaksi pemasukan, pengeluaran, transfer
- [ ] Multi-akun (bank, e-wallet, tunai) dengan saldo real-time
- [ ] Kategori dengan subkategori dan ikon
- [ ] Anggaran bulanan per kategori dengan progress bar
- [ ] Grafik pengeluaran (pie chart per kategori, bar chart tren)
- [ ] Laporan bulanan dengan perbandingan bulan sebelumnya
- [ ] Pencarian dan filter transaksi (tanggal, kategori, akun, keyword)
- [ ] Transaksi berulang (recurring)
- [ ] Export data ke CSV/Excel
- [ ] Dark mode
- [ ] PWA: installable, offline-capable
- [ ] Performance: daftar transaksi 10.000 item render < 1 detik
