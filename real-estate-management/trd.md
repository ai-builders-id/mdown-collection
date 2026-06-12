# TRD: EstateOS — Technical Requirements

## 1. Technical Strategy
EstateOS dibangun sebagai React SPA dengan fokus pada rendering galeri foto properti yang cepat, visualisasi data properti di peta, dan sistem generate tagihan otomatis. Arsitektur data menggunakan struktur relasional antara Property, Tenant, Lease, dan PaymentSchedule.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Framework | React | 18 | Vite build tool |
| Bahasa | TypeScript | 5 | Strict mode |
| Styling | Tailwind CSS | 3 | + daisyUI components |
| State | Zustand | 4 | Filter + selected property |
| Data Fetching | TanStack React Query | 5 | Caching + pagination |
| Routing | React Router | 6 | Nested property routes |
| Map | react-leaflet | 4 | Leaflet map |
| Charts | Recharts | 2 | Income chart |
| Gallery | yet-another-react-lightbox | 3 | Lightbox gallery |
| Forms | React Hook Form | 7 | Form validation |
| Date | date-fns | 3 | Date utilities |

## 3. Component Architecture (tree)

```
src/
├── components/
│   ├── layout/
│   │   ├── Sidebar.tsx
│   │   ├── Header.tsx
│   │   └── MainLayout.tsx
│   ├── property/
│   │   ├── PropertyGrid.tsx       # Card grid gallery
│   │   ├── PropertyCard.tsx       # Card with photo overlay
│   │   ├── PropertyForm.tsx       # Add/edit modal
│   │   ├── PropertyDetail.tsx     # Slide panel detail
│   │   ├── PropertyMap.tsx        # Map markers
│   │   └── PhotoGallery.tsx       # Lightbox gallery
│   ├── tenant/
│   │   ├── TenantTable.tsx
│   │   ├── TenantForm.tsx
│   │   ├── TenantCard.tsx         # Compact info card
│   │   └── TenantDetail.tsx
│   ├── lease/
│   │   ├── LeaseTable.tsx
│   │   ├── LeaseForm.tsx          # Create contract
│   │   ├── LeaseTimeline.tsx      # Duration visual
│   │   └── LeaseRenewal.tsx       # Extension form
│   ├── payment/
│   │   ├── PaymentTable.tsx
│   │   ├── PaymentForm.tsx        # Record payment
│   │   ├── PaymentStatusBadge.tsx
│   │   └── InvoiceCard.tsx
│   ├── dashboard/
│   │   ├── StatCard.tsx           # Quick stats
│   │   ├── OccupancyChart.tsx     # Chart okupansi
│   │   ├── IncomeChart.tsx        # Bar chart pendapatan
│   │   └── LeaseExpiringList.tsx  # List kontrak mau habis
│   └── shared/
│       ├── EmptyState.tsx
│       ├── ErrorBanner.tsx
│       ├── LoadingSkeleton.tsx
│       └── ConfirmDialog.tsx
├── stores/
│   ├── propertyStore.ts
│   └── filterStore.ts
├── hooks/
│   ├── useProperties.ts
│   ├── useTenants.ts
│   ├── useLeases.ts
│   └── usePayments.ts
├── services/
│   └── paymentScheduler.ts       # Generate tagihan logic
├── types/
│   └── index.ts
├── data/
│   └── mockData.ts
└── utils/
    ├── formatters.ts              # Currency, date, status
    ├── validators.ts              # NIK, email, phone
    └── contractGenerator.ts      # Nomor kontrak auto
```

## 4. Data Layer
- **MVP**: Zustand store dengan initial mock data, persist localStorage
- **Production**: PostgreSQL + Prisma ORM
- **Payment Scheduler**: Pure function `generateInvoices(lease, tahun, bulan)` → array PaymentSchedule
- **Photo Storage**: Base64 untuk MVP, Cloudinary upload untuk production
- **Export**: xlsx library untuk generate Excel, jsPDF untuk PDF

## 5. Performance Requirements
- Render properti grid 200 cards: < 2 detik (lazy load gambar)
- Lightbox open: < 500ms
- Map render 50 markers: < 2 detik
- Search properti: debounce 300ms, hasil < 200ms
- Generate tagihan 100 kontrak: < 1 detik (pure function)
- Bundle size: < 400KB gzipped

## 6. Styling & Theming System (CSS vars)

```css
:root {
  --est-primary: #0891b2;
  --est-primary-light: #22d3ee;
  --est-secondary: #0e7490;
  --est-surface: #ffffff;
  --est-surface-card: #ffffff;
  --est-text: #1e293b;
  --est-text-muted: #64748b;
  --est-border: #e2e8f0;
  --est-success: #16a34a;
  --est-warning: #d97706;
  --est-danger: #dc2626;
  --est-info: #0284c7;
  --sidebar-width: 260px;
  --card-image-height: 200px;
}

.dark {
  --est-surface: #0f172a;
  --est-surface-card: #1e293b;
  --est-text: #f1f5f9;
  --est-text-muted: #94a3b8;
  --est-border: #334155;
}
```

## 7. Validation Rules
- NIK: 16 digit numerik, validasi Luhn algorithm
- Email: Format email standar
- Telepon: Format Indonesia (+62 atau 08xx), 10-13 digit
- Harga sewa: Number > 0, maks Rp 1 Miliar
- Deposit: Minimal 2x biaya sewa bulanan
- Luas tanah/bangunan: Number > 0, maks 100.000 m2
- Nomor kontrak: Auto-format "KTR/{YYYY}/{MM}/{TIPE}/{URUT}"
- Tanggal selesai sewa: Harus > tanggal mulai
- Foto: Maks 5MB, tipe JPEG/PNG/WebP, maks 10 foto per properti
- Kamar: Integer 0-100

## 8. Error Handling Strategy
- **NIK duplicate**: "NIK {{nik}} sudah terdaftar atas nama {{nama}}"
- **Property occupied**: "Properti {{nama}} sedang dalam kontrak aktif dengan {{penyewa}}"
- **Date invalid**: "Tanggal selesai harus setelah tanggal mulai"
- **Payment under**: "Jumlah pembayaran kurang dari tagihan (Rp {{tagihan}})"
- **Payment over**: "Kelebihan Rp {{lebih}} akan dicatat sebagai deposit"
- **Photo upload fail**: "Gagal upload foto. Coba format JPEG/PNG, maks 5MB"
- **Map fail**: "Peta gagal dimuat. Tampilan list sebagai gantinya."
- **Contract overlap**: "Properti sudah memiliki kontrak aktif pada periode tersebut"

## 9. Security Considerations
- NIK data sensitif: Enkripsi di database (production), masking di UI (show only last 4 digits)
- Role-based: Owner, Manager, Admin, Viewer
- Upload validation: whitelist MIME types untuk foto dan dokumen
- Session timeout: 30 menit idle
- Audit log: Semua perubahan kontrak dan pembayaran tercatat
- Data export: Hanya role Manager dan Owner yang bisa export

## 10. Deployment & Build
- Build: `npm run build` → `/dist`
- Deploy: Vercel/Netlify
- Environment: `VITE_API_URL`, `VITE_MAPBOX_TOKEN` (opsional), `VITE_MAX_PHOTOS`, `VITE_MAX_PHOTO_SIZE`
- CI/CD: GitHub Actions → lint + type-check → build → deploy

## 11. Testing Strategy
- **Unit**: Vitest untuk paymentScheduler, validators (NIK), contractGenerator
- **Component**: Testing Library untuk PropertyForm, LeaseForm, PaymentForm
- **Integration**: Flow tambah properti → sewa → tagihan → bayar
- **E2E**: Playwright untuk CRUD property + lease cycle

## 12. Dependencies & Constraints
- **Dependencies**: React 18+, Tailwind v3+, date-fns, react-leaflet, recharts
- **Constraint**: Map tiles membutuhkan internet (OpenStreetMap tiles)
- **Constraint**: WhatsApp notification membutuhkan third-party gateway (Phase 2)
- **Constraint**: NIK validation hanya format — tidak ke API Dukcapil (Phase 2)
- **Constraint**: Foto properti disimpan sebagai base64 di MVP — terbatas 10 foto/properti
