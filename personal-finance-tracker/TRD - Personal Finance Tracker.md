# TRD: CatatanDuit — Technical Requirements

## 1. Technical Strategy

**Architecture Pattern:** Offline-first SPA dengan IndexedDB sebagai primary data store. Cloud sync sebagai lapisan opsional (premium feature). Mobile-first responsive design.

**Key Decisions:**
- **Offline-first:** Semua data disimpan di IndexedDB menggunakan Dexie.js. Aplikasi berfungsi penuh tanpa internet.
- **PWA:** Service Worker untuk caching app shell dan asset statis. Manifest untuk install prompt.
- **No build-time SSR:** SPA murni. SEO tidak kritikal untuk aplikasi pribadi.
- **Virtual scrolling:** Untuk daftar transaksi yang mungkin mencapai ribuan item.

## 2. Tech Stack

| Layer | Teknologi | Versi | Alasan |
|-------|-----------|-------|--------|
| **Build Tool** | Vite | 5.x | Fast dev server, PWA plugin |
| **Framework** | React | 18.x | Ecosystem, hooks |
| **Language** | TypeScript | 5.x | Type safety untuk data keuangan |
| **Styling** | Tailwind CSS | 3.x | Utility-first, small bundle |
| **UI Library** | shadcn/ui | latest | Accessible, Radix primitives |
| **Icons** | Lucide React | 0.3+ | Consistent icon set |
| **Animation** | Framer Motion | 11.x | Bottom sheet, page transitions |
| **Chart** | Recharts | 2.x | Pie chart, bar chart |
| **DB (Client)** | Dexie.js | 4.x | IndexedDB wrapper, reactive |
| **PWA** | vite-plugin-pwa | 0.17+ | Service worker, manifest |
| **Virtual List** | @tanstack/react-virtual | 3.x | High-performance list |
| **State (UI)** | Zustand | 4.x | Minimal boilerplate |
| **State (Data)** | Dexie (live query) | 4.x | Reactive IndexedDB queries |
| **Date** | date-fns | 3.x | Locale id |
| **Keyboard** | react-numpad | custom | Numeric keypad component |

## 3. Component Architecture

```
src/
├── app/
│   ├── App.tsx
│   ├── providers.tsx
│   ├── router.tsx
│   └── pwa.ts              # Service worker registration
├── shared/
│   ├── lib/
│   │   ├── utils.ts
│   │   ├── validators.ts
│   │   ├── constants.ts     # Categories default, colors
│   │   ├── db.ts            # Dexie database setup
│   │   ├── format.ts        # Currency, date formatters
│   │   └── storage.ts       # localStorage helpers
│   ├── ui/                  # shadcn/ui
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── card.tsx
│   │   ├── bottom-sheet.tsx
│   │   ├── dialog.tsx
│   │   ├── select.tsx
│   │   ├── tabs.tsx
│   │   └── skeleton.tsx
│   └── hooks/
│       ├── useTransactions.ts
│       ├── useAccounts.ts
│       ├── useCategories.ts
│       ├── useBudgets.ts
│       ├── useOnlineStatus.ts
│       └── useCurrencyFormat.ts
├── entities/
│   ├── transaction/
│   │   ├── types.ts
│   │   └── db.ts            # Dexie table definition
│   ├── account/
│   │   ├── types.ts
│   │   └── db.ts
│   ├── category/
│   │   ├── types.ts
│   │   └── db.ts
│   └── budget/
│       ├── types.ts
│       └── db.ts
├── features/
│   ├── add-transaction/
│   │   ├── AddTransactionSheet.tsx
│   │   ├── TransactionTypeSelector.tsx
│   │   ├── AmountInput.tsx    # Custom numpad
│   │   ├── CategoryPicker.tsx
│   │   ├── AccountPicker.tsx
│   │   └── RecurringToggle.tsx
│   ├── transaction-list/
│   │   ├── TransactionList.tsx
│   │   ├── TransactionItem.tsx
│   │   ├── TransactionFilter.tsx
│   │   └── TransactionSearch.tsx
│   ├── accounts/
│   │   ├── AccountList.tsx
│   │   ├── AccountCard.tsx
│   │   └── AccountForm.tsx
│   ├── categories/
│   │   ├── CategoryList.tsx
│   │   └── CategoryForm.tsx
│   ├── budgets/
│   │   ├── BudgetList.tsx
│   │   ├── BudgetCard.tsx
│   │   └── BudgetForm.tsx
│   └── reports/
│       ├── MonthlyReport.tsx
│       ├── ExpensePieChart.tsx
│       ├── SpendingTrendChart.tsx
│       └── ExportButton.tsx
├── widgets/
│   ├── dashboard/
│   │   ├── DashboardPage.tsx
│   │   ├── BalanceOverview.tsx
│   │   ├── WeeklySpending.tsx
│   │   └── RecentTransactions.tsx
│   ├── layout/
│   │   ├── BottomNav.tsx
│   │   ├── TopBar.tsx
│   │   └── Sidebar.tsx       # Tablet/desktop
│   └── common/
│       ├── FAB.tsx
│       ├── EmptyState.tsx
│       └── ProgressRing.tsx
└── pages/
    ├── DashboardPage.tsx
    ├── TransactionsPage.tsx
    ├── ReportsPage.tsx
    ├── BudgetPage.tsx
    ├── SettingsPage.tsx
    └── NotFoundPage.tsx
```

## 4. Data Layer

**Dexie Database Schema:**
```typescript
// db.ts
import Dexie, { Table } from 'dexie';

export class CatatanDuitDB extends Dexie {
  transactions!: Table<Transaction, string>;
  accounts!: Table<Account, string>;
  categories!: Table<Category, string>;
  budgets!: Table<Budget, string>;
  goals!: Table<FinancialGoal, string>;

  constructor() {
    super('CatatanDuitDB');
    this.version(1).stores({
      transactions: 'id, date, type, categoryId, accountId, [type+date]',
      accounts: 'id, type, isActive',
      categories: 'id, type, parentId',
      budgets: 'id, categoryId, [month+year]',
      goals: 'id, status',
    });
  }
}

export const db = new CatatanDuitDB();
```

**Data Flow:**
```
Component → Custom Hook (useTransactions)
  → Dexie liveQuery (reactive — auto-update on DB change)
    → IndexedDB
  ← Transaction[]
  ← UI auto re-render
```

**Sync Architecture (Premium — Future):**
```
Online? → Yes → Push local changes queue → Pull remote changes → Merge
       → No  → Queue changes locally → Resolve conflict on next sync
```

**Reactivity:** Dexie `liveQuery()` provides RxJS-like observables. Component subscribe dan auto re-render saat data di IndexedDB berubah.

## 5. Performance Requirements

| Metrik | Target |
|--------|--------|
| Cold start (PWA) | <2 detik |
| Transaction save | <100ms |
| List render (10.000 items) | <1 detik (virtual) |
| Chart render | <500ms |
| IndexedDB query (1.000 items) | <50ms |
| Bundle size (initial) | <150 KB gzip |
| PWA install size | <1 MB |

**Optimasi:**
- Virtual scrolling dengan @tanstack/react-virtual untuk transaction list
- IndexedDB indexes untuk query cepat (by date, type, category)
- Code splitting per page (Dashboard, Transactions, Reports, Settings)
- Lazy load Recharts hanya saat buka tab laporan
- Framer Motion `layoutId` untuk smooth transitions antar tab
- Pre-cache app shell via Service Worker

## 6. Styling & Theming System

```css
/* styles/theme.css */
:root {
  /* Primary — Teal */
  --color-primary: #14B8A6;
  --color-primary-light: #5EEAD4;
  --color-primary-dark: #0F766E;
  --color-primary-foreground: #FFFFFF;
  
  /* Accent */
  --color-accent: #8B5CF6;
  --color-accent-foreground: #FFFFFF;
  
  /* Surfaces */
  --color-background: #F9FAFB;
  --color-surface: #FFFFFF;
  --color-surface-secondary: #F3F4F6;
  
  /* Text */
  --color-text-primary: #111827;
  --color-text-secondary: #6B7280;
  --color-text-tertiary: #9CA3AF;
  --color-text-inverse: #FFFFFF;
  
  /* Semantic */
  --color-income: #10B981;
  --color-expense: #EF4444;
  --color-warning: #F59E0B;
  --color-info: #3B82F6;
  
  /* Category colors */
  --cat-makanan: #F97316;
  --cat-transport: #3B82F6;
  --cat-belanja: #EC4899;
  --cat-hiburan: #8B5CF6;
  --cat-kesehatan: #10B981;
  --cat-pendidikan: #06B6D4;
  --cat-tagihan: #F59E0B;
  --cat-gaji: #10B981;
  --cat-investasi: #6366F1;
  --cat-lainnya: #6B7280;
  
  /* Borders */
  --color-border: #E5E7EB;
  
  /* Typography */
  --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  
  /* Spacing */
  --page-padding: 16px;
  --card-radius: 16px;
  
  /* Shadows */
  --shadow-card: 0 1px 3px rgba(0,0,0,0.06);
  --shadow-float: 0 4px 16px rgba(0,0,0,0.1);
}

/* Dark Mode */
[data-theme="dark"] {
  --color-background: #0F172A;
  --color-surface: #1E293B;
  --color-surface-secondary: #334155;
  --color-text-primary: #F1F5F9;
  --color-text-secondary: #94A3B8;
  --color-border: #334155;
  --shadow-card: 0 1px 3px rgba(0,0,0,0.3);
}
```

## 7. Validation Rules

| Field | Rule | Message |
|-------|------|---------|
| Nominal | Required, > 0, max 999.999.999 | "Nominal harus diisi" |
| Kategori | Required (kecuali transfer) | "Pilih kategori" |
| Akun | Required | "Pilih akun" |
| Tanggal | Required, <= hari ini | "Tanggal tidak valid" |
| Nama akun | Required, max 30 chars | "Nama akun wajib diisi" |
| Target budget | Required, > 0 | "Target anggaran harus diisi" |
| Nama kategori | Required, unique | "Nama kategori sudah ada" |
| Nama goal | Required | "Nama tujuan finansial harus diisi" |
| Target goal | Required, > 0 | "Target nominal harus diisi" |

## 8. Error Handling Strategy

**Error Categories:**
1. **Validation Errors** — handled by Zod + React Hook Form
2. **IndexedDB Errors** — transaction rollback, retry with exponential backoff
3. **Network Errors** — offline mode (no error, just show sync status)
4. **Storage Quota** — warning when IndexedDB approaching quota
5. **Service Worker** — fallback to network-first on SW error

**Error Display:**
- **Toast:** Success/error non-blocking (auto-dismiss 2s success, 5s error)
- **Inline:** Form validation errors
- **Banner:** Storage quota warning (persistent, dismissable)
- **Snackbar:** Offline status (shows at bottom, auto-hide when back online)

**Error Recovery:**
- IndexedDB write failure → retry 3× → if still fail, show "Gagal menyimpan. Coba lagi."
- Synchronization conflict → last-write-wins dengan timestamp (simplifikasi untuk MVP)

## 9. Security Considerations

| Concern | Mitigation |
|---------|-----------|
| **Data lokal di device** | Data di IndexedDB (tidak di cloud untuk free users) |
| **Akses fisik ke device** | Opsi PIN/passcode untuk buka aplikasi |
| **Data sensitif (saldo, transaksi)** | Tidak ada data finansial yang dikirim ke server (free tier) |
| **Screen recording** | Opsi hide saldo dari notifikasi/preview |
| **PWA security** | HTTPS only, Service Worker scope terbatas |
| **IndexedDB** | Sandboxed per origin di browser — aman dari situs lain |

**Premium / Cloud Sync Security:**
- End-to-end encryption untuk data sebelum dikirim ke cloud
- JWT authentication
- Rate limiting untuk API sync

## 10. Deployment & Build

```bash
# Build production
npm run build  # → dist/

# PWA build with vite-plugin-pwa
# - Generates service worker
# - Pre-caches app shell (HTML, JS, CSS)
# - Generates manifest.json
# - Generates icons (192px, 512px)

# Environment variables
VITE_APP_NAME=CatatanDuit
VITE_APP_VERSION=1.0.0
VITE_ENABLE_SYNC=false  # Enable for premium

# Deploy
# Option 1: Vercel (recommended for PWA)
# Option 2: Firebase Hosting
# Option 3: Netlify

# PWA Requirements:
# - HTTPS (required for Service Worker)
# - manifest.json pointing to icons
# - Service Worker registered on load
```

**PWA Configuration:**
```typescript
// vite.config.ts
VitePWA({
  registerType: 'autoUpdate',
  includeAssets: ['favicon.ico', 'apple-touch-icon.png'],
  manifest: {
    name: 'CatatanDuit',
    short_name: 'CatatanDuit',
    description: 'Aplikasi pencatatan keuangan pribadi',
    theme_color: '#14B8A6',
    background_color: '#F9FAFB',
    display: 'standalone',
    orientation: 'portrait',
    start_url: '/',
    icons: [
      { src: 'icon-192.png', sizes: '192x192', type: 'image/png' },
      { src: 'icon-512.png', sizes: '512x512', type: 'image/png' }
    ]
  },
  workbox: {
    globPatterns: ['**/*.{js,css,html,ico,png,svg}'],
    runtimeCaching: [/* API caching rules for cloud sync */]
  }
})
```

## 11. Testing Strategy

| Level | Tools | Target |
|-------|-------|--------|
| **Unit** | Vitest + React Testing Library | ≥80% coverage on utils, hooks, validation |
| **Component** | Storybook | Shared UI components |
| **Integration** | Vitest + fake IndexedDB (fake-indexeddb) | Transaction CRUD, budget calculation |
| **E2E** | Playwright + Chrome (PWA) | Full transaction flow, offline mode |
| **PWA** | Lighthouse PWA audit | Score ≥90 |

**Test Priority:**
1. Transaction creation (most critical path)
2. Balance calculation (financial accuracy)
3. Budget tracking (aggregation logic)
4. Offline data persistence
5. Dark mode toggle

## 12. Dependencies & Constraints

**Dependencies:**
- Vite 5+ for build tooling
- Dexie.js 4+ for IndexedDB
- vite-plugin-pwa for service worker
- Browser with IndexedDB and Service Worker support
- Android Chrome 90+ / iOS Safari 15+ / Desktop Chrome 90+

**Constraints:**
- Mobile-first: layout dirancang untuk 320px - 428px width
- Offline-first: semua fitur harus berfungsi tanpa koneksi
- Storage: IndexedDB quota tergantung browser (umumnya > 50MB cukup)
- PWA: tidak bisa push notification di iOS Safari
- No SSR: SEO tidak relevan (aplikasi pribadi yang butuh login)

**Assumptions:**
- User menginstall sebagai PWA atau membuka di mobile browser
- Tidak ada akun/server untuk free users (data 100% di device)
- Premium users memiliki cloud account terpisah
- System font Inter sudah di-download atau menggunakan system-ui fallback
