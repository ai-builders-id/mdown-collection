# TRD: Human Resource Management — Technical Requirements

**Project:** Human Resource Management
**Version:** 1.0

---

## 1. Technical Strategy

Human Resource Management akan dibangun sebagai **static frontend boilerplate** dengan mock data. Arsitektur berbasis komponen dengan fokus pada rapid prototyping dan kemudahan integrasi backend di masa depan.

```text
[UI Component] --> [Hook Layer (useKaryawan, usePayroll, etc.)]
                       --> [Mock Data Layer (data/*.ts)]
                           --> [State Management (React Context)]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript + Tailwind CSS |
| Styling | Tailwind CSS + CSS Modules untuk komponen kompleks |
| Icons | Lucide React |
| Animations | Framer Motion |
| Charts | Recharts |
| UI Library | shadcn/ui |
| Data | Mock TypeScript interfaces & arrays |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/              # Shared UI components
|   |-- ui/                  # shadcn/ui primitives (Button, Input, Dialog, etc.)
|   |-- layout/              # AppShell, Sidebar, Header, DetailPanel
|   |-- StatusBadge.tsx      # Badge warna untuk status
|   |-- StatCard.tsx         # Kartu statistik
|   |-- DataTable.tsx        # Tabel generik dengan sorting, filter, pagination
|   |-- FormModal.tsx        # Modal form reusable
|   |-- EmptyState.tsx       # Empty state component
|   |-- ErrorState.tsx       # Error state dengan retry
|-- features/
|   |-- karyawan/            # Manajemen karyawan (pages, hooks, components)
|   |-- rekrutmen/           # Rekrutmen pipeline (kanban board)
|   |-- payroll/             # Payroll calculator & history
|   |-- onboarding/          # Onboarding checklist
|   |-- dashboard/           # Dashboard dengan statistik dan grafik
|-- lib/                     # Utilities, helpers, formatters
|   |-- format.ts            # Format currency, date
|   |-- utils.ts             # Helper functions
|-- data/                    # Mock data
|   |-- karyawanData.ts
|   |-- rekrutmenData.ts
|   |-- payrollData.ts
|-- types/                   # TypeScript interfaces
|   |-- index.ts
|-- hooks/                   # Custom React hooks
|   |-- useKaryawan.ts       # CRUD operations untuk karyawan
|   |-- useRekrutmen.ts
|   |-- usePayroll.ts
|   |-- useOnboarding.ts
`-- pages/                   # Page-level components
    |-- DashboardPage.tsx
    |-- KaryawanPage.tsx
    |-- RekrutmenPage.tsx
    |-- PayrollPage.tsx
    |-- OnboardingPage.tsx
```

---

## 4. Data Layer

### State Management
- Local state dengan React hooks (useState, useReducer)
- Context API untuk shared state:
  - AuthContext: mock login state (user role, name)
  - ThemeContext: dark/light mode
  - SelectionContext: selected karyawan untuk detail panel

### Mock Data Strategy
- Semua data di-hardcode dalam file TypeScript terpisah per modul
- Struktur data mengikuti interface yang didefinisikan di types/
- Data realistic dengan format Indonesia (Rp, DD/MM/YYYY, nama Indonesia)
- Delay simulasi 300-800ms untuk realistis loading state

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 500KB (gzipped) |
| Waktu render tabel 100 baris | < 500ms |

---

## 6. Styling & Theming System

- **Design Token:** CSS custom properties untuk warna, spacing, typography
- **Mode:** Light / Dark mode support dengan toggle di header
- **Responsive:** Mobile-first dengan breakpoints Tailwind

```css
:root {
  --color-primary: #8B5CF6;
  --color-primary-light: #A78BFA;
  --color-primary-dark: #7C3AED;
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-danger: #EF4444;
  --color-bg: #FFFFFF;
  --color-bg-secondary: #F9FAFB;
  --color-text: #111827;
  --color-text-secondary: #6B7280;
}

.dark {
  --color-bg: #111827;
  --color-bg-secondary: #1F2937;
  --color-text: #F9FAFB;
  --color-text-secondary: #9CA3AF;
}
```

---

## 7. Validation Rules

- Form validation menggunakan HTML5 built-in + custom validation messages
- Format currency: Rp (IDR) dengan pemisah ribuan dan format Indonesia
- Format tanggal: DD/MM/YYYY
- Validasi khusus:
  - NIK: minimum 5 karakter, unik
  - Email: format email valid
  - Telepon: minimal 10 digit angka
  - Gaji: numeric, minimal 3000000
- Semantic validation messages in Bahasa Indonesia

---

## 8. Error Handling Strategy

- Mock error states untuk setiap operasi CRUD (dengan probability 5% error simulasi)
- Global error boundary dengan fallback UI dan tombol "Muat Ulang"
- Toast notification system untuk feedback:
  - Sukses: hijau, icon centang, auto-dismiss 3 detik
  - Error: merah, icon x, dismiss manual
  - Warning: kuning, icon peringatan
- Confirm dialog sebelum operasi destroy (hapus karyawan, batalkan payroll)

---

## 9. Security Considerations

- **Mock only:** Tidak ada autentikasi real, tapi UI menyediakan halaman login simulasi
- Simulasi role-based access: HR Manager (full access) vs Manajer Departemen (read-only + rekrutmen request)
- Input sanitization untuk form demo (strip HTML tags, escape special characters)
- Tidak ada penyimpanan data real (semua di memory)

---

## 10. Deployment & Build

- **Build:** `npm run build` (Vite)
- **Output:** Static files (HTML, CSS, JS) di folder dist/
- **Hosting:** Static hosting siap (Netlify, Vercel, GitHub Pages)
- **CI/CD:** Opsional — GitHub Actions untuk deploy otomatis ke GitHub Pages

---

## 11. Testing Strategy

| Level | Tools | Coverage Target |
|-------|-------|----------------|
| Unit | Vitest | > 70% untuk utility functions dan hooks |
| Component | React Testing Library | Komponen kritis: DataTable, FormModal, StatusBadge |
| Visual | Storybook (opsional) | Semua komponen UI di components/ui/ |
| E2E | Playwright (opsional) | Flow utama: CRUD karyawan, proses payroll |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- Node.js 18+
- NPM / Yarn / PNPM
- Tailwind CSS 3+
- Recharts 2+
- Framer Motion 10+
- Lucide React (^0.300+)
- shadcn/ui (komponen individual)

### Constraints
- Tidak ada backend dependency — semua data in-memory
- Tidak ada API key / autentikasi real
- Semua fitur harus berfungsi offline (static files)
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
