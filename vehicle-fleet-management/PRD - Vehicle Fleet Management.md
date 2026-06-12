# PRD: FleetPro — Vehicle Fleet Management
**Project:** FleetPro
**Aesthetic:** Transport ops dashboard, data-driven, map-centric, fuel efficiency tracker

## 1. Core Layout
- Sidebar kiri: navigasi (Dashboard, Vehicles, Drivers, Trips, Fuel, Maintenance, Reports)
- Header: quick stats (active trips, fuel cost month, maintenance due)
- Main area: map view atau tabel list (toggle), trip timeline
- Panel detail (slide): kendaraan atau driver detail

## 2. Data Structure (Mock Only) — TypeScript interfaces

```typescript
interface Vehicle {
  id: string;
  platNomor: string;
  merk: string;
  model: string;
  tahun: number;
  tipe: 'angkutan-barang' | 'angkutan-orang' | 'operasional' | 'heavy-equipment';
  kapasitasMuatan: number;
  unitMuatan: 'kg' | 'ton' | 'orang';
  status: 'aktif' | 'perbaikan' | 'service' | 'non-aktif';
  kilometerTerakhir: number;
  pajakBerlaku: string;
  kirBerlaku: string;
  driverUtama: string;
  bahanBakar: 'bensin' | 'solar' | 'listrik' | 'hybrid';
  konsumsiBbmRata: number;
}

interface Driver {
  id: string;
  nama: string;
  nomorSim: string;
  simBerlaku: string;
  telp: string;
  alamat: string;
  status: 'tersedia' | 'bertugas' | 'istirahat' | 'cuti';
  rating: number;
  totalPerjalanan: number;
  pelanggaran: number;
  foto?: string;
}

interface Trip {
  id: string;
  vehicleId: string;
  driverId: string;
  tanggalMulai: string;
  tanggalSelesai?: string;
  kmAwal: number;
  kmAkhir?: number;
  asal: string;
  tujuan: string;
  muatan: string;
  status: 'berlangsung' | 'selesai' | 'dibatalkan';
  biayaTol: number;
  biayaBbm: number;
  biayaLain: number;
  catatan: string;
}

interface FuelRecord {
  id: string;
  vehicleId: string;
  tanggal: string;
  liter: number;
  hargaPerLiter: number;
  totalHarga: number;
  jenis: 'bensin' | 'solar';
  stasiun: string;
  kilometer: number;
  driverId: string;
}
```

## 3. Implementation Details — Vite + React + TypeScript + Tailwind CSS
- Vite + React 18 + TypeScript 5
- Zustand untuk trip state + filter
- React Query untuk data kendaraan dan driver
- Leaflet/Mapbox (react-leaflet) untuk map tracking
- Recharts untuk grafik konsumsi BBM dan biaya
- react-big-calendar untuk jadwal service

## 4. Styling & Theming
- CSS Variables: --fp-primary: #b91c1c; --fp-secondary: #dc2626; --fp-accent: #fbbf24
- Status badges: Aktif (hijau), Perbaikan (kuning), Service (biru), Non-Aktif (merah)
- Fuel efficiency sparkline di setiap card kendaraan
- Map markers: marker warna sesuai status kendaraan
- Dark mode opsional untuk operasi malam

## 5. UI Components Specification — ASCII layout

```
+----------------------------------------------+
| FleetPro           [Trips: 12] [Fuel: Rp45jt] |
+------+---------------------------------------+
|      |  [Map View]  [Table View]              |
| Kend |  +---------------------------+        |
| Drv  |  |   MAP AREA               |        |
| Trip |  |   [Marker] [Marker]      |        |
| BBM  |  |        🚚 🚛            |        |
| Serv |  +---------------------------+        |
| Lap  |                                        |
|      |  === Kendaraan Terdekat ===           |
|      |  Plat | Driver | Status  | KM         |
|      |  B123 | Budi   | Aktif   | 45.230     |
|      |  D456 | Amir   | Service | 78.100     |
|      |                                        |
|      |  === Grafik Konsumsi BBM ===          |
|      |  [=========================] 1:12km/L |
+------+---------------------------------------+
```

- **VehicleCard**: Card ringkas dengan plat, foto, driver, status, KM, progress bar pajak
- **TripTimeline**: Table/list perjalanan dengan start-end, jarak, biaya, durasi
- **FuelEfficiencyChart**: Bar/line chart per kendaraan per bulan, target vs aktual
- **ServiceSchedule**: Calendar dengan dot merah untuk service overdue
- **DriverCard**: Foto, nama, rating bintang, status, total trip, pelanggaran
- **MapView**: Marker kendaraan, polyline rute trip aktif, cluster untuk zoom out

## 6. Interactions & States — loading, empty, error, edge cases
- **Loading**: Skeleton map placeholder + spinner untuk tabel
- **Empty**: "Belum ada kendaraan terdaftar. Tambahkan kendaraan pertama."
- **Error**: "Gagal memuat data kendaraan" + tombol retry
- **No trips today**: "Tidak ada perjalanan hari ini" + ilustrasi kendaraan parkir
- **Fuel consumption spike**: Alert "Konsumsi BBM {{vehicle}} naik 20% — cek kondisi mesin"
- **Pajak expired**: Banner merah "Pajak {{plat}} sudah kedaluwarsa"
- **Driver unavailable**: Filter otomatis driver tidak tersedia dari dropdown penugasan
- **Overlapping trip**: Validasi "Driver {{nama}} sedang dalam perjalanan"

## 7. Language & Localization — Bahasa Indonesia
- Full Bahasa Indonesia
- Status: Berlangsung, Selesai, Dibatalkan
- Satuan: km, liter, Rp, km/L
- "Konsumsi BBM bulan ini: {{liter}} liter dengan biaya Rp {{biaya}}"
- Format jarak: "{{km}} km"
- "Terdapat {{count}} kendaraan perlu service dalam 7 hari"

## 8. Example Mock Data — data realistis nama Indonesia

```typescript
const mockVehicles: Vehicle[] = [
  {
    id: "VEH-001",
    platNomor: "B 1234 FUS",
    merk: "Mitsubishi",
    model: "Colt Diesel FE 71",
    tahun: 2022,
    tipe: "angkutan-barang",
    kapasitasMuatan: 4000,
    unitMuatan: "kg",
    status: "aktif",
    kilometerTerakhir: 45230,
    pajakBerlaku: "2025-08-15",
    kirBerlaku: "2025-06-20",
    driverUtama: "DRV-001",
    bahanBakar: "solar",
    konsumsiBbmRata: 8.5
  },
  {
    id: "VEH-002",
    platNomor: "D 5678 FG",
    merk: "Toyota",
    model: "Hiace Commuter",
    tahun: 2023,
    tipe: "angkutan-orang",
    kapasitasMuatan: 15,
    unitMuatan: "orang",
    status: "aktif",
    kilometerTerakhir: 18750,
    pajakBerlaku: "2026-01-10",
    kirBerlaku: "2025-09-05",
    driverUtama: "DRV-002",
    bahanBakar: "bensin",
    konsumsiBbmRata: 10.2
  },
  {
    id: "VEH-003",
    platNomor: "B 9012 TY",
    merk: "Hino",
    model: "Dutro 110 SDB",
    tahun: 2021,
    tipe: "angkutan-barang",
    kapasitasMuatan: 6000,
    unitMuatan: "kg",
    status: "perbaikan",
    kilometerTerakhir: 78100,
    pajakBerlaku: "2025-05-20",
    kirBerlaku: "2025-04-01",
    driverUtama: "DRV-003",
    bahanBakar: "solar",
    konsumsiBbmRata: 7.8
  }
];

const mockDrivers: Driver[] = [
  {
    id: "DRV-001",
    nama: "Budi Hartono",
    nomorSim: "SIM-8123-456789",
    simBerlaku: "2027-03-20",
    telp: "0812-3456-7890",
    alamat: "Jl. Merdeka No. 45, Jakarta Timur",
    status: "bertugas",
    rating: 4.8,
    totalPerjalanan: 234,
    pelanggaran: 1
  },
  {
    id: "DRV-002",
    nama: "Ahmad Saepudin",
    nomorSim: "SIM-8234-567890",
    simBerlaku: "2026-11-15",
    telp: "0813-4567-8901",
    alamat: "Jl. Diponegoro No. 12, Bandung",
    status: "tersedia",
    rating: 4.5,
    totalPerjalanan: 178,
    pelanggaran: 2
  }
];

const mockTrips: Trip[] = [
  {
    id: "TRP-001",
    vehicleId: "VEH-001",
    driverId: "DRV-001",
    tanggalMulai: "2025-06-12T06:00:00",
    asal: "Gudang Pulogadung, Jakarta",
    tujuan: "Gudang Waru, Sidoarjo",
    muatan: "Elektronik 2.5 ton",
    status: "berlangsung",
    biayaTol: 450000,
    biayaBbm: 0,
    biayaLain: 150000,
    catatan: "Perjalanan Surabaya via tol trans-jawa"
  },
  {
    id: "TRP-002",
    vehicleId: "VEH-002",
    driverId: "DRV-002",
    tanggalMulai: "2025-06-11T08:00:00",
    tanggalSelesai: "2025-06-11T17:00:00",
    kmAwal: 18500,
    kmAkhir: 18750,
    asal: "Kantor Pusat, Jakarta",
    tujuan: "Pabrik Cikarang",
    muatan: "15 penumpang",
    status: "selesai",
    biayaTol: 85000,
    biayaBbm: 350000,
    biayaLain: 50000,
    catatan: "Antar jemput karyawan shift"
  }
];
```

## 9. Success Criteria
- [ ] Dashboard menampilkan kendaraan aktif, perjalanan berlangsung, biaya BBM bulan ini
- [ ] CRUD kendaraan dan driver dengan validasi unik plat nomor
- [ ] Map menampilkan posisi kendaraan (mock marker)
- [ ] Trip tracker: start trip, end trip, input KM dan biaya
- [ ] Fuel efficiency report per kendaraan per periode
- [ ] Jadwal service + reminder pajak dan KIR
- [ ] Filter dan search kendaraan & driver berfungsi
