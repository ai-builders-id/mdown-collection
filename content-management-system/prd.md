# PRD: NusantaraCMS — Sistem Manajemen Konten Tanpa Coding untuk Publikasi Blog & Halaman
**Project:** NusantaraCMS
**Aesthetic:** Minimalis, bersih, hijau-putih dengan aksen emas — nuansa pemerintahan/korporasi Indonesia

## 1. Core Layout
Layout utama NusantaraCMS terdiri dari:
- **Sidebar kiri (256px)** — navigasi utama: Dashboard, Tulis Konten, Blog, Halaman, Media, Komentar, Pengaturan, Pengguna.
- **Top bar (64px)** — status publikasi, logo, avatar pengguna, indikator notifikasi.
- **Main panel (flex)** — area konten utama yang berubah berdasarkan menu aktif.
- **Right panel (opsional, 320px)** — sidebar kontekstual untuk statistik ringkas, SEO preview, rekomendasi konten terkait.
- **Bottom bar (32px)** — status autosave, koneksi, versi draf terakhir.

Layout bersifat responsif: sidebar menyusut menjadi ikon pada tablet (64px), dan menjadi bottom navigation pada layar < 768px.

## 2. Data Structure (Mock Only)

```typescript
// Interface untuk Artikel/Blog Post
interface BlogPost {
  id: string;                    // UUID, contoh: "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  judul: string;                 // Judul artikel, contoh: "Cara Membuat Website Desa"
  slug: string;                  // URL slug, contoh: "cara-membuat-website-desa"
  konten: string;                // HTML konten utama
  excerpt: string;               // Ringkasan singkat, maks 160 karakter
  status: 'draf' | 'terjadwal' | 'terbit' | 'arsip';
  kategori: KategoriArtikel[];
  tag: string[];                 // Contoh: ["teknologi", "pemerintahan", "digital"]
  penulis: Penulis;
  featuredImage: string | null;  // URL gambar unggulan
  seoTitle: string;              // Judul SEO
  seoDescription: string;        // Meta description
  createdAt: Date;
  updatedAt: Date;
  publishedAt: Date | null;
  scheduledAt: Date | null;
  views: number;
  estimasiBaca: number;          // Menit baca
}

// Interface untuk Halaman Statis
interface Halaman {
  id: string;
  judul: string;                 // Contoh: "Tentang Kami"
  slug: string;                  // Contoh: "tentang-kami"
  konten: string;
  template: 'default' | 'full-width' | 'landing' | 'kontak';
  status: 'draf' | 'terbit' | 'arsip';
  menuUtama: boolean;            // Tampil di navigasi utama?
  urutanMenu: number;            // Posisi di menu
  parentId: string | null;       // Halaman induk (untuk sub-halaman)
  seoTitle: string;
  seoDescription: string;
  createdAt: Date;
  updatedAt: Date;
  publishedAt: Date | null;
}

// Interface untuk Kategori
interface KategoriArtikel {
  id: string;
  nama: string;                  // Contoh: "Teknologi Informasi"
  slug: string;                  // Contoh: "teknologi-informasi"
  deskripsi: string;
  warna: string;                 // Hex color untuk badge
  icon: string;                  // Nama ikon Lucide
  parentId: string | null;       // Kategori induk
  jumlahPost: number;
  urutan: number;
}

// Interface untuk Media
interface MediaItem {
  id: string;
  nama: string;                  // Contoh: "banner-pembangunan-jembatan.jpg"
  url: string;
  thumbnailUrl: string;
  tipe: 'gambar' | 'dokumen' | 'video' | 'audio';
  mimeType: string;              // Contoh: "image/jpeg"
  ukuran: number;                // Bytes
  dimensi: { lebar: number; tinggi: number } | null;
  altText: string;
  caption: string;
  diuploadOleh: string;          // Nama pengguna
  createdAt: Date;
  digunakanDi: number;           // Counter berapa konten menggunakan media ini
}

// Interface untuk Komentar
interface Komentar {
  id: string;
  postId: string;
  nama: string;                  // Contoh: "Siti Rahmawati"
  email: string;
  konten: string;
  status: 'menunggu' | 'disetujui' | 'ditolak' | 'spam';
  parentId: string | null;       // Untuk reply komentar
  createdAt: Date;
  updatedAt: Date;
}

// Interface untuk Penulis / Pengguna
interface Penulis {
  id: string;
  nama: string;                  // Contoh: "Budi Hartono"
  email: string;
  avatar: string | null;
  role: 'admin' | 'editor' | 'penulis' | 'kontributor';
  bio: string;
  jabatan: string;               // Contoh: "Kepala Dinas Kominfo"
  totalPost: number;
  lastActive: Date;
  createdAt: Date;
}
```

## 3. Implementation Details
- **Vite 5.x** — build tool dengan React plugin
- **React 18.x** — library UI dengan functional components dan hooks
- **TypeScript 5.x** — type safety untuk seluruh kode
- **Tailwind CSS 3.x** — utility-first styling dengan custom config
- **React Router v6** — routing halaman dashboard
- **Zustand** — state management ringan
- **TipTap/Quill** — rich text editor untuk konten
- **React Query (TanStack Query)** — data fetching dan caching
- **date-fns** — manipulasi dan format tanggal dengan locale id
- **Lucide React** — ikon-ikon UI
- **React Hook Form + Zod** — validasi form

Struktur folder:
```
src/
├── components/        # UI components reusable
│   ├── layout/         # Sidebar, Topbar, MainLayout
│   ├── editor/         # Rich text editor components
│   ├── media/          # Media library components
│   └── common/         # Button, Card, Modal, Table, etc.
├── features/          # Feature-based modules
│   ├── dashboard/
│   ├── blog/
│   ├── pages/
│   ├── media/
│   ├── comments/
│   └── settings/
├── hooks/             # Custom React hooks
├── lib/               # Utilities dan helpers
├── stores/            # Zustand stores
├── types/             # TypeScript interfaces dan types
├── mocks/             # Mock data dan MSW handlers
└── utils/             # Formatting, validation, dll
```

## 4. Styling & Theming
**Palette Warna:**
- Primary: #0B6E4F (hijau tropis) — aksen utama, tombol, link
- Primary Light: #E8F5E9 (hijau muda) — background hover, badge
- Secondary: #F4A100 (emas) — aksen premium, label premium
- Neutral: #1E293B (slate 800) — teks utama
- Neutral Light: #F8FAFC (slate 50) — background halaman
- Danger: #DC2626 (red 600) — error, hapus
- Warning: #F59E0B (amber 500) — peringatan
- Success: #10B981 (emerald 500) — sukses, terbit
- Border: #E2E8F0 (slate 200)

**Tipografi:**
- Font utama: Inter (sans-serif) — untuk UI dan konten
- Font heading: Plus Jakarta Sans — untuk judul halaman
- Font monospace: JetBrains Mono — untuk kode
- Skala: 12/14/16/18/20/24/30/36/48px
- Line height: 1.5 untuk body, 1.25 untuk heading

**Spacing:**
- 4px base grid, menggunakan spacing Tailwind bawaan
- Padding konten utama: 24-32px
- Gap antar card: 16-24px
- Border radius: 8px untuk card, 6px untuk button, 4px untuk input

## 5. UI Components Specification

```
+----------------------------------------------------------+
| [Logo] NusantaraCMS    [Cari...]       [Notif] [Avatar]  | ← Topbar (64px)
+----------+-----------------------------------------------+
|          |  +------------------------------------------+ |
| DASHBOARD|  | Dashboard / Blog / Halaman Aktif         | |
|          |  |                                          | |
| Tulis    |  |  +------+ +------+ +------+              | |
| Konten   |  |  |Total  | |Terbit | |Draf   |  +----+  | |
|          |  |  |Post   | |Bulan  | |Perlu  |  |SEO |  | |
| Blog     |  |  | 1.234 | |ini    | |Review |  |Scor|  | |
|          |  |  +------+ |45     | |12     |  |e 92|  | |
| Halaman  |  |           +------+ +------+  |    |  | |
|          |  |                                          | |
| Media    |  |  +-------------------+ +--------------+  | |
|          |  |  | Artikel Terbaru   | | Kalender     |  | |
| Komentar |  |  | - Cara Digital... | | Publikasi    |  | |
|          |  |  | - Inovasi Layan.. | | [             |  | |
| Pengatu- |  |  | - Transformasi..  | |  Nov 2025  ] |  | |
| ran      |  |  +-------------------+ | Su Mo Tu..   |  | |
|          |  |                         +--------------+  | |
+----------+-----------------------------------------------+
| Autosave: 10:32 | Draf terakhir: 2 menit lalu           | ← Bottom bar
+----------------------------------------------------------+
```

**Komponen Detail:**

### Sidebar (kiri)
- Logo dan nama aplikasi di bagian atas
- Menu navigasi dengan ikon Lucide + label teks
- Indikator aktif dengan garis vertikal hijau
- Badge notifikasi untuk komentar menunggu
- Tombol "Tulis Konten Baru" yang menonjol
- Menu collapse untuk sub-item

### Topbar
- Breadcrumb navigasi kontekstual
- Search bar global (pencarian konten)
- Tombol notifikasi dengan dropdown
- Avatar pengguna + dropdown (Profil, Pengaturan, Keluar)
- Tombol "Lihat Situs" untuk preview front-end
- Indikator status koneksi (online/offline)

### Editor Konten (Halaman Tulis)
- Judul field (large input, font heading)
- Toolbar: Bold, Italic, Heading, List, Link, Media, Image, Code, Blockquote
- Sidebar kanan untuk meta: Kategori, Tag, Featured Image, SEO settings
- Tombol: Simpan Draf, Preview, Terbitkan, Jadwalkan
- Status autosave setiap 30 detik
- Word/karakter counter

### Card Statistics (Dashboard)
- Ikon latar belakang dengan opacity rendah
- Angka utama (besar, bold)
- Label deskripsi
- Perubahan persentase (hijau/merah)
- Sparkline chart mini (opsional)

### Tabel Data (Blog/Halaman)
- Kolom: Judul, Penulis, Kategori, Status, Tanggal, Aksi
- Sorting per kolom
- Filter: Semua/Terbit/Draf/Terjadwal
- Search per halaman
- Bulk action (pilih + aksi massal)
- Pagination dengan info "Menampilkan 1-10 dari 100"
- Row click untuk edit

### Media Library
- Grid view dengan thumbnail
- List view alternatif
- Upload drag-and-drop zone
- Filter by tipe (gambar/dokumen/video)
- Modal detail: preview, info file, alt text, caption
- Delete confirmation dialog

## 6. Interactions & States

**Loading States:**
- Skeleton loader untuk tabel (5 baris animasi pulse)
- Spinner untuk autosave (ikons kecil berputar)
- Progress bar untuk upload media
- Shimmer effect untuk card statistics

**Empty States:**
- Halaman dashboard baru: "Selamat datang di NusantaraCMS! Mulai dengan menulis artikel pertamamu."
- Tabel blog kosong: "Belum ada artikel. Klik 'Tulis Konten Baru' untuk memulai."
- Media kosong: "Belum ada media. Seret file ke sini atau klik upload."
- Komentar kosong: "Belum ada komentar dari pengunjung."
- Hasil pencarian kosong: "Tidak ditemukan konten dengan kata kunci '{{KATA_KUNCI}}'."
- Icon besar yang relevan diikuti teks dan CTA button.

**Error States:**
- Gagal muat data: "Gagal memuat data. Periksa koneksi Anda." + tombol "Muat Ulang"
- Gagal simpan: "Gagal menyimpan perubahan." + tombol "Coba Lagi"
- Gagal upload: "File {{NAMA_FILE}} gagal diupload. Format tidak didukung atau ukuran melebihi batas."
- 404 halaman: "Halaman tidak ditemukan." + tombol "Kembali ke Dashboard"
- Error network: Banner di topbar "Koneksi terputus. Perubahan akan disimpan secara lokal."

**Edge Cases:**
- Judul terlalu panjang: karakter counter berubah merah > 200 karakter
- Slug otomatis: dihasilkan dari judul, bisa diedit manual
- Konflik simpan: deteksi jika ada pengguna lain mengedit konten yang sama
- Jadwal publikasi di masa lalu: validasi error
- Hapus kategori yang memiliki 20 artikel: konfirmasi "Kategori akan dihapus. Artikel di dalamnya akan dipindahkan ke 'Tanpa Kategori'."
- Upload file duplikat: "File {{NAMA_FILE}} sudah ada. Timpa? [Timpa] [Simpan Sebagai Baru] [Batal]"
- Autosave conflict: "Terdeteksi dua versi. Pilih versi yang ingin disimpan."

## 7. Language & Localization
Semua teks antarmuka dalam Bahasa Indonesia:
- Tombol: "Simpan", "Batal", "Hapus", "Terbitkan", "Jadwalkan"
- Label: "Judul", "Kategori", "Status", "Penulis", "Tanggal Terbit"
- Pesan: "Apakah Anda yakin ingin menghapus konten ini?", "Perubahan berhasil disimpan"
- Notifikasi: "Artikel berhasil diterbitkan", "Komentar baru menunggu moderasi"
- Format tanggal: "25 November 2025" atau "2 hari yang lalu"
- Format angka: "1.234" (titik sebagai pemisah ribuan), "Rp 50.000"
- Waktu relatif: "baru saja", "5 menit lalu", "kemarin", "minggu lalu"

Locale date-fns: `locale: id` untuk `formatDistance` dan `format`.

## 8. Example Mock Data

```typescript
export const mockBlogPosts: BlogPost[] = [
  {
    id: "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    judul: "Transformasi Digital Pelayanan Publik di Kabupaten Banyuwangi",
    slug: "transformasi-digital-pelayanan-publik-banyuwangi",
    konten: "<h2>Latar Belakang</h2><p>Pemerintah Kabupaten Banyuwangi terus berinovasi dalam memberikan pelayanan publik berbasis digital...</p>",
    excerpt: "Kabupaten Banyuwangi meluncurkan 15 aplikasi baru untuk mempermudah akses layanan publik bagi warga.",
    status: "terbit",
    kategori: [
      { id: "kat-01", nama: "Transformasi Digital", slug: "transformasi-digital", deskripsi: "Artikel tentang digitalisasi", warna: "#0B6E4F", icon: "Monitor", parentId: null, jumlahPost: 45, urutan: 1 }
    ],
    tag: ["digitalisasi", "pelayanan-publik", "banyuwangi", "smart-city"],
    penulis: { id: "usr-01", nama: "Budi Hartono", email: "budi@banyuwangi.go.id", avatar: "/avatars/budi.jpg", role: "admin", bio: "Kepala Dinas Kominfo", jabatan: "Kepala Dinas", totalPost: 128, lastActive: new Date("2025-11-25"), createdAt: new Date("2023-01-15") },
    featuredImage: "/media/transformasi-digital-banyuwangi.jpg",
    seoTitle: "Transformasi Digital Pelayanan Publik Banyuwangi | Portal Resmi",
    seoDescription: "Kabupaten Banyuwangi meluncurkan 15 aplikasi digital untuk pelayanan publik. Baca selengkapnya di portal resmi pemerintah.",
    createdAt: new Date("2025-11-20"),
    updatedAt: new Date("2025-11-25"),
    publishedAt: new Date("2025-11-22"),
    scheduledAt: null,
    views: 3421,
    estimasiBaca: 8
  },
  {
    id: "b1a2c3d4-e5f6-7890-abcd-ef1234567890",
    judul: "Panduan Lengkap Mengelola Website Desa",
    slug: "panduan-mengelola-website-desa",
    konten: "<p>Website desa kini menjadi kebutuhan pokok setiap desa di Indonesia...</p>",
    excerpt: "Panduan praktis bagi perangkat desa untuk mengelola website desa secara mandiri tanpa perlu kemampuan coding.",
    status: "draf",
    kategori: [
      { id: "kat-02", nama: "Panduan", slug: "panduan", deskripsi: "Artikel panduan dan tutorial", warna: "#F4A100", icon: "BookOpen", parentId: null, jumlahPost: 32, urutan: 2 }
    ],
    tag: ["website-desa", "panduan", "digitalisasi-desa"],
    penulis: { id: "usr-02", nama: "Sari Indah Dewi", email: "sari@desamandiri.id", avatar: "/avatars/sari.jpg", role: "penulis", bio: "Penulis konten digital", jabatan: "Staf IT", totalPost: 56, lastActive: new Date("2025-11-24"), createdAt: new Date("2024-03-10") },
    featuredImage: "/media/panduan-website-desa.jpg",
    seoTitle: "Panduan Mengelola Website Desa | NusantaraCMS",
    seoDescription: "Pelajari cara mengelola website desa dengan mudah. Panduan lengkap untuk perangkat desa tanpa latar belakang IT.",
    createdAt: new Date("2025-11-24"),
    updatedAt: new Date("2025-11-25"),
    publishedAt: null,
    scheduledAt: new Date("2025-12-01"),
    views: 0,
    estimasiBaca: 12
  }
];

export const mockMediaItems: MediaItem[] = [
  {
    id: "med-001",
    nama: "banner-pembangunan-jembatan.jpg",
    url: "https://storage.example.com/media/banner-jembatan.jpg",
    thumbnailUrl: "https://storage.example.com/thumb/banner-jembatan-thumb.jpg",
    tipe: "gambar",
    mimeType: "image/jpeg",
    ukuran: 2457600,
    dimensi: { lebar: 1920, tinggi: 1080 },
    altText: "Banner pembangunan jembatan penghubung dua kecamatan",
    caption: "Dokumentasi proyek jembatan Kecamatan Suka Maju",
    diuploadOleh: "Budi Hartono",
    createdAt: new Date("2025-11-15"),
    digunakanDi: 3
  },
  {
    id: "med-002",
    nama: "laporan-keuangan-q3-2025.pdf",
    url: "https://storage.example.com/media/laporan-keuangan-q3.pdf",
    thumbnailUrl: null,
    tipe: "dokumen",
    mimeType: "application/pdf",
    ukuran: 1048576,
    dimensi: null,
    altText: "Laporan keuangan triwulan III 2025",
    caption: "Laporan keuangan yang telah diaudit",
    diuploadOleh: "Sari Indah Dewi",
    createdAt: new Date("2025-11-20"),
    digunakanDi: 1
  }
];

export const mockKomentar: Komentar[] = [
  {
    id: "kom-001",
    postId: "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    nama: "Agus Wijaya",
    email: "agus@email.com",
    konten: "Terima kasih informasinya. Apakah ada rencana untuk mengembangkan aplikasi mobile juga?",
    status: "menunggu",
    parentId: null,
    createdAt: new Date("2025-11-23"),
    updatedAt: new Date("2025-11-23")
  },
  {
    id: "kom-002",
    postId: "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    nama: "Dewi Sartika",
    email: "dewi@email.com",
    konten: "Sangat menginspirasi! Semoga daerah lain bisa mengikuti jejak Banyuwangi.",
    status: "disetujui",
    parentId: null,
    createdAt: new Date("2025-11-22"),
    updatedAt: new Date("2025-11-22")
  }
];
```

## 9. Success Criteria
1. Pengguna non-teknis berhasil membuat dan menerbitkan artikel dalam waktu < 10 menit tanpa pelatihan.
2. Waktu muat halaman dashboard < 2 detik (First Contentful Paint).
3. Editor konten memiliki autosave otomatis setiap 30 detik tanpa mengganggu alur menulis.
4. Media library mendukung upload drag-and-drop dengan preview instan.
5. Semua teks antarmuka menggunakan Bahasa Indonesia yang baik dan benar.
6. Role-based access control (Admin, Editor, Penulis, Kontributor) berfungsi penuh.
7. SEO metadata (title, description, Open Graph) tersedia untuk setiap konten.
8. Komentar dapat dimoderasi (setuju/tolak/spam) dalam satu klik.
9. Sistem dapat menangani minimal 1.000 artikel dan 500 media tanpa degradasi performa.
10. Responsif di desktop, tablet, dan mobile dengan navigasi yang tetap mudah diakses.
