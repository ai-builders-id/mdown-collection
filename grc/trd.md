# TRD: GRC — Technical Requirements

**Project:** GRC (Governance Risk Compliance)
**Version:** 1.0

---

## 1. Technical Strategy

GRC akan dibangun sebagai **static frontend boilerplate** dengan mock data. Arsitektur berfokus pada visualisasi data risiko dan kepatuhan yang interaktif dengan kanban board dan heatmap.

```text
[RiskBoard] --> [DragDropProvider] --> [RiskStore]
                [HeatmapChart]  --> [ScoreCalculator]
[Compliance] --> [FilterProvider] --> [ComplianceStore]
                [StatusBadge]   --> [DeadlineChecker]
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
|   |-- layout/                   # DashboardLayout, Sidebar, TopBar
|   |-- RiskKanbanBoard.tsx        # Kanban 5 kolom
|   |-- RiskCard.tsx              # Kartu risiko dalam kanban
|   |-- RiskHeatmap.tsx           # Matriks 5x5 heatmap
|   |-- RiskFormModal.tsx         # Form input risiko
|   |-- ComplianceTable.tsx       # Tabel compliance dengan filter
|   |-- ComplianceStatusBadge.tsx # Badge status kepatuhan
|   |-- AuditTemuanCard.tsx       # Kartu temuan audit
|   |-- SeverityBadge.tsx         # Badge severity
|   |-- KebijakanList.tsx         # Daftar kebijakan
|   |-- StatCard.tsx              # Kartu statistik
|   |-- EmptyState.tsx            # Empty state
|   |-- ErrorState.tsx            # Error state
|-- features/
|   |-- risk/                     # Risk register modul
|   |-- compliance/               # Compliance tracker modul
|   |-- audit/                    # Audit temuan modul
|   |-- kebijakan/                # Kebijakan modul
|   |-- dashboard/                # Dashboard modul
|-- lib/
|   |-- format.ts                 # Formatters
|   |-- riskCalculator.ts         # Skor risiko logic
|   |-- dateUtils.ts              # Date utilities
|-- data/
|   |-- riskData.ts               # Mock risiko
|   |-- complianceData.ts         # Mock compliance
|   |-- auditData.ts              # Mock audit temuan
|   |-- kebijakanData.ts          # Mock kebijakan
|-- types/
|   |-- index.ts                  # All interfaces
|-- hooks/
|   |-- useRisks.ts               # Risk CRUD + drag & drop
|   |-- useCompliance.ts          # Compliance management
|   |-- useAudit.ts               # Audit temuan management
|   |-- useKebijakan.ts           # Kebijakan management
`-- pages/
    |-- DashboardPage.tsx
    |-- RiskRegisterPage.tsx
    |-- CompliancePage.tsx
    |-- AuditPage.tsx
    |-- KebijakanPage.tsx
```

---

## 4. Data Layer

### State Management
- Local state dengan useReducer untuk complex state (kanban board)
- Context API: RiskContext (selected filters), NotificationContext

### Mock Data Strategy
- 25+ risiko dengan variasi kategori, skor, status
- 15+ compliance items dengan berbagai regulasi (UU ITE, UU PDP, ISO 27001, dll.)
- 10+ audit temuan dengan berbagai severity
- 5+ kebijakan dengan versi dan status

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 500KB (gzipped) |
| Drag & drop response | < 100ms |
| Render heatmap 5x5 | < 200ms |

---

## 6. Styling & Theming System

```css
:root {
  --color-primary: #78716C;
  --color-primary-light: #A8A29E;
  --color-primary-dark: #57534E;
  --color-risk-low: #22C55E;
  --color-risk-medium: #EAB308;
  --color-risk-high: #F97316;
  --color-risk-critical: #EF4444;
  --color-compliant: #22C55E;
  --color-partial: #EAB308;
  --color-noncompliant: #EF4444;
  --color-bg: #FAFAF9;
  --color-bg-secondary: #F5F5F4;
  --color-text: #1C1917;
  --color-text-secondary: #78716C;
}
```

---

## 7. Validation Rules

- Kode risiko: format unik otomatis (RISK-DEPT-NNN)
- Skor risiko: numeric 1-25 (hasil kalkulasi dampak x probabilitas)
- Dampak & probabilitas: 5 level (1=sangat rendah, 5=sangat tinggi)
- Tenggat: tanggal valid, tidak boleh di masa lalu untuk item baru
- Format tanggal: DD/MM/YYYY

---

## 8. Error Handling Strategy

- Error boundary per modul
- Form validation inline
- Toast notification untuk CRUD
- Confirm dialog untuk delete risk/compliance/audit
- Simulasi error untuk testing

---

## 9. Security Considerations

- **Mock only:** Tidak ada autentikasi real
- Role simulation: Risk & Compliance Officer (CRUD), Manajemen (read-only), Auditor (audit module)
- Sensitive data: semua data dummy, tidak ada data real perusahaan

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
| Unit | Vitest | > 70% (risk calculator, date utils) |
| Component | React Testing Library | RiskCard, RiskHeatmap, ComplianceTable |
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
- Drag & drop menggunakan @dnd-kit (ringan, modern)
- Tidak ada workflow approval real (simulasi)
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
