# TRD: Chat AI Prompt Generator — Technical Requirements

**Project:** Chat AI Prompt Generator
**Version:** 1.0

---

## 1. Technical Strategy

{{PROJECT_NAME}} dibangun sebagai **static frontend boilerplate** dengan fokus pada rich text editing dan real-time preview. Arsitektur berbasis komponen dengan state management lokal.

```text
[React Components] --> [Context API State] --> [Mock Data]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript |
| Styling | Tailwind CSS |
| Editor | CodeMirror 6 dengan language support |
| Icons | Lucide React |
| Animation | Framer Motion |
| State | React Context + useReducer |
| Data | Mock TypeScript interfaces |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- layout/
|   |   |-- Sidebar.tsx
|   |   |-- MainPanel.tsx
|   |   |-- RightPanel.tsx
|   |-- editor/
|   |   |-- PromptEditor.tsx
|   |   |-- VariableForm.tsx
|   |   |-- PreviewPanel.tsx
|   |-- library/
|   |   |-- TemplateGrid.tsx
|   |   |-- TemplateCard.tsx
|   |   |-- CategoryTree.tsx
|   |   |-- SearchBar.tsx
|   |-- history/
|   |   |-- HistoryList.tsx
|   |   |-- HistoryItem.tsx
|   |-- collections/
|       |-- CollectionList.tsx
|       |-- CollectionDetail.tsx
|-- data/
|   |-- templates.ts        # Mock template data
|   |-- categories.ts       # Mock categories
|   |-- history.ts          # Mock history
|-- types/
|   |-- prompt.ts           # TypeScript interfaces
|-- hooks/
|   |-- useVariableParser.ts  # Auto-detect {{variable}}
|   |-- useClipboard.ts       # Copy to clipboard
|   |-- useTemplateFilter.ts  # Filter & search
|-- utils/
|   |-- tokenCounter.ts     # Token estimation
|   |-- formatter.ts        # Text formatting
```

---

## 4. Data Layer

### State Management
- **AppContext:** Global state untuk theme, active template, history
- **EditorContext:** State editor (content, variables, preview)
- **LibraryContext:** Filter, search, pagination state

### Mock Data Strategy
- 50+ template prompt dalam file TypeScript
- 8 kategori (ChatGPT, Claude, Gemini, Midjourney, DALL-E, Stable Diffusion, Copilot, Custom)
- Data tersegmentasi berdasarkan use case (bisnis, coding, creative, academic, dll)

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Editor Latency | < 100ms |
| Library Load | < 500ms untuk 50+ template |
| Bundle Size | < 300KB (gzipped) |

---

## 6. Styling & Theming System

```css
:root {
  /* Light mode */
  --color-primary: #A855F7;
  --color-accent: #06B6D4;
  --color-bg: #FAFAFA;
  --color-surface: #FFFFFF;
  --color-text: #1A1A2E;
  --color-text-muted: #64748B;
  --color-border: #E2E8F0;
}

[data-theme="dark"] {
  --color-bg: #0F172A;
  --color-surface: #1E293B;
  --color-text: #F1F5F9;
  --color-text-muted: #94A3B8;
  --color-border: #334155;
}

:root {
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;
}
```

---

## 7. Validation Rules

- `{{variable}}` format: huruf kecil, underscore untuk spasi
- Required variable tidak boleh kosong untuk copy
- Token estimation: 1 token ≈ 4 karakter (estimasi)
- Judul template: 3-100 karakter
- Tags: maksimal 10 tags per template

---

## 8. Error Handling Strategy

- **Copy Error:** Fallback ke seleksi manual, tampilkan instruksi
- **Template Load Error:** Tampilkan template default
- **LocalStorage Full:** Notifikasi dan hapus history tertua
- **Editor Crash:** Save draft ke localStorage, reload editor

---

## 9. Security Considerations

- **XSS Prevention:** Sanitasi input variable sebelum ditampilkan di preview
- **LocalStorage:** Hanya untuk history dan collections — tidak ada data sensitif
- **Clipboard:** Menggunakan Clipboard API dengan fallback

---

## 10. Deployment & Build

- **Build:** `npm run build` (Vite)
- **Output:** Static files ke `dist/`
- **Hosting:** Vercel / Netlify / GitHub Pages
- **PWA:** manifest.json + service worker untuk offline

---

## 11. Testing Strategy

| Level | Tools | Coverage Target |
|-------|-------|----------------|
| Unit (Variable Parser) | Vitest | > 90% |
| Component (Editor, Form) | React Testing Library | > 70% |
| Integration | Vitest + RTL | Flow utama |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- CodeMirror 6 (untuk editor)
- Node.js 18+

### Constraints
- Tidak ada backend — semua data di frontend
- LocalStorage terbatas (5-10 MB)
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
- Wajib support offline mode
