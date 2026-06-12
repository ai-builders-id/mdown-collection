# TRD: Learning Management System — Technical Requirements

**Project:** Learning Management System
**Version:** 1.0

---

## 1. Technical Strategy

Learning Management System akan dibangun sebagai **static frontend boilerplate** dengan mock data. Arsitektur berfokus pada pengalaman belajar yang mulus dengan navigasi antar modul yang cepat tanpa reload.

```text
[CourseView] --> [ModuleRenderer] --> [VideoPlayer | Reader | QuizComponent]
                    |                         |
                    +--- [ProgressTracker] ---+
                            |
                    [CertificateEngine]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript + Tailwind CSS |
| Styling | Tailwind CSS |
| Icons | Lucide React |
| Animations | Framer Motion |
| Charts | Recharts |
| UI Library | shadcn/ui |
| Markdown | react-markdown |
| Data | Mock TypeScript interfaces |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- ui/                       # shadcn/ui primitives
|   |-- layout/                   # AppLayout, Header, Sidebar
|   |-- CourseCard.tsx            # Kartu kursus di katalog
|   |-- ModuleSidebar.tsx         # Sidebar daftar modul
|   |-- VideoPlayer.tsx           # Mock video player
|   |-- ContentReader.tsx         # Markdown content renderer
|   |-- QuizComponent.tsx         # Quiz pilihan ganda
|   |-- ProgressBar.tsx           # Progress bar
|   |-- CertificateCard.tsx       # Tampilan sertifikat
|   |-- RatingStars.tsx           # Rating interaktif
|   |-- EmptyState.tsx            # Empty / belum terdaftar
|   |-- ErrorState.tsx            # Error dengan retry
|-- features/
|   |-- katalog/                  # Katalog & detail kursus
|   |-- player/                   # Course player & modul
|   |-- quiz/                     # Quiz system
|   |-- sertifikat/               # Sertifikat engine
|   |-- dashboard/                # Dashboard siswa
|-- lib/
|   |-- format.ts                 # Formatters
|   |-- progress.ts               # Progress calculation utils
|-- data/
|   |-- kursusData.ts             # Mock kursus
|   |-- modulData.ts              # Mock modul per kursus
|   |-- quizData.ts               # Mock quiz questions
|-- types/
|   |-- index.ts                  # All interfaces
|-- hooks/
|   |-- useKursus.ts              # Kursus CRUD
|   |-- useProgress.ts            # Progress tracking
|   |-- useQuiz.ts                # Quiz state management
|   |-- useCertificate.ts         # Certificate generation
`-- pages/
    |-- KatalogPage.tsx
    |-- DetailKursusPage.tsx
    |-- CoursePlayerPage.tsx
    |-- DashboardPage.tsx
    |-- SertifikatPage.tsx
```

---

## 4. Data Layer

### State Management
- Local state dengan useState/useReducer per halaman
- Context API: AuthContext (mock user), EnrollmentContext (kursus terdaftar)
- Progress state: useReducer dengan actions (START_MODUL, COMPLETE_MODUL, UPDATE_PROGRESS)

### Mock Data Strategy
- 8+ kursus dengan variasi kategori, level, durasi
- 50+ modul tersebar di semua kursus
- 5 set quiz dengan 5-10 soal per set
- Data disimpan dalam array TypeScript, loading simulasi dengan Promise / setTimeout

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 500KB (gzipped) |
| Navigasi antar modul | < 200ms (instant, no reload) |

---

## 6. Styling & Theming System

- **Design Token:** CSS custom properties
- **Mode:** Light / Dark mode
- **Responsive:** Mobile-first

```css
:root {
  --color-primary: #F59E0B;
  --color-primary-light: #FBBF24;
  --color-primary-dark: #D97706;
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-danger: #EF4444;
  --color-bg: #FFFBEB;
  --color-bg-secondary: #FEF3C7;
  --color-text: #1F2937;
  --color-text-secondary: #6B7280;
}
```

---

## 7. Validation Rules

- Validasi jawaban quiz: harus memilih salah satu opsi sebelum submit
- Format tanggal: DD/MM/YYYY
- Format sertifikat: Nama siswa, judul kursus, tanggal selesai, no sertifikat
- Skor quiz: numeric 0-100

---

## 8. Error Handling Strategy

- Error loading modul: fallback text + retry button
- Error quiz submission: data tersimpan di local state, tidak hilang
- Global error boundary
- Toast untuk feedback sukses/gagal

---

## 9. Security Considerations

- **Mock only:** Tidak ada autentikasi real
- Simulasi role: Siswa (default) vs Instruktur (akses管理 konten, mock UI)
- Input sanitization untuk form komentar/diskusi

---

## 10. Deployment & Build

- **Build:** `npm run build` (Vite)
- **Output:** Static files
- **Hosting:** Netlify, Vercel, GitHub Pages
- **CI/CD:** Opsional

---

## 11. Testing Strategy

| Level | Tools | Coverage Target |
|-------|-------|----------------|
| Unit | Vitest | > 70% (progress utils, quiz score) |
| Component | React Testing Library | CourseCard, QuizComponent, CertificateCard |
| Visual | Storybook (opsional) | Komponen UI |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- Node.js 18+
- Tailwind CSS 3+
- Lucide React
- shadcn/ui
- Recharts
- react-markdown
- Framer Motion

### Constraints
- Tidak ada backend dependency
- Video player menggunakan mock embed (YouTube/Vimeo placeholder)
- Tidak ada streaming real
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
