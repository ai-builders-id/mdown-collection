# TRD: Business Intelligence — Technical Requirements

**Project:** Business Intelligence
**Version:** 1.0

---

## 1. Technical Strategy

Business Intelligence Dashboard akan dibangun sebagai **static frontend boilerplate** dengan mock data dan generator data realistis. Arsitektur berfokus pada widget-based dashboard dengan integrasi chart library dan layout grid interaktif.

```text
[Dashboard Grid] --> [Widget Container] --> [Chart Components]
      |                      |
      v                      v
[Date Range Filter]    [Mock Data Generator]
      |                      |
      v                      v
[Report Builder] <---- [Data Aggregator]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript + Tailwind CSS |
| Styling | Tailwind CSS |
| Icons | Lucide React |
| Animations | Framer Motion |
| Charts | Recharts (BarChart, LineChart, PieChart, AreaChart) |
| UI Library | shadcn/ui |
| Grid Layout | react-grid-layout (drag & resize widget) |
| Data | Mock data dengan generator fungsi |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- ui/                    # shadcn/ui base components
|   |-- dashboard/
|   |   |-- DashboardGrid.tsx
|   |   |-- WidgetContainer.tsx
|   |   |-- AddWidgetPanel.tsx
|   |   |-- DateRangePicker.tsx
|   |-- widgets/
|   |   |-- MetricCard.tsx
|   |   |-- BarChartWidget.tsx
|   |   |-- LineChartWidget.tsx
|   |   |-- PieChartWidget.tsx
|   |   |-- DataTableWidget.tsx
|   |   |-- SparklineChart.tsx
|   |-- reports/
|   |   |-- ReportList.tsx
|   |   |-- ReportBuilder.tsx
|   |   |-- ReportView.tsx
|   |-- filters/
|       |-- FilterBar.tsx
|       |-- FilterChip.tsx
|-- features/
|   |-- dashboard/
|   |   |-- useDashboard.ts
|   |   |-- useWidgetData.ts
|   |   |-- DashboardPage.tsx
|   |-- reports/
|       |-- useReports.ts
|       |-- ReportsPage.tsx
|-- lib/
|   |-- data-generators.ts     # Fungsi generator data realistis
|   |-- chart-config.ts        # Konfigurasi chart reusable
|   |-- format-utils.ts        # Format currency, number, date
|-- data/
|   |-- mockMetrics.ts
|   |-- mockSalesData.ts
|   |-- mockUserData.ts
|   |-- mockReports.ts
|-- types/
|   |-- dashboard.ts
|   |-- widget.ts
|   |-- report.ts
|   |-- data-source.ts
|-- hooks/
|   |-- useLocalStorage.ts
|   |-- useDebounce.ts
|   |-- useMediaQuery.ts
`-- pages/
    |-- DashboardPage.tsx
    |-- AnalyticsPage.tsx
    |-- ReportsPage.tsx
    |-- DataExplorerPage.tsx
```

---

## 4. Data Layer

### State Management
- Context API untuk shared state (date range, theme, active dashboard)
- useState untuk per-widget data
- useReducer untuk complex widget grid state (positions, sizes)

### Mock Data Generator
- Fungsi generator untuk data time-series (sales trends, user growth)
- Randomized data dengan seed untuk konsistensi
- Data points mengikuti pola realistis (weekend dip, month-end spike)
- Support multiple granularity: daily, weekly, monthly

### Layout Persistence
- Widget positions dan sizes disimpan di localStorage
- Layout restore on page reload
- Reset to default layout option

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 500KB (gzipped) |
| Widget render time | < 100ms per widget |
| Dashboard with 12 widgets | Load < 2 detik |

---

## 6. Styling & Theming System

- **Design Token:** CSS custom properties untuk warna, spacing, typography
- **Mode:** Dark mode default untuk dashboard; Light mode optional
- **Responsive:** Desktop-first (dashboard grid optimal di layar lebar)

```css
:root {
  --color-primary: #059669;
  --color-bg-dashboard: #0F172A;   /* dark mode */
  --color-bg-card: #1E293B;
  --color-chart-line: #059669;
  --color-chart-bar: #0D9488;
  --color-chart-pie-1: #059669;
  --color-chart-pie-2: #0D9488;
  --color-chart-pie-3: #14B8A6;
  /* ... tokens */
}
```

---

## 7. Validation Rules

- Date range: end date tidak boleh sebelum start date
- Max date range: tidak boleh > 1 tahun
- Report title: required, max 100 karakter
- Widget minimum size: 2 kolom x 1 baris
- Format currency: Rp (IDR)
- Format tanggal: DD/MM/YYYY
- Validation messages in Bahasa Indonesia

---

## 8. Error Handling Strategy

- **Per-widget error:** Satu widget gagal tidak memengaruhi widget lain — error card di widget tersebut
- **Data load error:** "Gagal memuat data untuk widget [nama]. Coba refresh." dengan tombol retry per widget
- **Empty data state:** "Tidak ada data untuk periode yang dipilih."
- **Global error boundary:** React Error Boundary
- **Chart render error:** Fallback text "Data tidak dapat divisualisasikan"

---

## 9. Security Considerations

- **Mock only:** Tidak ada autentikasi real, UI siap untuk login
- **Data isolation:** Data mock tidak mengandung informasi sensitif
- **Input sanitization:** Untuk filter input dan form report title

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
| Component | React Testing Library | Komponen kritis (MetricCard, ChartWidget, DateRangePicker) |
| Integration | Vitest | Widget data flow + filter |
| Visual | Storybook (opsional) | Semua widget chart |
| E2E | Playwright (opsional) | Dashboard layout, filter date range |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- Node.js 18+
- NPM / Yarn / PNPM
- Framer Motion 10+
- Lucide React
- Recharts 2+
- react-grid-layout
- shadcn/ui (via Tailwind + Radix UI)

### Constraints
- Tidak ada backend dependency
- Tidak ada API key / autentikasi real
- Semua fitur harus berfungsi offline
- Data generator harus deterministic (seed-based)
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
- react-grid-layout: tidak support drag di mobile (fallback ke layout statis)
