# TRD: BukuWarung Akuntansi — Technical Requirements

## 1. Technical Strategy

**Architecture Pattern:** Feature-Sliced Design (FSD) dengan pemisahan modul per domain akuntansi. Frontend monolitik dengan code-splitting per modul untuk optimasi bundle.

**Key Decisions:**
- **State management:** Zustand untuk UI state ringan, React Query (TanStack Query) untuk server state dengan cache stale-while-revalidate
- **Form handling:** React Hook Form + Zod schema validation — memastikan debit/kredit balance sebelum submit
- **Data grid:** @tanstack/react-table dengan virtual scrolling untuk handle ribuan baris jurnal
- **PDF generation:** jsPDF + jspdf-autotable di sisi client, tidak perlu server-side rendering
- **Date handling:** date-fns dengan locale `id` untuk format tanggal Indonesia
- **Number formatting:** Intl.NumberFormat('id-ID') untuk format Rupiah konsisten

## 2. Tech Stack

| Layer | Teknologi | Versi | Alasan |
|-------|-----------|-------|--------|
| **Build Tool** | Vite | 5.x | HMR cepat, tree-shaking, TypeScript native |
| **Framework** | React | 18.x | Ecosystem matang, concurrent features |
| **Language** | TypeScript | 5.x | Type safety untuk data keuangan |
| **Styling** | Tailwind CSS | 3.x | Utility-first, purging otomatis |
| **Icons** | Lucide React | 0.3+ | Icons konsisten, ringan |
| **UI Library** | shadcn/ui | latest | Aksesible, customisable, Radix-based |
| **Animation** | Framer Motion | 11.x | Smooth transitions, gesture support |
| **Chart** | Recharts | 2.x | Declarative, React-native, ringan |
| **Table** | @tanstack/react-table | 8.x | Headless, virtualisasi, sorting/filtering |
| **Form** | React Hook Form | 7.x | Performant, minim re-render |
| **Validation** | Zod | 3.x | Runtime type safety |
| **Date** | date-fns | 3.x | Tree-shakeable, locale id |
| **Router** | React Router | 6.x | Standar industri, lazy loading |
| **PDF** | jsPDF + autotable | 2.x | Client-side PDF generation |
| **State (client)** | Zustand | 4.x | Simple, no boilerplate |
| **State (server)** | TanStack Query | 5.x | Caching, refetch, optimistic update |

## 3. Component Architecture

```
src/
├── app/
│   ├── App.tsx
│   ├── providers.tsx          # QueryClient, Router, Theme
│   └── router.tsx             # Route definitions
├── shared/
│   ├── lib/
│   │   ├── utils.ts           # cn(), formatCurrency, formatDate
│   │   ├── validators.ts      # Zod schemas shared
│   │   └── constants.ts       # API_URL, DEBOUNCE_MS
│   ├── ui/                    # shadcn/ui components
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── table.tsx
│   │   ├── card.tsx
│   │   ├── dialog.tsx
│   │   ├── dropdown-menu.tsx
│   │   ├── select.tsx
│   │   ├── toast.tsx
│   │   ├── skeleton.tsx
│   │   └── badge.tsx
│   └── hooks/
│       ├── useDebounce.ts
│       └── useMediaQuery.ts
├── entities/                  # Domain models
│   ├── account/
│   │   ├── types.ts           # Account, AccountType interfaces
│   │   ├── api.ts             # fetchAccounts, createAccount
│   │   └── store.ts           # Zustand store for account filter
│   ├── journal/
│   │   ├── types.ts           # JournalEntry, JournalLine
│   │   ├── api.ts
│   │   └── store.ts
│   └── report/
│       ├── types.ts           # FinancialReport, ReportSection
│       ├── api.ts
│       └── utils.ts           # calculateTotals, formatReport
├── features/                  # Feature modules
│   ├── journal-entry/
│   │   ├── JournalEntryForm.tsx
│   │   ├── JournalLineRow.tsx
│   │   ├── JournalEntryTable.tsx
│   │   ├── JournalFilter.tsx
│   │   └── useJournalEntry.ts
│   ├── chart-of-accounts/
│   │   ├── AccountList.tsx
│   │   ├── AccountForm.tsx
│   │   ├── AccountTree.tsx
│   │   └── useAccounts.ts
│   ├── general-ledger/
│   │   ├── LedgerView.tsx
│   │   ├── LedgerTable.tsx
│   │   └── useGeneralLedger.ts
│   ├── trial-balance/
│   │   ├── TrialBalanceView.tsx
│   │   └── useTrialBalance.ts
│   ├── income-statement/
│   │   ├── IncomeStatementView.tsx
│   │   ├── IncomeStatementChart.tsx
│   │   └── useIncomeStatement.ts
│   ├── balance-sheet/
│   │   ├── BalanceSheetView.tsx
│   │   └── useBalanceSheet.ts
│   └── reports/
│       ├── ReportExporter.tsx
│       └── useReportExport.ts
├── widgets/                   # Composite widgets
│   ├── sidebar/
│   │   ├── Sidebar.tsx
│   │   ├── SidebarItem.tsx
│   │   └── PeriodSelector.tsx
│   ├── topbar/
│   │   ├── TopBar.tsx
│   │   ├── UserMenu.tsx
│   │   └── NotificationBell.tsx
│   └── dashboard/
│       ├── DashboardPage.tsx
│       ├── BalanceCards.tsx
│       ├── RecentTransactions.tsx
│       └── ProfitChart.tsx
└── pages/
    ├── DashboardPage.tsx
    ├── JournalPage.tsx
    ├── LedgerPage.tsx
    ├── TrialBalancePage.tsx
    ├── IncomeStatementPage.tsx
    ├── BalanceSheetPage.tsx
    ├── SettingsPage.tsx
    └── NotFoundPage.tsx
```

## 4. Data Layer

**Data Flow Architecture:**

```
User Action → Form (React Hook Form + Zod)
  → Zustand (optimistic UI update)
  → TanStack Query Mutation
    → API Client (fetch/axios)
      → Backend REST API
        → PostgreSQL Database
  ← Response ← Cache Invalidation ←
  ← UI Update (re-fetch + re-render)
```

**Caching Strategy:**
- Journal list: cache 30 detik, invalidate on mutation
- Account list: cache 5 menit (jarang berubah), stale while revalidate
- Report data: cache 2 menit, refetch on period change
- Dashboard cards: cache 1 menit, refetch on focus

**State Separation:**
- **Server State (TanStack Query):** Journal entries, accounts, ledger, reports
- **UI State (Zustand):** Active period, sidebar collapse, filter values, form drafts
- **URL State (React Router):** Current page, search params, period in URL

## 5. Performance Requirements

| Metrik | Target | Alat Ukur |
|--------|--------|-----------|
| First Contentful Paint (FCP) | <1.5 detik | Lighthouse |
| Largest Contentful Paint (LCP) | <2.5 detik | Lighthouse |
| Time to Interactive (TTI) | <3 detik | Lighthouse |
| Bundle size (initial) | <200 KB gzip | webpack-bundle-analyzer |
| Table render (10.000 rows) | <2 detik | Profiler |
| Form submit response | <500 ms | Custom metric |
| Chart render | <1 detik | Profiler |
| Lighthouse score | >90 | Lighthouse |

**Optimasi:**
- **Virtual scrolling** untuk jurnal dan buku besar (react-virtual atau @tanstack/react-virtual)
- **Code splitting** per modul — journal module hanya di-load saat navigasi ke jurnal
- **Debounced search** (300ms) untuk pencarian transaksi
- **Memoized components** (React.memo, useMemo) untuk tabel besar
- **Lazy load** Recharts — hanya load saat membuka halaman laporan

## 6. Styling & Theming System

```css
/* styles/theme.css */
:root {
  /* Primary palette */
  --color-primary: #0D5C3D;
  --color-primary-light: #1A8C5E;
  --color-primary-dark: #083A26;
  --color-primary-foreground: #FFFFFF;
  
  /* Accent */
  --color-accent: #F59E0B;
  --color-accent-foreground: #1E293B;
  
  /* Surfaces */
  --color-background: #F8FAFC;
  --color-surface: #FFFFFF;
  --color-surface-hover: #F1F5F9;
  
  /* Text */
  --color-text-primary: #1E293B;
  --color-text-secondary: #64748B;
  --color-text-tertiary: #94A3B8;
  --color-text-inverse: #FFFFFF;
  
  /* Semantic */
  --color-success: #10B981;
  --color-error: #EF4444;
  --color-warning: #F59E0B;
  --color-info: #3B82F6;
  
  /* Borders */
  --color-border: #E2E8F0;
  --color-border-hover: #CBD5E1;
  
  /* Sidebar */
  --sidebar-width: 280px;
  --sidebar-bg: #FFFFFF;
  --sidebar-border: #E2E8F0;
  
  /* Typography */
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  
  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
  
  /* Radius */
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
  
  /* Shadows */
  --shadow-card: 0 1px 3px rgba(0,0,0,0.08);
  --shadow-dropdown: 0 4px 12px rgba(0,0,0,0.12);
  --shadow-modal: 0 8px 24px rgba(0,0,0,0.16);
}

/* Tailwind config extension */
/* theme.extend.colors.primary, theme.extend.fontFamily, etc */
```

## 7. Validation Rules

| Field | Rule | Message |
|-------|------|---------|
| Tanggal | Required, valid date, dalam periode aktif | "Tanggal wajib diisi" |
| No. Bukti | Required, unique per periode | "Nomor bukti sudah digunakan" |
| Debit | > 0, number, max 999.999.999.999 | "Nilai debit harus lebih dari 0" |
| Kredit | > 0, number, max 999.999.999.999 | "Nilai kredit harus lebih dari 0" |
| Balance | Sum(debit) == sum(kredit) | "Total debit ({{DEBIT}}) dan kredit ({{KREDIT}}) harus sama" |
| Akun | Must be active, not deleted | "Akun tidak aktif atau sudah dihapus" |
| Periode | Period must be open | "Periode {{PERIODE}} sudah ditutup" |
| Baris jurnal | Minimal 1 debit + 1 kredit | "Jurnal harus memiliki minimal 1 debit dan 1 kredit" |
| Nama akun | Required, max 100 chars | "Nama akun wajib diisi" |
| Kode akun | Unique, format {{GOL}}-{{NOMOR}} | "Kode akun sudah digunakan" |

## 8. Error Handling Strategy

**Layered Error Handling:**

```
Layer 1: Zod Validation (Form level)
  - Validasi client-side sebelum submit
  - Error message per field
  - Submit button disabled sampai valid

Layer 2: API Client (Network level)
  - Axios interceptor untuk error response
  - 401 → redirect login
  - 403 → toast "Tidak memiliki akses"
  - 404 → toast "Data tidak ditemukan"
  - 409 → toast "Data konflik, refresh halaman"
  - 422 → mapping validation error ke form field
  - 500 → toast "Terjadi kesalahan server"

Layer 3: TanStack Query (Data level)
  - onError callback per query/mutation
  - Retry logic (3× untuk GET, no retry untuk POST/PUT)

Layer 4: React Error Boundary (Component level)
  - Per modul error boundary
  - Fallback UI dengan tombol "Muat Ulang"
  - Log error ke console/sentry
```

**Error Display:**
- **Toast notifications:** Untuk operasi sukses/gagal (auto-dismiss 5 detik)
- **Inline errors:** Untuk validasi form per field
- **Banner:** Untuk error jaringan/ server (non-dismissable sampai resolved)
- **Modal:** Untuk error kritis (session expired, data corruption)

## 9. Security Considerations

| Concern | Mitigation |
|---------|-----------|
| **Data keuangan sensitif** | Enkripsi AES-256 at-rest, TLS 1.3 in-transit |
| **Autentikasi** | JWT with refresh token, expiry 24 jam |
| **Autorisasi** | Role-based access: admin, akuntan, viewer |
| **XSS** | React escaping, Content-Security-Policy headers |
| **CSRF** | SameSite cookie, CSRF token untuk mutations |
| **Input validation** | Zod schema di client + server, sanitasi input |
| **Rate limiting** | Max 30 request/menit per endpoint per user |
| **Audit log** | Semua write operation tercatat (user, timestamp, action) |
| **Session management** | Force logout setelah 4 jam idle, concurrent session limit |
| **Data isolation** | Multi-tenant dengan row-level security di database |

## 10. Deployment & Build

```bash
# Build production
npm run build
# Output: dist/ — static files (SPA)

# Deployment — Docker + Nginx
docker build -t bukuwarung-akuntansi:latest .
docker push {{REGISTRY}}/bukuwarung-akuntansi:latest

# Environment Variables
VITE_API_URL=https://api.bukuwarung.com/v1
VITE_APP_NAME=BukuWarung Akuntansi
VITE_SENTRY_DSN={{SENTRY_DSN}}
VITE_GA_ID={{GA_TRACKING_ID}}

# CI Pipeline
# GitHub Actions: lint → test → build → deploy
# Deploy target: Vercel (web) + Cloudflare CDN
```

**Build Configuration:**
- Vite `build.rollupOptions.output.manualChunks`:
  - `vendor`: react, react-dom, react-router-dom
  - `table`: @tanstack/react-table
  - `chart`: recharts
  - `pdf`: jspdf, jspdf-autotable
  - `forms`: react-hook-form, zod
- Minifikasi: esbuild (default)
- Sourcemaps: hidden (production)

## 11. Testing Strategy

| Level | Tools | Target Coverage | Frekuensi |
|-------|-------|----------------|-----------|
| **Unit Test** | Vitest + React Testing Library | >80% | Setiap commit |
| **Component Test** | Storybook + Chromatic | Komponen kritis | Setiap PR |
| **Integration Test** | Vitest + MSW | Alur utama | Setiap PR |
| **E2E Test** | Playwright | 5 critical paths | Sebelum release |
| **Visual Regression** | Percy/Chromatic | UI components | Setiap PR |

**Test Priority:**
1. **Journal Entry Flow** — kemungkinan error finansial tertinggi
2. **Report Calculation** — akurasi laporan keuangan
3. **Balance Validation** — debit/kredit harus balance
4. **Period Management** — buka/tutup periode tidak boleh corrupt data

## 12. Dependencies & Constraints

**Dependencies:**
- Node.js 18+ (build)
- Backend REST API tersedia (belum termasuk scope frontend ini)
- PostgreSQL database dengan schema akuntansi
- Minimal RAM 512MB untuk hosting statis

**Constraints:**
- Vite-specific: tidak support IE11
- Browser support: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- Mobile: Android 8+, iOS 14+
- Tidak ada server-side rendering (static SPA)
- API response time maksimal 2 detik untuk user experience acceptable
- Service Worker untuk offline support (future scope)

**Assumptions:**
- Backend menyediakan endpoint REST sesuai contract API
- Database handle agregasi laporan keuangan
- File storage (untuk upload bukti) tersedia terpisah (S3/MinIO)
- User sudah memiliki akun dan login melalui sistem auth terpisah
