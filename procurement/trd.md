# TRD: PengadaanCerdas — Technical Requirements

## 1. Technical Strategy

**Architecture Pattern:** Feature-Sliced Design dengan 3 panel layout (sidebar + main content + detail panel). Arsitektur modular untuk PO management sebagai core domain.

**Key Decisions:**
- **3-Panel Layout:** Sidebar kiri (navigasi), panel tengah (list/content), panel kanan (detail/preview) -- optimal untuk B2B data-heavy apps
- **Multi-step form wizard:** PO creation dibagi menjadi 3 step (Info, Items, Review) dengan progress indicator
- **State machine untuk PO status:** Status PO diimplementasikan sebagai finite state machine untuk mencegah transisi invalid
- **Data persistence dengan Zustand + localStorage:** Data PO, supplier, approval disimpan di localStorage untuk persistensi tanpa backend

## 2. Tech Stack

| Layer | Teknologi | Versi | Alasan |
|-------|-----------|-------|--------|
| **Build Tool** | Vite | 5.x | Fast HMR, code-splitting |
| **Framework** | React | 18.x | Concurrent features, ecosystem |
| **Language** | TypeScript | 5.x | Type safety untuk data finansial |
| **Styling** | Tailwind CSS | 3.x | Utility-first, consistent design |
| **UI Library** | shadcn/ui | latest | Radix primitives, accessible |
| **Icons** | Lucide React | 0.3+ | Consistent, lightweight |
| **Animation** | Framer Motion | 11.x | Panel transitions, timeline animation |
| **Charts** | Recharts | 2.x | Dashboard spending charts |
| **Table** | @tanstack/react-table | 8.x | Sorting, filtering, pagination |
| **Form** | React Hook Form | 7.x | Multi-step forms |
| **Validation** | Zod | 3.x | Form + data validation |
| **Router** | React Router | 6.x | Nested routes, layout routes |
| **State (Client)** | Zustand | 4.x | Multiple stores per domain |
| **State (Server)** | TanStack Query | 5.x | Mock server state (future API) |
| **Date** | date-fns | 3.x | Locale id, formatting |
| **PDF** | jsPDF + autotable | 2.x | Cetak PO ke PDF |

## 3. Component Architecture

```
src/
├── app/
│   ├── App.tsx
│   ├── providers.tsx
│   └── router.tsx
├── shared/
│   ├── lib/
│   │   ├── utils.ts           # cn(), formatCurrency, generatePONumber
│   │   ├── validators.ts      # Zod schemas
│   │   ├── constants.ts       # Status list, department list
│   │   ├── mockData.ts        # Semua mock data terpusat
│   │   └── poMachine.ts       # Finite state machine untuk status PO
│   ├── ui/                    # shadcn/ui components
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── table.tsx
│   │   ├── card.tsx
│   │   ├── badge.tsx
│   │   ├── select.tsx
│   │   ├── dialog.tsx
│   │   ├── sheet.tsx          # Slide-over detail panel
│   │   ├── tabs.tsx
│   │   ├── stepper.tsx        # Multi-step indicator
│   │   ├── data-table.tsx     # Generic table wrapper
│   │   └── status-badge.tsx   # PO status with color
│   └── hooks/
│       ├── useDebounce.ts
│       └── useLocalStorage.ts
├── entities/
│   ├── purchase-order/
│   │   ├── types.ts
│   │   ├── store.ts           # Zustand store
│   │   ├── utils.ts           # PO calculations
│   │   └── machine.ts         # Status state machine
│   ├── supplier/
│   │   ├── types.ts
│   │   └── store.ts
│   ├── approval/
│   │   ├── types.ts
│   │   └── store.ts
│   └── contract/
│       ├── types.ts
│       └── store.ts
├── features/
│   ├── po-list/
│   │   ├── POList.tsx
│   │   ├── POFilter.tsx
│   │   ├── POStatusBadge.tsx
│   │   └── POActions.tsx
│   ├── po-form/
│   │   ├── POWizard.tsx          # Multi-step container
│   │   ├── StepInfo.tsx          # Step 1: info umum
│   │   ├── StepItems.tsx         # Step 2: items
│   │   ├── StepReview.tsx        # Step 3: review
│   │   ├── POItemRow.tsx
│   │   └── SupplierAutoComplete.tsx
│   ├── po-detail/
│   │   ├── PODetailPanel.tsx
│   │   ├── POInfo.tsx
│   │   ├── POItemTable.tsx
│   │   ├── ApprovalTimeline.tsx
│   │   └── POActionButtons.tsx
│   ├── approval/
│   │   ├── ApprovalQueue.tsx
│   │   ├── ApprovalCard.tsx
│   │   └── ApprovalDialog.tsx
│   ├── supplier/
│   │   ├── SupplierList.tsx
│   │   ├── SupplierForm.tsx
│   │   ├── SupplierCard.tsx
│   │   └── SupplierHistory.tsx
│   ├── receiving/
│   │   ├── ReceiveForm.tsx
│   │   └── ReceiveHistory.tsx
│   └── reports/
│       ├── ProcurementDashboard.tsx
│       ├── SpendingChart.tsx
│       ├── TopSuppliers.tsx
│       └── ExportButton.tsx
├── widgets/
│   ├── layout/
│   │   ├── AppLayout.tsx         # 3-panel layout
│   │   ├── Sidebar.tsx
│   │   ├── TopBar.tsx
│   │   ├── DetailPanel.tsx       # Right detail panel
│   │   └── StatusBar.tsx
│   └── dashboard/
│       ├── KPICards.tsx
│       ├── ApprovalWidget.tsx
│       └── SpendingOverview.tsx
└── pages/
    ├── DashboardPage.tsx
    ├── PurchaseOrdersPage.tsx
    ├── SuppliersPage.tsx
    ├── ApprovalPage.tsx
    ├── ReportsPage.tsx
    ├── SettingsPage.tsx
    └── NotFoundPage.tsx
```

## 4. Data Layer

**Zustand Stores:**
```typescript
// PO Store
interface POStore {
  purchaseOrders: PurchaseOrder[];
  filters: POFilter;
  selectedPOId: string | null;
  addPO(po: PurchaseOrder): void;
  updatePO(id: string, data: Partial<PurchaseOrder>): void;
  changeStatus(id: string, newStatus: POStatus): void;
  setFilter(filters: Partial<POFilter>): void;
  selectPO(id: string | null): void;
  getPOById(id: string): PurchaseOrder | undefined;
}

// Supplier Store
interface SupplierStore {
  suppliers: Supplier[];
  addSupplier(supplier: Supplier): void;
  updateSupplier(id: string, data: Partial<Supplier>): void;
  getSupplierById(id: string): Supplier | undefined;
}
```

**PO Status State Machine:**
```
States: draft, pending, approved, rejected, ordered, partial, received, cancelled

Transitions:
  draft -> pending     (submit)
  draft -> cancelled   (cancel)
  pending -> approved  (approve)
  pending -> rejected  (reject)
  pending -> draft     (revisi)
  approved -> ordered  (order to supplier)
  approved -> cancelled (cancel before order)
  ordered -> partial   (partial receive)
  ordered -> received  (full receive)
  ordered -> cancelled (cancel before delivery)
  partial -> received  (remaining received)
```

**Data Flow:**
```
User Action (UI) -> Zustand Store -> localStorage persist
                    -> State change -> UI re-render
                    -> Update status machine -> Validate transition
                    -> Log audit trail
```

## 5. Performance Requirements

| Metrik | Target |
|--------|--------|
| FCP | <2 detik |
| PO List render (500 items) | <1.5 detik |
| PO Form step transition | <300ms |
| Search across PO (500 items) | <500ms |
| Chart render | <1 detik |
| Panel transition (slide) | <200ms |
| Bundle size (gzip) | <200 KB |

**Optimasi:**
- Server-side pagination pattern (simulated dengan filter)
- Virtual scrolling untuk PO list (jika > 100 items)
- Memoized selectors di Zustand untuk derived data (total spending, counts)
- Lazy load Recharts untuk halaman reports
- Debounced search (300ms) untuk filter PO

## 6. Styling & Theming System

```css
/* styles/theme.css */
:root {
  /* Primary - Navy */
  --color-primary: #1E3A5F;
  --color-primary-light: #2D5A8E;
  --color-primary-dark: #142845;
  --color-primary-foreground: #FFFFFF;
  
  /* Accent - Sky Blue */
  --color-accent: #0EA5E9;
  --color-accent-foreground: #FFFFFF;
  
  /* Surfaces */
  --color-background: #F8FAFC;
  --color-surface: #FFFFFF;
  --color-surface-hover: #F1F5F9;
  
  /* Sidebar Dark */
  --sidebar-width: 280px;
  --sidebar-bg: #142845;
  --sidebar-text: #94A3B8;
  --sidebar-text-active: #FFFFFF;
  --sidebar-hover: rgba(255,255,255,0.06);
  --sidebar-active: rgba(255,255,255,0.1);
  
  /* Detail Panel */
  --detail-panel-width: 420px;
  --detail-panel-bg: #FFFFFF;
  
  /* Status Colors */
  --status-draft: #94A3B8;
  --status-pending: #F59E0B;
  --status-approved: #22C55E;
  --status-rejected: #EF4444;
  --status-ordered: #8B5CF6;
  --status-partial: #F97316;
  --status-received: #059669;
  --status-cancelled: #6B7280;
  
  /* Text */
  --color-text-primary: #1E293B;
  --color-text-secondary: #64748B;
  --color-text-tertiary: #94A3B8;
  
  /* Borders */
  --color-border: #E2E8F0;
  
  /* Typography */
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  
  /* Shadows */
  --shadow-card: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-elevated: 0 4px 12px rgba(0,0,0,0.1);
  --shadow-detail: -4px 0 12px rgba(0,0,0,0.08);
  
  /* Radius */
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
}
```

## 7. Validation Rules

| Field | Rule | Message |
|-------|------|---------|
| No. PO | Auto-generated, unique | "Nomor PO sudah ada (unexpected)" |
| Judul PO | Required, max 200 chars | "Judul PO wajib diisi" |
| Supplier | Required | "Supplier wajib dipilih" |
| Department | Required | "Departemen wajib dipilih" |
| Tanggal PO | Required, <= hari ini | "Tanggal PO tidak valid" |
| Nama Item | Required, max 100 chars | "Nama item wajib diisi" |
| Qty | Integer, > 0 | "Quantity harus lebih dari 0" |
| Unit Price | > 0 | "Harga satuan harus lebih dari 0" |
| Total | Sum(items.total) = grand total | "Total tidak match" |
| PPN | 11% of subtotal (default) | "Perhitungan PPN tidak valid" |
| Status transition | Must follow state machine | "Tidak bisa mengubah status dari {{CURRENT}} ke {{TARGET}}" |
| Approval | Harus sesuai level | "Anda tidak memiliki akses approval level ini" |

## 8. Error Handling Strategy

**Global Error Handler:**
```
Error Boundary (per page)
  -> 404 -> NotFoundPage
  -> Validation -> Inline field errors + toast
  -> State machine invalid -> "Status PO tidak bisa diubah"
  -> Duplicate submit -> "PO sudah disubmit sebelumnya"
  -> localStorage full -> "Penyimpanan penuh. Hapus data lama."
  -> Unknown error -> "Terjadi kesalahan. Hubungi support."
```

**Module-Specific Errors:**
- **PO Form:** Step tidak bisa lanjut jika required field belum diisi
- **Approval:** "PO ini sudah di-approve/ditolak oleh {{USER}}"
- **Supplier:** "NPWP sudah terdaftar dengan supplier {{SUPPLIER}}"
- **Receiving:** "Jumlah diterima melebihi quantity PO"
- **Delete:** "Tidak bisa hapus PO yang sudah disubmit"

**Data Recovery:**
- Auto-save draft PO ke localStorage setiap 30 detik
- Recovery prompt jika crash mid-form: "Ada draft PO yang belum selesai. Lanjutkan?"
- localStorage corruption detection dengan checksum

## 9. Security Considerations

| Concern | Mitigation |
|---------|-----------|
| **Role-based access** | 4 roles: Admin, Procurement, Manager, Director (simulasi) |
| **Approval integrity** | Approval tidak bisa diubah setelah dikonfirmasi |
| **Data tampering** | Audit trail dengan timestamp (client-side) |
| **Delete protection** | PO yang sudah disubmit tidak bisa dihapus, hanya di-cancel |
| **Form validation** | Client-side + Zod schema validation |
| **localStorage** | Tidak ada data sensitif (password, token) di localStorage |
| **CSRF (future)** | Token-based untuk API integration |

## 10. Deployment & Build

```bash
# Build
npm run build  # -> dist/

# Environment
VITE_APP_NAME=PengadaanCerdas
VITE_APP_VERSION=1.0.0

# Deploy targets
# Option 1: Vercel / Netlify (recommended)
# Option 2: Firebase Hosting
# Option 3: Docker + Nginx

# Docker (optional)
FROM nginx:alpine
COPY dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Build Optimasi:**
- Code splitting per page (lazy load)
- Vendor chunk (react, react-dom, react-router-dom)
- Data chunk (mock data hanya di-load saat development)

## 11. Testing Strategy

| Level | Tools | Target |
|-------|-------|--------|
| **Unit** | Vitest + RTL | Utils, validators, state machine |
| **Component** | Storybook | POForm, ApprovalTimeline, StatusBadge |
| **Integration** | Vitest | PO CRUD flow, approval workflow |
| **E2E** | Playwright | Create PO -> Submit -> Approve -> Receive |

**Critical Test Paths:**
1. Create PO -> Submit -> Approve -> Order -> Receive (full cycle)
2. Create PO -> Submit -> Reject (rejection flow)
3. Create PO -> Save Draft -> Edit -> Submit (draft flow)
4. Partial receive -> remaining receive (partial flow)
5. Filter PO by status, supplier, department

## 12. Dependencies & Constraints

**Dependencies:**
- Node.js 18+ for build
- Modern browser: Chrome 90+, Firefox 90+, Safari 15+, Edge 90+
- date-fns untuk formatting tanggal Indonesia

**Constraints:**
- Static SPA: SEO tidak kritikal (enterprise internal tool)
- Client-side data: Semua data di localStorage (kapasitas ~5-10MB)
- Single user: Multi-user hanya simulasi dengan role switching
- Responsive: Minimal 1024px width (optimal di 1366px+)
- No offline sync: Data tidak hilang karena localStorage persist

**Assumptions:**
- Backend REST API akan datang di fase berikutnya
- Data migrasi dari spreadsheet/Excel akan dilakukan via import tools
- User menggunakan device desktop/laptop (bukan mobile-first)
- Approver memiliki akses ke aplikasi via browser
- Font Inter sudah tersedia atau menggunakan system-ui fallback
