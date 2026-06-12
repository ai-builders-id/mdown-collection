# TRD: Catat.in — Technical Requirements

## 1. Technical Strategy
Catat.in dibangun sebagai aplikasi client-side 100% (no backend) dengan arsitektur offline-first. Semua data disimpan di localStorage dan IndexedDB. Fokus utama adalah kecepatan loading, responsivitas editor markdown, dan UX yang mulus tanpa lag. Menggunakan pendekatan local-first dengan opsi cloud sync di fase berikutnya.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Framework | React | 18 | Vite build tool |
| Bahasa | TypeScript | 5 | Strict mode |
| Styling | Tailwind CSS | 3 | + typography plugin |
| State | Zustand | 4 | Notes & UI state |
| Markdown | react-markdown + remark-gfm | 9 | Render markdown |
| Editor | @uiw/react-codemirror | 4 | CodeMirror 6 markdown editor |
| Search | Fuse.js | 7 | Client-side fuzzy search |
| Icons | lucide-react | latest | UI icons |
| Drag-Drop | dnd-kit | latest | Folder tree drag |
| Date | date-fns | 3 | Date formatting |

## 3. Component Architecture (tree)

```
src/
├── components/
│   ├── layout/
│   │   ├── AppLayout.tsx            # 3-pane layout
│   │   ├── Sidebar.tsx              # Folder + Tag panel
│   │   ├── NoteList.tsx             # List catatan
│   │   └── EditorPane.tsx           # Editor + Preview
│   ├── sidebar/
│   │   ├── FolderTree.tsx           # Nested folder tree
│   │   ├── FolderItem.tsx           # Single folder node
│   │   ├── TagList.tsx              # Tag cloud sidebar
│   │   └── TagBadge.tsx             # Colored tag chip
│   ├── note/
│   │   ├── NoteListItem.tsx         # Row di list
│   │   ├── NoteCard.tsx             # Card (alternate view)
│   │   ├── NoteMeta.tsx             # Metadata: dates, tags
│   │   └── NoteActions.tsx          # Pin, fav, archive, delete
│   ├── editor/
│   │   ├── MarkdownEditor.tsx       # CodeMirror wrapper
│   │   ├── MarkdownPreview.tsx      # react-markdown render
│   │   ├── SplitPane.tsx            # Resizable split
│   │   ├── FloatingToolbar.tsx      # Mini toolbar on select
│   │   └── SaveIndicator.tsx        # Save status dot
│   ├── search/
│   │   ├── SearchBar.tsx
│   │   └── SearchResults.tsx        # Highlighted results
│   ├── folder/
│   │   ├── FolderForm.tsx           # Create/rename modal
│   │   └── FolderIcon.tsx           # Emoji picker
│   ├── tag/
│   │   ├── TagForm.tsx              # Create/edit tag
│   │   ├── TagAutocomplete.tsx      # Tag input with suggestions
│   │   └── TagColorPicker.tsx
│   └── shared/
│       ├── EmptyState.tsx
│       ├── ConfirmDialog.tsx
│       └── KeyboardShortcutHint.tsx
├── stores/
│   ├── notesStore.ts                # CRUD notes + persist
│   ├── folderStore.ts
│   ├── tagStore.ts
│   └── uiStore.ts                   # Active note, theme, view
├── hooks/
│   ├── useAutoSave.ts               # 30s interval auto-save
│   ├── useKeyboardShortcuts.ts      # Global keyboard
│   └── useSearch.ts                 # Fuse.js hook
├── services/
│   ├── storageService.ts            # localStorage + IndexedDB
│   ├── searchService.ts             # Fuse.js instance
│   └── exportService.ts             # Export markdown/PDF
├── types/
│   └── index.ts
├── data/
│   └── mockData.ts
└── utils/
    ├── formatters.ts
    ├── validators.ts
    └── markdownHelpers.ts           # Cheatsheet, shortcuts
```

## 4. Data Layer
- **Primary Store**: Zustand with `persist` middleware → localStorage
- **Version History**: IndexedDB via `idb-keyval` library (lebih cocok untuk data besar)
- **Search Index**: Fuse.js diinisialisasi dari seluruh notes, di-rebuild saat notes berubah
- **No Backend**: 100% client-side. Backup manual via export .zip (Phase 2)
- **Data Migration**: Version key di schema untuk handle breaking changes

## 5. Performance Requirements
- Initial load (10.000 notes): < 2 detik
- New note creation: < 100ms
- Search 10.000 notes: < 200ms (Fuse.js dengan limit 50)
- Markdown render: < 50ms per note (react-markdown memoized)
- Auto-save: < 50ms write ke localStorage
- Bundle size: < 300KB gzipped
- Lighthouse Performance: > 90

## 6. Styling & Theming System (CSS vars)

```css
:root {
  --ct-primary: #6366f1;
  --ct-primary-light: #a5b4fc;
  --ct-surface: #ffffff;
  --ct-surface-secondary: #f8fafc;
  --ct-sidebar: #f1f5f9;
  --ct-text: #1e293b;
  --ct-text-muted: #64748b;
  --ct-text-heading: #0f172a;
  --ct-border: #e2e8f0;
  --sidebar-width: 240px;
  --note-list-width: 320px;
  --font-editor: 'JetBrains Mono', monospace;
  --font-ui: 'Inter', sans-serif;
  --font-preview: 'Inter', serif;
  --editor-font-size: 15px;
  --line-height: 1.8;
}
.dark {
  --ct-surface: #0f172a;
  --ct-surface-secondary: #1e293b;
  --ct-sidebar: #1e293b;
  --ct-text: #e2e8f0;
  --ct-text-muted: #94a3b8;
  --ct-text-heading: #f1f5f9;
  --ct-border: #334155;
}
.sepia {
  --ct-surface: #fbf7f0;
  --ct-surface-secondary: #f5ede3;
  --ct-sidebar: #f5ede3;
  --ct-text: #5c4b3a;
  --ct-text-muted: #8c7a6a;
  --ct-border: #e6d9c8;
}
```

## 7. Validation Rules
- Judul note: 1-200 karakter, trim whitespace
- Nama folder: 1-50 karakter, tidak boleh / \ : * ? " < > |
- Nama tag: 1-30 karakter, lowercase, alphanumeric + dash, unique
- Konten markdown: Tidak ada batasan (teoritis hingga 1MB per note)
- Kedalaman folder: Maks 5 level
- Tags per note: Maks 10
- Version history: Maks 10 versi per note (FIFO)

## 8. Error Handling Strategy
- **localStorage full**: Catch QuotaExceededError → notif "Penyimpanan penuh. Hapus beberapa catatan atau export."
- **IndexedDB error**: Fallback ke localStorage untuk version history
- **Markdown render error**: `try/catch` react-markdown → render plaintext
- **Auto-save conflict**: Last-write-wins (single user, minimal conflict)
- **Editor crash**: Error boundary → "Terjadi kesalahan. Catatan terakhir tersimpan."
- **Delete note**: Konfirmasi "Yakin hapus {{judul}}? Masih bisa dipulihkan dari trash."
- **Empty state guidance**: Tampilkan tips "Coba ketik `#` untuk heading" dan shortcut guide

## 9. Security Considerations
- Client-side only: Tidak ada data yang dikirim ke server (MVP)
- XSS: react-markdown tidak mengeksekusi HTML raw secara default (escape HTML)
- Sanitasi input: Judul dan konten di-trim, tag di-lowercase
- No authentication: Semua data di browser lokal (privasi penuh)
- Export: File .md didownload langsung, tidak melalui server

## 10. Deployment & Build
- Build: `npm run build` → `/dist`
- Deploy: Vercel (static site) atau GitHub Pages
- Environment: `VITE_APP_NAME`, `VITE_MAX_NOTES` (untuk performance warning), `VITE_VERSION`
- PWA: Workbox/VitePWA plugin untuk service worker + offline support
- CI/CD: GitHub Actions → lint + type-check + build

## 11. Testing Strategy
- **Unit**: Vitest untuk store operations (addNote, deleteNote, search), validators
- **Component**: Testing Library untuk NoteListItem, FolderTree, TagBadge
- **Integration**: Flow: create folder → create note → assign tag → search → find note
- **E2E**: Playwright untuk full cycle: create → edit → search → delete → restore
- **Storage**: Test localStorage + IndexedDB boundary conditions

## 12. Dependencies & Constraints
- **Dependencies**: React 18+, Tailwind v3+, @uiw/react-codemirror, react-markdown, Fuse.js
- **Constraint**: 100% client-side — tidak ada backup cloud (kecuali manual export)
- **Constraint**: localStorage terbatas ~5-10MB (tergantung browser) — perhatikan ukuran data
- **Constraint**: Tidak ada real-time collaboration (single user app)
- **Constraint**: Performance mungkin turun jika > 50.000 notes (perlu virtual scrolling)
- **Constraint**: Markdown editor (CodeMirror) mungkin berat di mobile — fallback ke textarea biasa
