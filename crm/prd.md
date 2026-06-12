# PRD: CRM — Kontak, Deals, Pipeline Penjualan, Aktivitas

**Project:** CRM
**Aesthetic:** Professional, modern, sales-oriented dengan warna biru yang trustworthy dan clean interface

---

## 1. Core Layout

Aplikasi menggunakan layout tiga panel yang dinamis:

- **Sidebar Kiri (18%):** Navigasi utama — Dashboard, Kontak, Perusahaan, Deals/Pipeline, Aktivitas, Laporan, Pengaturan.
- **Panel Tengah (50%):** Konten utama — daftar (tabel/kartu) dari entitas yang dipilih di sidebar.
- **Panel Kanan (32%):** Panel detail — menampilkan detail item yang dipilih di panel tengah, termasuk info terkait (deals, aktivitas, notes).

Pada mobile, panel kanan menjadi halaman terpisah dan sidebar menjadi bottom navigation.

---

## 2. Data Structure (Mock Only)

```typescript
export interface Contact {
  id: string;
  name: string;
  email: string;
  phone: string;
  companyId?: string;
  companyName?: string;
  position: string;
  avatar: string;
  tags: string[];
  source: 'referral' | 'website' | 'social_media' | 'event' | 'cold_call' | 'other';
  ownerId: string;
  ownerName: string;
  status: 'lead' | 'qualified' | 'customer' | 'churned';
  lastActivity: string;
  createdAt: string;
  updatedAt: string;
  deals: Deal[];
}

export interface Company {
  id: string;
  name: string;
  industry: string;
  size: '1-10' | '11-50' | '51-200' | '201-1000' | '1000+';
  website: string;
  phone: string;
  email: string;
  address: string;
  city: string;
  province: string;
  contacts: string[]; // contact IDs
  deals: string[]; // deal IDs
  totalRevenue: number;
  ownerId: string;
  ownerName: string;
  createdAt: string;
  updatedAt: string;
}

export interface Deal {
  id: string;
  title: string;
  value: number;
  currency: 'IDR' | 'USD';
  stage: 'lead_in' | 'qualified' | 'proposal' | 'negotiation' | 'closed_won' | 'closed_lost';
  probability: number; // persentase
  contactId: string;
  contactName: string;
  companyId?: string;
  companyName?: string;
  ownerId: string;
  ownerName: string;
  expectedCloseDate: string;
  closeDate?: string;
  lostReason?: string;
  products: DealProduct[];
  activities: Activity[];
  createdAt: string;
  updatedAt: string;
}

export interface DealProduct {
  productId: string;
  productName: string;
  quantity: number;
  unitPrice: number;
  totalPrice: number;
}

export interface Activity {
  id: string;
  type: 'call' | 'email' | 'meeting' | 'demo' | 'task' | 'note';
  title: string;
  description: string;
  contactId: string;
  contactName: string;
  dealId?: string;
  ownerId: string;
  ownerName: string;
  status: 'planned' | 'completed' | 'cancelled';
  dueDate: string;
  completedAt?: string;
  createdAt: string;
}

export interface PipelineStage {
  id: string;
  name: string;
  deals: Deal[];
  totalValue: number;
  color: string;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS dengan design tokens untuk CRM
- **Icons:** Lucide React
- **Animation:** Framer Motion (drag-and-drop pipeline, transisi panel detail)
- **Chart:** Recharts (pipeline value chart, activity timeline)
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary `#2563EB` (Blue), secondary slate untuk netral, green untuk won deals, red untuk lost deals.
- **Stage Colors:** Lead In (slate), Qualified (blue), Proposal (purple), Negotiation (amber), Won (green), Lost (red).
- **Typography:** Inter (sans-serif) dengan ukuran 14px untuk body, 16px untuk header tabel.
- **Spacing:** Tailwind scale, card dengan shadow-sm, border radius 8px.

---

## 5. UI Components Specification

```text
+-----------+---------------------------------------+-------------------------------+
| SIDEBAR   |         LIST VIEW                     |       DETAIL PANEL            |
| +-------+ | +---------------------------------+   | +---------------------------+ |
| |Dashboard| | [+ Add Contact] [Import] [Search] |   | | Budi Santoso             | |
| | Kontak  | | +-------------------------------+ |   | | Lead · PT Maju Jaya      | |
| |Perusahaan| || Name     | Company   | Stage  | |   | +---------------------------+ |
| | Deals   | || ---      | ---       | ---    | |   | | Info:                     | |
| | Pipeline| || Adi      | PT ABC    | Lead   | |   | | Email: budi@email.com     | |
| |Aktivitas| || Budi     | PT Maju   | Qual   | |   | | Phone: 0812-3456-7890     | |
| | Laporan | || Citra    | PT XYZ    | Prop   | |   | | Tags: [prioritas] [hot]   | |
| |Pengaturan| |+-------------------------------+   |   | +---------------------------+ |
| +-------+ | | Pipeline:                         |   | | Pipeline:                  | |
|          | | [Lead] [Qual] [Prop] [Nego] [Won]  |   | | [Qualified > Proposal]     | |
|          | | [===Progres Bar===]                 |   | | Nilai: Rp 150.000.000      | |
|          | +---------------------------------+   |   | | Prob: 60%                  | |
|          |                                       |   | +---------------------------+ |
|          |                                       |   | | Aktivitas Terbaru:         | |
|          |                                       |   | | [Call] 12/06 - Follow up  | |
|          |                                       |   | | [Demo] 10/06 - Demo produk | |
|          |                                       |   | | [Note] 09/06 - Minat tinggi| |
|          |                                       |   | +---------------------------+ |
+-----------+---------------------------------------+-------------------------------+
```

Komponen utama:
- **ContactTable:** Tabel kontak dengan sorting, filter, bulk actions.
- **ContactCard:** Kartu ringkas untuk tampilan grid.
- **PipelineBoard:** Kanban board dengan drag-and-drop antar stage.
- **DealCard:** Kartu deal di pipeline dengan nilai, probabilitas, dan avatar owner.
- **ActivityTimeline:** Timeline kronologis aktivitas per kontak/deal.
- **DetailPanel:** Panel sliding untuk detail entitas dengan tabs (Info, Deals, Aktivitas, Notes).
- **ActivityForm:** Form modal untuk mencatat aktivitas (call, email, meeting, demo, note).

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton tabel (5 baris); skeleton pipeline (card kosong di setiap stage); skeleton detail panel.
- **Empty:** "Belum ada kontak. Tambah kontak pertama Anda untuk memulai." Ilustrasi people dengan tombol "Tambah Kontak".
- **Error:** "Gagal memuat data. Silakan coba lagi." Alert dengan retry button.
- **Edge cases:** Deal dengan nilai Rp 0; pipeline stage kosong; kontak tanpa perusahaan; aktivitas tanpa deal.

**Flow Interaksi Utama:**
1. User membuka halaman Kontak → melihat tabel kontak → klik baris → panel detail terbuka.
2. User klik Pipeline → melihat kanban board → drag deal dari "Qualified" ke "Proposal" → stage berubah (simulasi animated).
3. User buka Kontak → klik "Tambah Kontak" → form modal → isi data → submit → kontak baru muncul di tabel.
4. User di detail kontak → tab Aktivitas → lihat timeline → klik "Catat Aktivitas" → form → submit.
5. User filter kontak berdasarkan status/tags → tabel terfilter → pilih salah satu untuk detail.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Label: "Dashboard", "Kontak", "Perusahaan", "Pipeline", "Aktivitas", "Laporan".
- Stage: "Awal", "Kualifikasi", "Proposal", "Negosiasi", "Menang", "Hilang".
- Tombol: "Tambah Kontak", "Tambah Perusahaan", "Buat Deal", "Catat Aktivitas", "Simpan", "Hapus".
- Status: "Rencana", "Selesai", "Dibatalkan".
- Notifikasi: "Kontak berhasil ditambahkan", "Deal dipindahkan ke {{stage}}", "Aktivitas dicatat".

---

## 8. Example Mock Data

```typescript
export const mockContacts: Contact[] = [
  {
    id: 'contact-001',
    name: 'Budi Santoso',
    email: 'budi@majujaya.co.id',
    phone: '0812-3456-7890',
    companyId: 'company-001',
    companyName: 'PT Maju Jaya',
    position: 'CEO',
    avatar: '/avatars/budi.png',
    tags: ['prioritas', 'hot-lead'],
    source: 'referral',
    ownerId: 'user-001',
    ownerName: 'Ahmad Rizki',
    status: 'qualified',
    lastActivity: '2026-06-12T10:00:00Z',
    createdAt: '2026-01-15T08:00:00Z',
    updatedAt: '2026-06-12T10:00:00Z',
    deals: [],
  },
  {
    id: 'contact-002',
    name: 'Citra Dewi',
    email: 'citra@xyz.co.id',
    phone: '0856-7890-1234',
    companyId: 'company-002',
    companyName: 'PT XYZ Indonesia',
    position: 'Marketing Director',
    avatar: '/avatars/citra.png',
    tags: ['prospek'],
    source: 'website',
    ownerId: 'user-001',
    ownerName: 'Ahmad Rizki',
    status: 'lead',
    lastActivity: '2026-06-10T14:00:00Z',
    createdAt: '2026-02-20T09:00:00Z',
    updatedAt: '2026-06-10T14:00:00Z',
    deals: [],
  },
];

export const mockDeals: Deal[] = [
  {
    id: 'deal-001',
    title: 'Enterprise Package - PT Maju Jaya',
    value: 150000000,
    currency: 'IDR',
    stage: 'qualified',
    probability: 60,
    contactId: 'contact-001',
    contactName: 'Budi Santoso',
    companyId: 'company-001',
    companyName: 'PT Maju Jaya',
    ownerId: 'user-001',
    ownerName: 'Ahmad Rizki',
    expectedCloseDate: '2026-08-30',
    products: [
      { productId: 'prod-001', productName: 'Enterprise License', quantity: 50, unitPrice: 3000000, totalPrice: 150000000 },
    ],
    activities: [],
    createdAt: '2026-03-01T08:00:00Z',
    updatedAt: '2026-06-12T10:00:00Z',
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] CRUD kontak, perusahaan, deal, aktivitas berfungsi
- [ ] Pipeline kanban dengan drag-and-drop antar stage bekerja
- [ ] Filter dan search pada semua list views berfungsi
- [ ] Panel detail menampilkan info lengkap terkait entitas
- [ ] Activity timeline per kontak/deal berfungsi
- [ ] Responsive design (desktop & mobile)
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
