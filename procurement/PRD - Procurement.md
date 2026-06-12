# PRD: PengadaanCerdas — Sistem Procurement & Purchase Order Management

**Project:** PengadaanCerdas
**Aesthetic:** Modern, professional, clean B2B SaaS dengan nuansa biru gelap (#1E3A5F) sebagai warna utama

## 1. Core Layout

```
┌──────────────────────────────────────────────────────────────────────────┐
│ [Logo] PengadaanCerdas                                    [🔔] [User] ▾ │
├────────┬─────────────────────────────────────────────────────────────────┤
│        │  Dashboard / Purchase Orders / Approvals / Suppliers / Reports  │
│ Sidebar│─────────────────────────────────────────────────────────────────│
│  📊    │  ┌──────┬──────┬──────┬──────┬──────┐                          │
│  📋    │  │Total │PO    │ PO   │ PO   │Angg- │                          │
│  ✅    │  │PO    │Pending│Approved│Ditolak│aran   │                          │
│  🏢    │  │ 25   │ 8    │ 15   │ 2    │ 4.2M │                          │
│  📄    │  └──────┴──────┴──────┴──────┴──────┘                          │
│  ⚙️    │                                                               │
│        │  ┌─────────────────────────────────────────────────────────┐   │
│        │  │ ✅ PO Menunggu Approval                                 │   │
│        │  │ ┌──────┬──────────┬──────────┬────────┬──────┬──────┐  │   │
│        │  │ │No PO │ Supplier │ Total    │ Tanggal│ Status│ Aksi │  │   │
│        │  │ ├──────┼──────────┼──────────┼────────┼──────┼──────┤  │   │
│        │  │ │PO-025│ PT ABC   │125.000.000│15/03 │Pending│[✅][❌]│  │   │
│        │  │ │PO-024│ PT XYZ   │ 85.000.000│14/03 │Pending│[✅][❌]│  │   │
│        │  │ │...   │ ...      │ ...      │ ...    │ ...  │ ...  │  │   │
│        │  │ └──────┴──────────┴──────────┴────────┴──────┴──────┘  │   │
│        │  └─────────────────────────────────────────────────────────┘   │
│        │                                                               │
│        │  ┌──────────────────┐  ┌──────────────────────────┐          │
│        │  │   PO by Status   │  │   Pengeluaran per Dept   │          │
│        │  │ [Pie Chart]      │  │   [Bar Chart]            │          │
│        │  │ Pending ██ 32%   │  │ IT ██████████ 1.2M      │          │
│        │  │ Approve ██ 60%   │  │ Ops ████████ 900JT      │          │
│        │  │ Reject  ██ 8%    │  │ Mktg ██████ 750JT      │          │
│        │  └──────────────────┘  └──────────────────────────┘          │
├────────┴────────────────────────────────────────────────────────────────┤
│ (c) 2025 PengadaanCerdas    Periode: Maret 2025    Terakhir sync: 2m lalu│
└─────────────────────────────────────────────────────────────────────────┘
```

- **Sidebar Kiri:** Dashboard, Purchase Orders, Approval Queue, Suppliers, Contracts, Reports, Settings
- **Top Bar:** Global search across PO/supplier, notifications for pending approvals, user profile
- **Panel Utama:** Content area with kanban-style lists for approval workflow
- **Status Bar:** Active fiscal period, pending approvals count, last sync

## 2. Data Structure (Mock Only)

```typescript
// === Purchase Order ===
interface PurchaseOrder {
  id: string; // e.g. "PO-2025-03-0025"
  poNumber: string; // e.g. "PO-2025-03-0025"
  title: string; // e.g. "Pembelian perlengkapan IT Q2 2025"
  supplierId: string;
  supplierName: string; // denormalized
  department: string; // e.g. "Teknologi Informasi"
  requesterId: string;
  requesterName: string; // e.g. "Andi Pratama"
  orderDate: string; // ISO date
  expectedDeliveryDate?: string;
  status: 'draft' | 'pending' | 'approved' | 'rejected' | 'ordered' | 'partial' | 'received' | 'cancelled';
  items: PurchaseOrderLine[];
  subtotal: number;
  tax: number; // PPN 11%
  shippingCost: number;
  otherCost: number;
  grandTotal: number;
  currency: string; // IDR
  notes: string;
  approvalHistory: ApprovalAction[];
  attachmentUrls: string[];
  createdAt: string;
  updatedAt: string;
}

interface PurchaseOrderLine {
  id: string;
  productName: string; // e.g. "Monitor Dell 27 inch"
  description: string;
  quantity: number;
  unit: string; // e.g. "unit", "pcs", "box"
  unitPrice: number;
  discount: number;
  totalPrice: number;
  accountCode?: string; // kode akun untuk pembebanan biaya
  receivedQuantity: number;
  notes?: string;
}

interface ApprovalAction {
  approverId: string;
  approverName: string;
  action: 'approved' | 'rejected' | 'revised';
  comment: string;
  timestamp: string;
  level: number; // 1 = manager, 2 = director
}

// === Supplier ===
interface Supplier {
  id: string; // e.g. "SUP-001"
  name: string; // e.g. "PT Sinar Jaya Komputer"
  npwp: string; // NPWP
  address: string;
  city: string;
  phone: string;
  email: string;
  website?: string;
  picName: string; // person in charge
  picPhone: string;
  paymentTerms: string; // e.g. "Net 30"
  bankName: string;
  bankAccount: string;
  bankBranch: string;
  category: string[]; // e.g. ["IT Equipment", "Office Supplies"]
  rating: number; // 1-5
  totalPoCount: number;
  totalPoValue: number;
  status: 'active' | 'inactive' | 'blacklisted';
  notes?: string;
  createdAt: string;
}

// === Contract ===
interface Contract {
  id: string;
  contractNumber: string; // e.g. "SPK-2025-001"
  title: string; // e.g. "Kontrak Tahunan IT Support"
  supplierId: string;
  supplierName: string;
  startDate: string;
  endDate: string;
  value: number;
  remainingValue: number;
  type: 'annual' | 'project' | 'subscription';
  status: 'active' | 'expired' | 'terminated';
  terms: string;
  attachmentUrl?: string;
  poIds: string[]; // PO yang mengacu ke kontrak ini
  createdAt: string;
}

// === Approval Workflow ===
interface ApprovalRule {
  id: string;
  name: string; // e.g. "Approval Manager + Director"
  conditions: {
    minAmount?: number;
    maxAmount?: number;
    department?: string;
  };
  steps: ApprovalStep[];
  isActive: boolean;
}

interface ApprovalStep {
  level: number;
  role: string; // e.g. "manager", "director", "finance"
  approverIds?: string[]; // specific users, if empty = all with role
  order: number;
}
```

## 3. Implementation Details

**Stack:** Vite + React 18 + TypeScript + Tailwind CSS 3 + Lucide React + shadcn/ui + Framer Motion + Recharts

- **State Management:** Zustand + TanStack Query
- **Forms:** React Hook Form + Zod untuk PO form (multi-step wizard)
- **Data Grid:** @tanstack/react-table untuk PO list dengan server-side sorting
- **Charts:** Recharts untuk dashboard spend analytics
- **PDF:** jsPDF + jspdf-autotable untuk cetak PO
- **Excel Export:** xlsx (SheetJS) untuk export laporan

## 4. Styling & Theming

**Palette:**
- Primary: `#1E3A5F` (navy) — kepercayaan, profesional, enterprise
- Primary Light: `#2D5A8E`
- Primary Dark: `#142845`
- Accent: `#0EA5E9` (sky blue) — untuk tombol aksi dan approval
- Success: `#22C55E` — approved
- Warning: `#F59E0B` — pending
- Error: `#EF4444` — rejected
- Background: `#F8FAFC`
- Surface: `#FFFFFF`
- Sidebar: dark navy `#142845`
- Text: `#1E293B` / `#64748B`
- Border: `#E2E8F0`

**Typography:**
- Font: Inter (sans-serif)
- PO Number: font-mono (JetBrains Mono)
- Headings: text-lg sampai text-2xl
- Body: text-sm (14px) untuk tabel

**Spacing:**
- Sidebar: 280px dark
- Content max-width: 1440px
- Card: rounded-xl with shadow-sm

## 5. UI Components Specification

### PO List Table
```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ Daftar Purchase Order                                        [+ Buat PO] [Filter] ▾   │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ ┌────────┬──────────┬──────────────┬──────────┬────────┬──────────┬───────┬────────┐ │
│ │ No PO  │ Supplier │ Judul        │ Dept     │ Total  │ Status   │ Tgl   │ Aksi   │ │
│ ├────────┼──────────┼──────────────┼──────────┼────────┼──────────┼───────┼────────┤ │
│ │PO-0025 │ PT ABC   │ Perlengkapan │ IT       │125.000K│ Pending  │15/03 │ [Y][N] │ │
│ │PO-0024 │ PT XYZ   │ Alat Tulis   │ Finance  │ 8.500K │ Approved │14/03 │ [D]    │ │
│ │PO-0023 │ PT DEF   │ Furniture    │ GA       │ 45.000K│ Rejected │13/03 │ [D]    │ │
│ │PO-0022 │ PT GHI   │ Bahan Baku   │ Produksi │250.000K│ Draft    │12/03 │ [E]    │ │
│ └────────┴──────────┴──────────────┴──────────┴────────┴──────────┴───────┴────────┘ │
│ Hal 1 dari 10                                                                 50/bin │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### PO Creation Form (Multi-Step)
```
┌──────────────────────────────────────────────────────────────────┐
│ Buat Purchase Order                                       [X]    │
├──────────────────────────────────────────────────────────────────┤
│ Langkah 1: Info Umum       Langkah 2: Item       Langkah 3: Review │
│ ==========================-----------------------------------        │
├──────────────────────────────────────────────────────────────────┤
│ Judul PO      [Pembelian perlengkapan IT Q2 2025          ]      │
│ Supplier      [PT Sinar Jaya Komputer                   ▾]      │
│ Department    [Teknologi Informasi                       ▾]      │
│ Tanggal PO    [15 Maret 2025                          ]          │
│ Pengiriman    [20 Maret 2025 (opsional)               ]          │
│ Mata Uang     [IDR ▾]                                          │
│ Catatan       [Pengiriman ke kantor pusat lantai 3       ]      │
│                                                               │
│         [Batal]                    [Lanjut ke Item >]          │
└──────────────────────────────────────────────────────────────────┘
```

### Approval Queue (Kanban)
```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│  Menunggu    │  │  Disetujui   │  │  Ditolak     │  │  Draft       │
│  8 PO        │  │  15 PO       │  │  2 PO        │  │  5 PO        │
├──────────────┤  ├──────────────┤  ├──────────────┤  ├──────────────┤
│ ┌──────────┐ │  │ ┌──────────┐│  │ ┌──────────┐ │  │ ┌──────────┐ │
│ │PO-025    │ │  │ │PO-024    ││  │ │PO-023    │ │  │ │PO-022    │ │
│ │PT ABC    │ │  │ │PT XYZ    ││  │ │PT DEF    │ │  │ │PT GHI    │ │
│ │Rp125JT   │ │  │ │Rp8,5JT   ││  │ │Rp45JT    │ │  │ │Rp250JT   │ │
│ │ 15 Mar   │ │  │ │14 Mar    ││  │ │13 Mar    │ │  │ │12 Mar    │ │
│ │[Setujui] │ │  │ │          ││  │ │          │ │  │ │[Edit]    │ │
│ └──────────┘ │  │ └──────────┘│  │ └──────────┘ │  │ └──────────┘ │
└──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘
```

## 6. Interactions & States

### Loading States
- **Dashboard:** Skeleton KPI cards (4 cards), skeleton chart area
- **PO List:** Table skeleton with 5 shimmer rows
- **PO Detail:** Left-right skeleton layout
- **Supplier List:** Card skeleton grid
- **Form submit:** Button spinner "Menyimpan..."

### Empty States
- **No PO:** "Belum ada purchase order. Buat PO pertama!" + illustration
- **No pending approvals:** "Semua PO sudah diproses. Tidak ada yang perlu disetujui."
- **No suppliers:** "Belum ada supplier. Tambahkan supplier pertama."
- **No search results:** "Tidak ditemukan PO dengan kata kunci '{{KEYWORD}}'"

### Error States
- **Network error:** "Gagal memuat data. Periksa koneksi." + retry button
- **Validation error:** Inline field errors with red border
- **Approval conflict:** "PO ini sudah di-approve oleh {{USER}}"
- **Delete protected:** "Tidak bisa hapus PO yang sudah di-approve"

### Edge Cases
- **Budget overrun:** Warning "Total PO melebihi anggaran departemen (Rp {{ANGGARAN}})"
- **Duplicate supplier:** Deteksi supplier duplikat berdasarkan NPWP
- **Approval delegation:** Approver bisa delegasikan ke user lain
- **Partial receipt:** Barang diterima sebagian, status jadi "partial"
- **Currency:** PO bisa dalam USD (untuk import), konversi ke IDR untuk laporan
- **Amendment:** PO yang sudah approved bisa di-revisi dengan version history

## 7. Language & Localization

Bahasa Indonesia penuh dengan istilah procurement:
- Labels: "Purchase Order", "Permintaan Pembelian", "Supplier", "Kontrak", "Anggaran"
- Actions: "Buat PO", "Setujui", "Tolak", "Revisi", "Cetak PO", "Terima Barang"
- Status: "Draft", "Menunggu", "Disetujui", "Ditolak", "Dipesan", "Sebagian", "Diterima"
- Departments: "Teknologi Informasi", "Keuangan", "Operasional", "Marketing", "SDM", "Produksi"
- Terms: "Net 30", "Net 60", "COD"
- Currency: "Rp" / "USD"
- Date: "dd MMMM yyyy"

## 8. Example Mock Data

```typescript
const mockSuppliers: Supplier[] = [
  {
    id: "SUP-001",
    name: "PT Sinar Jaya Komputer",
    npwp: "01.234.567.8-001.000",
    address: "Jl. Thamrin No. 88, Jakarta Pusat",
    city: "Jakarta",
    phone: "021-12345678",
    email: "sales@sinarajaya.co.id",
    picName: "Rudi Hartono",
    picPhone: "08119876543",
    paymentTerms: "Net 30",
    bankName: "BCA",
    bankAccount: "1234567890",
    bankBranch: "BCA Thamrin",
    category: ["IT Equipment", "Office Supplies"],
    rating: 4,
    totalPoCount: 15,
    totalPoValue: 850000000,
    status: "active",
    createdAt: "2024-01-15T00:00:00Z"
  },
  {
    id: "SUP-002",
    name: "CV Mitra Abadi",
    npwp: "02.345.678.9-002.000",
    address: "Jl. Raya Industri No. 45, Bekasi",
    city: "Bekasi",
    phone: "021-87654321",
    email: "info@mitraabadi.com",
    picName: "Siti Rahmawati",
    picPhone: "08123456789",
    paymentTerms: "Net 60",
    bankName: "Mandiri",
    bankAccount: "9876543210",
    bankBranch: "Mandiri Bekasi",
    category: ["Bahan Baku", "Packaging"],
    rating: 5,
    totalPoCount: 28,
    totalPoValue: 2500000000,
    status: "active",
    createdAt: "2024-01-15T00:00:00Z"
  }
];

const mockPurchaseOrder: PurchaseOrder = {
  id: "PO-2025-03-0025",
  poNumber: "PO-2025-03-0025",
  title: "Pembelian perlengkapan IT Q2 2025",
  supplierId: "SUP-001",
  supplierName: "PT Sinar Jaya Komputer",
  department: "Teknologi Informasi",
  requesterId: "USR-005",
  requesterName: "Andi Pratama",
  orderDate: "2025-03-15T09:00:00Z",
  expectedDeliveryDate: "2025-03-25T09:00:00Z",
  status: "pending",
  items: [
    {
      id: "POL-001",
      productName: "Monitor Dell 27 inch",
      description: "Dell UltraSharp U2724D, 2560x1440",
      quantity: 5,
      unit: "unit",
      unitPrice: 4500000,
      discount: 0,
      totalPrice: 22500000,
      accountCode: "5-2010",
      receivedQuantity: 0
    }
  ],
  subtotal: 33250000,
  tax: 3657500, // PPN 11%
  shippingCost: 500000,
  otherCost: 0,
  grandTotal: 37407500,
  currency: "IDR",
  notes: "Pengiriman ke kantor pusat lantai 3, divisi IT",
  approvalHistory: [],
  attachmentUrls: [],
  createdAt: "2025-03-15T09:15:00Z",
  updatedAt: "2025-03-15T09:15:00Z"
};

const mockApprovalRule: ApprovalRule = {
  id: "APPR-001",
  name: "Approval 2 Level (Manager + Director)",
  conditions: { minAmount: 10000000, maxAmount: 100000000 },
  steps: [
    { level: 1, role: "manager", approverIds: [], order: 1 },
    { level: 2, role: "director", approverIds: [], order: 2 }
  ],
  isActive: true
};

const mockContract: Contract = {
  id: "CTR-001",
  contractNumber: "SPK-2025-001",
  title: "Kontrak Tahunan IT Support & Maintenance",
  supplierId: "SUP-001",
  supplierName: "PT Sinar Jaya Komputer",
  startDate: "2025-01-01",
  endDate: "2025-12-31",
  value: 500000000,
  remainingValue: 350000000,
  type: "annual",
  status: "active",
  terms: "Pembayaran bulanan termin, dengan retensi 10%",
  poIds: ["PO-2025-03-0025"],
  createdAt: "2025-01-01T00:00:00Z"
};
```

## 9. Success Criteria

- [ ] CRUD Purchase Order dengan multi-line items
- [ ] Approval workflow multi-level (manager -> director) dengan configurable rules
- [ ] Supplier management (CRUD, rating, history)
- [ ] PO status tracking: draft -> pending -> approved -> ordered -> received
- [ ] Contract management (per tahun/per proyek)
- [ ] Dashboard dengan KPI: total PO, pending approval, budget utilization
- [ ] Cetak PO ke PDF format profesional
- [ ] Search & filter PO (status, supplier, department, date range)
- [ ] Budget checking against department budget
- [ ] Notification & reminder untuk pending approvals
- [ ] Audit trail: semua approval/rejection tercatat dengan timestamp
- [ ] Performance: PO list 10.000 items < 2 detik render
