# TRD: Healthcare EMR — Technical Requirements

**Project:** Healthcare EMR
**Version:** 1.0

---

## 1. Technical Strategy

Healthcare EMR akan dibangun sebagai **static frontend boilerplate** dengan mock data. Arsitektur berfokus pada pengelolaan data pasien yang kompleks dengan timeline medis yang terstruktur.

```text
[PatientView] --> [KunjunganTimeline] --> [VitalSignsCard]
                    |                           |
            [DiagnosisForm]               [ResepForm]
                    |                           |
                    +--- [PatientStore] ---------+
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
| Data | Mock TypeScript interfaces |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- ui/                       # shadcn/ui primitives
|   |-- layout/                   # AppLayout, ThreePanelLayout
|   |-- PatientSearch.tsx          # Search auto-suggest
|   |-- PatientCard.tsx            # Kartu ringkasan pasien
|   |-- VitalSignsCard.tsx         # Tanda-tanda vital
|   |-- KunjunganTimeline.tsx      # Timeline kronologis
|   |-- DiagnosisForm.tsx          # Form diagnosis + ICD-10
|   |-- ResepForm.tsx              # Form resep obat
|   |-- AlergiBadge.tsx            # Badge alergi
|   |-- CalendarView.tsx           # Kalender jadwal
|   |-- EmptyState.tsx             # Empty state
|   |-- ErrorState.tsx             # Error state
|-- features/
|   |-- pasien/                    # CRUD pasien
|   |-- kunjungan/                 # Rekam medis & kunjungan
|   |-- resep/                     # Resep obat
|   |-- jadwal/                    # Jadwal dokter
|   |-- dashboard/                 # Dashboard dokter
|-- lib/
|   |-- format.ts                  # Formatters
|   |-- icd10.ts                   # Mock ICD-10 codes
|-- data/
|   |-- pasienData.ts              # Mock pasien
|   |-- kunjunganData.ts           # Mock kunjungan
|   |-- resepData.ts               # Mock resep
|-- types/
|   |-- index.ts                   # All interfaces
|-- hooks/
|   |-- usePasien.ts               # Pasien CRUD
|   |-- useKunjungan.ts            # Kunjungan management
|   |-- useResep.ts                # Resep management
|   |-- useJadwal.ts               # Jadwal management
`-- pages/
    |-- DashboardPage.tsx
    |-- DaftarPasienPage.tsx
    |-- DetailPasienPage.tsx
    |-- JadwalPage.tsx
```

---

## 4. Data Layer

### State Management
- Local state dengan useState/useReducer per modul
- Context API: PatientContext (selected patient), NotificationContext

### Mock Data Strategy
- 20+ pasien dengan variasi usia, diagnosa, alergi
- 50+ kunjungan historis dengan data vital signs lengkap
- 30+ resep dengan obat-obatan Indonesia umum (Amoxicillin, Paracetamol, Metformin, dll.)
- Mock ICD-10 codes untuk diagnosis umum

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 500KB (gzipped) |
| Waktu render timeline 20 entri | < 300ms |

---

## 6. Styling & Theming System

```css
:root {
  --color-primary: #0D9488;
  --color-primary-light: #14B8A6;
  --color-primary-dark: #0F766E;
  --color-danger: #EF4444;
  --color-warning: #F59E0B;
  --color-success: #10B981;
  --color-bg: #F0FDFA;
  --color-bg-secondary: #CCFBF1;
  --color-text: #111827;
  --color-text-secondary: #6B7280;
}
```

---

## 7. Validation Rules

- Nomor RM: format unik otomatis (RM-YYYY-NNN)
- NIK/No. KTP: 16 digit angka
- Telepon: minimal 10 digit
- Tanggal: DD/MM/YYYY
- TD (Tekanan Darah): format "12x/xx" (sistol/diastol)
- Suhu: 35-42 derajat Celsius
- Berat badan: 1-300 kg
- Wajib: diagnosis, tekanan darah, suhu tubuh

---

## 8. Error Handling Strategy

- Form validation inline untuk setiap field
- Error boundary untuk setiap modul
- Toast notification untuk CRUD operations
- Confirmation dialog sebelum delete data pasien
- Simulasi error 5% untuk testing error states

---

## 9. Security Considerations

- **Mock only:** Tidak ada data pasien real
- Simulasi role: Dokter (full access), Perawat (read + vital signs)
- Data sensitif (diagnosis, alergi) hanya untuk role medis

---

## 10. Deployment & Build

- **Build:** `npm run build` (Vite)
- **Output:** Static files
- **Hosting:** Netlify, Vercel, GitHub Pages

---

## 11. Testing Strategy

| Level | Tools | Coverage Target |
|-------|-------|----------------|
| Unit | Vitest | > 70% |
| Component | React Testing Library | VitalSignsCard, DiagnosisForm, AlergiBadge |
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
- Framer Motion

### Constraints
- Tidak ada backend dependency
- Tidak ada integrasi SIMRS real
- ICD-10 mock terbatas pada 50 diagnosis umum
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
