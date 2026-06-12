# TRD: KolabPro — Technical Requirements
**Project:** KolabPro

## 1. Technical Strategy
KolabPro dibangun sebagai **Single-Page Application (SPA)** dengan arsitektur berorientasi komponen yang mengutamakan pengalaman drag-and-drop interaktif dan multiple view rendering.

Pendekatan teknis:
- **Mock-first development** — semua fitur dikembangkan dengan MSW mencegat API calls dan mengembalikan mock data in-memory
- **Optimistic UI** — state diupdate secara optimis di frontend sebelum konfirmasi server, dengan rollback jika gagal
- **View abstraction** — setiap view (Board, List, Timeline, Calendar) adalah komponen independen yang menggunakan data source yang sama (tasks dari store)
- **dnd-kit** — library drag-and-drop modern yang ringan, accessible, dan mendukung touch events
- **Modular state** — Zustand store dipisah per domain (workspace, proyek, task, UI) untuk menghindari re-render tidak perlu

Strategi mock: MSW di development menggunakan data generator yang menghasilkan workspace, proyek, task, komentar, dan anggota yang saling terhubung (relasional). State mutasi (CRUD) menggunakan Map in-memory sehingga perubahan task, drag-and-drop, dan komentar benar-benar berfungsi secara realistis.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Build Tool | Vite | 5.x | Fast HMR, optimized build |
| UI Library | React | 18.x | Functional components + Hooks |
| Bahasa | TypeScript | 5.x | Strict mode |
| Styling | Tailwind CSS | 3.x | Utility-first, JIT mode |
| Routing | React Router | 6.x | Nested routes, layout routes |
| State (Client) | Zustand | 4.x | Lightweight global state |
| State (Server) | TanStack Query | 5.x | Server state cache, mutations |
| Drag & Drop | dnd-kit | 1.x | @dnd-kit/core, @dnd-kit/sortable |
| Table | TanStack Table | 8.x | List view with sorting |
| Icons | Lucide React | 0.x | Consistent icon set |
| Date Utils | date-fns | 3.x | With `id` locale |
| Form Validation | React Hook Form + Zod | 7.x / 3.x | Task & project forms |
| Rich Text (optional) | TipTap | 2.x | Untuk deskripsi task |
| HTTP Mocking | MSW | 2.x | API mocking in development |
| Animations | Framer Motion | 10.x | Layout transitions, drag feedback |
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
│   │   ├── MainLayout.tsx            # Layout utama (sidebar + topbar + content + panel)
│   │   ├── Sidebar.tsx               # Sidebar navigasi
│   │   ├── SidebarItem.tsx           # Item navigasi individual
│   │   ├── WorkspaceSwitcher.tsx     # Dropdown pilih workspace
│   │   ├── ProjectList.tsx           # Daftar proyek dalam sidebar
│   │   ├── Topbar.tsx                # Top bar dengan breadcrumb + view switcher
│   │   ├── ViewSwitcher.tsx          # Board | List | Timeline | Calendar toggle
│   │   ├── FilterBar.tsx             # Filter chips (assignee, priority, label)
│   │   ├── SearchInput.tsx           # Global search
│   │   ├── Avatar.tsx                # Avatar komponen
│   │   ├── AvatarStack.tsx           # Stack avatar untuk multiple assignee
│   │   └── BottomBar.tsx             # Status bar (task count, overdue, progress)
│   │
│   ├── common/
│   │   ├── Button.tsx                # Tombol reusable
│   │   ├── Modal.tsx                 # Dialog modal
│   │   ├── ConfirmDialog.tsx         # Dialog konfirmasi
│   │   ├── EmptyState.tsx            # State kosong
│   │   ├── LoadingSkeleton.tsx       # Skeleton loading
│   │   ├── ErrorState.tsx            # State error
│   │   ├── Toast.tsx                 # Notifikasi toast
│   │   ├── Badge.tsx                 # Badge status
│   │   ├── PriorityBadge.tsx         # Badge prioritas (4 level)
│   │   ├── StatusBadge.tsx           # Badge status task
│   │   ├── LabelChip.tsx             # Chip label/tag
│   │   ├── ProgressBar.tsx           # Progress bar
│   │   ├── Dropdown.tsx              # Dropdown menu
│   │   ├── DatePicker.tsx            # Date picker komponen
│   │   ├── SearchInput.tsx           # Input pencarian
│   │   └── KebabMenu.tsx             # Three-dot menu aksi
│   │
│   ├── board/
│   │   ├── KanbanBoard.tsx           # Board container (DndContext)
│   │   ├── KanbanColumn.tsx          # Kolom board (sortable context)
│   │   ├── TaskCard.tsx              # Card task draggable
│   │   ├── TaskCardCompact.tsx       # Card task ringkas (untuk mobile)
│   │   ├── ColumnHeader.tsx          # Header kolom dengan counter
│   │   ├── EmptyColumn.tsx           # State kosong untuk kolom
│   │   └── QuickAddTask.tsx          # Input inline add task
│   │
│   ├── task/
│   │   ├── TaskDetailPanel.tsx       # Panel detail task (right panel)
│   │   ├── TaskDetailHeader.tsx      # Header panel: judul + close + aksi
│   │   ├── TaskStatusSelector.tsx    # Dropdown ubah status
│   │   ├── TaskAssigneeSelector.tsx  # Pilih assignee
│   │   ├── TaskPrioritySelector.tsx  # Pilih prioritas
│   │   ├── TaskDeadlinePicker.tsx    # Date picker deadline
│   │   ├── TaskLabels.tsx            # Manajemen label dalam task
│   │   ├── TaskDescription.tsx       # Deskripsi task (edit/view)
│   │   ├── TaskChecklist.tsx         # Checklist items
│   │   ├── ChecklistItem.tsx         # Item checklist individual
│   │   ├── TaskAttachments.tsx       # Daftar attachment
│   │   ├── TaskActivityLog.tsx       # Activity log
│   │   ├── TaskFormModal.tsx         # Modal form create/edit task lengkap
│   │   └── TaskFormInline.tsx        # Form create cepat inline
│   │
│   ├── comments/
│   │   ├── CommentSection.tsx        # Section komentar dalam panel task
│   │   ├── CommentItem.tsx           # Item komentar individual
│   │   ├── CommentInput.tsx          # Input komentar + mention
│   │   └── MentionSuggestions.tsx    # Dropdown suggestion mention
│   │
│   ├── views/
│   │   ├── BoardView.tsx             # View: Kanban board
│   │   ├── ListView.tsx              # View: Table list
│   │   ├── TimelineView.tsx          # View: Gantt chart
│   │   └── CalendarView.tsx          # View: Calendar
│   │
│   ├── timeline/
│   │   ├── GanttChart.tsx            # Gantt chart component
│   │   ├── GanttTaskBar.tsx          # Bar task dalam gantt
│   │   ├── GanttHeader.tsx           # Header timeline (hari/minggu/bulan)
│   │   ├── GanttTodayLine.tsx        # Garis indikator hari ini
│   │   └── TimelineZoom.tsx          # Zoom controls
│   │
│   ├── calendar/
│   │   ├── CalendarGrid.tsx          # Grid kalender
│   │   ├── CalendarDay.tsx           # Cell hari
│   │   └── CalendarTaskCard.tsx      # Task card dalam kalender
│   │
│   ├── project/
│   │   ├── ProjectHeader.tsx         # Header proyek (nama, progress, aksi)
│   │   ├── ProjectForm.tsx           # Form create/edit proyek
│   │   └── ProjectDashboard.tsx      # Dashboard overview proyek
│   │
│   └── members/
│       ├── MemberList.tsx            # Daftar anggota
│       ├── MemberCard.tsx            # Card anggota
│       ├── InviteMemberForm.tsx      # Form undang anggota
│       └── RoleSelector.tsx          # Selector role anggota
│
├── features/
│   ├── workspace/
│   │   ├── WorkspacePage.tsx         # Halaman workspace
│   │   └── WorkspaceSettings.tsx     # Pengaturan workspace
│   │
│   ├── project/
│   │   ├── ProjectPage.tsx           # Halaman proyek dengan view
│   │   └── ProjectSettings.tsx       # Pengaturan proyek
│   │
│   └── dashboard/
│       └── DashboardPage.tsx         # Dashboard utama
│
├── hooks/
│   ├── useWorkspace.ts               # React Query workspace
│   ├── useProject.ts                 # React Query proyek
│   ├── useTasks.ts                   # React Query tasks
│   ├── useTaskMutations.ts           # Mutation task (create, update, delete, drag)
│   ├── useComments.ts                # React Query komentar
│   ├── useMembers.ts                 # React Query anggota
│   ├── useDebounce.ts                # Debounce untuk search
│   ├── useMediaQuery.ts              # Responsive breakpoints
│   ├── useAutosave.ts                # Autosave debounced
│   └── useDragAndDrop.ts             # dnd-kit custom hook
│
├── stores/
│   ├── useWorkspaceStore.ts          # Active workspace state
│   ├── useProjectStore.ts            # Active project state
│   ├── useTaskStore.ts               # Optimistic task state (drag)
│   ├── useUIStore.ts                 # Sidebar, panel, view state
│   └── useFilterStore.ts             # Filter state (assignee, priority, label)
│
├── types/
│   ├── workspace.ts                  # Workspace interface
│   ├── project.ts                    # Proyek interface
│   ├── task.ts                       # Task, ChecklistItem, Label interface
│   ├── comment.ts                    # Komentar interface
│   ├── member.ts                     # AnggotaTim interface
│   ├── attachment.ts                 # Lampiran interface
│   └── common.ts                     # Shared types (Status, Priority, View, Role)
│
├── mocks/
│   ├── browser.ts                    # MSW browser setup
│   ├── handlers/
│   │   ├── workspaceHandlers.ts      # API handler workspace
│   │   ├── projectHandlers.ts        # API handler proyek
│   │   ├── taskHandlers.ts           # API handler task
│   │   ├── commentHandlers.ts        # API handler komentar
│   │   └── memberHandlers.ts         # API handler anggota
│   └── data/
│       ├── workspaces.ts             # Mock workspace
│       ├── projects.ts               # Mock proyek
│       ├── tasks.ts                  # Mock task
│       ├── comments.ts               # Mock komentar
│       └── members.ts               # Mock anggota
│
└── utils/
    ├── formatters.ts                 # Format tanggal, angka relative
    ├── taskCalculators.ts            # Progress, overdue, count calculations
    ├── statusFlow.ts                 # Status flow logic dan validasi
    └── constants.ts                  # Warna status, priority, role labels
```

## 4. Data Layer

### State Management Strategy

**Zustand Stores (Client State):**
```typescript
// useWorkspaceStore — workspace yang sedang aktif
interface WorkspaceState {
  activeWorkspaceId: string | null;
  workspaces: Workspace[];
  setActiveWorkspace: (id: string) => void;
}

// useUIStore — UI state global
interface UIState {
  sidebarCollapsed: boolean;
  rightPanelOpen: boolean;
  selectedTaskId: string | null;
  activeView: 'board' | 'list' | 'timeline' | 'calendar';
  toggleSidebar: () => void;
  openTaskDetail: (taskId: string) => void;
  closeTaskDetail: () => void;
  setView: (view: UIState['activeView']) => void;
}

// useFilterStore — filter untuk task
interface FilterState {
  assigneeId: string | null;
  priority: string | null;
  labelId: string | null;
  searchQuery: string;
  setAssignee: (id: string | null) => void;
  setPriority: (p: string | null) => void;
  setLabel: (id: string | null) => void;
  setSearch: (q: string) => void;
  clearFilters: () => void;
}
```

**Optimistic Updates untuk Drag-and-Drop:**
```typescript
// hooks/useTaskMutations.ts — mutation untuk drag task
function useMoveTask() {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: ({ taskId, newStatus, newPosition }: MoveTaskParams) =>
      moveTaskApi(taskId, newStatus, newPosition),
    
    // Optimistic update: update status di cache sebelum server response
    onMutate: async ({ taskId, newStatus, newPosition }) => {
      // Cancel outgoing queries
      await queryClient.cancelQueries({ queryKey: ['tasks'] });
      
      // Snapshot previous state
      const previousTasks = queryClient.getQueryData(['tasks']);
      
      // Optimistically update cache
      queryClient.setQueryData(['tasks'], (old: Task[]) =>
        old.map(t => t.id === taskId ? { ...t, status: newStatus, posisi: newPosition } : t)
      );
      
      return { previousTasks };
    },
    
    // Rollback if error
    onError: (_, __, context) => {
      queryClient.setQueryData(['tasks'], context?.previousTasks);
      toast.error('Gagal memindahkan task.');
    },
    
    // Refetch to ensure consistency
    onSettled: () => {
      queryClient.invalidateQueries({ queryKey: ['tasks'] });
    },
  });
}
```

### Mock Strategy
MSW handlers mendukung CRUD penuh dengan penyimpanan in-memory:

```typescript
// mocks/handlers/taskHandlers.ts
const tasks = new Map<string, Task>(mockTasks.map(t => [t.id, { ...t }]));

export const taskHandlers = [
  // GET /api/v1/tasks?projectId=xxx
  http.get('/api/v1/tasks', async ({ request }) => {
    await delay(200);
    const url = new URL(request.url);
    const projectId = url.searchParams.get('projectId');
    const status = url.searchParams.get('status');
    const assignee = url.searchParams.get('assignee');
    
    let result = Array.from(tasks.values())
      .filter(t => t.proyekId === projectId);
    
    if (status) result = result.filter(t => t.status === status);
    if (assignee) result = result.filter(t => t.penanggungJawabId === assignee);
    
    return HttpResponse.json({ data: result, total: result.length });
  }),

  // PATCH /api/v1/tasks/:id (untuk drag-and-drop / edit)
  http.patch('/api/v1/tasks/:id', async ({ params, request }) => {
    await delay(150);
    const body = await request.json() as Partial<Task>;
    const task = tasks.get(params.id as string);
    if (!task) return HttpResponse.json({ error: 'Task not found' }, { status: 404 });
    
    const updated = { ...task, ...body, updatedAt: new Date() };
    tasks.set(task.id, updated);
    return HttpResponse.json({ data: updated });
  }),

  // POST /api/v1/tasks
  http.post('/api/v1/tasks', async ({ request }) => {
    await delay(300);
    const body = await request.json() as any;
    const newTask: Task = {
      ...body,
      id: crypto.randomUUID(),
      status: 'belum',
      createdAt: new Date(),
      updatedAt: new Date(),
    };
    tasks.set(newTask.id, newTask);
    return HttpResponse.json({ data: newTask }, { status: 201 });
  }),

  // DELETE /api/v1/tasks/:id
  http.delete('/api/v1/tasks/:id', async ({ params }) => {
    await delay(200);
    tasks.delete(params.id as string);
    return HttpResponse.json({ success: true });
  }),
];
```

## 5. Performance Requirements

| Metrik | Target | Alat Ukur | Strategi |
|--------|--------|-----------|----------|
| First Contentful Paint (FCP) | < 1.5 detik | Lighthouse | Lazy loading routes |
| Largest Contentful Paint (LCP) | < 2.5 detik | Lighthouse | Preload critical assets |
| Drag-and-drop response | < 50 ms | Performance.now() | Optimistic update, no await |
| Kanban render (100 task) | < 1 detik | Chrome DevTools | Virtualisasi kolom (hanya render visible cards) |
| List view render (200 tasks) | < 500 ms | TanStack Table | Virtual row rendering |
| Timeline render (100 tasks) | < 1.5 detik | Performance.now() | Memoized Gantt bars |
| Search response (500 tasks) | < 200 ms | Performance.now() | Debounce 300ms + index |
| Bundle size (initial) | < 120 KB gzipped | Vite bundle analysis | Code splitting per view |
| Bundle size (total) | < 250 KB gzipped | Vite bundle analysis | Dynamic import editor, timeline |

**Code Splitting Strategy:**
```typescript
// Lazy load view components (berat karena dnd-kit dan chart)
const BoardView = lazy(() => import('@/components/views/BoardView'));
const ListView = lazy(() => import('@/components/views/ListView'));
const TimelineView = lazy(() => import('@/components/views/TimelineView'));
const CalendarView = lazy(() => import('@/components/views/CalendarView'));

// Lazy load panel detail
const TaskDetailPanel = lazy(() => import('@/components/task/TaskDetailPanel'));
```

## 6. Styling & Theming System

```css
/* styles/theme.css */
:root {
  /* Brand */
  --color-primary: #2563EB;
  --color-primary-light: #DBEAFE;
  --color-primary-dark: #1D4ED8;
  --color-primary-hover: #3B82F6;
  
  /* Accent */
  --color-accent: #8B5CF6;
  --color-accent-light: #EDE9FE;
  
  /* Semantic */
  --color-success: #10B981;
  --color-success-light: #D1FAE5;
  --color-warning: #F59E0B;
  --color-warning-light: #FEF3C7;
  --color-danger: #EF4444;
  --color-danger-light: #FEE2E2;
  --color-info: #3B82F6;
  --color-info-light: #DBEAFE;
  
  /* Neutral */
  --color-bg: #F8FAFC;
  --color-surface: #FFFFFF;
  --color-surface-hover: #F1F5F9;
  --color-text-primary: #0F172A;
  --color-text-secondary: #475569;
  --color-text-muted: #94A3B8;
  --color-border: #E2E8F0;
  --color-border-hover: #CBD5E1;
  
  /* Column */
  --color-column-bg: #F1F5F9;
  
  /* Status Colors */
  --color-status-belum: #94A3B8;
  --color-status-dikerjakan: #3B82F6;
  --color-status-tinjau: #F59E0B;
  --color-status-selesai: #10B981;
  
  /* Priority Colors */
  --color-priority-rendah: #94A3B8;
  --color-priority-sedang: #8B5CF6;
  --color-priority-tinggi: #3B82F6;
  --color-priority-urgent: #EF4444;
  
  /* Typography */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-heading: 'Plus Jakarta Sans', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  
  /* Layout */
  --sidebar-width: 240px;
  --sidebar-collapsed-width: 64px;
  --topbar-height: 56px;
  --bottombar-height: 32px;
  --right-panel-width: 380px;
  
  /* Radius */
  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 8px;
  
  /* Shadows */
  --shadow-card: 0 1px 3px rgba(0, 0, 0, 0.08);
  --shadow-drag: 0 10px 15px rgba(0, 0, 0, 0.15);
  --shadow-panel: -4px 0 10px rgba(0, 0, 0, 0.1);
  
  /* Transitions */
  --transition-drag: 150ms ease;
  --transition-panel: 200ms ease;
}
```

## 7. Validation Rules

| Field | Rule | Pesan Error |
|-------|------|-------------|
| Judul task | Required, min 2, max 200 karakter | "Judul task harus diisi (2-200 karakter)" |
| Nama proyek | Required, min 3, max 100 karakter | "Nama proyek harus diisi (3-100 karakter)" |
| Nama workspace | Required, min 3, max 100 karakter | "Nama workspace harus diisi" |
| Deadline | Harus valid date | "Format tanggal tidak valid" |
| Deadline | Jika di masa lalu, konfirmasi | "Deadline yang dipilih sudah lewat" |
| Email anggota | Format email valid | "Format email tidak valid" |
| Checklist item | Required, min 1, max 100 karakter | "Item checklist tidak boleh kosong" |
| File attachment | Maks 10 MB per file | "Ukuran file maksimal 10 MB" |
| File attachment | Format: jpg, png, pdf, doc, xls, zip | "Format file tidak didukung" |
| Label nama | Required, min 1, max 30 karakter | "Nama label harus diisi" |
| Komentar | Required, min 1 karakter | "Komentar tidak boleh kosong" |

## 8. Error Handling Strategy

**Optimistic Update Rollback:**
```typescript
// Pattern untuk semua mutation drag-and-drop
// 1. Snapshot state saat ini
// 2. Update UI secara optimis
// 3. Kirim request ke server
// 4. Jika sukses: invalidasi cache biar konsisten
// 5. Jika gagal: rollback ke snapshot + tampilkan toast error

// Error handling untuk komentar:
// Jika gagal kirim komentar, simpan draft di localStorage
// Tampilkan: "Komentar gagal terkirim. [Simpan Draft]" 
```

**Error Categories:**

1. **Drag-and-drop failures** — rollback optimis, toast: "Gagal memindahkan task"
2. **API/Server errors** — error boundary per halaman, retry logic
3. **Network errors** — banner "Koneksi terputus", data tetap tersimpan di local state
4. **Validation errors** — inline error messages di form
5. **Permission errors** — "Anda tidak memiliki izin untuk melakukan aksi ini"
6. **File upload errors** — progress bar merah, pesan spesifik (ukuran/format)
7. **Concurrent edit conflicts** — "Task ini telah diubah oleh pengguna lain. Muat ulang?"

## 9. Security Considerations

| Aspek | Implementasi |
|-------|--------------|
| **Authentication** | JWT token, httpOnly cookie |
| **Authorization (RBAC)** | Pemilik: all access. Admin: manage members, edit all. Anggota: create/edit own. Pengamat: read only |
| **Data Isolation** | Multi-tenant: setiap workspace data terpisah, tidak ada cross-workspace access |
| **XSS Protection** | React sanitasi. Untuk deskripsi (HTML): gunakan DOMPurify |
| **File Upload** | Validasi MIME type, scan virus, penyimpanan terpisah (S3) |
| **Rate Limiting** | API: 100 request/menit per user. Drag-and-drop (PATCH): 30 request/menit |
| **Audit Log** | Activity log mencatat: siapa, aksi apa, kapan, data sebelum-sesudah |

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
    rollupOptions: {
      output: {
        manualChunks: {
          'react-vendor': ['react', 'react-dom', 'react-router-dom'],
          'dnd-kit': ['@dnd-kit/core', '@dnd-kit/sortable', '@dnd-kit/utilities'],
          'table': ['@tanstack/react-table'],
        },
      },
    },
  },
});
```

**Environment Variables:**
```env
VITE_API_BASE_URL=https://api.kolabpro.com/v1
VITE_APP_NAME=KolabPro
VITE_MSW_ENABLED=true
VITE_DEFAULT_LOCALE=id
```

**CI/CD:**
```yaml
name: Deploy KolabPro
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

| Level | Tools | Target Coverage | Yang Diuji |
|-------|-------|-----------------|------------|
| Unit Test | Vitest | > 85% | Task calculators (progress, overdue), formatters, utils |
| Component Test | Vitest + RTL | > 70% | TaskCard rendering, KanbanColumn, CommentItem |
| Integration Test | Vitest + MSW | > 60% | Buat task -> muncul di board, Drag task -> berubah status |
| Drag-and-Drop Test | Vitest + RTL + @dnd-kit/testing | > 50% | Simulasi drag task antar kolom |
| E2E Test | Playwright | Critical paths | Login -> Buat proyek -> Buat task -> Drag -> Komentar |
| Accessibility | axe-core | Semua halaman | WCAG AA compliance |

**Contoh Test:**
```typescript
// tests/components/board/KanbanBoard.test.tsx
import { render, screen, within } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { KanbanBoard } from '@/components/board/KanbanBoard';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

describe('KanbanBoard', () => {
  it('menampilkan 4 kolom status', async () => {
    render(<KanbanBoard projectId="proy-001" />, { wrapper });
    
    expect(screen.getByText('Belum')).toBeInTheDocument();
    expect(screen.getByText('Dikerjakan')).toBeInTheDocument();
    expect(screen.getByText('Tinjau')).toBeInTheDocument();
    expect(screen.getByText('Selesai')).toBeInTheDocument();
  });

  it('menampilkan counter task di setiap kolom', async () => {
    render(<KanbanBoard projectId="proy-001" />, { wrapper });
    
    const belumColumn = screen.getByTestId('column-belum');
    expect(within(belumColumn).getByText('3')).toBeInTheDocument();
  });

  it('menampilkan task card dengan judul', async () => {
    render(<KanbanBoard projectId="proy-001" />, { wrapper });
    
    expect(screen.getByText('Integrasi API autentikasi')).toBeInTheDocument();
    expect(screen.getByText('Implementasi navbar responsif')).toBeInTheDocument();
  });

  it('dapat menambahkan task cepat via + Baru', async () => {
    const user = userEvent.setup();
    render(<KanbanBoard projectId="proy-001" />, { wrapper });
    
    await user.click(screen.getByTestId('add-task-belum'));
    await user.type(screen.getByPlaceholderText('Nama task...'), 'Task baru testing');
    await user.keyboard('{Enter}');
    
    expect(await screen.findByText('Task baru testing')).toBeInTheDocument();
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
    "@dnd-kit/core": "^1.1.0",
    "@dnd-kit/sortable": "^1.1.0",
    "@dnd-kit/utilities": "^1.1.0",
    "lucide-react": "^0.303.0",
    "date-fns": "^3.3.0",
    "framer-motion": "^10.18.0",
    "react-hook-form": "^7.49.0",
    "zod": "^3.22.0",
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

**Constraints:**
- **Node.js** >= 18.x
- **Browser support**: Chrome 90+, Firefox 90+, Safari 15+, Edge 90+
- **dnd-kit** membutuhkan React 16.8+ (compatible dengan React 18)
- **MSW** hanya aktif di development (`import.meta.env.DEV === true`)
- **Touch support**: dnd-kit mendukung touch events secara native untuk mobile
- **Font**: Inter + Plus Jakarta Sans via Google Fonts (self-hosted fallback untuk offline)
- **No PWA di MVP** (offline support akan ditambahkan nanti)
- **No WebSocket di MVP** — komentar menggunakan polling setiap 10 detik
