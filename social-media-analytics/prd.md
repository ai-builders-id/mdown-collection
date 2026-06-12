# PRD: SosmedInsight — Dashboard Analitik Media Sosial untuk Pertumbuhan Follower & Engagement
**Project:** SosmedInsight
**Aesthetic:** Modern, data-dense, biru-ungu gradien dengan aksen neon — gaya dashboard profesional

## 1. Core Layout
Layout utama SosmedInsight terdiri dari:
- **Sidebar kiri (240px)** — navigasi: Dashboard, Overview, Konten, Audiens, Kompetitor, Laporan, Pengaturan.
- **Top bar (56px)** — pemilihan akun/ brand yang dipantau, periode waktu, notifikasi, avatar.
- **Main panel (flex)** — area konten utama dengan grid card metrik.
- **Right panel (380px, toggleable)** — detail kontekstual: postingan terpilih, insight audiens, rekomendasi.
- **Bottom section (collapsible)** — tabel data mentah untuk export.

Layout responsif: sidebar collapse ke ikon di layar < 1024px, bottom navigation di mobile < 640px.

## 2. Data Structure (Mock Only)

```typescript
// Interface untuk Akun Media Sosial yang Dipantau
interface AkunSosmed {
  id: string;                          // UUID
  namaAkun: string;                    // Contoh: "PT Maju Jaya Official"
  platform: 'instagram' | 'tiktok' | 'youtube' | 'facebook' | 'twitter' | 'linkedin';
  username: string;                    // Contoh: "@ptmajujaya"
  avatarUrl: string;
  followers: number;                   // Contoh: 45230
  following: number;                   // Contoh: 1240
  totalPost: number;                   // Contoh: 892
  isVerified: boolean;
  kategori: string;                    // Contoh: "Perusahaan Teknologi"
  urlProfil: string;
  createdAt: Date;
  lastSync: Date;
}

// Interface untuk Metrik Harian
interface MetricHarian {
  id: string;
  akunId: string;
  tanggal: Date;
  followers: number;                   // Total followers pada tanggal tersebut
  followersBertambah: number;          // Follower baru hari itu
  followersKeluar: number;             // Unfollow hari itu
  impressions: number;                 // Total impressions
  reach: number;                       // Total reach (akun unik)
  engagement: number;                  // Total engagement (like + comment + share + save)
  engagementRate: number;              // (engagement / impressions) * 100
  likes: number;
  comments: number;
  shares: number;
  saves: number;
  clicks: number;                      // Link clicks
  totalVideoViews: number;
  averageWatchTime: number;            // Detik rata-rata
  storyViews: number;
  storyReplies: number;
}

// Interface untuk Postingan / Konten
interface Postingan {
  id: string;
  akunId: string;
  platform: string;
  tipeKonten: 'foto' | 'video' | 'carousel' | 'reels' | 'story' | 'teks' | 'link';
  caption: string;                     // Contoh: "Launching produk baru kami! 🚀"
  mediaUrl: string;
  thumbnailUrl: string;
  postedAt: Date;
  likes: number;
  comments: number;
  shares: number;
  saves: number;
  views: number;                       // Khusus video/reels
  reach: number;
  impressions: number;
  engagementRate: number;
  linkClicks: number;
  hashtags: string[];                  // Contoh: ["#inovasi", "#produkindonesia"]
  mentions: string[];                  // Contoh: ["@partner_bisnis"]
  lokasi: string | null;              // Contoh: "Jakarta, Indonesia"
  isPaid: boolean;                     // Konten berbayar/ads?
  sentiment: 'positif' | 'netral' | 'negatif' | 'mixed';
  kategoriKonten: string;             // Contoh: "product_launch", "educational", "promo"
}

// Interface untuk Insight Audiens
interface InsightAudiens {
  akunId: string;
  totalFollowers: number;
  demografiUsia: { rentang: string; persentase: number }[];   // [{rentang: "18-24", persentase: 32}]
  demografiGender: { gender: string; persentase: number }[];
  demografiLokasi: { kota: string; persentase: number }[];    // [{kota: "Jakarta", persentase: 28}]
  demografiBahasa: { bahasa: string; persentase: number }[];
  jamAktif: { jam: number; persentase: number }[];            // Distribusi per jam
  hariAktif: { hari: string; persentase: number }[];          // Distribusi per hari
  deviceType: { tipe: string; persentase: number }[];         // Mobile, Desktop, Tablet
  topHashtags: { tag: string; count: number }[];
  akunSerupa: { namaAkun: string; followers: number }[];
}

// Interface untuk Laporan
interface Laporan {
  id: string;
  akunId: string;
  judul: string;                       // Contoh: "Laporan Bulanan November 2025"
  periodeMulai: Date;
  periodeSelesai: Date;
  tipe: 'harian' | 'mingguan' | 'bulanan' | 'kustom';
  metrics: {
    totalFollowers: number;
    followersGrowth: number;            // Persentase growth
    totalImpressions: number;
    totalReach: number;
    avgEngagementRate: number;
    totalEngagement: number;
    topPosts: Postingan[];
    totalPostingan: number;
    totalVideoViews: number;
  };
  pdfUrl: string | null;
  createdAt: Date;
}

// Interface untuk Kompetitor
interface Kompetitor {
  id: string;
  brand: string;                        // Contoh: "PT Sejahtera Abadi"
  akunId: string;
  platform: string;
  username: string;
  followers: number;
  followerGrowth: number;               // Persentase growth
  avgEngagementRate: number;
  totalPost: number;
  postingFrequency: number;             // Rata-rata post per minggu
  topPost: Postingan | null;
  lastUpdated: Date;
}
```

## 3. Implementation Details
- **Vite 5.x** — build tool
- **React 18.x** — UI library
- **TypeScript 5.x** — strict mode
- **Tailwind CSS 3.x** — styling
- **React Router v6** — routing
- **Zustand** — client state
- **TanStack Query** — server state/data fetching
- **Recharts** — grafik dan chart
- **date-fns** — dengan locale id
- **Lucide React** — ikon
- **TanStack Table** — tabel data
- **React Hook Form + Zod** — validasi

## 4. Styling & Theming
**Palette Warna:**
- Primary: #6366F1 (indigo 500) — brand utama
- Primary Gradient: linear-gradient(135deg, #6366F1, #8B5CF6) — gradien header/chart
- Secondary: #06B6D4 (cyan 500) — aksen sekunder
- Accent: #F59E0B (amber 500) — highlight
- Background: #0F172A (slate 900) — dark theme
- Surface: #1E293B (slate 800) — card bg
- Surface Hover: #334155 (slate 700)
- Text Primary: #F1F5F9 (slate 100)
- Text Secondary: #94A3B8 (slate 400)
- Border: #334155 (slate 700)
- Success: #10B981 (emerald 500) — growth positif
- Danger: #EF4444 (red 500) — growth negatif

**Tipografi:**
- Font utama: Inter (sans-serif)
- Font data: JetBrains Mono (monospace) untuk angka metrik
- Skala: 11/13/14/16/18/20/24/30/36px
- Data display: angka besar dengan leading tight

## 5. UI Components Specification

```
+--------------------------------------------------------------+
| [Logo] SM Insight    [Akun: @ptmajujaya] [7 Hari] [Notif] [A]| ← Topbar
+-----------+--------------------------------------------------+
|           | +----------------------------------------------+ |
| DASHBOARD | | Total     | Followers | Engage   | Reach     | |
|           | | Followers | Growth    | Rate     | 45.2K     | |
| Overview  | | 45,230    | +5.2%     | 3.8%     | [spark]   | |
|           | +----------------------------------------------+ |
| Konten    |                                                  |
|           | +--Followers Trend (30 hari) -----------------+ |
| Audiens   | |  ▁▃▅▇▆▇▅▃▆▇▅▃▅▇▅▃▂▃▅▇▅▃▅▆▇▅▃▅▇             | |
|           | |  └────────────────────────────►            | |
| Kompetitor| +--------------------------------------------+ |
|           |                                                  |
| Laporan   | +---Top Posts---+ +---Engagement by Day----+    |
|           | | [img] Caption | |  ██ ██ ██ ██ ██       |    |
| Pengaturan| | ❤ 1.2K 💬 89  | |  Sen Sel Rab Kam Jum  |    |
|           | +---------------+ +------------------------+    |
|           |                                                  |
|           | +--------Tabel Postingan Terbaru---------------+ |
|           | | Post | Engage | Likes | Comments | Platform | |
|           | | [..] | 5.2%   | 1.2K  | 89       | 📷      | |
|           | | [..] | 3.1%   | 892   | 45       | 🎥      | |
|           | +----------------------------------------------+ |
+-----------+--------------------------------------------------+
```

**Komponen Detail:**

### StatCard (Kartu Metrik Utama)
- Ikon platform di pojok
- Angka utama (besar, monospace)
- Label metrik
- Persentase perubahan (hijau untuk naik, merah untuk turun)
- Sparkline chart kecil (20px height)
- Background subtle gradient

### TrendChart (Grafik Tren)
- Line chart interaktif dengan Tooltip detail
- Toggle: Followers, Engagement, Reach, Impressions
- Range selector: 7H, 30H, 3B, 12B, Max
- Area fill dengan gradient
- Grid lines subtle (opacity 0.1)

### TopPostsCard
- Grid 2x2 dari postingan dengan performa terbaik
- Thumbnail + caption singkat + metrik engagement
- Overlay kategori konten (promo, edukasi, dll)
- Modal detail saat diklik

### AudienceDemographic
- Bar chart untuk demografi usia
- Pie chart untuk gender
- Treemap untuk lokasi
- Heatmap untuk jam aktif

### KompetitorTable
- Tabel peringkat kompetitor
- Kolom: Brand, Followers, Growth, Engagement Rate, Post Frequency
- Perbandingan dengan akun kita (sebagai baseline)
- Conditional formatting: lebih baik/kurang dari kompetitor

### ContentCalendar
- Grid kalender bulanan
- Setiap hari menampilkan jumlah posting
- Warna menunjukkan engagement rate (hijau > 5%, kuning 2-5%, merah < 2%)
- Click untuk melihat detail postingan hari itu

## 6. Interactions & States

**Loading States:**
- Skeleton cards untuk stat overview (pulse animation)
- Skeleton chart area (baris bergelombang)
- Spinner untuk tabel data
- Progress shimmer untuk export laporan

**Empty States:**
- Dashboard baru: "Hubungkan akun media sosial Anda untuk memulai analitik."
- Tidak ada postingan di periode: "Tidak ada postingan pada periode ini."
- Data kompetitor kosong: "Tambahkan kompetitor untuk perbandingan."
- Pencarian postingan: "Tidak ditemukan postingan dengan kata kunci '{{KATA_KUNCI}}'."
- Ilustrasi karakter sesuai tipe empty state

**Error States:**
- Gagal sinkronisasi akun: "Gagal memperbarui data dari {{PLATFORM}}. API rate limit mungkin tercapai."
- Data tidak tersedia: "Data tidak tersedia untuk periode yang dipilih."
- Koneksi terputus: "Koneksi internet terputus. Data terakhir: {{TANGGAL}}"
- Token expired: "Sesi telah berakhir. Silakan login ulang ke akun {{PLATFORM}}."

**Edge Cases:**
- Akun dengan followers < 100: peringatan "Data mungkin kurang representatif"
- Engagement rate 0%: validasi apakah akun masih aktif
- Data dari platform berbeda: normalisasi metrik (tidak semua platform punya semua metrik)
- Perbandingan periode: jika periode sebelumnya tidak ada data, tampilkan "N/A"
- Data real-time vs historical: indikator perbedaan warna (real-time: biru, historical: abu-abu)
- Multiple akun: aggregate view vs per-akun toggle
- Timezone: konfigurasi zona waktu pengguna (WIB/WITA/WIT)

## 7. Language & Localization
Bahasa Indonesia untuk seluruh antarmuka:
- Metrik: "Pengikut", "Jangkauan", "Impresi", "Keterlibatan", "Tayangan"
- Periode: "7 Hari Terakhir", "30 Hari Terakhir", "Bulan Ini", "Kustom"
- Aksi: "Sinkronisasi", "Ekspor Laporan", "Hubungkan Akun"
- Label: "Pertumbuhan", "Rata-rata", "Perbandingan", "Peringkat"
- Pesan konfirmasi: "Apakah Anda yakin ingin memutuskan koneksi akun {{NAMA_AKUN}}?"
- Numerik: "45,3 rb" untuk ribuan, "2,1 jt" untuk jutaan
- Waktu: "3 hari yang lalu", "Kemarin pukul 14:30 WIB"

## 8. Example Mock Data

```typescript
export const mockAkun: AkunSosmed = {
  id: "akn-001",
  namaAkun: "PT Maju Jaya Official",
  platform: "instagram",
  username: "@ptmajujaya",
  avatarUrl: "/avatars/ptmajujaya.jpg",
  followers: 45230,
  following: 1240,
  totalPost: 892,
  isVerified: true,
  kategori: "Perusahaan Teknologi",
  urlProfil: "https://instagram.com/ptmajujaya",
  createdAt: new Date("2023-01-15"),
  lastSync: new Date("2025-11-25T14:30:00")
};

export const mockMetricsHarian: MetricHarian[] = Array.from({ length: 30 }, (_, i) => ({
  id: `met-${i}`,
  akunId: "akn-001",
  tanggal: new Date(2025, 10, 25 - i),
  followers: 45230 - (i * 10) + Math.floor(Math.random() * 50),
  followersBertambah: Math.floor(Math.random() * 80) + 10,
  followersKeluar: Math.floor(Math.random() * 30),
  impressions: Math.floor(Math.random() * 5000) + 1000,
  reach: Math.floor(Math.random() * 3000) + 500,
  engagement: Math.floor(Math.random() * 800) + 100,
  engagementRate: +(Math.random() * 5 + 1).toFixed(2),
  likes: Math.floor(Math.random() * 500) + 50,
  comments: Math.floor(Math.random() * 50) + 2,
  shares: Math.floor(Math.random() * 30) + 1,
  saves: Math.floor(Math.random() * 40) + 5,
  clicks: Math.floor(Math.random() * 100),
  totalVideoViews: Math.floor(Math.random() * 2000) + 200,
  averageWatchTime: Math.floor(Math.random() * 30) + 5,
  storyViews: Math.floor(Math.random() * 800) + 100,
  storyReplies: Math.floor(Math.random() * 20),
}));

export const mockPostingan: Postingan[] = [
  {
    id: "post-001",
    akunId: "akn-001",
    platform: "instagram",
    tipeKonten: "video",
    caption: "Launching produk baru kami! 🚀 After 6 bulan riset, akhirnya kami luncurkan SmartHome Pro. #inovasi #teknologiindonesia",
    mediaUrl: "https://cdn.example.com/posts/launching.mp4",
    thumbnailUrl: "https://cdn.example.com/thumbs/launching.jpg",
    postedAt: new Date("2025-11-24T10:00:00"),
    likes: 1247,
    comments: 89,
    shares: 234,
    saves: 156,
    views: 23400,
    reach: 18500,
    impressions: 28000,
    engagementRate: 5.8,
    linkClicks: 342,
    hashtags: ["#inovasi", "#teknologiindonesia", "#smartphone", "#produkindonesia"],
    mentions: ["@partner_tech", "@influencer_tech"],
    lokasi: "Jakarta, Indonesia",
    isPaid: false,
    sentiment: "positif",
    kategoriKonten: "product_launch"
  },
  {
    id: "post-002",
    akunId: "akn-001",
    platform: "instagram",
    tipeKonten: "carousel",
    caption: "Tips memilih laptop untuk pekerja remote 💻 Slide 5 paling penting! #tips #workfromhome",
    mediaUrl: "https://cdn.example.com/posts/tips-laptop.pdf",
    thumbnailUrl: "https://cdn.example.com/thumbs/tips-laptop.jpg",
    postedAt: new Date("2025-11-22T15:30:00"),
    likes: 892,
    comments: 45,
    shares: 512,
    saves: 789,
    views: 0,
    reach: 12400,
    impressions: 18200,
    engagementRate: 7.2,
    linkClicks: 156,
    hashtags: ["#tips", "#workfromhome", "#laptop"],
    mentions: [],
    lokasi: null,
    isPaid: false,
    sentiment: "positif",
    kategoriKonten: "educational"
  },
  {
    id: "post-003",
    akunId: "akn-001",
    platform: "instagram",
    tipeKonten: "foto",
    caption: "Promo Akhir Tahun! Diskon 30% untuk semua produk. Kode: THR2025 🎉",
    mediaUrl: "https://cdn.example.com/posts/promo-thr.jpg",
    thumbnailUrl: "https://cdn.example.com/thumbs/promo-thr.jpg",
    postedAt: new Date("2025-11-20T09:00:00"),
    likes: 2103,
    comments: 167,
    shares: 890,
    saves: 234,
    views: 0,
    reach: 32100,
    impressions: 45000,
    engagementRate: 9.2,
    linkClicks: 1200,
    hashtags: ["#promo", "#thr2025", "#diskon", "#belanjadiskon"],
    mentions: [],
    lokasi: "Jakarta, Indonesia",
    isPaid: true,
    sentiment: "positif",
    kategoriKonten: "promo"
  }
];

export const mockInsightAudiens: InsightAudiens = {
  akunId: "akn-001",
  totalFollowers: 45230,
  demografiUsia: [
    { rentang: "13-17", persentase: 5 },
    { rentang: "18-24", persentase: 32 },
    { rentang: "25-34", persentase: 38 },
    { rentang: "35-44", persentase: 18 },
    { rentang: "45-54", persentase: 5 },
    { rentang: "55+", persentase: 2 }
  ],
  demografiGender: [
    { gender: "Pria", persentase: 58 },
    { gender: "Wanita", persentase: 40 },
    { gender: "Tidak Diketahui", persentase: 2 }
  ],
  demografiLokasi: [
    { kota: "Jakarta", persentase: 28 },
    { kota: "Bandung", persentase: 15 },
    { kota: "Surabaya", persentase: 12 },
    { kota: "Yogyakarta", persentase: 8 },
    { kota: "Medan", persentase: 6 },
    { kota: "Lainnya", persentase: 31 }
  ],
  demografiBahasa: [
    { bahasa: "Bahasa Indonesia", persentase: 92 },
    { bahasa: "Inggris", persentase: 6 },
    { bahasa: "Lainnya", persentase: 2 }
  ],
  jamAktif: Array.from({ length: 24 }, (_, i) => ({
    jam: i,
    persentase: Math.random() * 10
  })),
  hariAktif: [
    { hari: "Senin", persentase: 14 },
    { hari: "Selasa", persentase: 15 },
    { hari: "Rabu", persentase: 16 },
    { hari: "Kamis", persentase: 14 },
    { hari: "Jumat", persentase: 17 },
    { hari: "Sabtu", persentase: 13 },
    { hari: "Minggu", persentase: 11 }
  ],
  deviceType: [
    { tipe: "Mobile Android", persentase: 62 },
    { tipe: "Mobile iOS", persentase: 28 },
    { tipe: "Desktop", persentase: 8 },
    { tipe: "Tablet", persentase: 2 }
  ],
  topHashtags: [
    { tag: "teknologiindonesia", count: 45 },
    { tag: "inovasi", count: 38 },
    { tag: "smartphone", count: 32 },
    { tag: "tips", count: 28 }
  ],
  akunSerupa: [
    { namaAkun: "PT Teknologi Nusantara", followers: 67200 },
    { namaAkun: "Innovatech Indonesia", followers: 45100 },
    { namaAkun: "Digital Solution ID", followers: 38900 }
  ]
};

export const mockKompetitor: Kompetitor[] = [
  {
    id: "komp-001",
    brand: "PT Sejahtera Abadi",
    akunId: "akn-komp-001",
    platform: "instagram",
    username: "@sejahteraabadi",
    followers: 52300,
    followerGrowth: 4.8,
    avgEngagementRate: 3.2,
    totalPost: 1200,
    postingFrequency: 5.5,
    topPost: null,
    lastUpdated: new Date("2025-11-25")
  },
  {
    id: "komp-002",
    brand: "CV Digital Kreatif",
    akunId: "akn-komp-002",
    platform: "instagram",
    username: "@digitalkreatif",
    followers: 28100,
    followerGrowth: 6.1,
    avgEngagementRate: 4.5,
    totalPost: 654,
    postingFrequency: 3.2,
    topPost: null,
    lastUpdated: new Date("2025-11-25")
  }
];
```

## 9. Success Criteria
1. Dashboard dapat menampilkan data analitik dari maksimal 5 akun media sosial secara simultan.
2. Waktu muat dashboard utama < 2 detik untuk data 30 hari dengan 5 akun.
3. Metrik followers, engagement, reach, dan impressions tersaji dalam bentuk angka dan grafik.
4. Pengguna dapat membandingkan kinerja antar periode (MoM, WoW, YoY).
5. Data disinkronkan setiap 6 jam atau on-demand dengan tombol sinkronisasi manual.
6. Sistem dapat mengidentifikasi dan menampilkan top 5 postingan dengan engagement terbaik.
7. Laporan dapat diekspor dalam format PDF siap presentasi.
8. Audiens demografi tersaji lengkap (usia, gender, lokasi, jam aktif).
9. Fitur perbandingan kompetitor untuk maksimal 3 kompetitor per akun.
10. Semua teks antarmuka dalam Bahasa Indonesia yang baku dan konsisten.
