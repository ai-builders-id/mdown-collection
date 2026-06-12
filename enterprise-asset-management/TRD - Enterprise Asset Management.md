# TRD: EAM Pro — Technical Requirements

## 1. Technical Strategy
EAM Pro dibangun sebagai SPA dengan React + TypeScript. Fokus utama pada rendering tabel aset yang bisa mencapai ribuan baris, visualisasi depresiasi, dan generate label QR. Data disimpan di Zustand store dengan persist ke localStorage untuk MVP, disiapkan untuk migrasi ke backend PostgreSQL.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Framework | React | 18 | Vite build tool |
| Bahasa | TypeScript | 5 | Strict mode |
| Styling | Tailwind CSS | 3 | + class-variance-authority |
| State | Zustand | 4 | Asset state + selected |
| Data Fetching | TanStack React Query | 5 | Caching maintenance |
| QR | qrcode.react | 3 | Generate QR code |
| Charts | Recharts | 2 | Depresiasi line chart |
| Export | jsPDF + xlsx | latest | Export PDF/Excel |
| Table | TanStack Table | 8 | Virtual scrolling |

## 3. Component Architecture (tree)

```
src/
├── components/
│   ├── layout/
│   │   ├── Sidebar.tsx
│   │   ├── Header.tsx
│   │   └── MainLayout.tsx
│   ├── asset/
│   │   ├── AssetTable.tsx        # Virtual-scroll table
│   │   ├── AssetForm.tsx         # Form add/edit modal
│   │   ├── AssetDetailPanel.tsx  # Slide panel detail
│   │   ├── AssetQRCode.tsx       # Generate QR label
│   │   └── AssetTimeline.tsx     # Siklus hidup aset
│   ├── maintenance/
│   │   ├── MaintenanceCalendar.tsx
│   │   ├── MaintenanceForm.tsx
│   │   └── MaintenanceHistory.tsx
│   ├── depreciation/
│   │   ├── DepreciationChart.tsx
│   │   └── DepreciationTable.tsx
│   ├── location/
│   │   └── LocationTree.tsx      # Hierarki lokasi
│   └── shared/
│       ├── DataTable.tsx
│       ├── EmptyState.tsx
│       ├── ConfirmationDialog.tsx
│       └── Badge.tsx             # Status badge reusable
├── stores/
│   ├── assetStore.ts
│   └── filterStore.ts
├── hooks/
│   ├── useAssets.ts
│   └── useMaintenance.ts
├── services/
│   └── depreciationService.ts    # Kalkulasi depresiasi
├── types/
│   └── index.ts
├── data/
│   └── mockData.ts
└── utils/
    ├── formatters.ts
    ├── validators.ts
    └── assetCodeGenerator.ts
```

## 4. Data Layer
- **MVP State**: Zustand store dengan initial mock data, persist ke localStorage
- **Production**: PostgreSQL dengan Prisma ORM, REST API
- **Depreciation Service**: Pure function yang menerima asset + tanggal, return schedule
- **Caching**: React Query cache 5 menit untuk data maintenance
- **Pagination**: TanStack Table dengan virtual scrolling untuk 5000+ row

## 5. Performance Requirements
- Render tabel 5000 aset: < 1 detik (virtual scrolling)
- Generate QR code: < 500ms
- Hitung depresiasi 100 aset: < 100ms (pure function)
- Search aset: debounce 300ms, hasil < 200ms
- Bundle size: < 350KB gzipped
- Lighthouse Performance: > 80

## 6. Styling & Theming System (CSS vars)

```css
:root {
  --eam-primary: #0f766e;
  --eam-primary-light: #14b8a6;
  --eam-surface: #ffffff;
  --eam-surface-card: #f0fdfa;
  --eam-text: #0f172a;
  --eam-text-muted: #64748b;
  --eam-border: #e2e8f0;
  --eam-success: #16a34a;
  --eam-warning: #d97706;
  --eam-danger: #dc2626;
  --eam-info: #0284c7;
  --sidebar-width: 260px;
}

.dark {
  --eam-surface: #0f172a;
  --eam-surface-card: #1e293b;
  --eam-text: #e2e8f0;
  --eam-text-muted: #94a3b8;
  --eam-border: #334155;
}
```

## 7. Validation Rules
- Kode aset: Format "EAM/{KATEGORI}/{TAHUN}/{XXX}" — auto generate
- Nomor seri: Unik, tidak boleh duplicate dengan aset lain
- Nilai perolehan: Number > 0, maks Rp 100 Miliar
- Masa manfaat: Integer 1-50 tahun
- Tanggal perolehan: Tidak boleh di masa depan
- Nama aset: 3-150 karakter
- Biaya maintenance: Number >= 0

## 8. Error Handling Strategy
- **Duplicate serial number**: Field error "Nomor seri {{sn}} sudah digunakan oleh aset {{kode}}"
- **Network error**: Retry 3x + toast error + fallback ke cache
- **Validation error**: Inline error merah di form field
- **Delete protected**: Warning "Aset memiliki riwayat maintenance. Hapus tetap akan menyimpan riwayat."
- **Not found**: Halaman 404 + link navigasi
- **Computation error**: Depresiasi gagal dihitung — fallback ke manual input

## 9. Security Considerations
- Role-based access: Admin, Staff, Viewer, Finance
- Audit log: Semua perubahan aset tercatat (siapa, apa, kapan)
- XSS protection: Sanitize input name, notes, description
- Session timeout: 30 menit idle → logout
- Data validation di client + server side

## 10. Deployment & Build
- Build: `npm run build` → `/dist`
- Deploy: Vercel/Netlify untuk cloud; Docker + Nginx untuk on-premise
- Environment: `VITE_API_URL`, `VITE_QR_SIZE`, `VITE_DEFAULT_DEPRECIATION_METHOD`
- CI/CD: GitHub Actions (lint → test → build → deploy)

## 11. Testing Strategy
- **Unit**: Vitest untuk depreciationService, validators, assetCodeGenerator
- **Component**: React Testing Library untuk AssetForm, AssetTable
- **Integration**: Test flow tambah aset → muncul di tabel
- **E2E**: Playwright untuk CRUD aset + generate QR

## 12. Dependencies & Constraints
- **Dependencies**: React 18+, Tailwind v3+, @tanstack/react-table
- **Constraint**: Mobile scan support via PWA (camera API)
- **Constraint**: Tidak menggunakan ORM di MVP — data plain object
- **Constraint**: Print label QR menggunakan CSS print media + browser print dialog
- **Constraint**: kompatibel dengan browser modern (Chrome/Firefox/Edge/Safari 2 tahun terakhir)
