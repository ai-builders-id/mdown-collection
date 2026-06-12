# PRD: AI Chatbot — AI Chatbot dengan Knowledge Base dan Custom Knowledge

**Project:** AI Chatbot
**Aesthetic:** Modern, clean, conversational AI interface dengan sentuhan futuristik

---

## 1. Core Layout

Aplikasi terdiri dari tiga area utama:

- **Sidebar Kiri (25%):** Daftar percakapan/history, tombol new chat, dan navigasi ke halaman Knowledge Base.
- **Panel Tengah (50%):** Area chat utama — menampilkan pesan user dan bot dalam format gelembung (bubble chat).
- **Panel Kanan (25%):** Panel konteks — menampilkan sumber knowledge yang dirujuk oleh bot, snippet dokumen, dan tombol aksi.

Layout responsif: pada mobile, sidebar dan panel kanan menjadi overlay/drawer.

---

## 2. Data Structure (Mock Only)

```typescript
export interface ChatMessage {
  id: string;
  sessionId: string;
  role: 'user' | 'assistant' | 'system';
  content: string;
  timestamp: string;
  sources?: KnowledgeSource[];
  metadata?: Record<string, unknown>;
}

export interface ChatSession {
  id: string;
  title: string;
  messages: ChatMessage[];
  createdAt: string;
  updatedAt: string;
  status: 'active' | 'archived';
  model: string;
}

export interface KnowledgeBase {
  id: string;
  name: string;
  description: string;
  documents: KnowledgeDocument[];
  createdAt: string;
  updatedAt: string;
  type: 'internal' | 'external' | 'custom';
}

export interface KnowledgeDocument {
  id: string;
  knowledgeBaseId: string;
  title: string;
  content: string;
  tags: string[];
  source: string;
  category: string;
  createdAt: string;
  updatedAt: string;
}

export interface KnowledgeSource {
  documentId: string;
  documentTitle: string;
  relevanceScore: number;
  excerpt: string;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS dengan class utility kustom
- **Icons:** Lucide React
- **Animation:** Framer Motion (transisi pesan, typing indicator)
- **Chart:** Recharts (untuk analytics knowledge usage)
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls, simulasi AI response dengan delay acak

---

## 4. Styling & Theming

- **Palette:** Primary `#4F46E5` (Indigo), secondary cyan untuk accent, surface putih/abu-abu, teks dark gray.
- **Typography:** Font system-native (Inter via CDN), ukuran 14px untuk body chat, 16px untuk header.
- **Spacing:** Konsisten menggunakan skala Tailwind (p-4, gap-3, dll).
- **Komponen Kustom:** Bubble chat dengan gradient subtle, shadow untuk depth, border-radius 16px untuk pesan.

---

## 5. UI Components Specification

```text
+---------------------------+-----------------------------------+---------------------------+
|        SIDEBAR            |          CHAT AREA                |    KNOWLEDGE PANEL        |
|  +---------------------+  |  +-----------------------------+  |  +---------------------+  |
|  | [+ New Chat]        |  |  |  Header: AI Chatbot         |  |  | Referenced Sources  |  |
|  +---------------------+  |  |  [Model Selector] [Clear]    |  |  +---------------------+  |
|  +---------------------+  |  +-----------------------------+  |  | [Doc Title 1]        |  |
|  | Search conversations|  |  |  +-----------------------+  |  |   excerpt...          |  |
|  +---------------------+  |  |  | User message (right)   |  |  | [Doc Title 2]        |  |
|  +-- Chat History ------+  |  |  +-----------------------+  |  |   excerpt...          |  |
|  | > Strategi Q4       |  |  |  +-----------------------+  |  +---------------------+  |
|  |   Penjelasan Kode    |  |  |  | Bot response (left)   |  |  | Knowledge Base Info  |  |
|  |   Kebijakan HR       |  |  |  | [Sources badge]       |  |  | Type: Internal       |  |
|  |   [archived...]      |  |  |  +-----------------------+  |  | Docs: 24             |  |
|  +---------------------+  |  |  |  ... typing ...        |  |  | Last updated: 12/06  |  |
|  +---------------------+  |  |  +-----------------------+  |  +---------------------+  |
|  | Knowledge Base      |  |  +-----------------------------+  |                           |
|  | [Manage KB]         |  |  |  Input: [Type message...]    |  |                           |
|  +---------------------+  |  |  [Send] [Attach] [Microphone]|  |                           |
+---------------------------+-----------------------------------+---------------------------+
```

Komponen utama:
- **ChatBubble:** Pesan user (kanan, bg indigo) / bot (kiri, bg white dengan border). Menampilkan avatar, timestamp, source badge.
- **ChatInput:** Textarea auto-resize + tombol send/attach/mic.
- **TypingIndicator:** Animasi tiga titik dengan Framer Motion.
- **Sidebar:** Daftar sesi chat, search, dropdown model.
- **KnowledgePanel:** Daftar sumber yang dirujuk, expandable.
- **KnowledgeManager:** Halaman CRUD untuk knowledge base dan dokumen.

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton untuk sidebar chat history; spinner di panel kanan saat memuat knowledge base.
- **Empty:** "Belum ada percakapan. Mulai chat baru untuk bertanya tentang knowledge base Anda." Ilustrasi bot vector.
- **Error:** "Gagal memuat pesan. Silakan refresh halaman." Card error dengan tombol retry.
- **Edge cases:** Pesan kosong tidak bisa dikirim; input melebihi 4000 karakter mendapat warning; dokumen duplikat di knowledge base mendapat notifikasi; pencarian chat history tanpa hasil.

**Flow Interaksi Utama:**
1. User membuka halaman → melihat daftar sesi (atau empty state) → klik "New Chat".
2. User mengetik pertanyaan → klik Send → animasi typing indicator muncul → setelah delay acak (1-3 detik), response bot muncul dengan sumber knowledge.
3. User klik badge sumber → panel kanan menampilkan excerpt dokumen lengkap.
4. User buka Knowledge Manager → tambah/edit/hapus dokumen → perubahan tercermin di chat.
5. User filter chat history → hasil filter ditampilkan di sidebar.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Semua UI labels: "Percakapan Baru", "Cari percakapan...", "Ketuk untuk mengirim", "Sumber", "Knowledge Base", "Tambah Dokumen", "Hapus", "Edit".
- Placeholder input: "Tanyakan sesuatu tentang knowledge base Anda..."
- Notifikasi error dalam Bahasa Indonesia.
- Timestamp: format relatif ("2 menit lalu", "1 jam lalu", "Kemarin").

---

## 8. Example Mock Data

```typescript
export const mockSessions: ChatSession[] = [
  {
    id: 'session-001',
    title: 'Strategi Pemasaran Q4',
    messages: [
      {
        id: 'msg-001',
        sessionId: 'session-001',
        role: 'user',
        content: 'Apa saja strategi pemasaran yang efektif untuk Q4?',
        timestamp: '2026-06-13T09:00:00Z',
      },
      {
        id: 'msg-002',
        sessionId: 'session-001',
        role: 'assistant',
        content: 'Berdasarkan knowledge base kami, strategi Q4 yang efektif meliputi: 1) Email marketing dengan segmentasi pelanggan, 2) Diskon early-bird untuk loyalis, 3) Konten video di media sosial. Saya akan lampirkan dokumen panduan lengkapnya.',
        timestamp: '2026-06-13T09:00:03Z',
        sources: [
          {
            documentId: 'doc-001',
            documentTitle: 'Strategi Pemasaran Tahunan 2026',
            relevanceScore: 0.95,
            excerpt: 'Q4 adalah momen kritis untuk memaksimalkan penjualan akhir tahun...',
          },
        ],
      },
    ],
    createdAt: '2026-06-13T09:00:00Z',
    updatedAt: '2026-06-13T09:00:03Z',
    status: 'active',
    model: 'GPT-4',
  },
];

export const mockKnowledgeBases: KnowledgeBase[] = [
  {
    id: 'kb-001',
    name: 'Pengetahuan Perusahaan',
    description: 'Dokumen internal perusahaan termasuk kebijakan, SOP, dan panduan',
    documents: [
      {
        id: 'doc-001',
        knowledgeBaseId: 'kb-001',
        title: 'Strategi Pemasaran Tahunan 2026',
        content: 'Dokumen ini berisi strategi pemasaran lengkap untuk tahun 2026...',
        tags: ['pemasaran', 'strategi', 'tahunan'],
        source: 'Tim Marketing',
        category: 'Strategi',
        createdAt: '2026-01-15T08:00:00Z',
        updatedAt: '2026-05-20T10:30:00Z',
      },
    ],
    createdAt: '2025-12-01T08:00:00Z',
    updatedAt: '2026-06-10T14:00:00Z',
    type: 'internal',
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] CRUD operations untuk knowledge base berfungsi (tambah, edit, hapus dokumen)
- [ ] Chat flow berfungsi: user input -> simulated response -> source display
- [ ] Search chat history dan filter knowledge base bekerja
- [ ] Responsive design (desktop & mobile) dengan sidebar/knowledge panel sebagai drawer di mobile
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
