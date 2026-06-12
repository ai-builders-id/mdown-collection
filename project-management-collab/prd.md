# PRD: KolabPro — Manajemen Proyek & Kolaborasi Tim untuk Produktivitas Maksimal
**Project:** KolabPro
**Aesthetic:** Modern, clean, produktif, putih-biru dengan aksen hijau — nuansa workspace profesional

## 1. Core Layout
Layout utama KolabPro terdiri dari:
- **Sidebar kiri (240px, collapsible ke 64px)** — daftar workspace, proyek favorit, navigasi: Dashboard, Tasks, Timeline, Files, Anggota, Pengaturan.
- **Top bar (56px)** — breadcrumb proyek, view switcher (Board/List/Timeline/Calendar), search global, filter cepat, avatar pengguna.
- **Main panel (flex)** — area konten utama yang berubah berdasarkan view aktif (kanban board, tabel, gantt chart, kalender).
- **Right panel (380px, slide-in)** — detail task: deskripsi, komentar, attachment, activity log, checklist. Muncul saat task diklik.
- **Bottom bar (32px)** — status task: "12 task tersisa", "3 overdue", "Progress 65%".

Layout responsif: sidebar collapse ke ikon di < 1024px, panel detail menjadi fullscreen overlay di < 768px, mobile bottom navigation di < 640px.

## 2. Data Structure (Mock Only)

```typescript
// Interface untuk Workspace
interface Workspace {
  id: string;                          // UUID
  nama: string;                        // Contoh: "PT Maju Jaya"
  deskripsi: string;                   // Contoh: "Workspace utama perusahaan"
  logo: string | null;                 // URL logo workspace
  warna: string;                       // Hex color untuk identitas workspace
  pemilikId: string;                   // ID user pemilik
  totalProyek: number;
  totalAnggota: number;
  tipe: 'personal' | 'tim' | 'perusahaan';
  createdAt: Date;
  updatedAt: Date;
}

// Interface untuk Proyek
interface Proyek {
  id: string;
  workspaceId: string;
  nama: string;                        // Contoh: "Redesign Website"
  deskripsi: string;                   // Contoh: "Redesign total website perusahaan"
  status: 'aktif' | 'dijeda' | 'selesai' | 'diarsipkan';
  prioritas: 'rendah' | 'sedang' | 'tinggi' | 'urgent';
  tanggalMulai: Date;
  tanggalTarget: Date | null;
  tanggalSelesai: Date | null;
  pemilikId: string;                   // Project manager
  anggotaIds: string[];                // Array user IDs
  tags: string[];                      // Contoh: ["desain", "frontend", "marketing"]
  progress: number;                    // 0-100, otomatis dari task completion
  kategory: string;                    // Contoh: "pengembangan", "pemasaran", "operasional"
  createdAt: Date;
  updatedAt: Date;
}

// Interface untuk Task
interface Task {
  id: string;
  proyekId: string;
  judul: string;                       // Contoh: "Buat desain homepage baru"
  deskripsi: string;                   // HTML deskripsi task
  status: 'belum' | 'dikerjakan' | 'tinjau' | 'selesai';
  prioritas: 'rendah' | 'sedang' | 'tinggi' | 'urgent';
  penanggungJawabId: string | null;    // User ID yang assigned
  tanggalDeadline: Date | null;
  tanggalSelesai: Date | null;
  estimasiJam: number | null;          // Estimasi waktu pengerjaan
  labels: LabelTask[];                 // Label/tag
  parentTaskId: string | null;         // Untuk subtask
  checklist: ChecklistItem[];          // Checklist dalam task
  attachmentIds: string[];             // File attachments
  posisi: number;                      // Urutan di kolom
  createdAt: Date;
  updatedAt: Date;
}

// Interface untuk Checklist Item
interface ChecklistItem {
  id: string;
  taskId: string;
  teks: string;                        // Contoh: "Buat wireframe"
  selesai: boolean;
  posisi: number;
}

// Interface untuk Label
interface LabelTask {
  id: string;
  nama: string;                        // Contoh: "Desain"
  warna: string;                       // Hex color
}

// Interface untuk Komentar
interface Komentar {
  id: string;
  taskId: string;
  pengirimId: string;
  konten: string;                      // Contoh: "Sudah selesai, cek ya @budi"
  attachmentIds: string[];
  createdAt: Date;
  updatedAt: Date;
}

// Interface untuk Anggota Tim
interface AnggotaTim {
  id: string;
  workspaceId: string;
  nama: string;                        // Contoh: "Budi Hartono"
  email: string;
  avatar: string | null;
  jabatan: string;                     // Contoh: "Frontend Developer"
  role: 'pemilik' | 'admin' | 'anggota' | 'pengamat';
  proyekIds: string[];                 // Proyek yang diikuti
  statusOnline: boolean;
  lastActive: Date;
  joinedAt: Date;
}

// Interface untuk Attachment
interface Lampiran {
  id: string;
  taskId: string;
  nama: string;                        // Contoh: "mockup-homepage-v2.png"
  url: string;
  tipe: 'gambar' | 'dokumen' | 'link';
  ukuran: number;                      // Bytes
  diuploadOleh: string;
  createdAt: Date;
}
```

## 3. Implementation Details
- **Vite 5.x** — build tool dengan React plugin
- **React 18.x** — UI library
- **TypeScript 5.x** — strict mode
- **Tailwind CSS 3.x** — styling utility-first
- **React Router v6** — routing
- **Zustand** — client state management
- **TanStack Query** — server state (mock)
- **dnd-kit** — drag-and-drop untuk kanban dan reordering
- **@dnd-kit/sortable** — sortable task cards
- **date-fns** — dengan locale id
- **Lucide React** — ikon
- **React Hook Form + Zod** — validasi form
- **TanStack Table** — list view table
- **Framer Motion** — animasi transisi dan drag

## 4. Styling & Theming
**Palette Warna:**
- Primary: #2563EB (blue 600) — brand utama
- Primary Light: #DBEAFE (blue 100) — background hover
- Primary Dark: #1D4ED8 (blue 700) — active state
- Accent: #8B5CF6 (violet 500) — aksen premium
- Success: #10B981 (emerald 500) — selesai
- Warning: #F59E0B (amber 500) — peringatan
- Danger: #EF4444 (red 500) — overdue, urgensi
- Neutral: #0F172A (slate 900) — teks utama
- Surface: #FFFFFF — background card
- Surface Alt: #F8FAFC (slate 50) — background halaman
- Border: #E2E8F0 (slate 200)
- Column Bg: #F1F5F9 (slate 100) — background kolom kanban

**Tipografi:**
- Font utama: Inter (sans-serif)
- Font heading: Plus Jakarta Sans — untuk judul proyek dan task
- Font mono: JetBrains Mono — untuk kode, ID task
- Skala: 12/13/14/15/16/18/20/24/30/36px

**Warna Status Task:**
- Belum: #94A3B8 (slate 400)
- Dikerjakan: #3B82F6 (blue 500)
- Tinjau: #F59E0B (amber 500)
- Selesai: #10B981 (emerald 500)

## 5. UI Components Specification

```
+------------------------------------------------------------------+
| [Logo] KolabPro   [View: Board | List | Timeline | Calendar] [🔍]| ← Topbar
+---------+--------------------------------------------------------+
|         | +----------------------------------------------------+ |
| 📁 WORKSPACE | [Nama Proyek: Redesign Website]                 | |
| PT Maju Jaya |  Progress: ████████████████░░░  65%            | |
|              | +----------+ +----------+ +--------+ +--------+ | |
| ⭐ Favorit   | | BELUM    | | DIKERJA- | | TINJAU | | SELESAI| |
| Redesign     | |          | | KAN      | |        | |        | | |
| Mobile App   | | [Task 1] | | [Task 4] | | [Task] | | [Task] | | |
|              | | [Task 2] | | [Task 5] | | [Task] | | [Task] | | |
| 📋 Semua     | | [Task 3] | |          | |        | | [Task] | | |
| Proyek Aktif | |          | |          | |        | | [Task] | | |
| Dashboard    | | [+ Baru] | | [+ Baru] | |[+ Baru]| |        | | |
| Tasks        | +----------+ +----------+ +--------+ +--------+ | |
| Timeline     |                                                  | |
| Files        |      +---- Detail Task (Right Panel) ----+       | |
| Anggota      |      | Judul: Desain homepage             |       | |
|              |      | Status: ● Dikerjakan               |       | |
| Pengaturan   |      | Prioritas: 🔴 Tinggi               |       | |
|              |      | Penanggung: Budi H.                |       | |
|              |      | Deadline: 20 Jun 2025              |       | |
|              |      | Checklist: [x] Wireframe [ ] UI     |       | |
|              |      | Komentar: ─────────────             |       | |
|              |      | [Budi]: Sudah selesai               |       | |
|              |      | [Input komentar...]                 |       | |
|              |      +────────────────────────────────────+       | |
+---------+--------------------------------------------------------+
| 12 task tersisa | 3 overdue | Progress: 65% selesai             | ← Bottom
+------------------------------------------------------------------+
```

**Komponen Detail:**

### Sidebar
- **Workspace Switcher** — dropdown dengan daftar workspace, setiap workspace memiliki ikon warna berbeda
- **Favorit** — proyek yang ditandai sebagai favorit (akses cepat)
- **Navigasi** — Dashboard, Tasks, Timeline, Files, Anggota, Pengaturan
- **Proyek List** — expandable list proyek dalam workspace aktif
- **Tombol "Proyek Baru"** — floating button di bagian bawah
- **User info** — avatar + nama di bagian paling bawah

### Topbar
- **Breadcrumb** — Workspace > Proyek > View
- **View Switcher** — 4 tombol: Board, List, Timeline, Calendar (active state dengan underline)
- **Filter Cepat** — filter by assignee, priority, label (dropdown chip)
- **Search Global** — pencarian task/proyek/anggota
- **Avatar** — notifikasi + dropdown profil

### KanbanBoard
- 4 kolom default: Belum, Dikerjakan, Tinjau, Selesai
- Kolom bisa di-collapse (header kolom toggle)
- Drag task antar kolom untuk update status
- Drag dalam kolom untuk reorder
- Tombol "+" di footer setiap kolom untuk add task cepat
- Counter badge di header kolom
- Smooth animation saat drag (Framer Motion)
- Scroll horizontal jika kolom melebihi lebar layar

### TaskCard
- Judul task (bold, 2 baris max, ellipsis)
- Avatar penanggung jawab (single atau stack)
- Priority badge (4 warna: abu/ungu/biru/merah)
- Deadline date (merah jika overdue)
- Label chips (max 2, "+N" untuk overflow)
- Checklist progress (jika ada): "2/5"
- Attachment indicator (📎 icon)
- Comments indicator (💬 N)
- Drag handle di sisi kiri (6 dots)

### TaskDetail Panel (Right Panel)
- Slide-in dari kanan, 380px
- Header: judul + tombol close
- Status dropdown
- Assignee selector
- Priority selector
- Deadline picker
- Labels section (add/remove)
- Description (markdown editor atau rich text)
- Checklist section (add, check, reorder)
- Activity log (riwayat perubahan)
- Comments thread (real-time via polling)
- Attachment list (preview gambar, download link)
- Tombol: Save, Delete, Duplicate

### TimelineView (Gantt Chart)
- Sumbu X: tanggal (hari/minggu/bulan tergantung zoom)
- Sumbu Y: daftar task
- Bar horizontal: panjang sesuai durasi task
- Dependency arrows (task A -> task B)
- Drag bar untuk ubah tanggal
- Zoom level: H, M, B
- Today indicator (garis vertikal merah)
- Milestone markers (diamond icon)

### ListView
- Table dengan kolom: Status, Judul, Prioritas, Assignee, Deadline, Labels
- Sorting per kolom
- Inline edit untuk judul dan status
- Bulk selection + action
- Expandable row untuk preview deskripsi

### CalendarView
- Month/week/day view
- Task sebagai card di tanggal deadline
- Drag task antar tanggal untuk ubah deadline
- Color coding berdasarkan prioritas

## 6. Interactions & States

**Loading States:**
- Skeleton columns untuk kanban board (3 kolom dengan cards palsu)
- Skeleton list untuk list view (10 baris)
- Spinner untuk panel detail
- Skeleton sidebar loading

**Empty States:**
- Workspace baru: "Selamat datang di KolabPro! Buat proyek pertama Anda untuk memulai."
- Proyek tanpa task: "Proyek ini masih kosong. Tambahkan task pertama untuk memulai pekerjaan."
- Filter tidak menghasilkan hasil: "Tidak ada task dengan filter yang dipilih. Coba ubah filter."
- Timeline tanpa data: "Belum ada task dengan deadline. Atur deadline task untuk melihat timeline."
- Komentar kosong: "Belum ada komentar. Mulai diskusi tentang task ini."
- Ilustrasi karakter: untuk setiap empty state

**Error States:**
- Gagal muat proyek: "Gagal memuat proyek. Periksa koneksi Anda." + tombol "Muat Ulang"
- Gagal simpan task: "Gagal menyimpan perubahan." + tombol "Coba Lagi"
- Gagal upload file: "File {{NAMA}} gagal diupload. Ukuran maksimal 10 MB."
- 404 route: "Halaman tidak ditemukan." + tombol "Kembali ke Dashboard"

**Edge Cases:**
- Task overdue: highlight merah, badge "Terlambat N hari", sorting otomatis ke atas
- Assignee tidak ditemukan: jika anggota dihapus dari workspace, task tetap ada dengan label "Unassigned"
- Delete workspace: konfirmasi berlapis "Semua proyek dan task akan dihapus. Yakin?"
- Multiple assignee: avatar stack dengan overflow "+N"
- Deadline tanpa waktu: "Deadline" tanpa jam (end of day)
- Subtask: tampil di dalam task card dengan indent, bisa drag terpisah
- Conflict edit: deteksi jika 2 user edit task yang sama
- Drag ke kolom penuh: kolom bisa scroll
- Progress 100% otomatis: ketika semua task di kolom "Selesai", progress proyek menjadi 100%

## 7. Language & Localization
Semua antarmuka dalam Bahasa Indonesia:
- Label: "Proyek", "Task", "Anggota", "Workspace", "Deadline", "Prioritas"
- Status: "Belum Dikerjakan", "Sedang Dikerjakan", "Dalam Tinjauan", "Selesai"
- Tombol: "Buat Task", "Simpan", "Arsipkan", "Hapus", "Bagikan"
- Prioritas: "Rendah", "Sedang", "Tinggi", "Urgent"
- Pesan: "Apakah Anda yakin ingin menghapus task ini?", "Perubahan berhasil disimpan"
- Tanggal: "20 Juni 2025", "2 hari yang lalu", "Besok"
- Angka: "1.234 task", "Rp. 500.000.000"

## 8. Example Mock Data

```typescript
export const mockWorkspace: Workspace = {
  id: "ws-001",
  nama: "PT Maju Jaya",
  deskripsi: "Workspace utama perusahaan teknologi",
  logo: "/logos/ptmajujaya.png",
  warna: "#2563EB",
  pemilikId: "usr-001",
  totalProyek: 8,
  totalAnggota: 15,
  tipe: "perusahaan",
  createdAt: new Date("2024-01-01"),
  updatedAt: new Date("2025-11-25")
};

export const mockProyek: Proyek[] = [
  {
    id: "proy-001",
    workspaceId: "ws-001",
    nama: "Redesign Website Perusahaan",
    deskripsi: "Redesign total website utama perusahaan dengan UI/UX modern dan performa optimal",
    status: "aktif",
    prioritas: "tinggi",
    tanggalMulai: new Date("2025-05-01"),
    tanggalTarget: new Date("2025-08-30"),
    tanggalSelesai: null,
    pemilikId: "usr-001",
    anggotaIds: ["usr-001", "usr-002", "usr-003", "usr-004"],
    tags: ["desain", "frontend", "ui-ux", "website"],
    progress: 65,
    kategory: "pengembangan",
    createdAt: new Date("2025-04-15"),
    updatedAt: new Date("2025-11-25")
  },
  {
    id: "proy-002",
    workspaceId: "ws-001",
    nama: "Aplikasi Mobile Pemesanan",
    deskripsi: "Pengembangan aplikasi mobile untuk pemesanan produk",
    status: "aktif",
    prioritas: "urgent",
    tanggalMulai: new Date("2025-06-01"),
    tanggalTarget: new Date("2025-12-01"),
    tanggalSelesai: null,
    pemilikId: "usr-002",
    anggotaIds: ["usr-002", "usr-003", "usr-005"],
    tags: ["mobile", "react-native", "backend"],
    progress: 25,
    kategory: "pengembangan",
    createdAt: new Date("2025-05-20"),
    updatedAt: new Date("2025-11-20")
  }
];

export const mockTasks: Task[] = [
  {
    id: "task-001",
    proyekId: "proy-001",
    judul: "Buat desain homepage baru",
    deskripsi: "<p>Buat desain homepage yang mencerminkan brand perusahaan dengan pendekatan modern. Gunakan Figma untuk prototyping.</p><p>Referensi: <a href='#'>link inspiras</a></p>",
    status: "selesai",
    prioritas: "tinggi",
    penanggungJawabId: "usr-001",
    tanggalDeadline: new Date("2025-05-15"),
    tanggalSelesai: new Date("2025-05-14"),
    estimasiJam: 24,
    labels: [
      { id: "lbl-001", nama: "Desain", warna: "#8B5CF6" },
      { id: "lbl-002", nama: "UI/UX", warna: "#3B82F6" }
    ],
    parentTaskId: null,
    checklist: [
      { id: "cl-001", taskId: "task-001", teks: "Buat wireframe halaman utama", selesai: true, posisi: 0 },
      { id: "cl-002", taskId: "task-001", teks: "Desain visual mockup", selesai: true, posisi: 1 },
      { id: "cl-003", taskId: "task-001", teks: "Presentasi ke tim", selesai: true, posisi: 2 },
      { id: "cl-004", taskId: "task-001", teks: "Revisi berdasarkan feedback", selesai: true, posisi: 3 }
    ],
    attachmentIds: ["att-001", "att-002"],
    posisi: 0,
    createdAt: new Date("2025-05-01"),
    updatedAt: new Date("2025-05-14")
  },
  {
    id: "task-002",
    proyekId: "proy-001",
    judul: "Implementasi navbar responsif",
    deskripsi: "Buat komponen navbar yang responsif untuk mobile, tablet, dan desktop menggunakan React + Tailwind CSS.",
    status: "dikerjakan",
    prioritas: "tinggi",
    penanggungJawabId: "usr-003",
    tanggalDeadline: new Date("2025-06-25"),
    tanggalSelesai: null,
    estimasiJam: 16,
    labels: [
      { id: "lbl-003", nama: "Frontend", warna: "#10B981" }
    ],
    parentTaskId: null,
    checklist: [
      { id: "cl-005", taskId: "task-002", teks: "Setup komponen dasar", selesai: true, posisi: 0 },
      { id: "cl-006", taskId: "task-002", teks: "Styling desktop", selesai: true, posisi: 1 },
      { id: "cl-007", taskId: "task-002", teks: "Responsive mobile", selesai: false, posisi: 2 },
      { id: "cl-008", taskId: "task-002", teks: "Animasi hamburger menu", selesai: false, posisi: 3 }
    ],
    attachmentIds: [],
    posisi: 0,
    createdAt: new Date("2025-05-10"),
    updatedAt: new Date("2025-11-24")
  },
  {
    id: "task-003",
    proyekId: "proy-001",
    judul: "Integrasi API autentikasi",
    deskripsi: "Hubungkan frontend dengan REST API untuk login, register, dan manajemen sesi pengguna.",
    status: "belum",
    prioritas: "sedang",
    penanggungJawabId: "usr-004",
    tanggalDeadline: new Date("2025-07-01"),
    tanggalSelesai: null,
    estimasiJam: 20,
    labels: [
      { id: "lbl-004", nama: "Backend", warna: "#EF4444" },
      { id: "lbl-005", nama: "API", warna: "#F59E0B" }
    ],
    parentTaskId: null,
    checklist: [
      { id: "cl-009", taskId: "task-003", teks: "Setup React Query", selesai: true, posisi: 0 },
      { id: "cl-010", taskId: "task-003", teks: "Auth context & hooks", selesai: false, posisi: 1 },
      { id: "cl-011", taskId: "task-003", teks: "Protected routes", selesai: false, posisi: 2 },
      { id: "cl-012", taskId: "task-003", teks: "Testing flow login", selesai: false, posisi: 3 }
    ],
    attachmentIds: ["att-003"],
    posisi: 0,
    createdAt: new Date("2025-05-20"),
    updatedAt: new Date("2025-05-20")
  }
];

export const mockAnggota: AnggotaTim[] = [
  {
    id: "usr-001",
    workspaceId: "ws-001",
    nama: "Budi Hartono",
    email: "budi@majujaya.com",
    avatar: "/avatars/budi.jpg",
    jabatan: "Project Manager",
    role: "pemilik",
    proyekIds: ["proy-001", "proy-002"],
    statusOnline: true,
    lastActive: new Date(),
    joinedAt: new Date("2024-01-01")
  },
  {
    id: "usr-002",
    workspaceId: "ws-001",
    nama: "Sari Indah Dewi",
    email: "sari@majujaya.com",
    avatar: "/avatars/sari.jpg",
    jabatan: "UI/UX Designer",
    role: "admin",
    proyekIds: ["proy-001", "proy-002"],
    statusOnline: true,
    lastActive: new Date(Date.now() - 5 * 60 * 1000),
    joinedAt: new Date("2024-02-01")
  },
  {
    id: "usr-003",
    workspaceId: "ws-001",
    nama: "Ahmad Rizki Fauzi",
    email: "ahmad@majujaya.com",
    avatar: null,
    jabatan: "Frontend Developer",
    role: "anggota",
    proyekIds: ["proy-001"],
    statusOnline: false,
    lastActive: new Date(Date.now() - 120 * 60 * 1000),
    joinedAt: new Date("2024-03-15")
  },
  {
    id: "usr-004",
    workspaceId: "ws-001",
    nama: "Dewi Lestari",
    email: "dewi@majujaya.com",
    avatar: "/avatars/dewi.jpg",
    jabatan: "Backend Developer",
    role: "anggota",
    proyekIds: ["proy-001", "proy-002"],
    statusOnline: true,
    lastActive: new Date(),
    joinedAt: new Date("2024-04-01")
  }
];

export const mockKomentar: Komentar[] = [
  {
    id: "kom-001",
    taskId: "task-001",
    pengirimId: "usr-002",
    konten: "Desainnya sudah sesuai dengan brand guideline. Saya approve untuk lanjut ke tahap development. 👍",
    attachmentIds: [],
    createdAt: new Date("2025-05-13T14:30:00"),
    updatedAt: new Date("2025-05-13T14:30:00")
  },
  {
    id: "kom-002",
    taskId: "task-001",
    pengirimId: "usr-001",
    konten: "Terima kasih Sar! @usr-003 tolong review dan siapkan untuk implementasi ya.",
    attachmentIds: [],
    createdAt: new Date("2025-05-13T15:00:00"),
    updatedAt: new Date("2025-05-13T15:00:00")
  }
];
```

## 9. Success Criteria
1. User dapat membuat workspace, proyek, dan task dalam waktu < 2 menit.
2. Drag-and-drop task antar kolom kanban berjalan mulus tanpa lag (< 100 ms response).
3. Empat view (Board, List, Timeline, Calendar) tersedia dan dapat di-toggle tanpa kehilangan data.
4. Task detail panel menampilkan semua informasi: deskripsi, checklist, komentar, attachment, activity log.
5. Filter task berdasarkan assignee, prioritas, label, dan status berfungsi real-time.
6. Timeline/Gantt chart menampilkan task dengan durasi dan dependencies.
7. Komentar dapat dikirim dengan mention user (@nama) dan attachment file.
8. Progress proyek otomatis terupdate berdasarkan persentase task selesai.
9. Responsif di desktop, tablet, dan mobile dengan navigasi yang adaptif.
10. Semua teks antarmuka dalam Bahasa Indonesia yang baku.
