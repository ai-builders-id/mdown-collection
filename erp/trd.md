# TRD: ERP Nusantara — Technical Requirements

## 1. Technical Strategy

**Architecture Pattern:** Modular monolith frontend dengan Feature-Sliced Design. Setiap modul (Finance, Inventory, Sales, Purchasing, HR) adalah folder independen dengan types, API, store, dan components sendiri. Code-splitting per modul untuk bundle optimasi.

**Key Decisions:**
- **Micro-Frontend Ready:** Struktur folder modular memungkinkan pemisahan menjadi micro-frontend di masa depan
- **Shared Libraries:** Semua modul share komponen UI (shadcn/ui), hooks, utils, dan types dasar
- **Cross-Module Communication:** Zustand store untuk event bus antar modul (e.g., SO created → inventory update)
- **Server-Side Pagination:** Semua tabel data menggunakan server-side pagination untuk skalabilitas

## 2. Tech Stack

| Layer | Teknologi | Versi | Alasan |
|-------|-----------|-------|--------|
| **Build Tool** | Vite | 5.x | Fast HMR, code-splitting |
| **Framework** | React | 18.x | Ecosystem, concurrent mode |
| **Language** | TypeScript | 5.x | Type safety |
| **Styling** | Tailwind CSS | 3.x | Utility-first, consistent |
| **UI Library** | shadcn/ui | latest | Radix primitives, accessible |
| **Icons** | Lucide React | 0.3+ | Lightweight, consistent |
| **Animation** | Framer Motion | 11.x | Page transitions, layout animations |
| **Charts** | Recharts | 2.x | Declarative, composable |
| **Table** | @tanstack/react-table | 8.x | Headless, virtual scrolling |
| **Form** | React Hook Form | 7.x | Performance, minimal re-render |
| **Validation** | Zod | 3.x | Runtime validation |
| **Router** | React Router | 6.x | Nested routes, data loading |
| **State (Client)** | Zustand | 4.x | Lightweight, no boilerplate |
| **State (Server)** | TanStack Query | 5.x | Caching, deduplication |
| **Date** | date-fns | 3.x | Tree-shakeable, locale id |
| **PDF** | jsPDF + autotable | 2.x | Client-side PDF |
| **Excel** | SheetJS (xlsx) | 0.20+ | Client-side XLSX export |

## 3. Component Architecture

```
src/
├── app/
│   ├── App.tsx
│   ├── providers.tsx
│   └── router.tsx
├── shared/
│   ├── lib/
│   │   ├── utils.ts           # cn, formatCurrency, formatDate
│   │   ├── validators.ts      # Zod schemas
│   │   ├── constants.ts
│   │   ├── permissions.ts     # RBAC helpers
│   │   └── api-client.ts      # Axios instance
│   ├── ui/                    # shadcn/ui components
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── table.tsx
│   │   ├── card.tsx
│   │   ├── dialog.tsx
│   │   ├── select.tsx
│   │   ├── tabs.tsx
│   │   ├── badge.tsx
│   │   ├── sheet.tsx          # Slide-over panel
│   │   ├── command.tsx        # Command palette
│   │   └── data-table.tsx     # Generic data table wrapper
│   └── hooks/
│       ├── useDebounce.ts
│       ├── usePagination.ts
│       └── usePermission.ts
├── modules/                   # Feature modules per domain
│   ├── finance/
│   │   ├── types.ts
│   │   ├── api.ts
│   │   ├── store.ts
│   │   ├── components/
│   │   │   ├── ChartOfAccounts/
│   │   │   ├── JournalEntry/
│   │   │   ├── GeneralLedger/
│   │   │   ├── TrialBalance/
│   │   │   ├── AccountsReceivable/
│   │   │   ├── AccountsPayable/
│   │   │   └── Reports/
│   │   └── pages/
│   │       ├── FinanceDashboardPage.tsx
│   │       ├── JournalPage.tsx
│   │       └── ReportPage.tsx
│   ├── inventory/
│   │   ├── types.ts
│   │   ├── api.ts
│   │   ├── store.ts
│   │   ├── components/
│   │   │   ├── ProductList/
│   │   │   ├── ProductForm/
│   │   │   ├── StockMovement/
│   │   │   ├── StockCard/
│   │   │   └── StockOpname/
│   │   └── pages/
│   │       ├── InventoryPage.tsx
│   │       └── ProductDetailPage.tsx
│   ├── sales/
│   │   ├── types.ts
│   │   ├── api.ts
│   │   ├── components/
│   │   │   ├── SalesOrderList/
│   │   │   ├── SalesOrderForm/
│   │   │   ├── CustomerList/
│   │   │   └── InvoiceView/
│   │   └── pages/
│   │       ├── SalesPage.tsx
│   │       └── CustomerPage.tsx
│   ├── purchasing/
│   │   ├── types.ts
│   │   ├── api.ts
│   │   ├── components/
│   │   │   ├── PurchaseOrderList/
│   │   │   ├── PurchaseOrderForm/
│   │   │   ├── GoodsReceipt/
│   │   │   └── SupplierList/
│   │   └── pages/
│   ├── hr/
│   │   ├── types.ts
│   │   ├── api.ts
│   │   ├── components/
│   │   │   ├── EmployeeList/
│   │   │   ├── EmployeeForm/
│   │   │   ├── Attendance/
│   │   │   ├── LeaveRequest/
│   │   │   └── Payroll/
│   │   └── pages/
│   └── reports/
│       ├── types.ts
│       ├── api.ts
│       ├── components/
│       │   ├── ReportBuilder/
│       │   └── ReportViewer/
│       └── pages/
├── widgets/
│   ├── sidebar/
│   │   ├── Sidebar.tsx
│   │   ├── MenuGroup.tsx
│   │   └── CompanySwitcher.tsx
│   ├── topbar/
│   │   ├── TopBar.tsx
│   │   ├── GlobalSearch.tsx
│   │   ├── NotificationCenter.tsx
│   │   └── UserMenu.tsx
│   └── dashboard/
│       ├── ExecutiveDashboard.tsx
│       ├── KpiCard.tsx
│       ├── ActivityFeed.tsx
│       ├── QuickActions.tsx
│       └── ChartWidget.tsx
└── pages/
    ├── DashboardPage.tsx
    ├── SettingsPage.tsx
    └── NotFoundPage.tsx
```

## 4. Data Layer

**Cross-Module Data Flow:**
```
Sales Order (confirmed)
  → Zustand event: 'sales:order-confirmed'
    → Inventory listener: kurangi stok
    → Finance listener: create journal (AR + Revenue)
    → UI listener: show notification
```

**Caching Strategy per Modul:**
- **Master Data** (produk, customer, supplier): cache 10 menit, stale-while-revalidate
- **Transactional Data** (SO, PO, jurnal): cache 30 detik, invalidate on mutation
- **Dashboard Aggregates**: cache 2 menit, re-fetch on tab focus
- **Reports**: no cache (always fresh), loading skeleton

**State Ownership:**
- **Zustand:** Active company, active period, sidebar state, filter states, notification queue
- **TanStack Query:** All server data (products, orders, employees, transactions)
- **URL:** Current page, search params, pagination state

## 5. Performance Requirements

| Metrik | Target |
|--------|--------|
| FCP | <2 detik |
| LCP | <3 detik |
| Time to Interactive | <4 detik |
| Modul Switch | <1 detik (code-splitting) |
| Table render (1.000 rows) | <1 detik (virtual scroll) |
| Dashboard load | <3 detik |
| Mutation response (save) | <1 detik |
| Bundle size per modul | <100 KB gzip |
| Lighthouse Performance | >85 |

**Optimasi Spesifik:**
- **Virtual scrolling** untuk inventory (mungkin >10.000 SKU) menggunakan @tanstack/react-virtual
- **Lazy load module chunks** — masing-masing modul di-code-split per route (e.g., `import('./modules/finance/pages/JournalPage')`)
- **Debounced search** (500ms) untuk global search
- **Pre-fetch** modul tetangga saat hover di sidebar
- **Persisted cache** untuk master data di localStorage

## 6. Styling & Theming System

```css
/* styles/theme.css */
:root {
  /* Primary enterprise blue */
  --color-primary: #0F4C81;
  --color-primary-light: #2A6DB5;
  --color-primary-dark: #0A355B;
  --color-primary-foreground: #FFFFFF;
  
  /* Accent */
  --color-accent: #00B4D8;
  --color-accent-foreground: #FFFFFF;
  
  /* Surfaces */
  --color-background: #F0F4F8;
  --color-surface: #FFFFFF;
  --color-surface-hover: #E8EEF4;
  
  /* Sidebar (dark) */
  --sidebar-width: 280px;
  --sidebar-bg: #0A355B;
  --sidebar-text: #A8C8E8;
  --sidebar-text-active: #FFFFFF;
  --sidebar-hover: rgba(255,255,255,0.08);
  --sidebar-active: rgba(255,255,255,0.12);
  --sidebar-border: rgba(255,255,255,0.08);
  
  /* Text */
  --color-text-primary: #1E293B;
  --color-text-secondary: #64748B;
  --color-text-tertiary: #94A3B8;
  
  /* Semantic */
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-error: #EF4444;
  --color-info: #3B82F6;
  
  /* Status badges */
  --status-draft-bg: #F1F5F9;
  --status-draft-text: #64748B;
  --status-confirmed-bg: #DBEAFE;
  --status-confirmed-text: #1D4ED8;
  --status-shipped-bg: #FEF3C7;
  --status-shipped-text: #B45309;
  --status-delivered-bg: #D1FAE5;
  --status-delivered-text: #065F46;
  --status-cancelled-bg: #FEE2E2;
  --status-cancelled-text: #991B1B;
  
  /* Typography */
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  
  /* Spacing & Radius */
  --spacing-card: 20px;
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  
  /* Shadows */
  --shadow-card: 0 1px 2px rgba(0,0,0,0.06);
  --shadow-elevated: 0 4px 12px rgba(0,0,0,0.1);
  --shadow-modal: 0 8px 30px rgba(0,0,0,0.15);
}
```

## 7. Validation Rules

| Modul | Field | Rule |
|-------|-------|------|
| **Finance** | Debit/Kredit Jurnal | Total debit = total kredit |
| **Finance** | Nominal | > 0, max 999.999.999.999 |
| **Finance** | Periode | Harus open/active |
| **Inventory** | SKU | Unique, max 50 chars |
| **Inventory** | Harga Jual | >= purchase price × 1.15 |
| **Inventory** | Stok min/max | min < max |
| **Sales** | Qty SO | <= stock available |
| **Sales** | Diskon | <= 30% (tanpa approval) |
| **Sales** | Harga | Must have price > 0 |
| **Purchasing** | Qty PO | > 0 |
| **HR** | NIK KTP | 16 digit numeric |
| **HR** | Gaji Pokok | >= UMR {{KOTA}} |
| **HR** | Cuti | <= 12 day/year, H-3 submission |
| **General** | Tanggal | Valid range, not in future (for transactions) |

## 8. Error Handling Strategy

**Global Error Handler:**
```
Error Boundary (per module)
  → 401 → Redirect to login
  → 403 → "Anda tidak memiliki akses ke modul ini"
  → 404 → Show "Data tidak ditemukan" page
  → Network Error → Retry button + "Periksa koneksi internet"
  → Validation Error → Inline field messages
  → Business Logic Error → Toast with detail + action
  → Unknown Error → Toast "Terjadi kesalahan" + support contact
```

**Module-Specific Errors:**
- **Finance**: Balance not zero → block submit + highlight imbalance
- **Inventory**: Stock negative → block + "Stok tidak mencukupi"
- **Sales**: Customer credit limit exceeded → warning + approval required
- **Purchasing**: Duplicate PO → "PO dengan supplier ini masih pending"
- **HR**: Attendance overlap → "Karyawan sudah check-in hari ini"

**Retry Strategy:**
- GET requests: 3 retries with exponential backoff (1s, 2s, 4s)
- Mutation (POST/PUT/DELETE): no retry (idempotency key di header)
- Query refetch: on window focus, on network reconnect, interval 30s for dashboard

## 9. Security Considerations

| Concern | Mitigation |
|---------|-----------|
| **Multi-tenant data isolation** | Tenant ID in every query, RLS policy |
| **Role-based access** | 5 roles: Super Admin, Admin, Manager, Staff, Viewer |
| **Audit trail** | All CUD operations logged with user + timestamp |
| **Sensitive data** | Gaji, NIK, BPJS encrypted at rest |
| **API security** | JWT + refresh token, rate limiting 100 req/min |
| **XSS** | DOMPurify for rich text, Content-Security-Policy |
| **CSRF** | SameSite strict, CSRF token per session |
| **Session** | 4 hour idle timeout, max 3 concurrent sessions |
| **Data export** | Watermark on exported PDFs with user info |

## 10. Deployment & Build

```bash
# Build
npm run build  # → dist/

# Docker
FROM node:18-alpine AS builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

# Environment
VITE_API_URL=https://api.erpnusantara.com/v1
VITE_APP_NAME=ERP Nusantara
VITE_SENTRY_DSN={{SENTRY_DSN}}
VITE_GA_ID={{GA_ID}}
VITE_ENABLE_MOCK=true

# Deploy target: AWS ECS / Google Cloud Run
# CDN: Cloudflare
# CI: GitHub Actions (lint → typecheck → test → build → deploy)
```

## 11. Testing Strategy

| Level | Tools | Target |
|-------|-------|--------|
| **Unit** | Vitest + React Testing Library | ≥80% coverage |
| **Component** | Storybook | All shared UI + critical module components |
| **Integration** | Vitest + MSW | Cross-module flows (SO → Inventory → Finance) |
| **E2E** | Playwright | 10 critical paths (order to cash, procure to pay, hire to retire) |
| **Visual Regression** | Percy | All pages on desktop + mobile |

**Critical Test Paths:**
1. Create Sales Order → Approve → Update Inventory → Invoice → Receive Payment
2. Create Purchase Order → Receive Goods → Update Inventory → Pay Supplier
3. Employee Onboarding → Attendance → Payroll Generation → GL Posting
4. Period End Closing → Generate Financial Statements

## 12. Dependencies & Constraints

**Dependencies:**
- Node.js 18+ for build
- REST API backend (NestJS / Golang recommended for ERP scale)
- PostgreSQL 14+ with proper indexing untuk query agregat
- Redis untuk cache dan session management
- Object storage (S3/MinIO) untuk dokumen dan bukti transaksi

**Constraints:**
- Browser support: Chrome 90+, Firefox 90+, Safari 15+, Edge 90+
- Mobile: Responsive web, native apps TBA
- Tidak support offline mode di MVP (future: PWA with service worker)
- SPA architecture (client-side rendering — SEO tidak kritikal untuk enterprise app)
- API response time: p95 < 2 detik untuk semua endpoint

**Assumptions:**
- Backend handles transactional integrity antar modul
- Database views/materialized views untuk laporan agregat
- File storage service untuk upload dokumen
- Auth service terpisah (Auth0, Keycloak, atau custom)
