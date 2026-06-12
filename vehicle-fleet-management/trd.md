# TRD: FleetPro — Technical Requirements

## 1. Technical Strategy
FleetPro dibangun sebagai React SPA dengan fokus pada data entry trip yang cepat, visualisasi data operasional, dan map integration. Arsitektur dirancang untuk dapat menerima data dari GPS tracker IoT di fase berikutnya. MVP menggunakan mock data dengan struktur yang siap untuk migrasi ke backend sungguhan.

## 2. Tech Stack

| Layer | Teknologi | Versi | Keterangan |
|-------|-----------|-------|------------|
| Framework | React | 18 | Vite build tool |
| Bahasa | TypeScript | 5 | Strict mode |
| Styling | Tailwind CSS | 3 | + daisyUI |
| State | Zustand | 4 | Trip & filter state |
| Data Fetching | TanStack React Query | 5 | Caching |
| Map | react-leaflet + Leaflet | 4 | Map & markers |
| Charts | Recharts | 2 | BBM, biaya grafik |
| Calendar | react-big-calendar | 1 | Jadwal service |
| Forms | React Hook Form | 7 | Form validation |
| Export | xlsx + jsPDF | latest | Export laporan |

## 3. Component Architecture (tree)

```
src/
├── components/
│   ├── layout/
│   │   ├── Sidebar.tsx
│   │   ├── Header.tsx            # Quick stats + user menu
│   │   └── FleetLayout.tsx
│   ├── vehicle/
│   │   ├── VehicleTable.tsx
│   │   ├── VehicleCard.tsx       # Compact card for dashboard
│   │   ├── VehicleForm.tsx
│   │   └── VehicleDetail.tsx     # Slide panel
│   ├── driver/
│   │   ├── DriverTable.tsx
│   │   ├── DriverCard.tsx
│   │   └── DriverForm.tsx
│   ├── trip/
│   │   ├── TripList.tsx          # Table + filter
│   │   ├── TripForm.tsx          # Check-in / check-out form
│   │   ├── TripDetail.tsx
│   │   └── ActiveTripBadge.tsx   # Badge untuk trip berlangsung
│   ├── fuel/
│   │   ├── FuelForm.tsx
│   │   ├── FuelEfficiencyChart.tsx
│   │   └── FuelAlert.tsx         # Anomali konsumsi
│   ├── service/
│   │   ├── ServiceCalendar.tsx
│   │   ├── ServiceTable.tsx
│   │   └── ReminderBanner.tsx
│   ├── map/
│   │   └── FleetMap.tsx          # Leaflet map + markers
│   └── shared/
│       ├── StatCard.tsx
│       ├── LoadingSkeleton.tsx
│       └── EmptyState.tsx
├── stores/
│   ├── tripStore.ts
│   └── filterStore.ts
├── hooks/
│   ├── useVehicles.ts
│   ├── useDrivers.ts
│   └── useTrips.ts
├── services/
│   └── fuelCalculator.ts         # Konsumsi BBM logic
├── types/
│   └── index.ts
├── data/
│   └── mockData.ts
└── utils/
    ├── formatters.ts
    └── validators.ts
```

## 4. Data Layer
- **MVP**: Zustand store persist ke localStorage, mock data array
- **Production**: PostgreSQL + Express.js REST API
- **Map Data**: GeoJSON untuk rute, LatLng untuk marker kendaraan
- **Offline Queue**: IndexedDB untuk menyimpan data trip saat offline (Phase 2)
- **File Upload**: Base64 untuk MVP, Cloudinary upload untuk production

## 5. Performance Requirements
- Dashboard render 100 kendaraan: < 2 detik
- Trip form submit: < 500ms
- Map render 50 markers: < 2 detik
- Search kendaraan: debounce 300ms
- Bundle size: < 400KB gzipped
- Lighthouse Performance: > 75 (karena map leaflet berat)

## 6. Styling & Theming System (CSS vars)

```css
:root {
  --fp-primary: #b91c1c;
  --fp-primary-dark: #991b1b;
  --fp-accent: #fbbf24;
  --fp-surface: #ffffff;
  --fp-surface-card: #fef2f2;
  --fp-text: #1f2937;
  --fp-text-muted: #6b7280;
  --fp-border: #e5e7eb;
  --fp-success: #059669;
  --fp-warning: #d97706;
  --fp-danger: #dc2626;
  --fp-info: #0284c7;
  --sidebar-width: 260px;
  --header-height: 64px;
}

.dark {
  --fp-surface: #111827;
  --fp-surface-card: #1f2937;
  --fp-text: #f3f4f6;
  --fp-text-muted: #9ca3af;
  --fp-border: #374151;
}
```

## 7. Validation Rules
- Plat nomor: Format Indonesia (1-2 huruf + 1-4 digit + 1-3 huruf), contoh: "B 1234 FUS"
- Nomor SIM: Format "SIM-XXXX-XXXXXX" — 16 karakter
- KM: Integer, tidak boleh negatif, KM akhir > KM awal
- Liter BBM: Number > 0, maks 200 liter
- Harga per liter: Number > 0, maks Rp 20.000
- Telepon: Format Indonesia (+62 atau 08xx)
- Tanggal: Tidak boleh di masa depan untuk trip selesai

## 8. Error Handling Strategy
- **Duplicate plat**: "Plat nomor {{plat}} sudah terdaftar pada kendaraan {{model}}"
- **Driver busy**: "Driver {{nama}} sedang bertugas hingga {{waktu}}"
- **Vehicle in use**: "Kendaraan {{plat}} sedang dalam perjalanan"
- **KM invalid**: "KM akhir harus lebih besar dari KM awal ({{kmAwal}})"
- **Network offline**: Simpan ke queue lokal, sync saat online
- **Map load error**: Fallback ke tampilan tabel kendaraan
- **Photo upload fail**: Simpan data tetap jalan, foto bisa diupload ulang

## 9. Security Considerations
- Role-based access: Super Admin, Manajer Armada, Supervisor, Driver
- Driver hanya bisa melihat dan menginput trip miliknya sendiri
- Geolocation SPBU: Cross-check lokasi saat input BBM (Phase 2)
- Session: JWT token dengan refresh token
- Audit log: Semua perubahan trip dan biaya tercatat
- Rate limiting: 60 request/menit per user

## 10. Deployment & Build
- Build: `npm run build`
- Deploy: Vercel untuk frontend; Railway/Render untuk backend (Phase 2)
- Environment: `VITE_API_URL`, `VITE_MAPBOX_TOKEN` (jika pakai Mapbox), `VITE_PHOTO_UPLOAD_URL`
- CI/CD: GitHub Actions → lint + type-check + test → build

## 11. Testing Strategy
- **Unit**: Vitest untuk fuelCalculator, validators, formatters
- **Component**: Testing Library untuk TripForm, VehicleForm
- **Integration**: Flow trip check-in → check-out → muncul di riwayat
- **E2E**: Playwright untuk CRUD kendaraan + trip cycle

## 12. Dependencies & Constraints
- **Dependencies**: React 18+, Leaflet 1.9+, React Hook Form 7+
- **Constraint**: Map membutuhkan koneksi internet (tileset online)
- **Constraint**: GPS tracking real-time membutuhkan device IoT (fase berikutnya)
- **Constraint**: Mode offline terbatas pada input data, tidak untuk map
- **Constraint**: Browser modern dengan GeoLocation API untuk driver mobile (via PWA)
