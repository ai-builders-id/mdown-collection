# PRD: DocuVault — Document Management System
**Project:** DocuVault
**Aesthetic:** Clean, paper-like, folder-tree sidebar, minimal typography, collaborative

## 1. Core Layout
- Sidebar kiri: folder tree (expandable) + tags filter
- Header: breadcrumb path, search global, sort, view toggle (grid/list)
- Main area: file grid/list view, preview panel
- Panel kanan (slide): preview dokumen + metadata + version history
- Bottom bar: status (selected count, total size, last sync)

## 2. Data Structure (Mock Only) — TypeScript interfaces

```typescript
interface Document {
  id: string;
  judul: string;
  deskripsi: string;
  tipe: 'pdf' | 'docx' | 'xlsx' | 'pptx' | 'txt' | 'image' | 'markdown' | 'other';
  ukuranByte: number;
  folderId: string;
  tags: string[];
  version: number;
  author: string;
  createdAt: string;
  updatedAt: string;
  status: 'draft' | 'published' | 'archived' | 'deleted';
  pinned: boolean;
  starred: boolean;
}

interface Folder {
  id: string;
  nama: string;
  parentId: string | null;
  path: string;
  createdAt: string;
  updatedAt: string;
  warna: string;
  icon?: string;
}

interface DocumentVersion {
  id: string;
  documentId: string;
  versionNumber: number;
  fileUrl: string;
  ukuranByte: number;
  catatanPerubahan: string;
  author: string;
  createdAt: string;
  checksum: string;
}

interface Comment {
  id: string;
  documentId: string;
  userId: string;
  namaUser: string;
  konten: string;
  createdAt: string;
  resolved: boolean;
  mentions: string[];
}
```

## 3. Implementation Details — Vite + React + TypeScript + Tailwind CSS
- Vite + React 18 + TypeScript 5
- Zustand untuk folder tree state + selected documents
- React Query untuk data dokumen
- React Router v6 dengan nested routes untuk folder browsing
- react-pdf / react-markdown untuk preview inline
- @tanstack/react-virtual untuk folder/file list virtual scrolling
- dnd-kit untuk drag-drop file antar folder

## 4. Styling & Theming
- CSS Variables: --dv-primary: #4f46e5; --dv-secondary: #6366f1; --dv-accent: #f59e0b
- File type icons: warna berbeda per ekstensi (PDF merah, DOCX biru, XLSX hijau, dll)
- Grid view: card dengan icon, nama, tanggal
- List view: table compact
- Folder tree: indent lines, expand arrow, icon folder warna
- Dark mode toggle

## 5. UI Components Specification — ASCII layout

```
+------------------------------------------------+
| DocuVault    [Folder Root] > [Project X] > [..] |
+------+-----------------------------------------+
|      |  [Grid View] [List View] [Sort ▼] [🔍]  |
| 📁   |                                          |
|  Pro |  +------+ +------+ +------+             |
|  📁  |  | 📄   | | 📄   | | 📄   |             |
|   Fin |  | Lapor | | Kontr | | Notul |          |
|  📁  |  | an    | | ak    | | ensi  |           |
|   HR |  | Q1    | | Vendor| | Rapat |           |
|  📁  |  +------+ +------+ +------+             |
|   IT |                                          |
|      |  === Preview Panel ===                   |
|  🏷️  |  +--------------------------+          |
|  tag |  | [Nama Dokumen]          |          |
|  tag |  | Versi: 3 | Ukuran: 2MB |          |
|      |  | Author: Budi | 12 Mei  |          |
|      |  | ────────────────────── |          |
|      |  | Preview content...     |          |
|      |  +--------------------------+          |
+------+-----------------------------------------+
```

- **FolderTree**: Nested expandable tree dengan ikon folder, context menu (rename, delete, move)
- **FileGridCard**: Card dengan icon file type, judul, tanggal, badge version
- **FileListView**: Table: icon, nama, ukuran, versi, diubah, tag, aksi
- **DocumentPreview**: Preview inline (PDF/markdown/image) + metadata panel
- **VersionHistory**: Timeline versi, siapa, kapan, notes, download
- **TagBadge**: Chip berwarna, clickable untuk filter
- **SearchBar**: Search dengan filter tipe, folder, tag, author, date range
- **UploadProgress**: Progress bar + file queue saat upload multiple

## 6. Interactions & States — loading, empty, error, edge cases
- **Loading**: Skeleton grid 6 cards, tree skeleton untuk sidebar
- **Empty folder**: "Folder ini masih kosong" + tombol "Upload file" dan "Buat folder"
- **No search results**: "Tidak ditemukan dokumen dengan kata kunci {{query}}"
- **Error loading**: "Gagal memuat dokumen" + retry button
- **File too large**: "File melebihi batas {{maxSize}}. Pilih file lain."
- **Unsupported preview**: "Preview tidak tersedia untuk tipe file ini. Download untuk melihat."
- **Drag & drop**: Visual feedback (highlight zone, drop indicator line)
- **Conflict version**: "Dokumen telah diubah oleh {{user}}. Simpan sebagai versi baru?"
- **Delete**: Soft delete → trash folder, restore dalam 30 hari
- **Bulk select**: Checkbox mode, batch move/tag/delete

## 7. Language & Localization — Bahasa Indonesia
- Full Bahasa Indonesia
- "Anda memiliki {{count}} dokumen yang perlu direview"
- Status: Draft, Publikasi, Arsip, Sampah
- Tanggal: "12 Mei 2025 pukul 14:30"
- Ukuran: "2.5 MB", "1.2 GB"
- "Folder {{nama}} berisi {{count}} dokumen"

## 8. Example Mock Data — data realistis nama Indonesia

```typescript
const mockFolders: Folder[] = [
  { id: "F-001", nama: "Project Alpha", parentId: null, path: "/Project Alpha", createdAt: "2024-01-10", updatedAt: "2025-05-20", warna: "#4f46e5" },
  { id: "F-002", nama: "Keuangan", parentId: "F-001", path: "/Project Alpha/Keuangan", createdAt: "2024-01-15", updatedAt: "2025-05-18", warna: "#059669" },
  { id: "F-003", nama: "Kontrak", parentId: "F-001", path: "/Project Alpha/Kontrak", createdAt: "2024-02-01", updatedAt: "2025-05-10", warna: "#dc2626" },
  { id: "F-004", nama: "HRD", parentId: null, path: "/HRD", createdAt: "2024-03-01", updatedAt: "2025-05-22", warna: "#0891b2" },
];

const mockDocuments: Document[] = [
  {
    id: "DOC-001",
    judul: "Laporan Keuangan Q1 2025",
    deskripsi: "Laporan laba rugi dan neraca periode Jan-Mar 2025",
    tipe: "pdf",
    ukuranByte: 2450000,
    folderId: "F-002",
    tags: ["keuangan", "laporan", "q1-2025"],
    version: 3,
    author: "Dewi Sartika",
    createdAt: "2025-04-01T09:00:00",
    updatedAt: "2025-05-10T14:30:00",
    status: "published",
    pinned: false,
    starred: true
  },
  {
    id: "DOC-002",
    judul: "Kontrak Vendor PT Sembada Makmur",
    deskripsi: "Perjanjian kerjasama pengadaan bahan baku 2025-2026",
    tipe: "pdf",
    ukuranByte: 3800000,
    folderId: "F-003",
    tags: ["kontrak", "vendor", "legal"],
    version: 2,
    author: "Bambang Supriyadi",
    createdAt: "2025-03-15T10:00:00",
    updatedAt: "2025-05-12T08:00:00",
    status: "published",
    pinned: true,
    starred: true
  },
  {
    id: "DOC-003",
    judul: "Notulensi Rapat Sprint 15",
    deskripsi: "Hasil daily standup dan planning sprint 15",
    tipe: "markdown",
    ukuranByte: 45000,
    folderId: "F-001",
    tags: ["rapat", "agile", "sprint"],
    version: 1,
    author: "Rizki Pratama",
    createdAt: "2025-05-20T16:00:00",
    updatedAt: "2025-05-20T16:00:00",
    status: "draft",
    pinned: false,
    starred: false
  },
  {
    id: "DOC-004",
    judul: "Data Karyawan 2025.xlsx",
    deskripsi: "Database karyawan aktif per Mei 2025",
    tipe: "xlsx",
    ukuranByte: 1200000,
    folderId: "F-004",
    tags: ["hrd", "data-karyawan", "2025"],
    version: 5,
    author: "Siti Rahmawati",
    createdAt: "2025-01-05T07:30:00",
    updatedAt: "2025-05-22T11:00:00",
    status: "published",
    pinned: false,
    starred: false
  }
];

const mockVersions: DocumentVersion[] = [
  {
    id: "VER-001",
    documentId: "DOC-001",
    versionNumber: 3,
    fileUrl: "/mock/documents/laporan-q1-v3.pdf",
    ukuranByte: 2450000,
    catatanPerubahan: "Update data Maret, perbaiki typo",
    author: "Dewi Sartika",
    createdAt: "2025-05-10T14:30:00",
    checksum: "a1b2c3d4e5f6..."
  },
  {
    id: "VER-002",
    documentId: "DOC-001",
    versionNumber: 2,
    fileUrl: "/mock/documents/laporan-q1-v2.pdf",
    ukuranByte: 2400000,
    catatanPerubahan: "Tambah grafik perbandingan",
    author: "Dewi Sartika",
    createdAt: "2025-04-15T09:00:00",
    checksum: "b2c3d4e5f6a7..."
  }
];
```

## 9. Success Criteria
- [ ] Folder tree navigasi dengan expand/collapse, breadcrumb path
- [ ] Upload dokumen dengan drag-drop dan progress
- [ ] Grid dan List view toggle
- [ ] Preview inline untuk PDF, markdown, image
- [ ] Version history tracking dengan diff notes
- [ ] Search + filter (tipe, folder, tag, author, date)
- [ ] Drag-drop file antar folder
- [ ] Tag management: create, assign, filter by tag
- [ ] Star/pin dokumen ke favorites
- [ ] Soft delete + trash dengan restore
