# TRD: CV Extractor — Technical Requirements

**Project:** CV Extractor
**Version:** 1.0

---

## 1. Technical Strategy

CV Extractor akan dibangun sebagai **static frontend boilerplate** dengan mock data dan simulasi parsing engine di frontend. Parsing dilakukan secara deterministik berdasarkan template CV yang sudah disediakan, bukan NLP/ML real.

```text
[DropZone Component] --> [useCVParser Hook] --> [Mock Parser Engine (data/*.ts)]
                                                        --> [CVData State] --> [Preview Components]
                                                                                --> [Export Utils]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript + Tailwind CSS |
| Styling | Tailwind CSS |
| Icons | Lucide React |
| Animations | Framer Motion |
| Charts | Recharts (untuk radar chart skill) |
| UI Library | shadcn/ui |
| Data | Mock TypeScript interfaces & CV library |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- ui/                       # shadcn/ui primitives
|   |-- layout/                   # TwoPanelLayout, Header
|   |-- DropZone.tsx              # Drag & drop upload area
|   |-- MockCVSelector.tsx        # Dropdown mock CV
|   |-- ExtractedForm.tsx         # Read-only form hasil ekstraksi
|   |-- SkillBadge.tsx            # Tag chip untuk skill
|   |-- TimelineView.tsx          # Timeline untuk pengalaman & pendidikan
|   |-- JSONPreview.tsx           # Collapsible raw JSON
|   |-- ExportDropdown.tsx        # Export menu
|   |-- EmptyState.tsx            # State sebelum upload
|   |-- ParsingProgress.tsx       # Loading animation parsing
|-- features/
|   |-- cv-extractor/             # Halaman utama CV Extractor
|-- lib/
|   |-- format.ts                 # Formatters
|   |-- parser.ts                 # Mock parser engine (simulasi)
|   |-- export.ts                 # Export functions (JSON, CSV)
|-- data/
|   |-- mockCVLibrary.ts          # 5-10 CV mock lengkap
|-- types/
|   |-- index.ts                  # Semua interfaces CV
|-- hooks/
|   |-- useCVParser.ts            # State management parsing
|   |-- useExport.ts              # Export logic
`-- pages/
    |-- CVExtractorPage.tsx       # Halaman utama
```

---

## 4. Data Layer

### State Management
- Local state dengan useState untuk current CV data
- useReducer untuk complex parsing state (idle, uploading, parsing, success, error)

### Mock Data Strategy
- 5-10 CV mock dengan variasi: fresh graduate, mid-level, senior, manager, C-level
- Data realistic dengan nama, perusahaan, universitas Indonesia
- Simulasi parsing delay 500-2000ms menggunakan setTimeout

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 400KB (gzipped) |
| Response mock parsing | < 3 detik |

---

## 6. Styling & Theming System

- **Design Token:** CSS custom properties untuk warna, spacing, typography
- **Mode:** Light / Dark mode support
- **Responsive:** Mobile-first dengan breakpoints Tailwind

```css
:root {
  --color-primary: #7C3AED;
  --color-primary-light: #A78BFA;
  --color-primary-dark: #5B21B6;
  --color-bg: #FFFFFF;
  --color-bg-secondary: #F5F3FF;
  --color-text: #1F2937;
  --color-text-secondary: #6B7280;
}

.dark {
  --color-bg: #1E1B4B;
  --color-bg-secondary: #2E1065;
  --color-text: #EDE9FE;
  --color-text-secondary: #C4B5FD;
}
```

---

## 7. Validation Rules

- Validasi tipe file upload: PDF (.pdf), DOCX (.docx), TXT (.txt)
- Ukuran file maksimal: 10MB
- Format tanggal: DD/MM/YYYY
- Validasi email, telepon pada data hasil parsing
- Input sanitization untuk teks dari CV

---

## 8. Error Handling Strategy

- Error upload: format tidak valid, ukuran terlalu besar
- Error parsing: data tidak bisa diekstrak (simulasi)
- Global error boundary
- Toast notification untuk setiap aksi: sukses export, error upload, dll.

---

## 9. Security Considerations

- **Mock only:** Tidak ada file upload real ke server — semua file dibaca sebagai ArrayBuffer lokal
- Tidak ada penyimpanan data — semua data in-memory
- Input sanitization untuk demo

---

## 10. Deployment & Build

- **Build:** `npm run build` (Vite)
- **Output:** Static files (HTML, CSS, JS)
- **Hosting:** Static hosting (Netlify, Vercel, GitHub Pages)
- **CI/CD:** Opsional — GitHub Actions

---

## 11. Testing Strategy

| Level | Tools | Coverage Target |
|-------|-------|----------------|
| Unit | Vitest | > 70% untuk utility functions, parser, export |
| Component | React Testing Library | DropZone, ExtractedForm, JSONPreview |
| Visual | Storybook (opsional) | Komponen UI |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- Node.js 18+
- NPM / Yarn / PNPM
- Tailwind CSS 3+
- Lucide React
- shadcn/ui
- Recharts (opsional untuk radar chart)

### Constraints
- Tidak ada backend dependency
- Parsing hanya simulasi — tidak menggunakan NLP/ML real
- Tidak ada penyimpanan data persisten
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
