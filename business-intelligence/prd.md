# PRD: Business Intelligence — Dashboard, Visualisasi, dan Laporan Interaktif

**Project:** Business Intelligence
**Aesthetic:** Data-driven, clean, modern dengan warna emerald yang profesional dan dashboard yang immersive

---

## 1. Core Layout

Aplikasi menggunakan layout header-sidebar-main yang fleksibel:

- **Top Header (8%):** Global search, notifikasi, date range picker, user avatar, breadcrumb.
- **Sidebar Kiri (18%):** Navigasi menu — Dashboard Overview, Analitik, Laporan, Data Explorer, Pengaturan.
- **Panel Utama (82%):** Area konten yang terdiri dari grid widget dashboard (drag-and-drop), halaman laporan, dan data table interaktif.

Untuk mobile, layout berubah menjadi single-column stack dengan sidebar sebagai bottom tab navigasi.

---

## 2. Data Structure (Mock Only)

```typescript
export interface DashboardWidget {
  id: string;
  type: 'bar_chart' | 'line_chart' | 'pie_chart' | 'metric_card' | 'data_table' | 'heatmap' | 'kpi_card';
  title: string;
  subtitle?: string;
  position: { x: number; y: number; w: number; h: number };
  dataSource: string;
  config: Record<string, unknown>;
  refreshInterval: number; // dalam detik, 0 = manual
}

export interface MetricKPI {
  id: string;
  label: string;
  value: number;
  previousValue: number;
  unit: string;
  trend: 'up' | 'down' | 'stable';
  trendPercentage: number;
  format: 'number' | 'currency' | 'percentage' | 'time';
  icon: string;
  color: string;
  sparklineData: number[];
}

export interface Report {
  id: string;
  title: string;
  description: string;
  type: 'summary' | 'detailed' | 'comparison' | 'forecast';
  createdBy: string;
  createdAt: string;
  updatedAt: string;
  filters: ReportFilter[];
  charts: string[]; // widget IDs
  schedule?: {
    frequency: 'daily' | 'weekly' | 'monthly' | 'custom';
    recipients: string[];
    lastSent: string;
  };
  status: 'draft' | 'active' | 'archived';
}

export interface ReportFilter {
  id: string;
  field: string;
  operator: 'equals' | 'contains' | 'greater_than' | 'less_than' | 'between' | 'in';
  value: string | number | string[];
}

export interface DataSource {
  id: string;
  name: string;
  type: 'sales' | 'marketing' | 'finance' | 'operations' | 'hr' | 'custom';
  lastSync: string;
  status: 'connected' | 'error' | 'syncing';
  rowCount: number;
  fields: DataField[];
}

export interface DataField {
  name: string;
  type: 'string' | 'number' | 'date' | 'boolean' | 'currency';
  label: string;
  nullable: boolean;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS dengan kustomisasi dark mode untuk dashboard
- **Icons:** Lucide React
- **Animation:** Framer Motion (transisi widget, animasi masuk data)
- **Chart:** Recharts (bar, line, pie, area, radar charts)
- **UI Library:** shadcn/ui
- **Data:** Mock data dengan generator realistis (sales trends, growth rates)

---

## 4. Styling & Theming

- **Palette:** Primary `#059669` (Emerald), dark background (`#0f172a`) untuk mode gelap, putih untuk mode terang. Aksen warna untuk setiap metrik.
- **Chart Colors:** Palet kustom untuk konsistensi emerald/teal/cyan/slate.
- **Typography:** Inter (sans-serif), monospace untuk angka/data-heavy display.
- **Dark Mode:** Default mengikuti sistem, bisa toggle manual. Dashboard tampil optimal di dark mode.

---

## 5. UI Components Specification

```text
+---------------------------------------------------------------------+
| TOP HEADER: [Logo] [Search data...]  [Date: Last 30 days v] [Notif] |
+---------------------------------------------------------------------+
| +--------+ +------------------------------------------------------+ |
| |SIDEBAR | |              MAIN DASHBOARD                          | |
| |        | |  +------------------+ +------------------+           | |
| |Overview| |  | Total Revenue    | | Active Users     |           | |
| |        | |  | Rp 2.4M  ↑12.5%  | | 1,892  ↑8.3%     |           | |
| |Analytics| |  +------------------+ +------------------+           | |
| |        | |  +------------------+ +------------------+           | |
| |Reports | |  | Conversion Rate  | | Avg Order Value   |           | |
| |        | |  | 3.2%  ↓0.5%     | | Rp 45.200  ↑2.1%  |           | |
| |Data    | |  +------------------+ +------------------+           | |
| |Settings| |  +--------------------------------------------------+ | |
| |        | |  | Revenue Trend (Last 30 Days)                      | | |
| |        | |  | [=====LINE CHART=====]                           | | |
| |        | |  +--------------------------------------------------+ | |
| |        | |  +------------------+ +-----------------------------+ | |
| |        | |  | Sales by Category| | Top Products (Table)        | | |
| |        | |  | (PIE CHART)      | | 1. Product A - Rp 120jt    | | |
| |        | |  |                  | | 2. Product B - Rp 95jt     | | |
| |        | |  |                  | | 3. Product C - Rp 78jt     | | |
| |        | |  +------------------+ +-----------------------------+ | |
| +--------+ +------------------------------------------------------+ |
+---------------------------------------------------------------------+
```

Komponen utama:
- **MetricCard:** Kartu KPI dengan ikon, nilai, tren, sparkline.
- **ChartWidget:** Container chart yang bisa di-resize dan drag (react-grid-layout).
- **DataTable:** Tabel interaktif dengan sorting, filtering, export CSV, pagination.
- **DateRangePicker:** Picker rentang tanggal dengan preset (Today, Last 7 days, Last 30 days, Custom).
- **FilterPanel:** Panel filter multi-kriteria untuk laporan.
- **DashboardGrid:** Layout grid responsif dengan draggable widgets.
- **ReportBuilder:** Antarmuka untuk membuat laporan kustom dengan drag-and-drop chart.

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton kustom untuk setiap tipe widget (bar skeleton, line skeleton); shimmer untuk metric cards.
- **Empty:** "Belum ada data untuk periode ini. Pilih rentang tanggal lain." Ilustrasi chart kosong.
- **Error:** "Gagal memuat data. Periksa koneksi data source." Kartu error per widget (tidak mengganggu widget lain).
- **Edge cases:** Data 0 untuk semua metrik; outlier nilai sangat besar; data dengan banyak decimal; filter yang menghasilkan kosong.

**Flow Interaksi Utama:**
1. User membuka Dashboard → melihat grid widget dengan data 30 hari terakhir.
2. User mengubah date range → semua widget refresh dengan data baru (simulasi delay).
3. User drag widget ke posisi baru → grid menyusun ulang (persist di localStorage).
4. User klik widget chart → drill-down ke halaman analitik detail.
5. User buka Report Builder → pilih metrik → atur filter → simpan sebagai laporan baru.
6. User export laporan ke CSV → download file mock.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Label: "Ringkasan", "Analitik", "Laporan", "Sumber Data", "Ekspor".
- Metrik: "Total Pendapatan", "Rata-rata Nilai Pesanan", "Tingkat Konversi", "Pengguna Aktif".
- Filter: "Semua Kategori", "Berdasarkan Tanggal", "Urutkan Berdasarkan".
- Tombol: "Simpan Laporan", "Ekspor CSV", "Atur Ulang", "Perbarui", "Terapkan Filter".
- Tooltip dan empty state dalam Bahasa Indonesia.

---

## 8. Example Mock Data

```typescript
export const mockMetrics: MetricKPI[] = [
  {
    id: 'metric-001',
    label: 'Total Pendapatan',
    value: 248500000,
    previousValue: 221000000,
    unit: 'Rp',
    trend: 'up',
    trendPercentage: 12.5,
    format: 'currency',
    icon: 'trending-up',
    color: '#059669',
    sparklineData: [210, 225, 218, 240, 235, 248],
  },
  {
    id: 'metric-002',
    label: 'Pengguna Aktif',
    value: 1892,
    previousValue: 1748,
    unit: 'orang',
    trend: 'up',
    trendPercentage: 8.3,
    format: 'number',
    icon: 'users',
    color: '#0284c7',
    sparklineData: [1748, 1765, 1800, 1822, 1855, 1892],
  },
  {
    id: 'metric-003',
    label: 'Tingkat Konversi',
    value: 3.2,
    previousValue: 3.7,
    unit: '%',
    trend: 'down',
    trendPercentage: -13.5,
    format: 'percentage',
    icon: 'target',
    color: '#dc2626',
    sparklineData: [3.7, 3.5, 3.4, 3.3, 3.2, 3.2],
  },
];

export const mockSalesByCategory = [
  { name: 'Elektronik', value: 45000000 },
  { name: 'Fashion', value: 32000000 },
  { name: 'Makanan & Minuman', value: 28000000 },
  { name: 'Kesehatan', value: 19000000 },
  { name: 'Lainnya', value: 15000000 },
];

export const mockWidgets: DashboardWidget[] = [
  {
    id: 'widget-001',
    type: 'metric_card',
    title: 'Ringkasan Pendapatan',
    position: { x: 0, y: 0, w: 3, h: 1 },
    dataSource: 'sales',
    config: { metrics: ['metric-001', 'metric-002', 'metric-003'] },
    refreshInterval: 0,
  },
];
```

---

## 9. Success Criteria

- [ ] Semua widget dashboard render dengan mock data
- [ ] 4+ tipe chart berbeda berfungsi (bar, line, pie, metric card)
- [ ] Filter date range dan filter kategori bekerja
- [ ] Drag-and-resize widget berfungsi (react-grid-layout)
- [ ] Dark mode dan light mode berfungsi
- [ ] Data table interaktif dengan sorting dan pagination
- [ ] Responsive design (desktop & mobile)
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
