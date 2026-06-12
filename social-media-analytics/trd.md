# TRD: SosmedInsight — Technical Requirements
**Project:** SosmedInsight

## 1. Technical Strategy
SosmedInsight dibangun sebagai **Single-Page Application (SPA)** dengan fokus pada data visualization dan interactive charting. Arsitektur menggunakan pendekatan **JAMstack** di sisi frontend dengan backend REST API terpisah untuk agregasi data multi-platform.

Pendekatan teknis:
- **Mock-first development** — semua fitur dikembangkan dengan data palsu menggunakan MSW sebelum API nyata
- **Dark theme by default** — dashboard analitik profesional dengan tema gelap
- **Data aggregation layer** — normalisasi metrik dari berbagai platform ke format internal yang seragam
- **Progressive enhancement** — fallback ke data statis jika API platform tidak tersedia
- **Lazy loading charts** — grafik dimuat hanya saat terlihat (Intersection Observer)

Strategi mock: MSW berjalan di development dengan data generator yang menghasilkan metrik harian realistis (dengan variasi acak + tren). Semua data mock disimpan di `src/mocks/` dengan struktur per modul. Saat produksi, MSW dinonaktifkan dan data diambil dari API backend yang mengagregasi data dari berbagai platform.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Build Tool | Vite | 5.x | Fast HMR, optimized build |
| UI Library | React | 18.x | Functional components + Hooks |
| Bahasa | TypeScript | 5.x | Strict mode enabled |
| Styling | Tailwind CSS | 3.x | Utility-first, JIT, dark mode |
| Routing | React Router | 6.x | Nested layout routes |
| State (Client) | Zustand | 4.x | Global UI state (filter, selected account) |
| State (Server) | TanStack Query | 5.x | Server cache, background refetch |
| Charts | Recharts | 2.x | Line, Bar, Area, Pie, Composed charts |
| Icons | Lucide React | 0.x | Platform icons + UI icons |
| HTTP Mocking | MSW | 2.x | Service worker for API mocking |
| Table | TanStack Table | 8.x | Sorting, filtering, pagination |
| Date Utils | date-fns | 3.x | With `id` locale |
| PDF Export | jsPDF + html2canvas | 2.x / 1.x | Generate laporan PDF |
| CSV Export | papaparse | 5.x | Export data ke CSV |
| Animations | Framer Motion | 10.x | Chart transitions, layout animations |
| Form Validation | React Hook Form + Zod | 7.x / 3.x | Form settings |
| Testing (Unit) | Vitest | 1.x | Unit + component tests |
| Testing (E2E) | Playwright | 1.x | Browser testing |

## 3. Component Architecture

```
src/
├── main.tsx                          # Entry point + MSW init
├── App.tsx                           # Root component
├── router.tsx                        # Route definitions
│
├── components/
│   ├── layout/
│   │   ├── DashboardLayout.tsx       # Layout utama dashboard
│   │   ├── Sidebar.tsx               # Navigasi sidebar
│   │   ├── Topbar.tsx                # Top bar (account selector, filter periode)
│   │   ├── AccountSelector.tsx       # Dropdown pilih akun
│   │   ├── PeriodFilter.tsx          # Range selector (7H, 30H, 3B, 12B)
│   │   └── PlatformBadge.tsx         # Badge platform (IG, TikTok, dll)
│   │
│   ├── common/
│   │   ├── StatCard.tsx              # Kartu metrik dengan sparkline
│   │   ├── Sparkline.tsx             # Mini line chart untuk card
│   │   ├── DataTable.tsx             # Tabel generik
│   │   ├── EmptyState.tsx            # State kosong
│   │   ├── LoadingSkeleton.tsx       # Skeleton loading
│   │   ├── ErrorState.tsx            # State error
│   │   ├── Toast.tsx                 # Notifikasi
│   │   ├── Modal.tsx                 # Dialog modal
│   │   ├── Badge.tsx                 # Badge perubahan (positif/negatif)
│   │   ├── KPIChange.tsx             # Indikator naik/turun dengan warna
│   │   └── PeriodComparison.tsx      # Perbandingan periode (MoM/WoW)
│   │
│   ├── charts/
│   │   ├── FollowersTrendChart.tsx   # Line chart followers over time
│   │   ├── EngagementChart.tsx       # Bar/line chart engagement
│   │   ├── ReachImpressionsChart.tsx # Area chart reach + impressions
│   │   ├── PlatformComparisonChart.tsx # Bar chart perbandingan platform
│   │   ├── DemografiUsiaChart.tsx    # Bar chart demografi usia
│   │   ├── DemografiGenderChart.tsx  # Pie chart gender
│   │   ├── DemografiLokasiChart.tsx  # Bar chart top lokasi
│   │   ├── JamAktifHeatmap.tsx       # Heatmap jam aktif
│   │   ├── KompetitorTrendChart.tsx  # Dual line chart kompetitor
│   │   └── HashtagCloud.tsx          # Word cloud / bubble chart hashtag
│   │
│   ├── content/
│   │   ├── PostTable.tsx             # Tabel postingan
│   │   ├── PostDetailPanel.tsx       # Panel detail postingan
│   │   ├── PostCard.tsx              # Card postingan untuk grid
│   │   ├── TopPostsGrid.tsx          # Grid 2x2 top posts
│   │   └── PostFilter.tsx            # Filter postingan
│   │
│   ├── audience/
│   │   ├── AudienceOverview.tsx      # Ringkasan demografi
│   │   ├── DemografiPanel.tsx        # Panel lengkap demografi
│   │   └── JamAktifSection.tsx       # Section jam aktif + rekomendasi
│   │
│   ├── kompetitor/
│   │   ├── KompetitorTable.tsx       # Tabel perbandingan kompetitor
│   │   ├── KompetitorForm.tsx        # Form tambah kompetitor
│   │   └── KompetitorChart.tsx       # Grafik perbandingan
│   │
│   └── reports/
│       ├── ReportBuilder.tsx         # Builder laporan
│       ├── ReportPreview.tsx         # Preview PDF
│       └── ReportHistory.tsx         # Riwayat laporan
│
├── features/
│   ├── dashboard/
│   │   ├── DashboardPage.tsx         # Halaman dashboard utama
│   │   ├── StatOverview.tsx          # Grid stat card
│   │   └── ChartsSection.tsx         # Section grafik
│   │
│   ├── content/
│   │   ├── ContentPage.tsx           # Halaman analisis konten
│   │   └── PostDetailPage.tsx        # Halaman detail postingan
│   │
│   ├── audience/
│   │   └── AudiencePage.tsx          # Halaman insight audiens
│   │
│   ├── kompetitor/
│   │   └── KompetitorPage.tsx        # Halaman kompetitor
│   │
│   ├── reports/
│   │   └── ReportsPage.tsx           # Halaman laporan
│   │
│   └── settings/
│       ├── SettingsPage.tsx           # Pengaturan akun
│       └── AccountConnection.tsx     # Manajemen koneksi platform
│
├── hooks/
│   ├── useMetrics.ts                 # React Query hook untuk metrics
│   ├── usePosts.ts                   # React Query hook untuk posts
│   ├── useAudience.ts                # React Query hook untuk audience
│   ├── useKompetitor.ts              # React Query hook untuk kompetitor
│   ├── useSyncStatus.ts              # Status sinkronisasi
│   ├── useDebounce.ts                # Debounce untuk filter
│   └── useMediaQuery.ts              # Responsive breakpoints
│
├── stores/
│   ├── useFilterStore.ts             # Filter global (akun, periode, platform)
│   ├── useSelectedPostStore.ts       # Postingan yang dipilih untuk detail
│   └── useUIStore.ts                 # Sidebar collapse, panel state
│
├── types/
│   ├── account.ts                    # AkunSosmed interface
│   ├── metrics.ts                    # MetricHarian, Insights, dll
│   ├── post.ts                       # Postingan interface
│   ├── audience.ts                   # InsightAudiens interface
│   ├── kompetitor.ts                 # Kompetitor interface
│   ├── report.ts                     # Laporan interface
│   └── common.ts                     # Shared types (Period, Platform, Filter)
│
├── lib/
│   ├── normalizers.ts                # Normalisasi metrik antar platform
│   ├── calculators.ts                # Perhitungan ER, growth, dll
│   ├── mockGenerator.ts              # Generator data mock realistis
│   └── formatters.ts                 # Format angka, tanggal, dll
│
├── mocks/
│   ├── browser.ts                    # MSW browser setup
│   ├── handlers/
│   │   ├── metricsHandlers.ts        # API handler metrik
│   │   ├── postsHandlers.ts          # API handler postingan
│   │   ├── audienceHandlers.ts       # API handler audiens
│   │   ├── kompetitorHandlers.ts     # API handler kompetitor
│   │   └── accountHandlers.ts        # API handler akun
│   └── data/
│       ├── accounts.ts               # Mock akun
│       ├── metrics.ts                # Mock metrik harian
│       ├── posts.ts                  # Mock postingan
│       ├── audience.ts               # Mock audiens
│       └── kompetitors.ts            # Mock kompetitor
│
└── utils/
    ├── formatAngka.ts                # Format angka Indonesia (rb, jt, dll)
    ├── formatTanggal.ts              # Format tanggal Indonesia
    ├── formatWaktu.ts                # Format waktu relatif
    ├── formatPersentase.ts           # Format persentase dengan tanda +/- 
    └── constants.ts                  # Warna platform, tier pricing, dll
```

## 4. Data Layer

### State Management Strategy

**Zustand Stores (Client State):**
```typescript
// useFilterStore — filter global yang memengaruhi seluruh dashboard
interface FilterState {
  selectedAccountId: string | null;
  period: '7H' | '30H' | '3B' | '12B' | 'custom';
  customStartDate: Date | null;
  customEndDate: Date | null;
  selectedPlatform: 'all' | 'instagram' | 'tiktok' | 'youtube' | 'facebook' | 'twitter' | 'linkedin';
  comparePeriod: boolean;
  setAccount: (id: string) => void;
  setPeriod: (period: FilterState['period']) => void;
  setPlatform: (platform: FilterState['selectedPlatform']) => void;
  toggleCompare: () => void;
}

// useSelectedPostStore — postingan yang dipilih untuk panel detail
interface SelectedPostState {
  selectedPostId: string | null;
  isDetailOpen: boolean;
  selectPost: (id: string) => void;
  closeDetail: () => void;
}

// useUIStore — UI state
interface UIState {
  sidebarCollapsed: boolean;
  rightPanelOpen: boolean;
  toggleSidebar: () => void;
  toggleRightPanel: () => void;
}
```

**TanStack Query (Server State):**
```typescript
// useMetrics — data metrik untuk dashboard
function useMetrics(accountId: string, period: PeriodFilter) {
  return useQuery({
    queryKey: ['metrics', accountId, period],
    queryFn: () => fetchMetrics(accountId, period),
    staleTime: 5 * 60_000,      // 5 menit cache
    gcTime: 30 * 60_000,        // 30 menit garbage collection
    refetchOnWindowFocus: false, // Jangan refetch saat focus (menghemat API)
  });
}

// usePosts — daftar postingan
function usePosts(accountId: string, filters: PostFilters) {
  return useQuery({
    queryKey: ['posts', accountId, filters],
    queryFn: () => fetchPosts(accountId, filters),
    staleTime: 2 * 60_000,
  });
}

// useSyncAccount — mutation untuk sinkronisasi manual
function useSyncAccount() {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: (accountId: string) => syncAccount(accountId),
    onSuccess: (_, accountId) => {
      // Invalidasi semua query terkait akun tersebut
      queryClient.invalidateQueries({ queryKey: ['metrics', accountId] });
      queryClient.invalidateQueries({ queryKey: ['posts', accountId] });
      queryClient.invalidateQueries({ queryKey: ['audience', accountId] });
    },
  });
}
```

### Mock Data Generator Strategy
Data generator menghasilkan metrik harian realistis dengan:
- **Seed tetap** — data konsisten antar render (gunakan seed berdasarkan tanggal)
- **Tren** — followers naik secara linear dengan variasi harian
- **Siklus mingguan** — engagement lebih tinggi di akhir pekan
- **Anomali jarang** — 2% probabilitas spike/drop untuk testing anomaly detection
- **Real-time simulation** — data "hari ini" menggunakan partial data dengan estimasi

```typescript
// lib/mockGenerator.ts
export function generateMetrics(accountId: string, days: number): MetricHarian[] {
  const baseFollowers = 45000;
  const dailyGrowth = 15; // Rata-rata followers baru per hari
  
  return Array.from({ length: days }, (_, i) => {
    const dayOffset = days - 1 - i;
    const date = new Date();
    date.setDate(date.getDate() - dayOffset);
    
    const isWeekend = date.getDay() === 0 || date.getDay() === 6;
    const growthMultiplier = isWeekend ? 0.7 : 1.0;
    const dayFollowers = baseFollowers + (dailyGrowth * dayOffset);
    const gained = Math.floor((Math.random() * 60 + 20) * growthMultiplier);
    const lost = Math.floor(Math.random() * 20 + 5);
    
    return {
      id: `met-${accountId}-${dayOffset}`,
      akunId: accountId,
      tanggal: date,
      followers: dayFollowers,
      followersBertambah: gained,
      followersKeluar: lost,
      impressions: Math.floor(Math.random() * 5000 + 2000),
      reach: Math.floor(Math.random() * 3000 + 1000),
      engagement: Math.floor(Math.random() * 600 + 200),
      engagementRate: +((Math.random() * 4 + 1.5) * (isWeekend ? 0.8 : 1.0)).toFixed(2),
      likes: Math.floor(Math.random() * 400 + 50),
      comments: Math.floor(Math.random() * 30 + 2),
      shares: Math.floor(Math.random() * 40 + 1),
      saves: Math.floor(Math.random() * 50 + 5),
      clicks: Math.floor(Math.random() * 100 + 10),
      totalVideoViews: Math.floor(Math.random() * 1500 + 200),
      averageWatchTime: Math.floor(Math.random() * 25 + 5),
      storyViews: Math.floor(Math.random() * 600 + 100),
      storyReplies: Math.floor(Math.random() * 15 + 1),
    };
  });
}
```

## 5. Performance Requirements

| Metrik | Target | Alat Ukur | Strategi |
|--------|--------|-----------|----------|
| First Contentful Paint (FCP) | < 1.5 detik | Lighthouse | Lazy loading, code splitting |
| Largest Contentful Paint (LCP) | < 2.5 detik | Lighthouse | Preload critical charts |
| Time to Interactive (TTI) | < 3 detik | Lighthouse | Deferred non-critical JS |
| Bundle Size (initial) | < 150 KB JS gzipped | Vite bundle analysis | Split by route + chart library |
| Chart Render (365 data points) | < 500 ms | Performance.now() | Memo + Reselect pattern |
| Chart Re-render (filter change) | < 200 ms | Performance.now() | React.memo, useMemo untuk data |
| PDF Export (10 pages) | < 10 detik | Stopwatch | Web Worker untuk rendering |
| Data Refresh | < 1 detik dari cache | Network tab | TanStack Query cache |
| Image Load (thumbnails) | < 1 detik | Web Vitals | Lazy loading, WebP, blur placeholder |

**Code Splitting Strategy:**
```typescript
// Lazy load semua halaman fitur
const DashboardPage = lazy(() => import('@/features/dashboard/DashboardPage'));
const ContentPage = lazy(() => import('@/features/content/ContentPage'));
const AudiencePage = lazy(() => import('@/features/audience/AudiencePage'));
const KompetitorPage = lazy(() => import('@/features/kompetitor/KompetitorPage'));
const ReportsPage = lazy(() => import('@/features/reports/ReportsPage'));

// Lazy load chart components (library berat)
const FollowersTrendChart = lazy(() => import('@/components/charts/FollowersTrendChart'));
const EngagementChart = lazy(() => import('@/components/charts/EngagementChart'));
```

## 6. Styling & Theming System

```css
/* styles/theme.css */
:root {
  /* Brand */
  --color-primary: #6366F1;
  --color-primary-light: #818CF8;
  --color-primary-dark: #4F46E5;
  --color-gradient-start: #6366F1;
  --color-gradient-end: #8B5CF6;
  
  /* Semantic */
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-danger: #EF4444;
  --color-info: #3B82F6;
  
  /* Dark Theme */
  --color-bg: #0F172A;
  --color-surface: #1E293B;
  --color-surface-hover: #334155;
  --color-surface-active: #475569;
  --color-text-primary: #F1F5F9;
  --color-text-secondary: #94A3B8;
  --color-text-muted: #64748B;
  --color-border: #334155;
  --color-border-light: #1E293B;
  
  /* Platform Colors */
  --color-instagram: #E4405F;
  --color-tiktok: #00F2EA;
  --color-youtube: #FF0000;
  --color-facebook: #1877F2;
  --color-twitter: #1DA1F2;
  --color-linkedin: #0A66C2;
  
  /* Typography */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  
  /* Layout */
  --sidebar-width: 240px;
  --sidebar-collapsed-width: 64px;
  --topbar-height: 56px;
  --right-panel-width: 380px;
  
  /* Charts */
  --chart-grid-color: rgba(148, 163, 184, 0.1);
  --chart-tooltip-bg: #1E293B;
  --chart-area-gradient: linear-gradient(180deg, rgba(99, 102, 241, 0.3) 0%, rgba(99, 102, 241, 0) 100%);
  
  /* Shadows */
  --shadow-card: 0 1px 3px rgba(0, 0, 0, 0.3);
  --shadow-glow: 0 0 20px rgba(99, 102, 241, 0.15);
}

/* Kustomisasi platform colors di chart */
.platform-instagram { --chart-color: var(--color-instagram); }
.platform-tiktok { --chart-color: var(--color-tiktok); }
.platform-youtube { --chart-color: var(--color-youtube); }
```

## 7. Validation Rules

| Field | Rule | Pesan Error |
|-------|------|-------------|
| Username kompetitor | Required, format username valid (tanpa @) | "Masukkan username akun yang valid" |
| Platform kompetitor | Required, pilih dari daftar | "Pilih platform kompetitor" |
| Nama akun | Required, min 3 karakter | "Nama akun minimal 3 karakter" |
| Period custom | Start date < end date | "Tanggal mulai harus sebelum tanggal selesai" |
| Period custom | Maks 365 hari | "Periode maksimal 365 hari" |
| Email registrasi | Format email valid | "Masukkan alamat email yang valid" |
| Password | Min 8 karakter | "Password minimal 8 karakter" |
| Jumlah kompetitor | Sesuai batas tier | "Tier Anda hanya dapat membandingkan {{MAX}} kompetitor" |

## 8. Error Handling Strategy

**Error Categories:**
1. **API Platform Errors** — token expired, rate limit, data not available
2. **Network Errors** — koneksi terputus, timeout
3. **Data Errors** — data tidak konsisten, missing required fields
4. **Render Errors** — chart gagal render, komponen crash

**Error Handling Implementation:**
```typescript
// hooks/useMetrics.ts — Error handling untuk data fetching
export function useMetrics(accountId: string, period: PeriodFilter) {
  return useQuery({
    queryKey: ['metrics', accountId, period],
    queryFn: async () => {
      try {
        const data = await fetchMetrics(accountId, period);
        return data;
      } catch (error) {
        if (error instanceof RateLimitError) {
          throw new Error('Batas permintaan API tercapai. Coba lagi dalam beberapa menit.');
        }
        if (error instanceof TokenExpiredError) {
          throw new Error('Token akses kedaluwarsa. Silakan hubungkan ulang akun Anda.');
        }
        if (error instanceof NetworkError) {
          throw new Error('Koneksi internet terputus. Menampilkan data terakhir yang tersimpan.');
        }
        throw new Error('Gagal memuat data. Silakan coba lagi.');
      }
    },
    retry: 2,
    retryDelay: (attempt) => Math.min(1000 * 2 ** attempt, 10000),
  });
}

// Global error handling patterns:
// 1. ErrorBoundary per halaman fitur — jika satu halaman error, halaman lain tetap berfungsi
// 2. Toast notifications — untuk error non-bloking (gagal sync, rate limit)
// 3. Inline error state — untuk data gagal muat, tampilkan komponen ErrorState
// 4. Fallback data — jika data real-time gagal, tampilkan data terakhir yang di-cache
// 5. Platform offline indicator — di topbar saat koneksi terputus

// Platform connection status:
// 🟢 Terhubung — data real-time
// 🟡 Sync delay — data > 6 jam
// 🔴 Error — token expired / koneksi bermasalah
```

## 9. Security Considerations

| Aspek | Implementasi |
|-------|--------------|
| **API Token Storage** | Token platform disimpan di backend (httpOnly cookie), tidak pernah di localStorage/JS |
| **OAuth Flow** | Semua koneksi platform menggunakan OAuth 2.0. Frontend redirect ke backend untuk auth |
| **Refresh Token** | Refresh token otomatis sebelum expired. Jika gagal, notifikasi pengguna |
| **Data Isolation** | Data setiap pengguna terisolasi (multi-tenant). Tidak ada akses antar akun |
| **CORS** | Backend hanya menerima request dari origin yang terdaftar |
| **Rate Limiting** | Endpoint API frontend: 100 request/menit. Sinkronisasi: 1x/30 menit manual |
| **XSS Protection** | React sanitasi otomatis. Untuk konten caption, gunakan DOMPurify sebelum render |
| **PDF Injection** | Sanitasi semua input yang masuk ke template laporan |

## 10. Deployment & Build

**Vite Build Configuration:**
```typescript
// vite.config.ts
export default defineConfig({
  plugins: [react()],
  build: {
    target: 'es2020',
    outDir: 'dist',
    sourcemap: false,
    chunkSizeWarningLimit: 500,
    rollupOptions: {
      output: {
        manualChunks: {
          'react-vendor': ['react', 'react-dom', 'react-router-dom'],
          'chart-vendor': ['recharts'],
          'table-vendor': ['@tanstack/react-table'],
          'pdf-vendor': ['jspdf', 'html2canvas'],
        },
      },
    },
  },
});
```

**Environment Variables:**
```env
VITE_API_BASE_URL=https://api.sosmedinsight.com/v1
VITE_APP_NAME=SosmedInsight
VITE_MSW_ENABLED=true
VITE_DEFAULT_THEME=dark
VITE_DEFAULT_PERIOD=30H
VITE_DEFAULT_LOCALE=id
```

**CI/CD:**
- Build: `npm run build`
- Preview: `npm run preview`
- Deploy: Vercel / Netlify auto-deploy dari GitHub main branch
- Docker support untuk self-hosted enterprise

## 11. Testing Strategy

| Level | Tools | Target Coverage | Yang Diuji |
|-------|-------|-----------------|------------|
| Unit Test | Vitest | > 85% | Calculator functions (ER, growth rate), formatters (angka, tanggal), normalizers |
| Component Test | Vitest + RTL | > 70% | StatCard rendering, filter interactions, chart rendering with mock data |
| Integration Test | Vitest + MSW | > 60% | Alur data lengkap: filter → fetch → render chart + table |
| E2E Test | Playwright | Critical paths | Login → View Dashboard → Filter Platform → View Post Detail |
| Chart Test | Vitest + RTL | Grafik utama | FollowersTrendChart renders correct lines, tooltip on hover |
| Visual Regression | Playwright + Percy | Halaman kunci | Dashboard, Content page, Audience page |

**Contoh Test:**
```typescript
// tests/components/StatCard.test.tsx
import { render, screen } from '@testing-library/react';
import { StatCard } from '@/components/common/StatCard';

describe('StatCard', () => {
  it('menampilkan label dan nilai', () => {
    render(
      <StatCard
        label="Total Followers"
        value={45230}
        change={5.2}
        isPositive={true}
        icon="Users"
      />
    );
    expect(screen.getByText('Total Followers')).toBeInTheDocument();
    expect(screen.getByText('45,230')).toBeInTheDocument();
    expect(screen.getByText('+5.2%')).toBeInTheDocument();
  });

  it('menampilkan warna merah untuk perubahan negatif', () => {
    render(
      <StatCard
        label="Engagement Rate"
        value={2.1}
        change={-0.5}
        isPositive={false}
        icon="Heart"
      />
    );
    const changeElement = screen.getByText('-0.5%');
    expect(changeElement).toHaveClass('text-danger');
  });

  it('menampilkan sparkline jika disediakan', () => {
    render(
      <StatCard
        label="Followers"
        value={1000}
        change={1.0}
        isPositive={true}
        icon="Users"
        sparklineData={[100, 102, 105, 103, 108]}
      />
    );
    expect(document.querySelector('.recharts-wrapper')).toBeInTheDocument();
  });
});
```

## 12. Dependencies & Constraints

**Runtime Dependencies:**
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.0",
    "@tanstack/react-query": "^5.12.0",
    "@tanstack/react-table": "^8.10.0",
    "zustand": "^4.4.0",
    "recharts": "^2.10.0",
    "lucide-react": "^0.303.0",
    "date-fns": "^3.3.0",
    "framer-motion": "^10.18.0",
    "jspdf": "^2.5.0",
    "html2canvas": "^1.4.0",
    "papaparse": "^5.4.0",
    "react-hook-form": "^7.49.0",
    "zod": "^3.22.0",
    "dompurify": "^3.0.0",
    "clsx": "^2.0.0"
  },
  "devDependencies": {
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "@vitejs/plugin-react": "^4.2.0",
    "tailwindcss": "^3.4.0",
    "postcss": "^8.4.0",
    "autoprefixer": "^10.4.0",
    "msw": "^2.0.0",
    "vitest": "^1.2.0",
    "@testing-library/react": "^14.1.0",
    "@testing-library/jest-dom": "^6.2.0",
    "@testing-library/user-event": "^14.5.0",
    "eslint": "^8.56.0",
    "prettier": "^3.2.0",
    "playwright": "^1.40.0",
    "@playwright/test": "^1.40.0"
  }
}
```

**Constraints:**
- **Node.js** >= 18.x
- **Browser support**: Chrome 90+, Firefox 90+, Safari 15+, Edge 90+
- **Recharts version 2.x** — tidak kompatibel dengan React 19 (saat ini)
- **MSW** hanya aktif di development, tidak di production build
- **Dark theme only** — tidak ada light theme di MVP (akan ditambahkan di fase 2)
- **Font loading** via Google Fonts (Inter + JetBrains Mono) dengan fallback system fonts
- **No PWA support** di MVP (akan ditambahkan nanti untuk akses offline)
- **PDF export** menggunakan jsPDF + html2canvas, bukan server-side rendering (kualitas baik untuk laporan internal, tidak untuk high-fidelity print)
