# TRD: CRM — Technical Requirements

**Project:** CRM
**Version:** 1.0

---

## 1. Technical Strategy

CRM akan dibangun sebagai **static frontend boilerplate** dengan mock data. Arsitektur berfokus pada tiga-panel layout dengan state management terpusat, drag-and-drop pipeline, dan activity timeline.

```text
[List Panel] --> [Detail Panel] --> [Activity Timeline]
      |               |
      v               v
[Filter/Search]   [CRUD Forms]
      |
      v
[Pipeline Kanban] --> [Drag & Drop] --> [Stage Update]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript + Tailwind CSS |
| Styling | Tailwind CSS |
| Icons | Lucide React |
| Animations | Framer Motion |
| Charts | Recharts (pipeline value chart) |
| UI Library | shadcn/ui |
| DnD Library | @hello-pangea/dnd (drag-and-drop pipeline) |
| Data | Mock JSON / TypeScript interfaces |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- ui/                    # shadcn/ui base components
|   |-- contacts/
|   |   |-- ContactTable.tsx
|   |   |-- ContactCard.tsx
|   |   |-- ContactForm.tsx
|   |   |-- ContactDetail.tsx
|   |-- companies/
|   |   |-- CompanyTable.tsx
|   |   |-- CompanyForm.tsx
|   |   |-- CompanyDetail.tsx
|   |-- deals/
|   |   |-- DealCard.tsx
|   |   |-- DealForm.tsx
|   |   |-- DealDetail.tsx
|   |   |-- PipelineBoard.tsx
|   |   |-- PipelineStage.tsx
|   |-- activities/
|   |   |-- ActivityTimeline.tsx
|   |   |-- ActivityItem.tsx
|   |   |-- ActivityForm.tsx
|   |-- dashboard/
|       |-- SalesMetrics.tsx
|       |-- PipelineChart.tsx
|       |-- RecentActivity.tsx
|-- features/
|   |-- contacts/
|   |   |-- useContacts.ts
|   |   |-- ContactsPage.tsx
|   |-- deals/
|   |   |-- useDeals.ts
|   |   |-- usePipeline.ts
|   |   |-- PipelinePage.tsx
|   |-- activities/
|       |-- useActivities.ts
|-- lib/
|   |-- stage-utils.ts
|   |-- format-utils.ts
|-- data/
|   |-- mockContacts.ts
|   |-- mockCompanies.ts
|   |-- mockDeals.ts
|   |-- mockActivities.ts
|-- types/
|   |-- contact.ts
|   |-- company.ts
|   |-- deal.ts
|   |-- activity.ts
|-- hooks/
|   |-- useLocalStorage.ts
|   |-- useDebounce.ts
`-- pages/
    |-- DashboardPage.tsx
    |-- ContactsPage.tsx
    |-- CompaniesPage.tsx
    |-- PipelinePage.tsx
    |-- ActivitiesPage.tsx
```

---

## 4. Data Layer

### State Management
- **Context API** untuk shared state: active contact, active deal, pipeline state
- **useState + useReducer** untuk complex state (deal list, filtered contacts)
- **localStorage** untuk persist data antar session (opsional)

### Mock Data Strategy
- 15+ kontak dengan data realistis Indonesia
- 8+ perusahaan dengan industri beragam
- 12+ deal di berbagai stage pipeline
- 20+ aktivitas dengan tipe berbeda
- Data relasional: kontak terhubung ke perusahaan dan deal

### Pipeline Logic
- Stage order: Lead In → Qualified → Proposal → Negotiation → Closed Won / Closed Lost
- Drag-and-drop menggunakan @hello-pangea/dnd
- Stage transition validation: deal tidak bisa mundur stage
- Total value accumulation per stage

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 500KB (gzipped) |

---

## 6. Styling & Theming System

- **Design Token:** CSS custom properties untuk warna, spacing, typography
- **Mode:** Light mode default
- **Stage Colors:** Lead In (#64748B), Qualified (#2563EB), Proposal (#7C3AED), Negotiation (#D97706), Won (#059669), Lost (#DC2626)

```css
:root {
  --color-primary: #2563EB;
  --color-stage-lead: #64748B;
  --color-stage-qualified: #2563EB;
  --color-stage-proposal: #7C3AED;
  --color-stage-negotiation: #D97706;
  --color-stage-won: #059669;
  --color-stage-lost: #DC2626;
  /* ... tokens */
}
```

---

## 7. Validation Rules

- Kontak: nama required (min 3 char), email valid format, telepon required (min 10 digit)
- Perusahaan: nama required (min 3 char), website valid URL format
- Deal: judul required, nilai > 0, expected close date harus di masa depan
- Aktivitas: tipe required, deskripsi required (min 10 char)
- Stage transition: alasan required untuk Closed Lost
- Format currency: Rp (IDR)
- Format tanggal: DD/MM/YYYY

---

## 8. Error Handling Strategy

- **CRUD errors:** "Gagal menyimpan data. Silakan coba lagi." dengan retry
- **Form validation:** Inline error pada setiap field
- **Drag-and-drop error:** Deal kembali ke stage asal jika validasi gagal
- **Empty states:** Kustom per modul (no contacts, no deals, no activities)
- **Global error boundary:** React Error Boundary
- **Optimistic updates:** Drag langsung tampil di stage baru, rollback jika error

---

## 9. Security Considerations

- **Mock only:** Tidak ada autentikasi real
- **Role simulation:** Basic role check (admin vs sales rep) untuk akses fitur
- **Data privacy:** Deal values hanya visible untuk owner dan admin (simulasi)
- **Input sanitization:** Untuk form input

---

## 10. Deployment & Build

- **Build:** `npm run build` (Vite)
- **Output:** Static files (HTML, CSS, JS)
- **Hosting:** Static hosting siap (Netlify, Vercel, GitHub Pages)
- **CI/CD:** Opsional — GitHub Actions untuk deploy otomatis

---

## 11. Testing Strategy

| Level | Tools | Coverage Target |
|-------|-------|----------------|
| Unit | Vitest | > 70% |
| Component | React Testing Library | Komponen kritis (ContactForm, PipelineBoard, DealCard) |
| Integration | Vitest | Pipeline drag-and-drop logic, contact-deal relationship |
| E2E | Playwright (opsional) | Create contact, move deal in pipeline, log activity |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- Node.js 18+
- NPM / Yarn / PNPM
- Framer Motion 10+
- Lucide React
- Recharts
- @hello-pangea/dnd
- shadcn/ui (via Tailwind + Radix UI)

### Constraints
- Tidak ada backend dependency
- Tidak ada API key / autentikasi real
- Semua fitur harus berfungsi offline
- @hello-pangea/dnd terbatas untuk touch devices
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
