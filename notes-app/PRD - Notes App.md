# PRD: Catat.in — Notes App
**Project:** Catat.in
**Aesthetic:** Minimal, typography-first, markdown-native, folder-organizer, distraction-free

## 1. Core Layout
- Sidebar kiri: folder tree + tag list + settings
- Main area: split pane (kiri: list notes, kanan: editor/preview)
- Header minimal: judul note, save indicator, action buttons
- Bottom bar: word count, last saved, character count
- No toolbar — markdown keyboard shortcuts + minimal floating toolbar on select

## 2. Data Structure (Mock Only) — TypeScript interfaces

```typescript
interface Note {
  id: string;
  judul: string;
  konten: string; // markdown
  folderId: string | null;
  tags: string[];
  isPinned: boolean;
  isArchived: boolean;
  isFavorite: boolean;
  warna?: string; // accent color
  createdAt: string;
  updatedAt: string;
  lastEditedBy?: string;
  previewText: string; // plaintext first 150 chars
}

interface NoteFolder {
  id: string;
  nama: string;
  parentId: string | null;
  icon: string; // emoji
  warna: string;
  createdAt: string;
  updatedAt: string;
  noteCount: number;
}

interface NoteTag {
  id: string;
  nama: string;
  warna: string;
  createdAt: string;
  noteCount: number;
}

interface NoteHistory {
  id: string;
  noteId: string;
  judul: string;
  konten: string;
  versionNumber: number;
  createdAt: string;
}
```

## 3. Implementation Details — Vite + React + TypeScript + Tailwind CSS
- Vite + React 18 + TypeScript 5
- Zustand untuk notes state + active folder/filter
- react-markdown + remark-gfm untuk render markdown
- CodeMirror 6 atau react-simple-code-editor untuk editing markdown
- Local Storage + sync (IndexedDB untuk offline)
- React Router v6 untuk routing minimal
- Fuse.js untuk search notes

## 4. Styling & Theming
- CSS Variables: --ct-primary: #6366f1; --ct-secondary: #8b5cf6; --ct-accent: #f59e0b
- Typography: Inter font, ukuran 16px body, line-height 1.8 untuk readability
- Editor: background putih/cream, font monospace (JetBrains Mono) untuk editing
- Preview: font serif atau sans-serif sesuai preferensi
- Dark mode: dark sepia untuk mata nyaman
- Tag colors: solid pastel
- Minimal shadows, banyak whitespace

## 5. UI Components Specification — ASCII layout

```
+---------------------------------------------------+
| Catat.in                          [🔍] [⚙️] [👤] |
+------+----------------+--------------------------+
| 📁   |  My Notes (12) |                          |
|  📁  |  [Pinned]      |  # Judul Note           |
|   Prj |  +---------+  |                          |
|  📁  |  | Catatan  |  | ## Markdown content      |
|   Pbl |  | Harian   |  |                          |
|  📁  |  | Rapat    |  | Lorem ipsum **bold**     |
|   Jrn |  +---------+  | - list item              |
|      |                | 1. numbered              |
| 🏷️   |  [Recents]    |                          |
| tag  |  - Rapat      | > blockquote             |
| tag  |  - Ide        | ``` code block           |
| tag  |  - Todo       |                          |
|      |                | [ ] checklist            |
|      |  [Folder: All] |                          |
|      |  📄 Rapat Q1  |                          |
|      |  📄 Ide Buku  |                          |
|      |  📄 Harian    |                          |
|      |                |                          |
|      |  [+ New Note] |     [Save] [⋮]          |
+------+----------------+--------------------------+
| Words: 342 | Saved: 12:30 | 3 tags               |
+---------------------------------------------------+
```

- **NoteListPanel**: List dengan judul, preview 1 baris, tanggal, tag chips
- **MarkdownEditor**: CodeMirror 6 atau textarea with markdown shortcuts, line numbers
- **MarkdownPreview**: Rendered HTML dengan styling typography indah
- **FolderTree**: Nested folder dengan emoji icon, drag-drop untuk move note
- **TagManager**: Input tag dengan autocomplete, color picker, tag cloud
- **SearchBar**: Full-text search dengan highlight di hasil
- **NoteCard**: Card compact untuk grid view alternatif
- **FloatingToolbar**: Muncul saat select text: bold, italic, heading, link, list
- **VersionHistory**: Slide panel dengan list versi, diff view

## 6. Interactions & States — loading, empty, error, edge cases
- **Loading**: Skeleton list 5 items, skeleton editor area
- **Empty folder**: "Folder ini kosong. Buat catatan baru."
- **Empty search**: "Tidak ada catatan dengan kata kunci {{query}}"
- **No note selected (split pane)**: "Pilih catatan atau buat baru" dengan tombol aksi
- **Auto-save**: Simpan otomatis setiap 30 detik + visual indicator "Tersimpan" / "Menyimpan..."
- **Unsaved changes**: Indikator dot merah di judul, prompt sebelum navigasi
- **Conflict (multi-device)**: Deteksi perubahan dari luar, prompt "Pilih versi"
- **Delete**: Soft delete → trash, restore dalam 7 hari
- **Markdown rendering error**: Tampilkan raw text sebagai fallback
- **Large note (>100KB)**: Warning "Catatan ini besar. Performa mungkin terpengaruh."
- **Offline**: "Mode offline — perubahan akan disimpan saat online"
- **Keyboard shortcuts**: `Ctrl+S` save, `Ctrl+P` toggle preview, `Ctrl+B` bold, `Ctrl+K` link

## 7. Language & Localization — Bahasa Indonesia
- Full Bahasa Indonesia
- "Catatan terakhir diedit {{tanggal}}"
- "{{count}} kata, {{count}} karakter"
- "Tersimpan" / "Menyimpan..." / "Belum tersimpan"
- "Folder {{nama}}" / "Tag {{nama}}"
- Menu: "Buat Catatan Baru", "Pindahkan ke Folder", "Arsipkan", "Hapus"

## 8. Example Mock Data — data realistis nama Indonesia

```typescript
const mockFolders: NoteFolder[] = [
  { id: "FLDR-001", nama: "Catatan Harian", parentId: null, icon: "📓", warna: "#6366f1", createdAt: "2024-01-01", updatedAt: "2025-06-10", noteCount: 15 },
  { id: "FLDR-002", nama: "Project", parentId: null, icon: "💼", warna: "#059669", createdAt: "2024-02-15", updatedAt: "2025-06-12", noteCount: 8 },
  { id: "FLDR-003", nama: "Rapat", parentId: "FLDR-002", icon: "🤝", warna: "#d97706", createdAt: "2024-02-20", updatedAt: "2025-06-11", noteCount: 5 },
  { id: "FLDR-004", nama: "Jurnal Pribadi", parentId: null, icon: "🔒", warna: "#dc2626", createdAt: "2024-03-01", updatedAt: "2025-06-01", noteCount: 30 },
];

const mockTags: NoteTag[] = [
  { id: "TAG-001", nama: "penting", warna: "#ef4444", createdAt: "2024-01-01", noteCount: 12 },
  { id: "TAG-002", nama: "ide", warna: "#f59e0b", createdAt: "2024-01-01", noteCount: 8 },
  { id: "TAG-003", nama: "todo", warna: "#3b82f6", createdAt: "2024-01-05", noteCount: 20 },
  { id: "TAG-004", nama: "referensi", warna: "#8b5cf6", createdAt: "2024-02-01", noteCount: 6 },
];

const mockNotes: Note[] = [
  {
    id: "NOTE-001",
    judul: "Rapat Sprint Planning - Sprint 16",
    konten: "# Rapat Sprint Planning\n\n**Tanggal:** 12 Juni 2025\n**Hadir:** Tim Engineering (7 orang)\n\n## Goals Sprint 16\n\n1. **Feature A** — Selesai backend + frontend\n2. **Feature B** — Minimal API selesai\n3. **Bug fixing** — 5 high priority bugs\n\n## Action Items\n\n- [x] Setup CI/CD pipeline — *Budi*\n- [ ] Unit testing modul payment — *Siti*\n- [ ] Code review PR #234 — *Andi*\n\n> Sprint berakhir 25 Juni 2025. Demo hari Jumat.",
    folderId: "FLDR-003",
    tags: ["penting", "todo"],
    isPinned: true,
    isArchived: false,
    isFavorite: true,
    warna: "#d97706",
    createdAt: "2025-06-12T09:00:00",
    updatedAt: "2025-06-12T10:30:00",
    previewText: "Rapat Sprint Planning Tanggal: 12 Juni 2025 Hadir: Tim Engineering (7 orang)..."
  },
  {
    id: "NOTE-002",
    judul: "Ide Buku: Self-Tracking untuk Produktivitas",
    konten: "# Ide Buku: Self-Tracking\n\n## Konsep\nBuku tentang bagaimana melacak kebiasaan harian dengan sistem yang sederhana (tidak perlu apps rumit).\n\n## Outline\n\n### Bab 1: Mengapa Tracking?\n- Data > Perasaan\n- Accountability\n\n### Bab 2: Sistem\n- Daily log: 5 menit sebelum tidur\n- Weekly review: 15 menit hari Minggu\n- Monthly audit: 1 jam akhir bulan\n\n### Tools\n- Buku catatan fisik + pulpen\n- Atau markdown file di folder `~/journal`\n\n> \"What gets measured gets managed.\" — Peter Drucker",
    folderId: "FLDR-002",
    tags: ["ide", "referensi"],
    isPinned: false,
    isArchived: false,
    isFavorite: false,
    createdAt: "2025-06-10T20:00:00",
    updatedAt: "2025-06-11T08:15:00",
    previewText: "Ide Buku: Self-Tracking Konsep Buku tentang bagaimana melacak kebiasaan harian..."
  },
  {
    id: "NOTE-003",
    judul: "Catatan Harian: 12 Juni 2025",
    konten: "# Catatan Harian — Kamis, 12 Juni 2025\n\n## Pagi\nBangun jam 5:30. Olahraga ringan 15 menit. Kopi + baca artikel tentang sistem design.\n\n## Siang\nRapat sprint planning berjalan lancar. Tim sepakat untuk fokus ke feature A dulu.\n\n## Malam\nMakan malam dengan keluarga di resto baru. Enak! 😋\n\n## Refleksi\nHari ini produktif. Besok harus selesaikan review PR.",
    folderId: "FLDR-001",
    tags: [],
    isPinned: false,
    isArchived: false,
    isFavorite: false,
    createdAt: "2025-06-12T22:00:00",
    updatedAt: "2025-06-12T22:30:00",
    previewText: "Catatan Harian — Kamis, 12 Juni 2025 Pagi Bangun jam 5:30. Olahraga ringan..."
  }
];
```

## 9. Success Criteria
- [ ] Create, edit, delete notes dengan markdown
- [ ] Split pane: list notes + editor/preview
- [ ] Folder tree dengan nested folder, drag-drop note antar folder
- [ ] Tag management: assign, filter, color-coded
- [ ] Markdown editor dengan live preview
- [ ] Search full-text (judul + konten) dengan highlight
- [ ] Auto-save dengan visual indicator
- [ ] Pin, favorite, archive notes
- [ ] Keyboard shortcuts untuk editing cepat
- [ ] Dark mode + preferensi font
- [ ] Word count, character count, reading time
- [ ] Version history (local storage based)
