# TRD: DocuVault — Technical Requirements

## 1. Technical Strategy
DocuVault dibangun sebagai SPA React dengan fokus pada UX browsing folder yang cepat, preview inline, dan versioning. Arsitektur data menggunakan struktur tree (folder) dengan adjacency list untuk query efisien. Preview dokumen dilakukan client-side menggunakan library Web-standar.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Framework | React | 18 | Vite build tool |
| Bahasa | TypeScript | 5 | Strict mode |
| Styling | Tailwind CSS | 3 | + headless UI |
| State | Zustand | 4 | Folder tree + selection |
| Data Fetching | TanStack React Query | 5 | Caching dokumen |
| Routing | React Router | 6 | Nested folder routes |
| Virtual | @tanstack/react-virtual | 3 | File list virtual scroll |
| Preview | react-pdf + react-markdown | latest | Inline preview |
| Search | Fuse.js | 7 | Client-side fuzzy search |
| Icons | lucide-react | latest | File type icons |
| Drag-Drop | dnd-kit | latest | Move folder/file |

## 3. Component Architecture (tree)

```
src/
├── components/
│   ├── layout/
│   │   ├── Sidebar.tsx            # Folder tree
│   │   ├── Header.tsx             # Search + breadcrumb
│   │   ├── MainLayout.tsx
│   │   └── Breadcrumb.tsx
│   ├── folder/
│   │   ├── FolderTree.tsx         # Nested tree + context menu
│   │   ├── FolderTreeItem.tsx     # Single folder node
│   │   ├── FolderBreadcrumb.tsx
│   │   └── FolderActions.tsx      # Rename, move, delete
│   ├── document/
│   │   ├── FileGrid.tsx           # Card grid view
│   │   ├── FileList.tsx           # Table list view
│   │   ├── FileCard.tsx           # Single card
│   │   ├── FileRow.tsx            # Single row
│   │   ├── FileIcon.tsx           # Icon by file type
│   │   └── UploadZone.tsx         # Drag-drop upload area
│   ├── preview/
│   │   ├── PreviewPanel.tsx       # Slide panel
│   │   ├── PDFPreview.tsx
│   │   ├── MarkdownPreview.tsx
│   │   ├── ImagePreview.tsx       # Lightbox
│   │   └── UnsupportedPreview.tsx
│   ├── version/
│   │   ├── VersionHistory.tsx     # Timeline versi
│   │   └── VersionUploadForm.tsx
│   ├── tag/
│   │   ├── TagBadge.tsx           # Chip component
│   │   ├── TagManager.tsx         # CRUD tag modal
│   │   └── TagFilter.tsx          # Filter sidebar
│   ├── search/
│   │   ├── SearchBar.tsx
│   │   └── SearchResults.tsx
│   └── shared/
│       ├── EmptyState.tsx
│       ├── ErrorBanner.tsx
│       ├── LoadingSkeleton.tsx
│       └── ContextMenu.tsx        # Right-click menu
├── hooks/
│   ├── useDocuments.ts
│   ├── useFolders.ts
│   ├── useSearch.ts               # Fuse.js hook
│   └── useUpload.ts               # Upload queue + progress
├── stores/
│   ├── folderStore.ts
│   ├── documentStore.ts
│   └── uiStore.ts                 # View mode, selected
├── services/
│   ├── searchService.ts           # Fuse.js setup + search
│   └── fileService.ts             # File size, type validation
├── types/
│   └── index.ts
├── data/
│   └── mockData.ts
└── utils/
    ├── formatters.ts              # File size, date
    ├── validators.ts
    └── treeUtils.ts               # Flatten/nest tree helpers
```

## 4. Data Layer
- **MVP**: Zustand store with normalized tree data (byId + rootIds pattern)
- **Folder Structure**: Adjacency list (parentId) — bisa flat array
- **Search**: Fuse.js dengan keys: judul, deskripsi, tags — di-initialize dari seluruh dokumen
- **File Storage**: Blob URL untuk mock, upload base64 untuk MVP
- **Production**: PostgreSQL + Prisma ORM + MinIO/S3 untuk file storage
- **Thumbnail**: canvas API resize untuk image thumbnail

## 5. Performance Requirements
- Folder tree render 500 folder: < 500ms (virtual tree)
- File list render 1000 items: < 1 detik (virtual scrolling)
- Search (Fuse.js) 10000 dokumen: < 200ms
- PDF preview load: < 3 detik untuk 50 halaman
- Upload speed: 5MB/detik untuk file besar
- Bundle size: < 400KB gzipped

## 6. Styling & Theming System (CSS vars)

```css
:root {
  --dv-primary: #4f46e5;
  --dv-primary-light: #818cf8;
  --dv-surface: #ffffff;
  --dv-surface-sidebar: #f8fafc;
  --dv-surface-card: #ffffff;
  --dv-text: #1e293b;
  --dv-text-muted: #64748b;
  --dv-border: #e2e8f0;
  --dv-accent: #f59e0b;
  --dv-folder-color: #eab308;
  --sidebar-width: 280px;
  --header-height: 60px;
  --preview-panel-width: 400px;
}
.dark {
  --dv-surface: #0f172a;
  --dv-surface-sidebar: #1e293b;
  --dv-surface-card: #1e293b;
  --dv-text: #f1f5f9;
  --dv-text-muted: #94a3b8;
  --dv-border: #334155;
}
```

## 7. Validation Rules
- Nama folder: 1-100 karakter, tidak boleh mengandung / \ : * ? " < > |
- Nama file: Sama aturan dengan folder
- Tipe file: Harus dalam daftar izin (whitelist ekstensi)
- Ukuran file: Maks 50MB, bisa dikonfigurasi
- Kedalaman folder: Maks 10 level
- Tag: 1-30 karakter, alphanumeric + dash, lowercase, unique
- Catatan versi: Wajib diisi, minimal 5 karakter

## 8. Error Handling Strategy
- **Upload fail**: Retry 2x + "Upload gagal. Cek koneksi dan coba lagi."
- **File type rejected**: "{{tipe}} tidak diizinkan. Gunakan: {{whitelist}}"
- **File too large**: "File {{nama}} terlalu besar ({{ukuran}}). Maks {{maksSize}}"
- **Duplicate name**: "Sudah ada folder/file dengan nama {{nama}} di lokasi ini"
- **Delete protected**: "Folder tidak kosong. Pindahkan atau hapus isinya terlebih dulu."
- **Preview error**: "Gagal memuat preview" + tombol download
- **Search no result**: "Tidak ada hasil untuk '{{query}}'. Coba kata kunci lain."

## 9. Security Considerations
- File upload whitelist extension + MIME type validation
- Malware scan pada upload (Phase 2 integrasi ClamAV)
- XSS: Sanitize judul, deskripsi, tag yang di-render
- Access control: User hanya bisa lihat folder yang dishare (Phase 2)
- CSRF protection untuk upload
- File access: Hash filename di storage, bukan nama asli
- Rate limit upload: 50 file/jam per user

## 10. Deployment & Build
- Build: `npm run build` → `/dist`
- Deploy: Vercel (frontend) + DigitalOcean/S3 (storage)
- Environment: `VITE_MAX_UPLOAD_SIZE`, `VITE_ALLOWED_FILE_TYPES`, `VITE_APP_NAME`
- CI/CD: GitHub Actions → lint + type-check → test → build → deploy

## 11. Testing Strategy
- **Unit**: Vitest untuk treeUtils, validators, fuse search scoring
- **Component**: Testing Library untuk FolderTree, UploadZone, PreviewPanel
- **Integration**: Flow: upload doc → muncul di grid → click → preview terbuka
- **E2E**: Playwright untuk folder CRUD, search, upload version

## 12. Dependencies & Constraints
- **Dependencies**: React 18+, Tailwind v3+, react-pdf, react-markdown, Fuse.js
- **Constraint**: PDF preview menggunakan PDF.js — tidak support semua PDF (scan, encrypted)
- **Constraint**: Search hanya untuk metadata (judul, deskripsi, tag) — full-text isi dokumen Phase 2
- **Constraint**: Storage di MVP menggunakan localStorage/blob URL — data hilang saat refresh
- **Constraint**: Tidak ada real-time kolaborasi di MVP (Google Docs-style)
