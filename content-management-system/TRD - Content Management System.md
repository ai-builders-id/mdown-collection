# TRD: NusantaraCMS — Technical Requirements
**Project:** NusantaraCMS

## 1. Technical Strategy
NusantaraCMS dibangun sebagai **Single-Page Application (SPA)** modern dengan arsitektur **JAMstack** di sisi frontend, dipisahkan dari backend REST API. Pendekatan ini memungkinkan:

- **Decoupled frontend & backend** — tim bisa bekerja paralel, frontend bisa menggunakan 100% mock data untuk development
- **Client-side rendering** dengan React untuk interaktivitas tinggi di dashboard
- **Mock-first development** — semua fitur dikembangkan dengan data palsu (MSW/MSW 2.0) sebelum API nyata tersedia
- **Server-side generation** untuk halaman publik (SSG via VitePress atau Next.js statis) agar SEO optimal
- **Progressive enhancement** — aplikasi tetap berfungsi dengan JavaScript minimal untuk konten publik

Strategi mock: selama development, **Mock Service Worker (MSW)** diaktifkan di browser untuk mencegat semua request API dan mengembalikan mock data. Saat mode produksi, MSW dinonaktifkan dan koneksi ke API nyata diaktifkan. Semua mock data disimpan di folder `src/mocks/` dengan struktur per modul.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Build Tool | Vite | 5.x | Fast dev server, HMR, optimized build |
| UI Library | React | 18.x | Functional components + Hooks |
| Bahasa | TypeScript | 5.x | Strict mode |
| Styling | Tailwind CSS | 3.x | Utility-first, JIT mode |
| Routing | React Router | 6.x | Nested routes, loaders |
| State (Client) | Zustand | 4.x | Store ringan untuk UI state |
| State (Server) | TanStack Query | 5.x | Caching, fetching, mutations |
| HTTP Mocking | MSW | 2.x | Browser service worker mock |
| Form Validation | React Hook Form + Zod | 7.x / 3.x | Performant forms + schema validation |
| Rich Text Editor | TipTap | 2.x | Headless editor framework |
| Icons | Lucide React | 0.x | Tree-shakeable icon set |
| Date Utils | date-fns | 3.x | With `id` locale |
| Table | TanStack Table | 8.x | Headless table with sorting/filtering |
| Charts | Recharts | 2.x | Responsive charts untuk dashboard |
| Animations | Framer Motion | 10.x | Transisi dan animasi halus |
| Testing (Unit) | Vitest | 1.x | Unit test + React Testing Library |
| Testing (E2E) | Playwright | 1.x | End-to-end browser testing |
| Linting | ESLint + Prettier | 8.x / 3.x | Code quality dan formatting |

## 3. Component Architecture

```
src/
├── main.tsx                          # Entry point, mount React + MSW init
├── App.tsx                           # Root component dengan routing
├── router.tsx                        # React Router configuration
│
├── components/
│   ├── layout/
│   │   ├── MainLayout.tsx            # Layout utama dashboard
│   │   ├── Sidebar.tsx               # Navigasi sidebar
│   │   ├── Topbar.tsx                # Top bar dengan breadcrumb + avatar
│   │   ├── BottomBar.tsx             # Status bar bawah
│   │   ├── SidebarItem.tsx           # Item navigasi individual
│   │   └── Breadcrumb.tsx            # Breadcrumb navigasi
│   │
│   ├── common/
│   │   ├── Button.tsx                # Tombol reusable
│   │   ├── Card.tsx                  # Card container
│   │   ├── Modal.tsx                 # Dialog modal
│   │   ├── ConfirmDialog.tsx         # Dialog konfirmasi
│   │   ├── DataTable.tsx             # Tabel generik dengan TanStack Table
│   │   ├── Badge.tsx                 # Status badge
│   │   ├── StatusBadge.tsx           # Badge khusus status (draf/terbit/dll)
│   │   ├── SearchInput.tsx           # Input pencarian
│   │   ├── EmptyState.tsx            # Komponen state kosong
│   │   ├── LoadingSkeleton.tsx       # Skeleton loading
│   │   ├── ErrorState.tsx            # Komponen error
│   │   ├── Toast.tsx                 # Notifikasi toast
│   │   ├── Pagination.tsx            # Navigasi halaman
│   │   ├── Dropdown.tsx              # Dropdown menu
│   │   ├── Avatar.tsx                # Avatar pengguna
│   │   └── StatCard.tsx              # Kartu statistik dashboard
│   │
│   ├── editor/
│   │   ├── RichTextEditor.tsx        # TipTap editor wrapper
│   │   ├── EditorToolbar.tsx         # Toolbar editor
│   │   ├── EditorSidebar.tsx         # Panel samping editor
│   │   ├── SlugInput.tsx             # Input slug otomatis
│   │   └── AutosaveIndicator.tsx     # Indikator autosave
│   │
│   └── media/
│       ├── MediaGrid.tsx             # Grid gallery media
│       ├── MediaListItem.tsx         # List item media
│       ├── MediaUploader.tsx         # Drag-drop upload zone
│       ├── MediaPreview.tsx          # Preview modal
│       └── MediaFilter.tsx           # Filter dan search media
│
├── features/
│   ├── auth/
│   │   ├── LoginPage.tsx
│   │   ├── RegisterPage.tsx
│   │   ├── ForgotPasswordPage.tsx
│   │   └── useAuth.ts               # Auth hook + Zustand store
│   │
│   ├── dashboard/
│   │   ├── DashboardPage.tsx         # Halaman dashboard utama
│   │   ├── StatOverview.tsx          # Ringkasan statistik
│   │   ├── ViewsChart.tsx            # Grafik views
│   │   ├── PostsChart.tsx            # Grafik publikasi
│   │   ├── RecentActivity.tsx        # Aktivitas terbaru
│   │   └── TopPosts.tsx              # Artikel terpopuler
│   │
│   ├── blog/
│   │   ├── BlogListPage.tsx          # Daftar artikel
│   │   ├── BlogCreatePage.tsx        # Buat artikel baru
│   │   ├── BlogEditPage.tsx          # Edit artikel
│   │   ├── BlogFilters.tsx           # Filter artikel
│   │   └── useBlogPosts.ts           # React Query hooks
│   │
│   ├── pages/
│   │   ├── PageListPage.tsx          # Daftar halaman
│   │   ├── PageCreatePage.tsx        # Buat halaman baru
│   │   └── PageEditPage.tsx          # Edit halaman
│   │
│   ├── media/
│   │   ├── MediaPage.tsx             # Halaman media library
│   │   └── useMedia.ts               # React Query hooks media
│   │
│   ├── comments/
│   │   ├── CommentsPage.tsx          # Halaman komentar
│   │   └── useComments.ts            # React Query hooks komentar
│   │
│   └── settings/
│       ├── SettingsPage.tsx          # Pengaturan umum
│       ├── ProfilePage.tsx           # Profil pengguna
│       └── TeamPage.tsx              # Manajemen tim
│
├── hooks/
│   ├── useDebounce.ts                # Debounce hook
│   ├── useMediaQuery.ts              # Responsive breakpoints
│   ├── useAutosave.ts                # Autosave hook
│   └── usePagination.ts              # Pagination state
│
├── stores/
│   ├── useSidebarStore.ts            # State sidebar (collapse, mobile)
│   ├── useNotificationStore.ts       # State notifikasi
│   └── useAuthStore.ts               # State autentikasi
│
├── types/
│   ├── blog.ts                       # BlogPost, Kategori, Tag interfaces
│   ├── page.ts                       # Halaman interface
│   ├── media.ts                      # MediaItem interface
│   ├── comment.ts                    # Komentar interface
│   ├── user.ts                       # Penulis/User interface
│   └── common.ts                     # Shared types (Pagination, Sort, dll)
│
├── mocks/
│   ├── browser.ts                    # MSW browser setup
│   ├── handlers/
│   │   ├── blogHandlers.ts           # Handler API blog
│   │   ├── pageHandlers.ts           # Handler API halaman
│   │   ├── mediaHandlers.ts          # Handler API media
│   │   ├── commentHandlers.ts        # Handler API komentar
│   │   └── authHandlers.ts           # Handler API autentikasi
│   └── data/
│       ├── blogPosts.ts              # Mock data artikel
│       ├── pages.ts                  # Mock data halaman
│       ├── media.ts                  # Mock data media
│       ├── comments.ts               # Mock data komentar
│       └── users.ts                  # Mock data pengguna
│
└── utils/
    ├── formatters.ts                 # Format tanggal, angka, dll
    ├── validators.ts                 # Validasi form dengan Zod
    ├── slugify.ts                    # Generate slug dari judul
    └── constants.ts                  # Konstanta global
```

## 4. Data Layer

### State Management Strategy

**Zustand Stores (Client State):**
```typescript
// useSidebarStore — UI state untuk sidebar
interface SidebarState {
  isCollapsed: boolean;
  isMobileOpen: boolean;
  activeMenu: string;
  toggleCollapse: () => void;
  setActiveMenu: (menu: string) => void;
}

// useNotificationStore — notifikasi dan toast
interface NotificationState {
  notifications: Notification[];
  unreadCount: number;
  addNotification: (n: Notification) => void;
  markAsRead: (id: string) => void;
}

// useAuthStore — state autentikasi lokal
interface AuthState {
  user: Penulis | null;
  token: string | null;
  isAuthenticated: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}
```

**TanStack Query (Server State):**
```typescript
// useBlogPosts — React Query hooks untuk data blog
function useBlogPosts(filters: BlogFilters) {
  return useQuery({
    queryKey: ['blog-posts', filters],
    queryFn: () => fetchBlogPosts(filters),
    staleTime: 30_000,     // 30 detik sebelum data dianggap stale
    gcTime: 5 * 60_000,    // 5 menit di cache
  });
}

function useCreateBlogPost() {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: (post: CreateBlogPostDTO) => createPost(post),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['blog-posts'] });
    },
  });
}
```

### Mock Strategy
1. MSW diinisialisasi di `main.tsx` hanya saat `import.meta.env.DEV === true`
2. Setiap handler mengembalikan data dari folder `src/mocks/data/`
3. MSW mendukung delay realistis (200-800ms) untuk simulasi network latency
4. State mutation di mock menggunakan Map/Array in-memory agar CRUD realistis
5. Data di-reset setiap kali HMR trigger (development refresh)

```typescript
// Contoh MSW handler untuk blog posts
import { http, HttpResponse, delay } from 'msw'
import { mockBlogPosts } from '../data/blogPosts'

const posts = new Map(mockBlogPosts.map(p => [p.id, p]));

export const blogHandlers = [
  http.get('/api/v1/posts', async ({ request }) => {
    await delay(300);
    const url = new URL(request.url);
    const status = url.searchParams.get('status') || 'all';
    const search = url.searchParams.get('search') || '';
    
    let filtered = Array.from(posts.values());
    if (status !== 'all') filtered = filtered.filter(p => p.status === status);
    if (search) filtered = filtered.filter(p => p.judul.toLowerCase().includes(search));
    
    return HttpResponse.json({ data: filtered, total: filtered.length });
  }),

  http.post('/api/v1/posts', async ({ request }) => {
    await delay(400);
    const body = await request.json() as any;
    const newPost = { ...body, id: crypto.randomUUID(), createdAt: new Date(), views: 0 };
    posts.set(newPost.id, newPost);
    return HttpResponse.json({ data: newPost }, { status: 201 });
  }),

  http.patch('/api/v1/posts/:id/publish', async ({ params }) => {
    await delay(200);
    const post = posts.get(params.id as string);
    if (!post) return HttpResponse.json({ error: 'Not found' }, { status: 404 });
    post.status = 'terbit';
    post.publishedAt = new Date();
    return HttpResponse.json({ data: post });
  }),
];
```

## 5. Performance Requirements

| Metrik | Target | Alat Ukur | Strategi |
|--------|--------|-----------|----------|
| First Contentful Paint (FCP) | < 1.5 detik | Lighthouse | Lazy loading routes, code splitting per fitur |
| Largest Contentful Paint (LCP) | < 2.5 detik | Lighthouse | Optimasi gambar, preload critical assets |
| Time to Interactive (TTI) | < 3 detik | Lighthouse | Code splitting, deferred JS |
| Bundle Size (initial) | < 150 KB JS gzipped | Vite bundle analysis | Dynamic import untuk editor, charts |
| Bundle Size (total) | < 300 KB JS gzipped | Vite bundle analysis | Tree-shaking, manual chunks |
| API Response (mock) | < 300 ms simulated | MSW delay config | Data pre-loaded di memory Map |
| Image Load | < 2 detik untuk 100 KB | Web Vitals | WebP format, lazy loading, blur placeholder |
| Runtime | 60 FPS saat scrolling/animasi | Chrome DevTools Performance | `will-change` untuk animasi, virtualisasi tabel |

**Code Splitting Strategy:**
```typescript
// Lazy load rute agar bundle awal kecil
const BlogListPage = lazy(() => import('@/features/blog/BlogListPage'));
const BlogCreatePage = lazy(() => import('@/features/blog/BlogCreatePage'));
const MediaPage = lazy(() => import('@/features/media/MediaPage'));
const CommentsPage = lazy(() => import('@/features/comments/CommentsPage'));

// Import editor hanya saat diperlukan
const RichTextEditor = lazy(() => import('@/components/editor/RichTextEditor'));
```

## 6. Styling & Theming System

**CSS Custom Properties (Tailwind + CSS Variables):**
```css
/* styles/theme.css */
:root {
  /* Primary */
  --color-primary: #0B6E4F;
  --color-primary-light: #E8F5E9;
  --color-primary-dark: #08543C;
  --color-primary-hover: #0A5E44;
  
  /* Secondary */
  --color-secondary: #F4A100;
  --color-secondary-light: #FFF3D6;
  
  /* Neutral */
  --color-bg: #F8FAFC;
  --color-surface: #FFFFFF;
  --color-text: #1E293B;
  --color-text-secondary: #64748B;
  --color-border: #E2E8F0;
  
  /* Semantic */
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-danger: #DC2626;
  --color-info: #3B82F6;
  
  /* Typography */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-heading: 'Plus Jakarta Sans', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  
  /* Spacing */
  --sidebar-width: 256px;
  --sidebar-collapsed-width: 64px;
  --topbar-height: 64px;
  --bottombar-height: 32px;
  
  /* Radius */
  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 8px;
  --radius-xl: 12px;
  
  /* Shadows */
  --shadow-card: 0 1px 3px rgba(0, 0, 0, 0.08), 0 1px 2px rgba(0, 0, 0, 0.06);
  --shadow-dropdown: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-modal: 0 20px 25px rgba(0, 0, 0, 0.15);
}

/* Tailwind config extension */
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: 'var(--color-primary)',
          light: 'var(--color-primary-light)',
          dark: 'var(--color-primary-dark)',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
        heading: ['Plus Jakarta Sans', 'sans-serif'],
        mono: ['JetBrains Mono', 'monospace'],
      },
    },
  },
};
```

## 7. Validation Rules

| Field | Rule | Pesan Error |
|-------|------|-------------|
| Judul artikel | Required, min 5, max 200 karakter | "Judul harus diisi (5-200 karakter)" |
| Slug | Required, lowercase, hanya huruf/angka/dash | "Slug hanya boleh mengandung huruf, angka, dan tanda strip" |
| Slug | Unique per-tenant | "Slug sudah digunakan, gunakan slug lain" |
| Konten | Required, min 10 karakter | "Konten tidak boleh kosong" |
| Email | Format email valid | "Format email tidak valid" |
| Password | Min 8 karakter, ada huruf dan angka | "Password minimal 8 karakter dengan kombinasi huruf dan angka" |
| Nama kategori | Required, min 2, max 50 karakter | "Nama kategori harus diisi (2-50 karakter)" |
| File upload | Maks 50 MB | "Ukuran file maksimal 50 MB" |
| File upload | Format: jpg, png, webp, pdf, doc, mp4 | "Format file tidak didukung" |
| Meta title | Maks 60 karakter | "Meta title maksimal 60 karakter" |
| Meta description | Maks 160 karakter | "Meta description maksimal 160 karakter" |
| Tanggal jadwal | Harus di masa depan | "Tanggal publikasi harus di masa depan" |

## 8. Error Handling Strategy

**Global Error Boundary:**
```typescript
// components/common/ErrorBoundary.tsx
class DashboardErrorBoundary extends React.Component {
  state = { hasError: false, error: null };
  
  static getDerivedStateFromError(error: Error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error: Error, info: React.ErrorInfo) {
    // Log error ke monitoring service (Sentry)
    console.error('Dashboard Error:', error, info);
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div className="flex flex-col items-center justify-center p-8">
          <AlertCircle className="w-16 h-16 text-danger mb-4" />
          <h2 className="text-xl font-heading font-semibold mb-2">
            Terjadi Kesalahan
          </h2>
          <p className="text-text-secondary mb-4">
            Kami mengalami masalah teknis. Silakan coba muat ulang halaman.
          </p>
          <Button onClick={() => window.location.reload()}>
            Muat Ulang Halaman
          </Button>
        </div>
      );
    }
    return this.props.children;
  }
}
```

**Error Handling Patterns:**

1. **API Errors:** TanStack Query `onError` callback menampilkan toast error
2. **Network Errors:** Deteksi `navigator.onLine` + event listener, banner "Koneksi Terputus"
3. **Validation Errors:** React Hook Form menampilkan error per-field, berwarna merah
4. **404 Routes:** Route catch-all dengan komponen "Halaman Tidak Ditemukan"
5. **403 Forbidden:** Middleware routing cek role, redirect ke dashboard dengan toast error
6. **File Upload Errors:** Progress bar berubah merah, pesan error spesifik
7. **Autosave Conflicts:** Modal konfirmasi dengan opsi "Gunakan Versi Ini" atau "Kembali ke Versi Sebelumnya"

## 9. Security Considerations

| Aspek | Implementasi |
|-------|--------------|
| **XSS Protection** | React sanitasi otomatis. Untuk konten HTML (TipTap), gunakan DOMPurify sebelum render |
| **CSRF Protection** | Token CSRF di header untuk semua mutation request. SameSite cookie |
| **SQL Injection** | Parameterized queries di backend (tidak relevan di frontend mock, tapi dokumentasi untuk API) |
| **Authentication** | JWT dengan refresh token. Token disimpan di httpOnly cookie, bukan localStorage |
| **Authorization** | Role-based access control (RBAC) di setiap API endpoint. Frontend hiding button saja tidak cukup |
| **Rate Limiting** | API dibatasi 100 request/menit per user. Endpoint login: 5 percobaan per menit |
| **Upload Security** | Validasi MIME type + scan virus. Eksekusi file dilarang. Ekstensi terbatas |
| **Password** | Tidak pernah disimpan di log. Frontend tidak menerima password dalam response API |
| **Session** | Session timeout setelah 30 menit inactive. Refresh token 7 hari |
| **Data Privacy** | Data antar tenant (instansi berbeda) diisolasi. Tidak ada cross-tenant data leak |
| **Audit Log** | Setiap perubahan konten tercatat: siapa, apa, kapan |

## 10. Deployment & Build

**Build Configuration:**
```typescript
// vite.config.ts
export default defineConfig({
  plugins: [react()],
  build: {
    target: 'es2020',
    outDir: 'dist',
    sourcemap: false,
    minify: 'terser',
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom', 'react-router-dom'],
          editor: ['@tiptap/react', '@tiptap/starter-kit'],
          charts: ['recharts'],
          table: ['@tanstack/react-table'],
        },
      },
    },
  },
});
```

**Deployment Pipeline:**
1. Developer push ke branch `dev` → GitHub Actions trigger
2. Run lint + type check + unit test
3. Build production bundle (`vite build`)
4. Deploy ke staging (Vercel Preview / Netlify Deploy Preview)
5. Jika PR ke `main`: deploy ke production
6. CDN cache invalidation setelah deploy

**Environment Variables:**
```env
VITE_API_BASE_URL=https://api.nusantaracms.com/v1
VITE_APP_NAME=NusantaraCMS
VITE_MSW_ENABLED=true               # Development only
VITE_SENTRY_DSN=                    # Error tracking
VITE_GA_TRACKING_ID=                # Analytics
VITE_DEFAULT_LOCALE=id
```

**CI/CD Pipeline (GitHub Actions):**
```yaml
name: Deploy NusantaraCMS
on:
  push:
    branches: [main, dev]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20 }
      - run: npm ci
      - run: npm run type-check
      - run: npm run lint
      - run: npm run test
  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
      - run: npx vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
```

## 11. Testing Strategy

| Level | Tools | Target Coverage | Apa yang Diuji |
|-------|-------|-----------------|----------------|
| Unit Test | Vitest + React Testing Library | > 80% | Fungsi utilitas, hooks, validasi, state logic |
| Component Test | Vitest + RTL + user-event | > 70% | Render komponen, interaksi user, state changes |
| Integration Test | Vitest + MSW | > 60% | Alur data lengkap dari UI ke mock API dan kembali |
| E2E Test | Playwright | Critical paths | Login → Buat artikel → Publikasi, CRUD lengkap |
| Visual Regression | Playwright + Percy | Halaman utama | Dashboard, editor, tabel per halaman utama |
| Accessibility | axe-core + Playwright | Semua halaman | WCAG AA compliance |

**Contoh Test:**
```typescript
// tests/features/blog/BlogListPage.test.tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { BlogListPage } from '@/features/blog/BlogListPage';
import { QueryClientProvider, QueryClient } from '@tanstack/react-query';

describe('BlogListPage', () => {
  it('menampilkan daftar artikel', async () => {
    render(<BlogListPage />, { wrapper });
    
    expect(await screen.findByText('Transformasi Digital Pelayanan Publik')).toBeInTheDocument();
    expect(screen.getByText('Cara Membuat Website Desa')).toBeInTheDocument();
  });

  it('dapat memfilter artikel berdasarkan status', async () => {
    render(<BlogListPage />, { wrapper });
    const user = userEvent.setup();
    
    await user.click(screen.getByText('Draf'));
    expect(screen.getByText('Cara Membuat Website Desa')).toBeInTheDocument();
    expect(screen.queryByText('Transformasi Digital Pelayanan Publik')).not.toBeInTheDocument();
  });

  it('menampilkan empty state saat tidak ada artikel', async () => {
    // Mock empty data
    render(<BlogListPage />, { wrapper });
    expect(await screen.findByText('Belum ada artikel. Klik Tulis Konten Baru untuk memulai.')).toBeInTheDocument();
  });
});
```

## 12. Dependencies & Constraints

**Eksternal Dependencies:**
- **Node.js** >= 18.x (runtime development)
- **npm** >= 9.x (package manager)
- Browser modern dengan ES2020 support

**Constraint Teknis:**
- Rich text editor content disimpan sebagai HTML string (TipTap output)
- Semua text direction LTR (tidak perlu RTL support untuk Bahasa Indonesia)
- Font loading via Google Fonts dengan fallback system fonts
- Tidak ada dukungan offline penuh (hanya cache baca dengan service worker)

**Daftar npm Dependencies (utama):**
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.0",
    "@tanstack/react-query": "^5.12.0",
    "@tanstack/react-table": "^8.10.0",
    "zustand": "^4.4.0",
    "@tiptap/react": "^2.1.0",
    "@tiptap/starter-kit": "^2.1.0",
    "@tiptap/extension-image": "^2.1.0",
    "@tiptap/extension-link": "^2.1.0",
    "lucide-react": "^0.303.0",
    "date-fns": "^3.3.0",
    "react-hook-form": "^7.49.0",
    "zod": "^3.22.0",
    "recharts": "^2.10.0",
    "framer-motion": "^10.18.0",
    "dompurify": "^3.0.0",
    "react-hot-toast": "^2.4.0",
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
    "playwright": "^1.40.0"
  }
}
```
