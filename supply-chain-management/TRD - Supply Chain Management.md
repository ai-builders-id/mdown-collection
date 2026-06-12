# TRD: SCM Pro — Technical Requirements

## 1. Technical Strategy
SCM Pro dibangun sebagai single-page application (SPA) berbasis React dengan arsitektur component-based. Backend menggunakan REST API (opsional di Phase 2). Untuk MVP, data disimpan di local storage sebagai mock, dengan struktur yang siap dipindahkan ke backend sungguhan. Prioritas utama adalah performa rendering data dense (tables + kanban) dan UX yang responsif di berbagai ukuran layar.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Framework | React | 18 | Vite sebagai build tool |
| Bahasa | TypeScript | 5 | Strict mode |
| Styling | Tailwind CSS | 3 | + daisyUI komponen |
| State | Zustand | 4 | Inventory & filter state |
| Data Fetching | TanStack React Query | 5 | Caching + pagination |
| Routing | React Router | 6 | Nested layout |
| Charts | Recharts | 2 | Grafik supply-demand |
| DnD | dnd-kit | latest | Kanban drag & drop |

## 3. Component Architecture (tree)

```
src/
├── components/
│   ├── layout/
│   │   ├── Sidebar.tsx          # Navigasi modul
│   │   ├── Header.tsx           # Search global + notif
│   │   └── MainLayout.tsx       # Layout wrapper
│   ├── inventory/
│   │   ├── InventoryTable.tsx   # Tabel produk dengan filter
│   │   ├── ProductForm.tsx      # Modal form add/edit produk
│   │   ├── StockBadge.tsx       # Indikator status stok
│   │   └── StockHistory.tsx     # Riwayat transaksi stok
│   ├── shipment/
│   │   ├── ShipmentKanban.tsx   # 4 kolom status
│   │   ├── ShipmentCard.tsx     # Card di kanban
│   │   └── TrackingTimeline.tsx # Timeline estimasi vs aktual
│   ├── warehouse/
│   │   ├── WarehouseGrid.tsx    # Visual layout gudang
│   │   └── ZoneCard.tsx         # Detail zona
│   ├── supplier/
│   │   └── SupplierCard.tsx
│   └── shared/
│       ├── DataTable.tsx        # Generic table komponen
│       ├── EmptyState.tsx       # Ilustrasi empty
│       ├── ErrorBanner.tsx      # Error state
│       └── LoadingSkeleton.tsx
├── stores/
│   ├── inventoryStore.ts        # Zustand store
│   └── filterStore.ts           # Filter & search state
├── hooks/
│   ├── useProducts.ts           # React Query hook
│   └── useShipments.ts
├── types/
│   └── index.ts                 # TypeScript interfaces
├── data/
│   └── mockData.ts              # Mock data Indonesia
└── utils/
    ├── formatters.ts            # Currency, date, status
    └── validators.ts
```

## 4. Data Layer
- **MVP**: Zustand store dengan initial data dari mockData.ts; state persist di localStorage
- **Production**: REST API dengan endpoint `/api/v1/scm/*`, database PostgreSQL
- **Caching**: React Query cache 5 menit untuk data inventory
- **Pagination**: Server-side pagination untuk tabel > 100 baris
- **Real-time**: WebSocket untuk update status pengiriman (Phase 2)

## 5. Performance Requirements
- Waktu render tabel inventory (5000 baris): < 1 detik dengan virtual scrolling
- Kanban drag-drop response: < 100ms
- Search inventory: debounce 300ms, hasil dalam 200ms
- Bundle size: < 300KB (gzipped)
- Lighthouse Performance score: > 85

## 6. Styling & Theming System (CSS vars)

```css
:root {
  --scm-primary: #1e3a5f;
  --scm-primary-light: #2d5a8e;
  --scm-surface: #ffffff;
  --scm-surface-card: #f8fafc;
  --scm-text: #1e293b;
  --scm-text-muted: #64748b;
  --scm-border: #e2e8f0;
  --scm-success: #16a34a;
  --scm-warning: #f59e0b;
  --scm-danger: #dc2626;
  --scm-info: #0ea5e9;
  --sidebar-width: 260px;
  --header-height: 64px;
  --kanban-card-width: 280px;
}

.dark {
  --scm-surface: #0f172a;
  --scm-surface-card: #1e293b;
  --scm-text: #f1f5f9;
  --scm-text-muted: #94a3b8;
  --scm-border: #334155;
}
```

## 7. Validation Rules
- SKU: Alphanumeric + dash, 3-20 karakter, unique
- Harga beli: Number, > 0, maks 999.999.999
- Stok: Integer non-negatif, maks 999.999
- Nomor Resi: Format "SCM-YYYY-MM/XXX" otomatis generate
- Tanggal estimasi: Tidak boleh di masa lalu saat create
- Nama produk: 3-100 karakter, tidak boleh hanya spasi
- Email supplier: Format email valid

## 8. Error Handling Strategy
- **Network error**: React Query retry 3x exponential backoff; fallback ke cache terakhir
- **Validation error**: Inline error message di form field (merah)
- **Concurrent edit**: Optimistic update + rollback jika conflict
- **Server error (5xx)**: Toast "Gagal menyimpan data" + log ke console
- **Not found (404)**: Redirect ke halaman 404 dengan tombol "Kembali ke Dashboard"
- **Rate limit**: Queue request + notifikasi "Terlalu banyak permintaan"

## 9. Security Considerations
- XSS: Semua input user di-sanitize sebelum render
- Role-based access (Admin Gudang, Supervisor, Viewer)
- CSRF token untuk semua POST/PUT/DELETE (saat backend terhubung)
- Rate limiting: 100 request/menit per user
- Logout otomatis setelah 30 menit idle
- Validasi otorisasi di setiap operasi mutasi data

## 10. Deployment & Build
- Build command: `npm run build` → output di `/dist`
- Deploy target: Vercel / Netlify untuk SPA; Docker untuk self-hosted
- Environment variables: `VITE_API_URL`, `VITE_APP_NAME`
- CI/CD: GitHub Actions → lint + type-check + build → deploy
- Analitik: Vercel Analytics atau Plausible

## 11. Testing Strategy
- **Unit**: Vitest untuk pure functions (validators, formatters)
- **Component**: React Testing Library untuk render + interaction
- **E2E**: Playwright untuk flow utama (CRUD produk, kanban drag)
- **Coverage target**: > 80% untuk utils & hooks; > 60% untuk komponen

## 12. Dependencies & Constraints
- **Dependencies**: React 18+, Tailwind v3+, Node 18+
- **Constraint**: Mobile-first tidak prioritas utama — optimasi dashboard di layar > 1024px
- **Constraint**: Tidak menggunakan library UI heavy (MUI, Ant Design) — prioritaskan bundle size
- **Constraint**: Backend belum tersedia di MVP — semua data mock + localStorage
- **Asumsi**: User menggunakan browser modern (Chrome/Firefox/Edge 2 tahun terakhir)
