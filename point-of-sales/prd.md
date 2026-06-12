# PRD: KasirCerdas — Aplikasi POS untuk Toko Retail Indonesia

**Project:** KasirCerdas
**Aesthetic:** Modern, clean, fast-paced dengan nuansa oranye (#F97316) sebagai warna utama — energik, retail-friendly

## 1. Core Layout

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ [≡] KasirCerdas        🏪 Toko Sinar Jaya        [🛒 0]  [🔔]  [User] ▾    │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────── Pilih Produk ──────────┐    ┌─────── Pesanan Saat Ini ──────────┐│
│  │                                │    │                                   ││
│  │ [Cari nama/SKU/barcode...]  🔍 │    │ Pelanggan: [Umum ▾]             ││
│  │                                │    │                                   ││
│  │ ┌──────────┬──────────┬──────┐│    │ ┌──────────┬──────┬──────┬────┐  ││
│  │ │Kategori  │ Semua    │  ⌄   ││    │ │Produk    │Qty   │Harga │Sub │  ││
│  │ └──────────┴──────────┴──────┘│    │ ├──────────┼──────┼──────┼────┤  ││
│  │                                │    │ │Semen Tiga│  5   │55.000│275 │  ││
│  │ ┌──────┬──────┬──────┬──────┐│    │ │Pasir     │  2   │25.000│ 50 │  ││
│  │ │Semen │Pasir │Bata  │Cat   ││    │ │...       │ ...  │...   │... │  ││
│  │ │      │      │Merah │      ││    │ └──────────┴──────┴──────┴────┘  ││
│  │ ├──────┼──────┼──────┼──────┤│    │                                   ││
│  │ │Paku  │Palu  │Kayu  │Pipa  ││    │ Subtotal:       Rp 325.000       ││
│  │ └──────┴──────┴──────┴──────┘│    │ Diskon:         Rp 0             ││
│  │                                │    │ PPN 11%:        Rp 35.750       ││
│  │ ┌────────────────────────────┐│    │ ═══════════════════════════════  ││
│  │ │ Grid Produk (scroll)       ││    │ Total:          Rp 360.750       ││
│  │ │                            ││    │                                   ││
│  │ │ ┌────┐ ┌────┐ ┌────┐      ││    │              [Bayar]              ││
│  │ │ │Semen│ │Pasir│ │Bata │    ││    │                                   ││
│  │ │ │55rb │ │25rb │ │1.200│    ││    │              [Simpan Draft]      ││
│  │ │ └────┘ └────┘ └────┘      ││    └───────────────────────────────────┘│
│  │ │ ┌────┐ ┌────┐ ┌────┐      ││                                        │
│  │ │ │Cat │ │Paku │ │Palu │    ││                                        │
│  │ │ │120 │ │5rb  │ │8rb  │    ││                                        │
│  │ │ └────┘ └────┘ └────┘      ││                                        │
│  │ └────────────────────────────┘│                                        │
│  └──────────────────────────────┘                                        │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│ [F1] Favorit  [F2] Cari  [F3] Pelanggan  [F4] Diskon  [F5] Riwayat         │
│                                       Jumlah Item: 7 | Total: Rp 360.750    │
└──────────────────────────────────────────────────────────────────────────────┘
```

- **Split Panel:** Left 60% produk grid / Right 40% cart
- **Product Grid:** Category tabs di atas, grid produk di bawah
- **Cart Panel:** Daftar item, subtotal, diskon, total, tombol bayar
- **Top Bar:** Toko active, kasir, status sync
- **Bottom Bar:** Quick action keys untuk keyboard shortcut (desktop)

## 2. Data Structure (Mock Only)

```typescript
// === Product (POS) ===
interface PosProduct {
  id: string; // e.g. "PRD-001"
  barcode: string; // e.g. "8991234567890"
  name: string; // e.g. "Semen Tiga Roda 50kg"
  category: string; // e.g. "Material Bangunan"
  unit: string; // e.g. "sak", "pcs", "kg"
  price: number; // harga jual
  cost: number; // harga pokok (untuk hitung margin)
  stock: number;
  isActive: boolean;
  imageUrl?: string;
  isFavorite: boolean;
  displayOrder: number;
}

// === Cart / Transaction ===
interface CartItem {
  productId: string;
  productName: string;
  quantity: number;
  unit: string;
  price: number; // harga satuan
  discount: number; // diskon per item (nominal)
  subtotal: number; // (price - discount) * qty
  note?: string;
}

interface Transaction {
  id: string; // e.g. "TRX-2025-03-0001"
  receiptNumber: string; // e.g. "INV-20250315-001"
  storeId: string;
  cashierId: string;
  cashierName: string;
  customerId?: string;
  customerName?: string;
  items: CartItem[];
  subtotal: number;
  discountTotal: number;
  tax: number;
  grandTotal: number;
  paymentMethod: 'cash' | 'debit' | 'credit' | 'qris' | 'ewallet' | 'mixed';
  paymentDetails: PaymentDetail[];
  amountPaid: number; // jumlah dibayar
  changeAmount: number; // kembalian
  status: 'completed' | 'voided' | 'refunded';
  voidReason?: string;
  createdAt: string; // ISO date
  syncedAt?: string;
}

interface PaymentDetail {
  method: 'cash' | 'debit' | 'credit' | 'qris' | 'ewallet';
  amount: number;
  referenceNumber?: string; // untuk debit/kredit/QRIS
  cardLast4?: string;
}

// === Customer ===
interface Customer {
  id: string; // e.g. "CUST-001"
  name: string; // e.g. "Budi Santoso"
  phone: string; // e.g. "081234567890"
  email?: string;
  address?: string;
  totalPurchases: number; // total transaksi
  totalSpent: number; // total nominal
  lastPurchase: string; // ISO date
  createdAt: string;
  loyaltyPoints: number;
  notes?: string;
}

// === Shift / Cash Management ===
interface CashierShift {
  id: string;
  cashierId: string;
  storeId: string;
  openedAt: string;
  closedAt?: string;
  openingBalance: number;
  closingBalance?: number;
  expectedBalance?: number;
  totalCashSales: number;
  totalNonCashSales: number;
  totalTransactions: number;
  status: 'open' | 'closed';
  notes?: string;
}

// === Store ===
interface Store {
  id: string;
  name: string; // e.g. "Toko Sinar Jaya - Cabang Menteng"
  address: string;
  phone: string;
  taxId: string; // NPWP
  receiptFooter: string; // teks footer struk
  currency: string; // IDR
  timezone: string; // "Asia/Jakarta"
  printerConfig?: {
    printerName: string;
    paperSize: '58mm' | '80mm';
    copies: number;
  };
}
```

## 3. Implementation Details

**Stack:** Vite + React 18 + TypeScript + Tailwind CSS 3 + Lucide React + shadcn/ui + Framer Motion

- **POS-Specific:** Keyboard shortcut support (F1-F12), barcode scanner input
- **Offline-capable:** Queue transactions offline, sync when online
- **Printer Support:** WebUSB / Bluetooth untuk thermal printer (ESC/POS)
- **Sound:** Audio feedback for scan success, payment complete
- **Payment Integration:** Midtrans API untuk debit/credit/QRIS

## 4. Styling & Theming

**Palette:**
- Primary: `#F97316` (orange) — energik, retail
- Primary Light: `#FB923C`
- Primary Dark: `#EA580C`
- Accent: `#2563EB` (blue) — untuk tombol bayar
- Success: `#22C55E` — item added, payment success
- Background: `#F8FAFC`
- Surface: `#FFFFFF`
- Cart Panel: `#FEF9C3` (kuning soft) — highlight area transaksi
- Text: `#1E293B` / `#64748B`
- Price: `#DC2626` (merah) — harga coret/diskon
- Border: `#E2E8F0`

**Typography:**
- Font: Inter (sans-serif)
- Harga: text-xl (20px) hingga text-3xl (30px) bold
- Nama produk: text-sm
- Struk: font-mono untuk tampilan receipt
- Nominal besar: tracking-tight

**Spacing & Layout:**
- POS mode: 100vh, no scroll (kecuali grid produk)
- Cart panel: sticky right
- Product grid: 4-6 kolom tergantung layar
- Tombol produk: min-h-24, rounded-xl

## 5. UI Components Specification

### Product Grid
```
┌──────────────────────────────────────┐
│ [Semua] [Material] [Alat] [Cat] ...  │ — kategori tabs (horizontal scroll)
├──────────────────────────────────────┤
│ ┌────────┐ ┌────────┐ ┌────────┐    │
│ │ Semen  │ │ Pasir  │ │ Bata   │    │ 4 x N grid
│ │ Tiga   │ │ Beton  │ │ Merah  │    │
│ │ Roda   │ │        │ │        │    │
│ │ Rp55rb │ │ Rp25rb │ │Rp1.200 │    │
│ │ [Stok] │ │ [Stok] │ │ [Stok] │    │
│ └────────┘ └────────┘ └────────┘    │
│ ┌────────┐ ┌────────┐ ┌────────┐    │
│ │ Cat    │ │ Paku   │ │ Palu   │    │
│ │ Nippon │ │ 5cm    │ │ Besi   │    │
│ │ Paint  │ │        │ │        │    │
│ │ Rp120rb│ │ Rp5rb  │ │ Rp8rb  │    │
│ └────────┘ └────────┘ └────────┘    │
└──────────────────────────────────────┘
```

- Tap produk → Tambah ke cart (default qty 1)
- Tap & hold → Tampilkan qty picker
- Stok rendah: badge merah "Sisa 3"
- Harga: format Rp

### Cart Panel
```
┌───────────────────────────────────────┐
│ 🛒 Pesanan                    [Kosong]│
│ Pelanggan: [Umum ▾]                 │
├───────────────────────────────────────┤
│ ┌──────────────────────────────┐     │
│ │ Semen Tiga Roda 50kg    [X] │     │
│ │ Rp55.000  [−] 5 [+] 275.000│     │
│ │                           │     │
│ │ Pasir Beton           [X] │     │
│ │ Rp25.000  [−] 2 [+]  50.000│     │
│ └──────────────────────────────┘     │
│                                       │
│ Subtotal              Rp 325.000     │
│ Diskon                Rp 0           │
│ PPN 11%               Rp 35.750     │
│ ═══════════════════════════════════  │
│ Total              Rp 360.750        │
│                                       │
│ [💳 Bayar Rp 360.750]  (Primary btn) │
│                                       │
│ [📄 Draft] [🖨️ Struk] [❌ Batal]    │
└───────────────────────────────────────┘
```

### Payment Modal
```
┌────────────────────────────────────────────┐
│ 💳 Pembayaran                      [X]     │
│ Total: Rp 360.750                          │
├────────────────────────────────────────────┤
│ ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│ │ 💵 Tunai │ │ 💳 Debit │ │ 💳 Kredit│    │
│ └──────────┘ └──────────┘ └──────────┘    │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│ │ 📱 QRIS  │ │ GoPay    │ │ ShopePay │    │
│ └──────────┘ └──────────┘ └──────────┘    │
│                                            │
│ [Tunai — selected]                         │
│ Dibayar: [Rp 400.000          ]            │
│ Kembali: Rp 39.250                         │
│                                            │
│            [Konfirmasi Bayar]              │
└────────────────────────────────────────────┘
```

### Receipt Struk (Print Preview)
```
┌──────────────────────┐
│ TOKO SINAR JAYA      │
│ Menteng, Jakarta Pus │
│ NPWP: 01.234.567.8   │
├──────────────────────┤
│ Invoice: INV-001     │
│ Kasir: Andi          │
│ 15 Mar 2025 14:30    │
├──────────────────────┤
│ Semen TR   5 × 55.000│
│ Pasir      2 × 25.000│
│ ...                  │
├──────────────────────┤
│ Subtotal    325.000  │
│ PPN 11%     35.750   │
│ TOTAL      360.750   │
│ Tunai      400.000   │
│ Kembali     39.250   │
├──────────────────────┤
│ Terima kasih!        │
│ Barang yang sudah    │
│ dibeli tidak dapat   │
│ dikembalikan.        │
└──────────────────────┘
```

## 6. Interactions & States

### Loading States
- **Product grid:** Skeleton grid 3×4 card dengan pulse animation
- **Cart:** No loading — instant response (optimistic)
- **Payment processing:** Overlay spinner + "Memproses pembayaran..."
- **Printer:** Toast "Mencetak struk..." → "Struk tercetak"

### Empty States
- **No products:** "Belum ada produk. Tambahkan produk di menu Inventaris."
- **Empty cart:** Keranjang kosong ilustrasi + "Scan atau pilih produk untuk memulai"
- **No transaction history:** "Belum ada transaksi hari ini"
- **Search not found:** "Produk '{{BARCODE}}' tidak ditemukan"

### Error States
- **Scanner error:** Toast "Gagal membaca barcode. Coba lagi."
- **Payment failed:** "Pembayaran gagal. Kode: {{ERROR}}" + tombol "Coba Lagi"
- **Printer offline:** "Printer tidak terdeteksi. Periksa koneksi." + "Cetak Nanti"
- **Offline mode:** Banner "Mode offline — transaksi akan tersimpan lokal"
- **Stock insufficient:** "Stok tidak mencukupi. Tersedia: {{STOK}}"

### Edge Cases
- **Mixed payment:** Bayar dengan 2 metode (e.g., tunai 200rb + QRIS 160rb)
- **Split bill:** Pisahkan item ke beberapa bill (belum di MVP)
- **Void transaction:** Butuh password supervisor untuk void
- **Price override:** Harga special perlu otorisasi
- **Negative qty:** Tidak bisa input qty < 1
- **Scanner continuous:** Debounce barcode input (200ms) untuk mencegah duplikasi

## 7. Language & Localization

Bahasa Indonesia penuh dengan istilah retail:
- Labels: "Produk", "Kategori", "Jumlah", "Harga", "Subtotal", "Diskon", "Total"
- Payment: "Tunai", "Debit", "Kredit", "QRIS", "E-Wallet"
- Actions: "Bayar", "Cetak Struk", "Batal", "Simpan Draft", "Void"
- Status: "Lunas", "Batal", "Refund", "Draft"
- Messages: "Pembayaran berhasil", "Transaksi dibatalkan"
- Receipt: "Terima kasih telah berbelanja", "Barang yang sudah dibeli tidak dapat dikembalikan"

## 8. Example Mock Data

```typescript
const mockPosProducts: PosProduct[] = [
  { id: "PRD-001", barcode: "8991234567890", name: "Semen Tiga Roda 50kg", category: "Material", unit: "sak", price: 55000, cost: 45000, stock: 1250, isActive: true, isFavorite: true, displayOrder: 1 },
  { id: "PRD-002", barcode: "8991234567891", name: "Pasir Beton 1 Truk", category: "Material", unit: "truk", price: 2500000, cost: 1800000, stock: 8, isActive: true, isFavorite: true, displayOrder: 2 },
  { id: "PRD-003", barcode: "8991234567892", name: "Bata Merah", category: "Material", unit: "pcs", price: 1200, cost: 800, stock: 5000, isActive: true, isFavorite: false, displayOrder: 3 },
  { id: "PRD-004", barcode: "8991234567893", name: "Cat Nippon Paint 5kg", category: "Cat", unit: "kaleng", price: 120000, cost: 95000, stock: 45, isActive: true, isFavorite: true, displayOrder: 4 },
  { id: "PRD-005", barcode: "8991234567894", name: "Paku 5cm", category: "Alat", unit: "kg", price: 5000, cost: 3500, stock: 200, isActive: true, isFavorite: false, displayOrder: 5 },
  { id: "PRD-006", barcode: "8991234567895", name: "Palu Besi", category: "Alat", unit: "pcs", price: 8500, cost: 5500, stock: 60, isActive: true, isFavorite: false, displayOrder: 6 },
  { id: "PRD-007", barcode: "8991234567896", name: "Kayu Balok 4x6", category: "Kayu", unit: "batang", price: 45000, cost: 32000, stock: 300, isActive: true, isFavorite: false, displayOrder: 7 },
  { id: "PRD-008", barcode: "8991234567897", name: "Pipa PVC 3/4 inch", category: "Pipa", unit: "batang", price: 22000, cost: 16000, stock: 150, isActive: true, isFavorite: false, displayOrder: 8 },
];

const mockTransaction: Transaction = {
  id: "TRX-2025-03-0001",
  receiptNumber: "INV-20250315-001",
  storeId: "STORE-001",
  cashierId: "USER-001",
  cashierName: "Andi Pratama",
  customerId: "CUST-001",
  customerName: "Budi Santoso",
  items: [
    { productId: "PRD-001", productName: "Semen Tiga Roda 50kg", quantity: 5, unit: "sak", price: 55000, discount: 0, subtotal: 275000 },
    { productId: "PRD-002", productName: "Pasir Beton 1 Truk", quantity: 2, unit: "truk", price: 2500000, discount: 100000, subtotal: 4900000 },
  ],
  subtotal: 5175000,
  discountTotal: 100000,
  tax: 558250, // PPN 11%
  grandTotal: 5733250,
  paymentMethod: "mixed",
  paymentDetails: [
    { method: "cash", amount: 3000000 },
    { method: "qris", amount: 2733250 }
  ],
  amountPaid: 5733250,
  changeAmount: 0,
  status: "completed",
  createdAt: "2025-03-15T14:30:00Z"
};

const mockCustomer: Customer = {
  id: "CUST-001",
  name: "Budi Santoso",
  phone: "081234567890",
  email: "budi@email.com",
  address: "Jl. Merdeka No. 10, Jakarta",
  totalPurchases: 25,
  totalSpent: 48500000,
  lastPurchase: "2025-03-15T14:30:00Z",
  createdAt: "2024-06-01T00:00:00Z",
  loyaltyPoints: 1250,
};

const mockShift: CashierShift = {
  id: "SHIFT-2025-03-15-001",
  cashierId: "USER-001",
  storeId: "STORE-001",
  openedAt: "2025-03-15T07:00:00Z",
  closingBalance: undefined,
  openingBalance: 500000,
  totalCashSales: 3500000,
  totalNonCashSales: 5200000,
  totalTransactions: 18,
  status: "open",
};
```

## 9. Success Criteria

- [ ] Barcode scanning (keyboard wedge / camera) untuk input produk cepat
- [ ] Product grid dengan kategori filter dan pencarian
- [ ] Cart management (tambah, ubah qty, hapus item, diskon per item)
- [ ] Payment multi-metode (tunai, debit, kredit, QRIS, e-wallet)
- [ ] Generate receipt number otomatis
- [ ] Cetak struk thermal (ESC/POS) via WebUSB / Bluetooth
- [ ] Manajemen shift kasir (buka/tutup shift, setoran)
- [ ] Void transaksi (dengan password supervisor)
- [ ] Offline mode: transaksi tetap bisa jalan, sync saat online
- [ ] Riwayat transaksi dengan filter tanggal
- [ ] Performance: scan & add to cart < 200ms
- [ ] Customer management dengan loyalty points
