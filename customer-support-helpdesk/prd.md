# PRD: Customer Support & Helpdesk — Sistem Tiket, Prioritas, dan Workflow Support

**Project:** Customer Support & Helpdesk
**Aesthetic:** Professional, clean, enterprise-grade dengan aksen cyan yang menenangkan

---

## 1. Core Layout

Aplikasi menggunakan layout dua panel utama:

- **Sidebar Kiri (20%):** Navigasi utama — Dashboard, Tiket, Knowledge Base, Laporan, Pengaturan. Dilengkapi badge notifikasi dan user profile.
- **Panel Utama (80%):** Area konten dinamis yang berubah berdasarkan menu aktif. Untuk halaman Tiket: tabel daftar tiket dengan filter dan panel detail tiket (split vertical).

Pada mobile, sidebar menjadi hamburger menu dan panel detail menjadi halaman terpisah.

---

## 2. Data Structure (Mock Only)

```typescript
export interface Ticket {
  id: string;
  ticketNumber: string;
  title: string;
  description: string;
  status: 'open' | 'in_progress' | 'waiting_customer' | 'resolved' | 'closed';
  priority: 'low' | 'medium' | 'high' | 'urgent';
  category: 'technical' | 'billing' | 'account' | 'feature_request' | 'complaint';
  customerId: string;
  customerName: string;
  customerEmail: string;
  assigneeId?: string;
  assigneeName?: string;
  createdAt: string;
  updatedAt: string;
  resolvedAt?: string;
  tags: string[];
  attachments: string[];
}

export interface TicketMessage {
  id: string;
  ticketId: string;
  senderId: string;
  senderName: string;
  senderRole: 'customer' | 'agent' | 'system';
  content: string;
  attachments: string[];
  createdAt: string;
  isInternal: boolean;
}

export interface SupportAgent {
  id: string;
  name: string;
  email: string;
  avatar: string;
  role: 'admin' | 'agent' | 'manager';
  status: 'online' | 'away' | 'offline';
  activeTickets: number;
  resolvedToday: number;
  averageResponseTime: number; // dalam menit
  skills: string[];
}

export interface KnowledgeArticle {
  id: string;
  title: string;
  content: string;
  category: string;
  tags: string[];
  views: number;
  helpfulCount: number;
  notHelpfulCount: number;
  createdAt: string;
  updatedAt: string;
  status: 'draft' | 'published' | 'archived';
}

export interface TicketStats {
  total: number;
  open: number;
  inProgress: number;
  resolved: number;
  closed: number;
  urgent: number;
  averageResponseTime: number;
  satisfactionRate: number; // persentase 0-100
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS dengan design system enterprise
- **Icons:** Lucide React
- **Animation:** Framer Motion (transisi panel, notifikasi real-time simulasi)
- **Chart:** Recharts (dashboard stats, tren tiket)
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary `#0891B2` (Cyan), secondary blue-600 untuk aksen, slate/neutral untuk background, teks slate-900.
- **Status Colors:** Open (blue), In Progress (amber), Waiting (purple), Resolved (green), Closed (slate).
- **Priority Colors:** Low (slate), Medium (blue), High (orange), Urgent (red).
- **Typography:** Font system-native, 14px body, 16px headings.
- **Spacing:** Konsisten Tailwind scale.

---

## 5. UI Components Specification

```text
+-------------+----------------------------------------------------+
|  SIDEBAR    |                 MAIN CONTENT                        |
| +---------+ | +------------------------------------------------+ |
| | Dashboard| | | [Ticket List] [+ New Ticket] [Export]         | |
| | [Badge]  | | +----------------------------------------------+ |
| | Tiket    | | | Filter Bar: [Status v] [Priority v] [Search] | |
| | [Badge]  | | +----------------------------------------------+ |
| | KB Articles| | Table:                                        | |
| | Laporan  | | | # | Title | Customer | Priority | Status |   | |
| | Pengaturan| | |----+-------+----------+----------+--------+   | |
| +---------+ | | |001 | PC error... | Budi    | Urgent | Open   | |
| | Profile  | | |002 | Billing... | Sari    | Medium | In Prog| |
| | Avatar   | | +----------------------------------------------+ |
| | Nama     | | +----------------------------------------------+ |
| | Status   | | |         PANEL DETAIL TIKET                    | |
| +---------+ | | [TIK-001] PC error setelah update               | |
|             | | Status: Open | Priority: Urgent | Assign: --   | |
|             | | +------ Messages Thread ---------------------+ | |
|             | | | Customer: Saya tidak bisa login...         | | |
|             | | | Agent: Silakan coba reset password...      | | |
|             | | | [Type a reply...] [Send] [Internal Note]   | | |
|             | | +--------------------------------------------+ | |
|             | +------------------------------------------------+ |
+-------------+----------------------------------------------------+
```

Komponen utama:
- **TicketTable:** Tabel tiket dengan sorting, filtering, pagination.
- **TicketDetail:** Panel detail dengan thread pesan, info customer, dan aksi.
- **MessageThread:** Daftar pesan kronologis, balasan, internal notes.
- **StatusBadge:** Badge warna-warni untuk status/prioritas.
- **FilterBar:** Kombinasi dropdown filter + search input.
- **DashboardStats:** Kartu metrik (total, open, resolved) + grafik tren.
- **NewTicketModal:** Form wizard untuk membuat tiket baru.

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton tabel (5 baris shimmer) saat memuat daftar tiket; spinner di panel detail.
- **Empty:** "Belum ada tiket. Semua tiket support akan muncul di sini." Ilustrasi inbox kosong dengan tombol "Buat Tiket Baru".
- **Error:** "Gagal memuat data tiket. Silakan coba lagi." Card error dengan tombol retry.
- **Edge cases:** Tiket dengan >50 pesan (scroll virtual); filter yang tidak menghasilkan data; input kosong pada form tiket baru.

**Flow Interaksi Utama:**
1. Agent login (simulasi) → melihat Dashboard dengan stats dan grafik → klik "Tiket".
2. Agent klik baris tiket → panel detail terbuka di sebelah kanan → agent membaca thread → membalas pesan.
3. Agent mengganti status tiket (Open → In Progress → Resolved) → notifikasi sukses.
4. Agent membuat tiket baru → form modal → submit → tiket muncul di tabel.
5. Filter tiket berdasarkan status/priority → tabel terfilter → klik tiket untuk detail.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Label: "Dashboard", "Tiket", "Pengetahuan", "Laporan", "Pengaturan".
- Tombol: "Buat Tiket Baru", "Simpan", "Batalkan", "Ekspor CSV", "Balas", "Catatan Internal".
- Status: "Terbuka", "Diproses", "Menunggu Pelanggan", "Terselesaikan", "Ditutup".
- Prioritas: "Rendah", "Sedang", "Tinggi", "Darurat".
- Notifikasi: "Tiket berhasil dibuat", "Status diperbarui", "Gagal memuat data".

---

## 8. Example Mock Data

```typescript
export const mockTickets: Ticket[] = [
  {
    id: 'ticket-001',
    ticketNumber: 'TIK-001',
    title: 'PC error setelah update sistem',
    description: 'Setelah update terbaru, komputer saya terus blue screen saat menjalankan aplikasiAkuntansi.',
    status: 'open',
    priority: 'urgent',
    category: 'technical',
    customerId: 'cust-001',
    customerName: 'Budi Santoso',
    customerEmail: 'budi@email.com',
    assigneeId: null,
    assigneeName: null,
    createdAt: '2026-06-13T08:30:00Z',
    updatedAt: '2026-06-13T08:30:00Z',
    tags: ['blue-screen', 'update', 'windows'],
    attachments: ['screenshot-error.png'],
  },
  {
    id: 'ticket-002',
    ticketNumber: 'TIK-002',
    title: 'Tagihan tidak sesuai dengan pemakaian',
    description: 'Saya ditagih Rp 500.000 tapi pemakaian hanya Rp 350.000 sesuai catatan saya.',
    status: 'in_progress',
    priority: 'high',
    category: 'billing',
    customerId: 'cust-002',
    customerName: 'Sari Dewi',
    customerEmail: 'sari@email.com',
    assigneeId: 'agent-001',
    assigneeName: 'Ahmad Rizki',
    createdAt: '2026-06-12T14:00:00Z',
    updatedAt: '2026-06-13T09:00:00Z',
    tags: ['billing', 'dispute'],
    attachments: [],
  },
];

export const mockAgents: SupportAgent[] = [
  {
    id: 'agent-001',
    name: 'Ahmad Rizki',
    email: 'ahmad@support.co.id',
    avatar: '/avatars/ahmad.png',
    role: 'agent',
    status: 'online',
    activeTickets: 3,
    resolvedToday: 5,
    averageResponseTime: 12,
    skills: ['technical', 'billing'],
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] CRUD tickets berfungsi (create, read, update status, delete)
- [ ] Filter dan search tiket berdasarkan status, prioritas, kategori bekerja
- [ ] Message thread dengan balasan dan internal notes berfungsi
- [ ] Dashboard stats dan grafik menampilkan data mock dengan benar
- [ ] Responsive design (desktop & mobile)
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
