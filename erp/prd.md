# PRD: ERP Nusantara — Sistem ERP All-in-One untuk Perusahaan Menengah Indonesia

**Project:** ERP Nusantara
**Aesthetic:** Modern, professional, clean enterprise SaaS dengan nuansa biru laut (#0F4C81) sebagai warna utama

## 1. Core Layout

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Logo] ERP Nusantara                                      [🔔] [User] ▾│
├────────┬────────────────────────────────────────────────────────────────┤
│        │  Dashboard / Finance / Inventory / HR / Operasional / Laporan  │
│ Sidebar│────────────────────────────────────────────────────────────────│
│  📊    │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐          │
│  💰    │  │Pendapatan    │ │Biaya         │ │Laba Bersih   │          │
│  📦    │  │Rp 1,2 M      │ │Rp 890 Juta   │ │Rp 310 Juta   │          │
│  👥    │  └──────────────┘ └──────────────┘ └──────────────┘          │
│  ⚙️    │                                                               │
│  📄    │  ┌────────────────────────────────────────────────────────┐   │
│        │  │ Aktivitas Terbaru                                       │   │
│        │  │ ┌──────┬──────────┬────────────┬────────┬──────────┐   │   │
│        │  │ │Waktu │ Modul    │ Deskripsi  │ User   │ Status   │   │   │
│        │  │ ├──────┼──────────┼────────────┼────────┼──────────┤   │   │
│        │  │ │09:15 │ Inventory│SO-0012     │ Andi   │ Selesai  │   │   │
│        │  │ │08:45 │ Finance  │Pembayaran  │ Rina   │ Pending  │   │   │
│        │  │ │...   │ ...      │...         │ ...    │ ...      │   │   │
│        │  │ └──────┴──────────┴────────────┴────────┴──────────┘   │   │
│        │  └────────────────────────────────────────────────────────┘   │
│        │                                                               │
│        │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│        │  │ Inventory│  │ HR       │  │ Order    │  │ Approval │    │
│        │  │ 1.250 SKU│  │ 45 Emp   │  │ 23 Open  │  │ 5 Pending│    │
│        │  └──────────┘  └──────────┘  └──────────┘  └──────────┘    │
├────────┴────────────────────────────────────────────────────────────────┤
│ © 2025 ERP Nusantara — v1.0.0    Terakhir sync: 10 detik lalu          │
└─────────────────────────────────────────────────────────────────────────┘
```

- **Sidebar Kiri:** Navigasi utama — Dashboard, Finance, Inventory, HR, Sales & Orders, Purchasing, Operational, Reports, Settings
- **Top Bar:** Global search, module switcher, notifications, user profile with company selector
- **Panel Utama:** Content area dengan sub-navigation tabs per modul
- **Status Bar:** System status, last sync, active fiscal period, connection status

## 2. Data Structure (Mock Only)

```typescript
// === Product / Inventory ===
interface Product {
  id: string; // e.g. "PRD-2025-0001"
  sku: string; // e.g. "BRG-001"
  barcode: string; // e.g. "8991234567890"
  name: string; // e.g. "Semen Tiga Roda 50kg"
  category: string; // e.g. "Material Bangunan"
  unit: string; // e.g. "sak", "pcs", "kg"
  purchasePrice: number; // harga beli
  sellingPrice: number; // harga jual
  currentStock: number;
  minStock: number; // safety stock
  maxStock: number;
  location: string; // gudang / rak
  warehouse: string; // "Gudang Utama", "Gudang Cabang"
  status: 'active' | 'discontinued' | 'out-of-stock';
  supplierIds: string[];
  createdAt: string;
  updatedAt: string;
}

// === Employee ===
interface Employee {
  id: string; // e.g. "EMP-2025-001"
  employeeNumber: string; // e.g. "NIP-2025-001"
  fullName: string; // e.g. "Andi Pratama"
  nik: string; // NIK KTP
  placeOfBirth: string;
  dateOfBirth: string; // ISO date
  gender: 'L' | 'P';
  address: string;
  phone: string;
  email: string;
  department: string; // e.g. "Keuangan"
  position: string; // e.g. "Staff Akuntansi"
  level: string; // e.g. "Staff", "Supervisor", "Manager"
  joinDate: string;
  status: 'active' | 'resigned' | 'terminated';
  salary: {
    basicSalary: number;
    allowance: number;
    bankName: string;
    bankAccount: string;
  };
  bpjsKesehatan: string;
  bpjsKetenagakerjaan: string;
  emergencyContact: { name: string; phone: string; relation: string };
}

// === Sales Order ===
interface SalesOrder {
  id: string; // e.g. "SO-2025-03-0012"
  orderNumber: string;
  customerId: string;
  customerName: string; // denormalized
  orderDate: string;
  deliveryDate?: string;
  status: 'draft' | 'confirmed' | 'processing' | 'shipped' | 'delivered' | 'cancelled';
  items: SalesOrderLine[];
  subtotal: number;
  discount: number;
  tax: number; // PPN 11%
  grandTotal: number;
  paymentStatus: 'unpaid' | 'partial' | 'paid';
  paymentDueDate: string;
  notes: string;
  createdBy: string;
  approvedBy?: string;
  createdAt: string;
}

interface SalesOrderLine {
  productId: string;
  productName: string;
  quantity: number;
  unit: string;
  unitPrice: number;
  discount: number;
  totalPrice: number;
}

// === Purchase Order ===
interface PurchaseOrder {
  id: string;
  poNumber: string; // e.g. "PO-2025-03-0050"
  supplierId: string;
  supplierName: string;
  orderDate: string;
  expectedDelivery: string;
  status: 'draft' | 'sent' | 'partial' | 'received' | 'cancelled';
  items: PurchaseOrderLine[];
  subtotal: number;
  tax: number;
  grandTotal: number;
  paymentTerms: string; // e.g. "Net 30"
  notes: string;
  approvedBy?: string;
  createdAt: string;
}

interface PurchaseOrderLine {
  productId: string;
  productName: string;
  quantity: number;
  unit: string;
  unitPrice: number;
  receivedQuantity: number;
  totalPrice: number;
}

// === Attendance ===
interface Attendance {
  id: string;
  employeeId: string;
  employeeName: string;
  date: string;
  checkIn: string; // e.g. "08:00"
  checkOut: string; // e.g. "17:00"
  status: 'present' | 'late' | 'absent' | 'leave' | 'sick';
  lateMinutes: number;
  overtimeMinutes: number;
  notes: string;
  approvedBy?: string;
}

// === Leave Request ===
interface LeaveRequest {
  id: string;
  employeeId: string;
  employeeName: string;
  type: 'annual' | 'sick' | 'maternity' | 'religious' | 'other';
  startDate: string;
  endDate: string;
  totalDays: number;
  reason: string;
  status: 'pending' | 'approved' | 'rejected';
  approvedBy?: string;
  approvedAt?: string;
  attachment?: string; // surat dokter dll
}
```

## 3. Implementation Details

**Stack:** Vite + React 18 + TypeScript + Tailwind CSS 3 + Lucide React + shadcn/ui + Framer Motion + Recharts

- **State Management:** Zustand + TanStack Query
- **Multi-Module:** Feature-Sliced Design dengan 6 domain modul
- **Data Table:** @tanstack/react-table dengan server-side pagination
- **Forms:** React Hook Form + Zod
- **Charts:** Recharts untuk dashboard finance dan inventory trends
- **PDF:** jsPDF untuk purchase order dan invoice
- **Excel Export:** xlsx (SheetJS) untuk export data

## 4. Styling & Theming

**Palette:**
- Primary: `#0F4C81` (biru laut) — kepercayaan, stabilitas enterprise
- Primary Light: `#2A6DB5`
- Primary Dark: `#0A355B`
- Accent: `#00B4D8` (cyan) — modern dan segar
- Background: `#F0F4F8`
- Surface: `#FFFFFF`
- Sidebar: `#0A355B` (dark sidebar) — enterprise look
- Sidebar Text: `#A8C8E8`
- Text Primary: `#1E293B`
- Text Secondary: `#64748B`
- Success: `#10B981`
- Error: `#EF4444`
- Warning: `#F59E0B`
- Info: `#3B82F6`
- Border: `#E2E8F0`

**Typography:**
- Font: Inter (sans-serif), JetBrains Mono untuk kode/nomor
- Headings: text-xl (20px) hingga text-3xl (30px)
- Body: text-sm (14px)

**Spacing:**
- Sidebar: 280px dark
- Content max-width: 1600px (dashboard overview)
- Card padding: 20px

## 5. UI Components Specification

### Dark Sidebar Navigation
```
┌──────────────────────┐
│ [●] ERP Nusantara     │ — logo putih
├──────────────────────┤
│ 📊  Dashboard         │
│ 💰  Finance ▸         │ — submenu: Jurnal, Buku Besar, AR/AP
│ 📦  Inventory ▸       │ — submenu: Produk, Stok, Mutasi
│ 👥  HR ▸              │ — submenu: Karyawan, Absensi, Payroll
│ 📋  Sales ▸           │ — submenu: Sales Order, Customer
│ 📥  Purchasing ▸      │ — submenu: PO, Supplier
│ ⚙️  Operational ▸     │
│ 📄  Reports ▸         │
│ ⚙️  Settings          │
├──────────────────────┤
│ 🏢 PT Maju Jaya ▾    │ — company switcher
│ 📅 Periode: Mar 2025 │
└──────────────────────┘
```

### Finance Dashboard
```
┌──────────────────────────────────────────────────────────────────┐
│ 💰 Finance Dashboard                                      [Periode] │
├──────────────────────────────────────────────────────────────────┤
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐            │
│ │ Revenue  │ │ COGS     │ │ Gross    │ │ Net      │            │
│ │ 1.2 M    │ │ 780 JT   │ │ 420 JT   │ │ 310 JT   │            │
│ │ ▲ 12%    │ │ ▼ 3%     │ │ ▲ 15%    │ │ ▲ 18%    │            │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘            │
│                                                                  │
│ ┌───────────────────────────┐ ┌──────────────────────────────┐  │
│ │ Revenue Trend             │ │ AR / AP Aging                │  │
│ │ [Line Chart — 6 bulan]    │ │ [Bar Chart — 30/60/90+]     │  │
│ │                           │ │                              │  │
│ │ Jan ██ 980JT              │ │ AR: 150JT | 80JT | 45JT    │  │
│ │ Feb ██ 1.1M               │ │ AP: 120JT | 60JT | 30JT    │  │
│ │ Mar ██ 1.2M               │ └──────────────────────────────┘  │
│ └───────────────────────────┘                                   │
└──────────────────────────────────────────────────────────────────┘
```

### Inventory Table
```
┌─────────────────────────────────────────────────────────────────────────┐
│ 📦 Inventory                                  [Cari SKU...] [+ Produk] │
├─────────────────────────────────────────────────────────────────────────┤
│ ┌────────┬──────────┬──────────┬──────┬──────┬──────┬──────────┬─────┐│
│ │ SKU    │ Nama     │ Kategori │ Stok │ Min  │ Max  │ Harga    │     ││
│ ├────────┼──────────┼──────────┼──────┼──────┼──────┼──────────┼─────┤│
│ │BRG-001 │ Semen    │ Material │ 1.250│ 500  │ 2.000│ Rp55.000 │ ✓   ││
│ │BRG-002 │ Pasir    │ Material │  🔴  │ 1.000│ 5.000│ Rp25.000 │ ⚠️  ││
│ │BRG-003 │ Bata Merah│ Material│ 3.000│ 2.000│10.000│ Rp1.200  │ ✓   ││
│ │...     │ ...      │ ...      │ ...  │ ...  │ ...  │ ...      │ ... ││
│ └────────┴──────────┴──────────┴──────┴──────┴──────┴──────────┴─────┘│
│ Hal 1 dari 25                                                  50/bin │
└─────────────────────────────────────────────────────────────────────────┘
```

## 6. Interactions & States

### Loading States
- **Dashboard:** Skeleton cards with pulse animation (4 shimmer cards)
- **Table:** Shimmer rows (5 rows) with table header skeleton
- **Detail view:** Content skeleton with image placeholder for product
- **Form submit:** Button loading spinner + disabled state
- **Charts:** Framer Motion fade-in animation with skeleton placeholder

### Empty States
- **No inventory:** "Gudang masih kosong. Tambahkan produk pertama!"
- **No sales data:** "Belum ada penjualan. Buat sales order baru."
- **No employees:** "Belum ada data karyawan. Daftarkan karyawan pertama."
- **Search empty:** "Tidak ditemukan hasil untuk '{{KEYWORD}}'"
- **No reports:** "Pilih periode untuk generate laporan"

### Error States
- **Network:** "Gagal memuat data. Periksa koneksi." + "Muat Ulang" button
- **Validation:** Field errors with red border + message under field
- **Server:** Toast "Server error ({{CODE}}). Tim kami sedang menangani."
- **Concurrent edit:** Conflict dialog

### Edge Cases
- **Below min stock:** Card/badge merah "Stok Menipis" + reorder suggestion
- **Over max stock:** Card kuning "Stok Berlebih" 
- **Double approval:** Check if already approved before second approval
- **Data integrity:** Prevent delete if product exists in active orders
- **Currency:** All in IDR, prevent negative pricing
- **Date conflicts:** Leave request overlap detection

## 7. Language & Localization

Seluruh UI dalam Bahasa Indonesia. Istilah bisnis Indonesia:
- Modul: "Keuangan", "Inventaris", "SDM", "Penjualan", "Pembelian"
- Actions: "Tambah", "Ubah", "Hapus", "Setujui", "Tolak", "Cetak"
- Status: "Draft", "Dikonfirmasi", "Diproses", "Dikirim", "Selesai", "Dibatalkan"
- HR: "Karyawan", "Absensi", "Cuti", "Penggajian", "BPJS"
- Finance: "Piutang", "Utang", "Pendapatan", "Beban"
- Period: Indonesia month names
- Currency format: Rp 1.000.000,00
- Date format: dd MMMM yyyy (15 Maret 2025)

## 8. Example Mock Data

```typescript
const mockProducts: Product[] = [
  {
    id: "PRD-2025-0001",
    sku: "BRG-001",
    barcode: "8991234567890",
    name: "Semen Tiga Roda 50kg",
    category: "Material Bangunan",
    unit: "sak",
    purchasePrice: 45000,
    sellingPrice: 55000,
    currentStock: 1250,
    minStock: 500,
    maxStock: 2000,
    location: "Gudang A - Rak 3",
    warehouse: "Gudang Utama",
    status: "active",
    supplierIds: ["SUP-001"],
    createdAt: "2024-01-15T08:00:00Z",
    updatedAt: "2025-03-15T10:30:00Z"
  },
  {
    id: "PRD-2025-0002",
    sku: "BRG-002",
    barcode: "8999876543210",
    name: "Pasir Beton 1 Truk",
    category: "Material Bangunan",
    unit: "truk",
    purchasePrice: 1800000,
    sellingPrice: 2200000,
    currentStock: 8,
    minStock: 10,
    maxStock: 50,
    location: "Gudang B - Lapangan",
    warehouse: "Gudang Utama",
    status: "active",
    supplierIds: ["SUP-001", "SUP-002"],
    createdAt: "2024-01-15T08:00:00Z",
    updatedAt: "2025-03-14T14:00:00Z"
  }
];

const mockEmployees: Employee[] = [
  {
    id: "EMP-2025-001",
    employeeNumber: "NIP-2025-001",
    fullName: "Andi Pratama",
    nik: "3273011508900001",
    placeOfBirth: "Jakarta",
    dateOfBirth: "1990-08-15",
    gender: "L",
    address: "Jl. Merdeka No. 45, Jakarta Pusat",
    phone: "081234567890",
    email: "andi@majujaya.com",
    department: "Keuangan",
    position: "Staff Akuntansi",
    level: "Staff",
    joinDate: "2020-03-01",
    status: "active",
    salary: {
      basicSalary: 6500000,
      allowance: 1500000,
      bankName: "BCA",
      bankAccount: "1234567890"
    },
    bpjsKesehatan: "BPJS-001234",
    bpjsKetenagakerjaan: "BPJS-005678",
    emergencyContact: { name: "Siti Rahma", phone: "087654321098", relation: "Istri" }
  }
];

const mockSalesOrder: SalesOrder = {
  id: "SO-2025-03-0012",
  orderNumber: "SO-2025-03-0012",
  customerId: "CUST-001",
  customerName: "PT Bangun Sejahtera",
  orderDate: "2025-03-15T09:00:00Z",
  deliveryDate: "2025-03-20T09:00:00Z",
  status: "confirmed",
  items: [
    {
      productId: "PRD-2025-0001",
      productName: "Semen Tiga Roda 50kg",
      quantity: 100,
      unit: "sak",
      unitPrice: 55000,
      discount: 0,
      totalPrice: 5500000
    },
    {
      productId: "PRD-2025-0002",
      productName: "Pasir Beton 1 Truk",
      quantity: 2,
      unit: "truk",
      unitPrice: 2200000,
      discount: 100000,
      totalPrice: 4300000
    }
  ],
  subtotal: 9800000,
  discount: 100000,
  tax: 1078000, // PPN 11%
  grandTotal: 10778000,
  paymentStatus: "unpaid",
  paymentDueDate: "2025-04-15",
  notes: "Pengiriman ke proyek Cililitan",
  createdBy: "EMP-2025-001",
  createdAt: "2025-03-15T09:05:00Z"
};

const mockAttendance: Attendance[] = [
  {
    id: "ATT-2025-03-15-001",
    employeeId: "EMP-2025-001",
    employeeName: "Andi Pratama",
    date: "2025-03-15",
    checkIn: "07:55",
    checkOut: "17:10",
    status: "present",
    lateMinutes: 0,
    overtimeMinutes: 10,
    notes: ""
  },
  {
    id: "ATT-2025-03-15-002",
    employeeId: "EMP-2025-002",
    employeeName: "Siti Nurhaliza",
    date: "2025-03-15",
    checkIn: "08:20",
    checkOut: "17:00",
    status: "late",
    lateMinutes: 20,
    overtimeMinutes: 0,
    notes: "Macet"
  }
];
```

## 9. Success Criteria

- [ ] 6 modul berfungsi penuh: Finance, Inventory, HR, Sales, Purchasing, Operational
- [ ] Integrasi data antar modul (SO → Inventory → Finance)
- [ ] Dashboard real-time dengan data agregat dari seluruh modul
- [ ] Role-based access per modul
- [ ] Multi-perusahaan (entitas)
- [ ] Report consolidated (cross-module)
- [ ] Performance: halaman dashboard load < 3 detik
- [ ] Approval workflow di setiap modul transaksional
- [ ] Export data ke PDF/Excel
- [ ] Audit log untuk semua transaksi
