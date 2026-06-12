# TRD: Point of Sales — Technical Requirements

**Project:** Point of Sales (KasirCerdas)
**Version:** 1.0

---

## 1. Technical Strategy

KasirCerdas dibangun sebagai **static frontend single-page application (SPA)** khusus POS dengan arsitektur **offline-first**. Seluruh logika bisnis berjalan di sisi klien tanpa backend dependency. Data disimpan di **localStorage** dengan struktur yang sudah ditentukan, dan state dikelola menggunakan React Context + useReducer untuk memastikan responsivitas tinggi saat transaksi.

### Prinsip Arsitektur

- **Offline-first:** Semua fitur (transaksi, CRUD produk, laporan) berfungsi tanpa koneksi internet. Sinkronisasi ke server opsional dan tidak ada di MVP.
- **Optimistic UI:** Setiap aksi (tambah item ke cart, bayar) langsung tercermin di UI tanpa menunggu proses async.
- **POS-centric Layout:** Layout dua panel (produk grid 60% / cart 40%) dengan fixed top-bar dan bottom-bar, tanpa scrolling di panel utama — hanya grid produk yang scroll.
- **Keyboard-first:** Dukungan shortcut keyboard (F1-F12, Enter, Escape, arrow keys) untuk mempercepat input kasir.
- **Mock-as-real:** Data mock disimpan dalam file TypeScript dengan struktur dan volume yang realistis (50+ produk, ribuan transaksi) untuk menguji performa.

```text
[UI Component] --> [React Context + useReducer] --> [localStorage Persistence]
       |                        |                            |
       v                        v                            v
[Input Events]          [State Changes]               [Data Recovery]
```

---

## 2. Tech Stack

| Layer | Technology | Keterangan |
|-------|-----------|------------|
| **Framework** | Vite 5 + React 18 + TypeScript 5 | Build cepat, HMR instan, strict typing |
| **Routing** | React Router DOM v6 | Route-based pages: /pos, /products, /reports, /settings |
| **Styling** | Tailwind CSS 3.4 | Utility-first dengan custom design tokens |
| **UI Library** | shadcn/ui (Radix primitives) | Komponen aksesibel: Dialog, Dropdown, Select, Button, Badge |
| **Icons** | Lucide React | Icons konsisten untuk actions POS |
| **Animations** | Framer Motion | Transisi cart, skeleton loading, page transitions |
| **Charts** | Recharts | Laporan penjualan harian/bulanan (bar chart, line chart) |
| **State Management** | React Context + useReducer | State global untuk cart, aplikasi, dan auth mock |
| **Data Persistence** | localStorage (serialize JSON) | Cart draft, riwayat transaksi, data produk, shift |
| **Barcode** | Keyboard wedge scanner (native) | Input dari barcode scanner USB — tidak perlu library tambahan |
| **Sound** | Web Audio API / HTMLAudioElement | Feedback suara untuk scan berhasil, bayar sukses |
| **Printer** | WebUSB / Web Serial (opsional) | Cetak struk thermal ESC/POS |
| **Linting** | ESLint + Prettier | Kode konsisten, error prevention |
| **Testing** | Vitest + React Testing Library | Unit test + component test |

### Node & Package Manager

| Requirement | Minimum |
|-------------|---------|
| Node.js | 18.x LTS atau lebih baru |
| npm / yarn / pnpm | npm 9+ / yarn 1.22+ / pnpm 8+ |

---

## 3. Component Architecture

### Struktur Folder Lengkap

```text
src/
|
|-- components/                    # Shared/global components
|   |-- ui/                        # shadcn/ui primitives
|   |   |-- button.tsx
|   |   |-- dialog.tsx
|   |   |-- badge.tsx
|   |   |-- input.tsx
|   |   |-- select.tsx
|   |   |-- toast.tsx
|   |   |-- skeleton.tsx
|   |   |-- card.tsx
|   |   |-- separator.tsx
|   |   |-- table.tsx
|   |   `-- tabs.tsx
|   |-- layout/
|   |   |-- AppLayout.tsx          # Layout utama dengan sidebar + topbar
|   |   |-- PosLayout.tsx          # Layout khusus POS (fullscreen, 2 panel)
|   |   |-- TopBar.tsx             # Store info, waktu, user
|   |   |-- Sidebar.tsx            # Navigasi menu
|   |   `-- BottomBar.tsx          # Quick actions bar (F1-F12)
|   |-- common/
|   |   |-- LoadingSpinner.tsx
|   |   |-- EmptyState.tsx
|   |   |-- ErrorBoundary.tsx
|   |   |-- ConfirmDialog.tsx
|   |   `-- OfflineBanner.tsx
|
|-- features/                      # Feature-based modules
|   |-- pos/                       # === MODUL POS KASIR ===
|   |   |-- components/
|   |   |   |-- ProductGrid.tsx    # Grid produk dengan kategori tabs
|   |   |   |-- ProductCard.tsx    # Card produk (nama, harga, stok)
|   |   |   |-- SearchBar.tsx      # Pencarian produk + barcode
|   |   |   |-- CategoryTabs.tsx   # Filter kategori horizontal
|   |   |   |-- CartPanel.tsx      # Panel keranjang (kanan)
|   |   |   |-- CartItem.tsx       # Item di keranjang (qty, subtotal)
|   |   |   |-- CartSummary.tsx    # Ringkasan: subtotal, diskon, PPN, total
|   |   |   |-- PaymentModal.tsx   # Modal pembayaran (pilih metode, input nominal)
|   |   |   |-- PaymentMethodGrid.tsx # Grid metode bayar (tunai, debit, QRIS, dll)
|   |   |   |-- ChangeDisplay.tsx  # Tampilan kembalian
|   |   |   |-- ReceiptPreview.tsx # Preview struk sebelum cetak
|   |   |   |-- CustomerSelect.tsx # Pilih pelanggan (dropdown)
|   |   |   |-- DiscountInput.tsx  # Input diskon per item / per transaksi
|   |   |   `-- QuickKeyGuide.tsx  # Panduan shortcut keyboard
|   |   |-- hooks/
|   |   |   |-- useCart.ts         # State + logic keranjang
|   |   |   |-- useBarcode.ts      # Barcode scanner input handler
|   |   |   |-- useProductSearch.ts # Search + filter produk
|   |   |   |-- usePayment.ts      # Flow pembayaran
|   |   |   `-- usePrinter.ts      # Cetak struk (WebUSB/Bluetooth)
|   |   |-- context/
|   |   |   `-- CartContext.tsx     # Context provider untuk cart state
|   |   |-- utils/
|   |   |   |-- cartCalculations.ts # Hitung subtotal, diskon, PPN, total
|   |   |   `-- receiptFormatter.ts # Format struk thermal
|   |   `-- index.ts
|   |
|   |-- products/                  # === MODUL MANAJEMEN PRODUK ===
|   |   |-- components/
|   |   |   |-- ProductList.tsx    # Tabel daftar produk
|   |   |   |-- ProductForm.tsx    # Form tambah/edit produk
|   |   |   |-- ProductTable.tsx   # Table dengan sorting & filter
|   |   |   `-- CategoryManager.tsx # Kelola kategori
|   |   |-- hooks/
|   |   |   `-- useProducts.ts    # CRUD produk
|   |   `-- utils/
|   |       `-- productValidation.ts
|   |
|   |-- customers/                 # === MODUL PELANGGAN ===
|   |   |-- components/
|   |   |   |-- CustomerList.tsx
|   |   |   `-- CustomerForm.tsx
|   |   `-- hooks/
|   |       `-- useCustomers.ts
|   |
|   |-- transactions/              # === MODUL RIWAYAT TRANSAKSI ===
|   |   |-- components/
|   |   |   |-- TransactionList.tsx
|   |   |   |-- TransactionDetail.tsx
|   |   |   `-- VoidTransactionDialog.tsx
|   |   `-- hooks/
|   |       `-- useTransactions.ts
|   |
|   |-- reports/                   # === MODUL LAPORAN ===
|   |   |-- components/
|   |   |   |-- DailyReport.tsx
|   |   |   |-- MonthlyReport.tsx
|   |   |   |-- SalesChart.tsx     # Recharts line/bar chart
|   |   |   |-- TopProducts.tsx    # Produk terlaris
|   |   |   |-- StockReport.tsx    # Laporan stok (habis/menipis)
|   |   |   |-- PaymentSummary.tsx # Per metode pembayaran
|   |   |   `-- ReportFilter.tsx   # Filter tanggal
|   |   `-- hooks/
|   |       `-- useReports.ts
|   |
|   |-- shifts/                    # === MODUL SHIFT KASIR ===
|   |   |-- components/
|   |   |   |-- ShiftPanel.tsx
|   |   |   |-- ShiftHistory.tsx
|   |   |   `-- ShiftSummary.tsx
|   |   `-- hooks/
|   |       `-- useShift.ts
|   |
|   `-- settings/                  # === MODUL PENGATURAN ===
|       |-- components/
|       |   |-- StoreSettings.tsx   # Informasi toko
|       |   |-- PrinterSettings.tsx # Konfigurasi printer thermal
|       |   |-- UserSettings.tsx    # Manajemen user (kasir/admin)
|       |   `-- TaxSettings.tsx     # Pengaturan PPN
|       `-- hooks/
|           `-- useSettings.ts
|
|-- data/                          # Mock data files
|   |-- products.ts                # 50+ produk mock
|   |-- transactions.ts            # Transaksi sample (100+)
|   |-- customers.ts               # 20+ pelanggan mock
|   |-- categories.ts              # Kategori produk
|   |-- shifts.ts                  # Data shift sample
|   `-- stores.ts                  # Data toko
|
|-- types/                         # TypeScript interfaces
|   |-- index.ts                   # Export all types
|   |-- product.ts
|   |-- transaction.ts
|   |-- customer.ts
|   |-- cart.ts
|   |-- shift.ts
|   `-- store.ts
|
|-- hooks/                         # Global hooks
|   |-- useLocalStorage.ts         # Generic localStorage hook
|   |-- useDebounce.ts             # Debounce untuk search & barcode
|   |-- useKeyboard.ts             # Keyboard shortcut handler
|   `-- useMediaQuery.ts           # Responsive breakpoints
|
|-- lib/                           # Utilities
|   |-- utils.ts                   # Utility functions (cn, formatCurrency, dll)
|   |-- constants.ts               # Konstanta global (PPN, dll)
|   |-- formatters.ts              # Format currency, date, phone
|   `-- idGenerator.ts             # Generate ID: invoice, produk, transaksi
|
|-- context/                       # Global context providers
|   |-- AppContext.tsx              # App-level state (theme, sidebar)
|   |-- AuthContext.tsx             # Mock auth (current user, role)
|   `-- SettingsContext.tsx         # Settings global (toko, preferensi)
|
|-- pages/                         # Page-level components
|   |-- PosPage.tsx                # Halaman utama POS
|   |-- ProductsPage.tsx           # Manajemen produk
|   |-- CustomersPage.tsx          # Manajemen pelanggan
|   |-- TransactionsPage.tsx       # Riwayat transaksi
|   |-- ReportsPage.tsx            # Laporan
|   |-- ShiftsPage.tsx             # Manajemen shift
|   `-- SettingsPage.tsx           # Pengaturan
|
|-- App.tsx                        # Root component + routing
|-- main.tsx                       # Entry point
`-- index.css                      # Global styles + CSS custom properties
```

### Hierarki Komponen POS (Flow Transaksi)

```text
PosPage
 |-- PosLayout (fullscreen)
      |-- TopBar (toko, kasir, waktu, status)
      |-- ProductGridPanel (left 60%)
      |   |-- CategoryTabs
      |   |-- SearchBar
      |   `-- ProductGrid
      |       `-- ProductCard (xN)
      |-- CartPanel (right 40%)
      |   |-- CustomerSelect
      |   |-- CartItem (xN)
      |   |   |-- qty controls
      |   |   `-- discount input
      |   |-- CartSummary
      |   |   |-- subtotal
      |   |   |-- discount
      |   |   |-- PPN 11%
      |   |   `-- grand total
      |   `-- ActionButtons (Bayar, Draft, Batal)
      |-- BottomBar (F1-F12 shortcuts)
      `-- PaymentModal (overlay)
           |-- PaymentMethodGrid
           |-- AmountInput + ChangeDisplay
           `-- ConfirmButton
```

---

## 4. Data Layer

### 4.1 State Management

| State | Pendekatan | Scope | Persistence |
|-------|-----------|-------|-------------|
| **Cart** | useReducer + CartContext | POS page | localStorage draft |
| **Products** | useReducer + Context | Global | localStorage |
| **Transactions** | useReducer + Context | Global | localStorage |
| **Customers** | Context | Global | localStorage |
| **Auth (mock)** | AuthContext | Global | sessionStorage |
| **Settings** | Context | Global | localStorage |
| **Shift** | Context + useReducer | Per session | localStorage |
| **UI State** | useState lokal | Per component | Tidak persist |

### 4.2 Cart Reducer (useReducer)

```typescript
type CartAction =
  | { type: 'ADD_ITEM'; payload: { product: PosProduct; quantity: number } }
  | { type: 'REMOVE_ITEM'; payload: { productId: string } }
  | { type: 'UPDATE_QUANTITY'; payload: { productId: string; quantity: number } }
  | { type: 'SET_DISCOUNT_ITEM'; payload: { productId: string; discount: number } }
  | { type: 'SET_DISCOUNT_TOTAL'; payload: { discount: number } }
  | { type: 'SET_CUSTOMER'; payload: { customerId?: string; customerName?: string } }
  | { type: 'SET_NOTES'; payload: { note: string } }
  | { type: 'CLEAR_CART' }
  | { type: 'LOAD_DRAFT'; payload: CartState };

interface CartState {
  items: CartItem[];
  customerId?: string;
  customerName?: string;
  discountTotal: number;
  notes: string;
}
```

### 4.3 Mock Data Strategy

- **Volume realistik:** 50+ produk, 20+ pelanggan, 100+ transaksi sample, 5 kategori
- **Format data sesuai PRD:** Mengikuti interface PosProduct, Transaction, Customer, CashierShift, Store
- **Data toko default:** "Toko Sinar Jaya" dengan NPWP dan konfigurasi printer
- **Seed data:** Saat pertama load (localStorage kosong), aplikasi akan seed data default dari file mock
- **ID generator:** Nomor invoice auto-generate format `INV-YYYYMMDD-XXX`, ID produk format `PRD-XXX`, ID transaksi `TRX-YYYYMM-XXXX`

### 4.4 localStorage Persistence Strategy

| Key | Data | Ukuran Estimasi | Update Trigger |
|-----|------|----------------|----------------|
| `kc_products` | Array produk | ~50 KB | CRUD produk |
| `kc_transactions` | Array transaksi | ~500 KB (1000 tx) | Setelah bayar |
| `kc_customers` | Array pelanggan | ~10 KB | CRUD pelanggan |
| `kc_categories` | Array kategori | ~2 KB | Kelola kategori |
| `kc_settings` | Settings toko | ~5 KB | Update settings |
| `kc_shifts` | Riwayat shift | ~20 KB | Buka/tutup shift |
| `kc_cart_draft` | Cart draft terakhir | ~5 KB | Auto-save cart (debounce 500ms) |
| `kc_last_sync` | Timestamp sync | ~0,1 KB | Setiap transaksi |

**Catatan:** Total estimasi penggunaan localStorage < 1 MB untuk 1000 transaksi — aman dalam batas localStorage (5-10 MB per origin).

### 4.5 Data Flow Transaksi

```text
[Tap ProductCard] --> dispatch ADD_ITEM --> CartContext update state
                                              |
                                              v
                    [localStorage auto-save draft (debounced 500ms)]
                                              |
                    [User klik Bayar] --> dispatch CLEAR_CART
                                              |
                                              v
                    [PaymentModal] --> [Konfirmasi]
                                              |
                                              v
                    [Simpan Transaction] --> [Update stok localStorage]
                                              |
                                              v
                    [Cetak struk] --> [ReceiptPreview / WebUSB Printer]
```

---

## 5. Performance Requirements

| Kriteria | Target | Notes |
|----------|--------|-------|
| **First Contentful Paint (FCP)** | < 1,5 detik | Kritis — POS dipakai di produksi |
| **Time to Interactive (TTI)** | < 3 detik | Cold start, pertama load |
| **Cart Operation (add/remove/qty)** | < 50 ms | Harus instant — optimistic UI |
| **Search Response** | < 100 ms | Filter produk dari 50+ item |
| **Payment Processing** | < 200 ms | Hitung total, simpan ke localStorage |
| **Bundle Size (gzipped)** | < 300 KB | Vite code splitting otomatis |
| **Largest Contentful Paint (LCP)** | < 2 detik | Product grid harus cepat muncul |
| **Cumulative Layout Shift (CLS)** | < 0,1 | Hindari layout shift saat skeleton ke real |

### Strategi Optimasi

- **Code splitting:** React.lazy + Suspense untuk tiap page (/pos, /products, /reports, dll)
- **Dynamic import untuk komponen berat:** PaymentModal, ReceiptPreview, Charts
- **Virtual scrolling** jika grid produk > 100 item (react-window opsional)
- **Debounce search** (300ms) dan barcode input (200ms) untuk mencegah re-render berlebihan
- **Memoization:** React.memo untuk ProductCard, CartItem; useMemo untuk perhitungan subtotal
- **LocalStorage batch write** — tidak write tiap state change, tapi debounce 500ms
- **Image lazy loading** untuk gambar produk (jika ada)

---

## 6. Styling & Theming System

### 6.1 Design Tokens — CSS Custom Properties

Didefinisikan di `index.css` dan dikonsumsi oleh Tailwind via `tailwind.config.ts`.

```css
:root {
  /* === Primary: Orange (Retail, Energik) === */
  --color-primary-50: #FFF7ED;
  --color-primary-100: #FFEDD5;
  --color-primary-200: #FED7AA;
  --color-primary-300: #FDBA74;
  --color-primary-400: #FB923C;
  --color-primary-500: #F97316;     /* Primary utama */
  --color-primary-600: #EA580C;
  --color-primary-700: #C2410C;
  --color-primary-800: #9A3412;
  --color-primary-900: #7C2D12;

  /* === Accent: Blue (untuk tombol bayar, aksi utama) === */
  --color-accent-500: #2563EB;
  --color-accent-600: #1D4ED8;

  /* === Semantic Colors === */
  --color-success: #22C55E;         /* Berhasil, stok ok */
  --color-warning: #F59E0B;         /* Stok menipis, peringatan */
  --color-danger: #DC2626;          /* Error, stok habis, void */
  --color-info: #3B82F6;            /* Informasi */

  /* === Neutral / Background === */
  --color-bg: #F8FAFC;              /* Background utama */
  --color-surface: #FFFFFF;         /* Card, panel, modal */
  --color-surface-alt: #F1F5F9;    /* Background alternatif */
  --color-cart-panel: #FEF9C3;     /* Cart panel — kuning soft highlight */
  --color-border: #E2E8F0;
  --color-border-light: #F1F5F9;

  /* === Text === */
  --color-text-primary: #1E293B;    /* Teks utama */
  --color-text-secondary: #64748B;  /* Teks sekunder, label */
  --color-text-muted: #94A3B8;      /* Placeholder, disabled */
  --color-text-on-primary: #FFFFFF;  /* Teks di atas primary */
  --color-text-price: #DC2626;       /* Harga coret/diskon */

  /* === Typography === */
  --font-family-base: 'Inter', system-ui, -apple-system, sans-serif;
  --font-family-mono: 'JetBrains Mono', 'Fira Code', monospace; /* Struk */

  --font-size-xs: 0,75rem;    /* 12px — label, meta */
  --font-size-sm: 0,875rem;   /* 14px — body text, nama produk */
  --font-size-base: 1rem;     /* 16px — default */
  --font-size-lg: 1,125rem;   /* 18px */
  --font-size-xl: 1,25rem;    /* 20px — harga produk */
  --font-size-2xl: 1,5rem;    /* 24px — subtotal */
  --font-size-3xl: 1,875rem;  /* 30px — grand total */

  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;

  --line-height-tight: 1,25;
  --line-height-normal: 1,5;

  --letter-spacing-tight: -0,025em;
  --letter-spacing-normal: 0;

  /* === Spacing Scale === */
  --spacing-0: 0px;
  --spacing-1: 0,25rem;    /* 4px */
  --spacing-2: 0,5rem;     /* 8px */
  --spacing-3: 0,75rem;    /* 12px */
  --spacing-4: 1rem;       /* 16px */
  --spacing-5: 1,25rem;    /* 20px */
  --spacing-6: 1,5rem;     /* 24px */
  --spacing-8: 2rem;       /* 32px */
  --spacing-10: 2,5rem;    /* 40px */
  --spacing-12: 3rem;      /* 48px */

  /* === Border Radius === */
  --radius-sm: 0,375rem;   /* 6px */
  --radius-md: 0,5rem;     /* 8px */
  --radius-lg: 0,75rem;    /* 12px */
  --radius-xl: 1rem;       /* 16px — produk card */
  --radius-full: 9999px;

  /* === Shadows === */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0,05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0,1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0,1);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0,1);

  /* === Layout Spesifik POS === */
  --pos-topbar-height: 56px;       /* Top bar */
  --pos-bottombar-height: 48px;    /* Bottom bar */
  --pos-sidebar-width: 240px;      /* Sidebar navigasi */
  --pos-cart-width: 40%;           /* Cart panel (persentase) */
  --pos-min-product-card: 140px;   /* Minimum card width */
  --pos-max-product-card: 180px;   /* Maximum card width */
}
```

### 6.2 Tailwind Configuration

```typescript
// tailwind.config.ts
export default {
  content: ['./index.html', './src/**/*.{ts,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: {
          50:  '#FFF7ED',  100: '#FFEDD5',  200: '#FED7AA',
          300: '#FDBA74',  400: '#FB923C',  500: '#F97316',
          600: '#EA580C',  700: '#C2410C',  800: '#9A3412',
          900: '#7C2D12',
        },
        accent: { 500: '#2563EB', 600: '#1D4ED8' },
        success: { DEFAULT: '#22C55E', light: '#BBF7D0' },
        warning: { DEFAULT: '#F59E0B', light: '#FDE68A' },
        danger:  { DEFAULT: '#DC2626', light: '#FECACA' },
        cart:    { DEFAULT: '#FEF9C3', border: '#FDE68A' },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
        mono: ['JetBrains Mono', 'Fira Code', 'monospace'],
      },
      fontSize: {
        'price-xl': ['1,25rem', { lineHeight: '1,75rem', fontWeight: '700' }],
        'price-2xl': ['1,5rem', { lineHeight: '2rem', fontWeight: '700' }],
        'price-3xl': ['1,875rem', { lineHeight: '2,25rem', fontWeight: '800' }],
      },
      borderRadius: {
        'product': '1rem',
      },
      minHeight: {
        'product-card': '6rem',
      },
      gridTemplateColumns: {
        'products': 'repeat(auto-fill, minmax(140px, 1fr))',
      },
    },
  },
  plugins: [require('tailwindcss-animate')],
};
```

### 6.3 Dark Mode (Persiapan)

Menggunakan `class` strategy (`darkMode: 'class'` di Tailwind). Di MVP dark mode belum aktif, tapi CSS vars sudah siap dengan prefix `dark:`.

### 6.4 Responsive Breakpoints

| Breakpoint | Target Device | Layout Behavior |
|-----------|--------------|-----------------|
| `sm: 640px` | HP kecil | Single column (cart di bawah) |
| `md: 768px` | Tablet portrait | 2 panel lebih sempit |
| `lg: 1024px` | Tablet landscape / Desktop | 2 panel penuh |
| `xl: 1280px` | Desktop lebar | Grid produk 5-6 kolom |

**Device utama:** Tablet 10 inch (landscape) -> layout lg breakpoint.

---

## 7. Validation Rules

### 7.1 Aturan Validasi Data

| Field | Aturan | Pesan Error |
|-------|--------|-------------|
| **Nama Produk** | Required, min 3 karakter, max 100 | "Nama produk harus diisi (min. 3 karakter)" |
| **Harga Jual** | Required, number, > 0 | "Harga jual harus lebih dari Rp 0" |
| **Harga Pokok** | Optional, jika diisi harus > 0 | "Harga pokok harus lebih dari Rp 0" |
| **Stok** | Required, number, >= 0 | "Stok tidak boleh negatif" |
| **Barcode** | Optional, unique (jika diisi), numeric, max 20 digit | "Barcode sudah terdaftar" / "Format barcode tidak valid" |
| **Nama Pelanggan** | Required, min 2 karakter | "Nama pelanggan harus diisi" |
| **No. Telepon** | Optional, format: 08xxxxxxxxxx atau +62xxxxxxxxxx | "Format nomor telepon tidak valid" |
| **Quantity di Cart** | Integer, min 1, max 999, tidak boleh melebihi stok | "Stok tidak mencukupi. Tersedia: {stok}" |
| **Nominal Bayar (Tunai)** | Required, number, >= grandTotal | "Nominal bayar kurang Rp {selisih}" |
| **Nominal Bayar (Non-tunai)** | Required, number, harus === grandTotal | "Nominal bayar harus sama dengan total" |
| **Diskon Item** | Number, 0 <= diskon <= harga * qty | "Diskon tidak boleh melebihi subtotal item" |
| **Diskon Total** | Number, 0 <= diskon <= grandTotal | "Diskon tidak boleh melebihi grand total" |
| **PPN** | Boolean toggle, rate 11% | — |
| **Password Supervisor (Void)** | Required, harus cocok dengan password supervisor | "Password supervisor salah" |
| **Nama Toko** | Required, max 100 karakter | "Nama toko harus diisi" |

### 7.2 Validasi Stok Real-time

Saat item ditambahkan ke cart, sistem langsung cek:
```
if (product.stock - currentQtyInCart <= 0) -> tampilkan toast warning
if (product.stock - currentQtyInCart <= product.minStockAlert) -> badge "Stok: {sisa}"
```

### 7.3 Format Data

| Tipe Data | Format | Contoh |
|-----------|--------|--------|
| **Currency** | IDR (Rp), no decimal | `Rp 55.000`, `Rp 1.250.000` |
| **Tanggal** | DD/MM/YYYY atau DD Month YYYY | `15/03/2025` atau `15 Maret 2025` |
| **Jam** | HH:mm (24 jam) | `14:30` |
| **Timestamp** | ISO 8601 untuk penyimpanan | `2025-03-15T14:30:00.000Z` |
| **Invoice Number** | INV-YYYYMMDD-XXX | `INV-20250315-001` |
| **Product ID** | PRD-XXX (auto-increment) | `PRD-001` |
| **Transaction ID** | TRX-YYYYMM-XXXX | `TRX-202503-0001` |
| **Customer ID** | CUST-XXX | `CUST-001` |
| **Nomor Telepon** | 08xxxxxxxxxx (disimpan tanpa spasi) | `081234567890` |

---

## 8. Error Handling Strategy

### 8.1 Global Error Boundary

```typescript
// ErrorBoundary.tsx — React error boundary
// Menangkap error dari komponen anak
// Tampilkan fallback UI + tombol "Muat Ulang"
// Log error ke console
```

**Fallback UI:**
```text
+-------------------------------+
|  Warning Terjadi Kesalahan    |
|  Maaf, terjadi kesalahan yang |
|  tidak terduga.               |
|  [Muat Ulang Aplikasi]        |
+-------------------------------+
```

### 8.2 Error State per Fitur

| Skenario | Visual | Aksi |
|----------|--------|------|
| **Barcode tidak terbaca** | Toast error "Gagal membaca barcode. Coba lagi." | User scan ulang |
| **Produk tidak ditemukan** | Empty state "Produk '{query}' tidak ditemukan." + saran | User coba keyword lain |
| **Stok habis saat checkout** | Toast + item di-cart marked "Stok tidak cukup" | User kurangi qty / hapus |
| **Pembayaran gagal (simulasi)** | Toast "Pembayaran gagal. Silakan coba lagi." | User klik "Coba Lagi" |
| **Printer tidak terdeteksi** | Warning dialog "Printer tidak terdeteksi. Periksa koneksi." + tombol "Cetak Nanti" | User bisa skip cetak |
| **localStorage penuh** | Alert "Penyimpanan penuh. Hapus beberapa transaksi lama." | User hapus data lama lewat Settings |
| **Data rusak (parse error)** | Error boundary + reload data dari seed default | User reload atau reset data |
| **Cart hilang (refresh)** | Cart di-recovery dari localStorage draft (auto 30 detik) | Auto — tidak perlu aksi |
| **Shift belum dibuka** | Warning "Shift belum dibuka. Buka shift terlebih dahulu." | User buka shift |
| **Transaksi void** | Confirm dialog + input password supervisor | User input password |

### 8.3 Toast Notification System

Menggunakan shadcn/ui toast component:

| Level | Durasi | Contoh |
|-------|--------|--------|
| **Success** | 3 detik | "Produk berhasil ditambahkan" |
| **Error** | 5 detik | "Gagal membaca barcode" |
| **Warning** | 4 detik | "Stok menipis: Semen (sisa 3)" |
| **Info** | 3 detik | "Mode offline - transaksi disimpan lokal" |

### 8.4 Network Error Simulation (Opsional)

Untuk development, tambahkan toggle di settings untuk simulasi offline mode:
- Nonaktifkan localStorage write
- Tampilkan offline banner
- Queue transaksi (tapi di MVP semua sync opsional)

### 8.5 Data Recovery

- **Seed data default:** Saat localStorage kosong (first load), aplikasi akan seed data dari `src/data/*.ts`
- **Reset data:** Settings halaman memiliki tombol "Reset ke Data Default" dengan konfirmasi
- **Export data:** Opsional - download localStorage sebagai JSON

---

## 9. Security Considerations

### 9.1 Mock Authentication

```typescript
// AuthContext.tsx — mock sederhana
interface AuthUser {
  id: string;       // "USER-001"
  username: string; // "andi"
  name: string;     // "Andi Pratama"
  role: 'cashier' | 'admin' | 'supervisor';
  storeId: string;
}

// Mock users (hardcoded)
const MOCK_USERS: AuthUser[] = [
  { id: 'USER-001', username: 'andi',   name: 'Andi Pratama',   role: 'cashier',   storeId: 'STORE-001' },
  { id: 'USER-002', username: 'sari',   name: 'Sari Dewi',      role: 'cashier',   storeId: 'STORE-001' },
  { id: 'USER-003', username: 'hendra', name: 'Hendra Gunawan', role: 'admin',     storeId: 'STORE-001' },
  { id: 'USER-004', username: 'super',  name: 'Supervisor',     role: 'supervisor',storeId: 'STORE-001' },
];
```

### 9.2 Role-Based Access (Simulasi)

| Fitur / Aksi | Cashier | Admin | Supervisor |
|-------------|---------|-------|------------|
| Input transaksi | Ya | Ya | Ya |
| Void transaksi | Tidak | Ya (butuh password) | Ya |
| CRUD produk | Tidak | Ya | Ya |
| CRUD pelanggan | Ya | Ya | Ya |
| Laporan harian | Ya (own shift) | Ya (all) | Ya (all) |
| Manajemen shift | Ya (buka/tutup own) | Ya | Ya |
| Settings toko | Tidak | Ya | Ya |
| Reset data | Tidak | Tidak | Ya |
| Price override | Tidak | Tidak | Ya |
| Manajemen user | Tidak | Tidak | Ya |

### 9.3 Input Sanitization

- **XSS Prevention:** Semua input user (nama produk, nama pelanggan, catatan) di-render via React JSX (auto-escaped). Tidak ada `dangerouslySetInnerHTML`.
- **Numeric validation:** Pastikan input number benar-benar number, parse dengan `Number()` atau `parseInt()` dengan fallback.
- **localStorage injection:** Parse JSON dengan try-catch; jika gagal, gunakan data default.

### 9.4 Void Transaction Security

- Void membutuhkan password supervisor (default: `super123` - hanya untuk mock)
- Password disimpan di localStorage (hash sederhana untuk mock)
- Log void: siapa, kapan, alasan void

### 9.5 Catatan Keamanan

> **Peringatan:** Aplikasi ini murni frontend dengan data lokal. Tidak ada enkripsi data di localStorage. Jangan gunakan untuk data sensitif riil. Semua password adalah mock dan hardcoded.

---

## 10. Deployment & Build

### 10.1 Build Configuration (Vite)

```typescript
// vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
  build: {
    outDir: 'dist',
    sourcemap: false,
    chunkSizeWarningLimit: 300,
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom', 'react-router-dom'],
          ui: ['@radix-ui/react-dialog', '@radix-ui/react-select', '@radix-ui/react-tabs'],
          charts: ['recharts'],
        },
      },
    },
  },
  server: {
    port: 3000,
    host: true,
  },
});
```

### 10.2 Environment Variables

```env
# .env — tidak ada API key, murni frontend
VITE_APP_NAME=KasirCerdas
VITE_APP_VERSION=1.0.0
VITE_DEFAULT_STORE=Toko Sinar Jaya
VITE_TAX_RATE=11
VITE_CURRENCY=IDR
VITE_LOCALE=id-ID
```

### 10.3 Deployment Targets

| Platform | Command | Output |
|----------|---------|--------|
| **Development** | `npm run dev` | Vite dev server (port 3000) |
| **Production Build** | `npm run build` | `dist/` folder (static files) |
| **Preview Build** | `npm run preview` | Preview production build lokal |

### 10.4 Hosting (Static)

| Platform | Konfigurasi | Notes |
|----------|------------|-------|
| **Vercel** | Auto-detect Vite, deploy dari `dist/` | SPA fallback untuk routing |
| **Netlify** | `_redirects`: `/* /index.html 200` | SPA fallback |
| **GitHub Pages** | `npm run build` -> deploy `dist/` | Butuh `--base=/repo-name/` |
| **Surge.sh** | `surge dist/` | Sederhana untuk demo |

### 10.5 Service Worker (Offline Cache — Masa Depan)

Di MVP belum ada Service Worker. Untuk offline penuh:
- Cache static assets (Vite PWA plugin - `vite-plugin-pwa`)
- Cache data dari localStorage (sudah offline-native)

---

## 11. Testing Strategy

### 11.1 Testing Stack

| Level | Tools | Target Coverage |
|-------|-------|-----------------|
| **Unit** | Vitest | > 70% |
| **Component** | Vitest + React Testing Library | Semua komponen kritis POS |
| **Integration** | React Testing Library | Flow transaksi end-to-end |
| **Visual (opsional)** | Storybook | Komponen UI untuk dokumentasi |

### 11.2 Unit Tests (Vitest)

```typescript
// Targets:
// - cartCalculations.ts: subtotal, diskon, PPN 11%, grandTotal, kembalian
// - formatters.ts: formatCurrency, formatDate, formatPhone
// - idGenerator.ts: generate invoice number, product ID
// - productValidation.ts: semua aturan validasi produk
// - useLocalStorage.ts: read, write, error handling
```

**Minimum test cases:**
```typescript
describe('cartCalculations', () => {
  it('menghitung subtotal dengan benar (qty x price)');
  it('menghitung subtotal dengan diskon item');
  it('menghitung PPN 11% dari subtotal');
  it('menghitung grand total = subtotal - diskon + ppn');
  it('menghitung kembalian = bayar - grandTotal');
  it('melempar error jika bayar < grandTotal');
  it('menghitung mixed payment dengan 2 metode');
});

describe('productValidation', () => {
  it('validasi nama produk required');
  it('validasi harga jual > 0');
  it('validasi stok >= 0');
  it('validasi barcode unique');
  it('validasi quantity cart tidak melebihi stok');
});
```

### 11.3 Component Tests (React Testing Library)

```typescript
// Priority components to test:
describe('ProductGrid', () => {
  it('menampilkan grid produk sesuai kategori');
  it('filter produk berdasarkan kategori');
  it('search produk berdasarkan nama');
  it('tambah produk ke cart via klik');
});

describe('CartPanel', () => {
  it('menampilkan daftar item di cart');
  it('mengubah quantity (+/-)');
  it('menghapus item dari cart');
  it('menampilkan subtotal, diskon, PPN, total dengan benar');
  it('disabled tombol bayar jika cart kosong');
});

describe('PaymentModal', () => {
  it('menampilkan grand total');
  it('hitung kembalian otomatis saat input nominal');
  it('validasi nominal bayar >= total');
  it('konfirmasi bayar sukses -> transaksi tersimpan');
});

describe('CartItem', () => {
  it('menampilkan nama produk, harga, qty, subtotal');
  it('klik + menambah qty');
  it('klik - mengurangi qty (min 1)');
  it('klik X menghapus item');
  it('menampilkan stok warning jika qty >= stok');
});
```

### 11.4 Integration Test — Flow Transaksi Lengkap

```typescript
describe('Complete POS Transaction Flow', () => {
  it('flow lengkap: pilih produk -> cart -> bayar -> transaksi tersimpan', async () => {
    // 1. Render PosPage dengan mock produk
    // 2. Klik product card -> item masuk cart
    // 3. Ubah quantity
    // 4. Klik Bayar
    // 5. Pilih metode Tunai
    // 6. Input nominal bayar
    // 7. Konfirmasi -> transaksi sukses
    // 8. Cart kosong
    // 9. Cek localStorage transaksi bertambah
    // 10. Cek stok produk berkurang
  });
});
```

### 11.5 Storybook (Opsional)

Komponen yang direkomendasikan untuk Storybook:
- `ProductCard` — default, selected, out-of-stock, low-stock states
- `EmptyState` — berbagai pesan dan ilustrasi
- `PaymentModal` — berbagai metode pembayaran
- `ReceiptPreview` — template struk

---

## 12. Dependencies & Constraints

### 12.1 Production Dependencies

| Package | Version | Fungsi |
|---------|---------|--------|
| `react` | ^18.3.1 | UI library |
| `react-dom` | ^18.3.1 | React DOM rendering |
| `react-router-dom` | ^6.26.x | Client-side routing |
| `@radix-ui/react-dialog` | ^1.1.x | Modal aksesibel (shadcn/ui base) |
| `@radix-ui/react-select` | ^2.1.x | Dropdown aksesibel |
| `@radix-ui/react-tabs` | ^1.1.x | Tabs aksesibel |
| `@radix-ui/react-toast` | ^1.2.x | Toast notifications |
| `@radix-ui/react-badge` | ^1.1.x | Badge / status indicator |
| `class-variance-authority` | ^0.7.x | Variant management (shadcn/ui) |
| `clsx` | ^2.1.x | Conditional classes |
| `tailwind-merge` | ^2.5.x | Merge Tailwind classes |
| `lucide-react` | ^0.441.x | Icons |
| `framer-motion` | ^11.11.x | Animasi |
| `recharts` | ^2.12.x | Charts laporan |
| `tailwindcss-animate` | ^1.0.x | Tailwind animation utilities |

### 12.2 Dev Dependencies

| Package | Version | Fungsi |
|---------|---------|--------|
| `typescript` | ^5.5.x | Type checking |
| `vite` | ^5.4.x | Build tool |
| `@vitejs/plugin-react` | ^4.3.x | Vite React plugin |
| `tailwindcss` | ^3.4.x | CSS framework |
| `postcss` | ^8.4.x | CSS processor |
| `autoprefixer` | ^10.4.x | CSS vendor prefixes |
| `eslint` | ^8.57.x | Linter |
| `@typescript-eslint/eslint-plugin` | ^7.x | TypeScript ESLint rules |
| `prettier` | ^3.3.x | Code formatter |
| `vitest` | ^2.0.x | Unit test runner |
| `@testing-library/react` | ^16.x | Component testing |
| `@testing-library/jest-dom` | ^6.5.x | DOM matchers |
| `@testing-library/user-event` | ^14.5.x | User event simulation |
| `jsdom` | ^24.x | DOM environment for tests |

### 12.3 Constraints

| Aspek | Constraint |
|-------|-----------|
| **Backend** | Tidak ada backend. Semua data lokal. Tidak ada REST API, GraphQL, atau database server. |
| **API Key** | Tidak ada API key atau integrasi pihak ketiga yang membutuhkan autentikasi. |
| **Autentikasi** | Mock only. Password hardcoded. Tidak ada enkripsi. |
| **Data Storage** | Hanya localStorage (max 5-10 MB per origin). Untuk toko dengan >5000 transaksi perlu strategi archiving. |
| **Browser Support** | Chrome 100+, Firefox 100+, Safari 15+, Edge 100+ (2 versi terakhir dari tiap browser). |
| **Offline Mode** | Semua fitur harus berfungsi tanpa koneksi internet. Tidak ada Service Worker di MVP - data dari localStorage. |
| **Printer** | WebUSB / Web Serial hanya didukung Chrome/Chromium (termasuk Android WebView). Tidak support Safari/Firefox. |
| **Barcode Scanner** | Input dari keyboard wedge scanner (otomatis ketik di input). Tidak perlu library tambahan di MVP. |
| **Responsive** | Layout utama adalah tablet 10" landscape. Desktop dan HP adalah secondary target. |
| **Node.js** | Minimum Node.js 18.x LTS untuk development. |
| **Package Manager** | npm, yarn, atau pnpm — bebas. |
| **No SSR** | Pure static SPA. Tidak pakai Next.js, Remix, atau SSR framework lainnya. |

### 12.4 Browser Feature Requirements

| Fitur | API | Fallback |
|-------|-----|----------|
| **Data storage** | `localStorage` | — (wajib ada) |
| **WebUSB (printer)** | `navigator.usb` | Simpan PDF / cetak browser default |
| **Web Serial (printer)** | `navigator.serial` | Simpan PDF / cetak browser default |
| **Audio feedback** | `HTMLAudioElement` / Web Audio API | Silent mode |
| **Clipboard** | `navigator.clipboard` | Fallback select manual |
| **Fullscreen** | `Element.requestFullscreen()` | Layout standard |

### 12.5 File `package.json` (Ringkasan Scripts)

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src/ --ext .ts,.tsx",
    "format": "prettier --write src/",
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage"
  }
}
```
