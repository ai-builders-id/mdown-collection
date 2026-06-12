# TRD: Customer Support & Helpdesk — Technical Requirements

**Project:** Customer Support & Helpdesk
**Version:** 1.0

---

## 1. Technical Strategy

Customer Support & Helpdesk akan dibangun sebagai **static frontend boilerplate** dengan mock data. Arsitektur berbasis komponen dengan fokus pada simulasi workflow tiket dan respons cepat.

```text
[Ticket List] --> [Ticket Detail Panel] --> [Message Thread]
      |                  |                        |
      v                  v                        v
[Filter Manager]   [Status Workflow]      [Message Store (Mock)]
      |
      v
[Dashboard Stats] --> [Chart Components]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript + Tailwind CSS |
| Styling | Tailwind CSS |
| Icons | Lucide React |
| Animations | Framer Motion |
| Charts | Recharts (dashboard trends) |
| UI Library | shadcn/ui |
| Data | Mock JSON / TypeScript interfaces |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- ui/                    # shadcn/ui base components
|   |-- tickets/
|   |   |-- TicketTable.tsx
|   |   |-- TicketRow.tsx
|   |   |-- TicketDetail.tsx
|   |   |-- TicketForm.tsx
|   |   |-- StatusBadge.tsx
|   |   |-- PriorityBadge.tsx
|   |   |-- FilterBar.tsx
|   |-- messages/
|   |   |-- MessageThread.tsx
|   |   |-- MessageBubble.tsx
|   |   |-- ReplyInput.tsx
|   |   |-- InternalNote.tsx
|   |-- dashboard/
|   |   |-- StatsCard.tsx
|   |   |-- TrendsChart.tsx
|   |   |-- UrgentTickets.tsx
|   |   |-- AgentPerformance.tsx
|   |-- knowledge/
|       |-- ArticleList.tsx
|       |-- ArticleCard.tsx
|       |-- ArticleForm.tsx
|-- features/
|   |-- tickets/
|   |   |-- useTickets.ts
|   |   |-- useTicketFilter.ts
|   |   |-- TicketsPage.tsx
|   |-- dashboard/
|   |   |-- useDashboardStats.ts
|   |   |-- DashboardPage.tsx
|-- lib/
|   |-- ticket-utils.ts
|   |-- filter-utils.ts
|-- data/
|   |-- mockTickets.ts
|   |-- mockMessages.ts
|   |-- mockAgents.ts
|   |-- mockArticles.ts
|-- types/
|   |-- ticket.ts
|   |-- message.ts
|   |-- agent.ts
|   |-- dashboard.ts
|-- hooks/
|   |-- useLocalStorage.ts
|   |-- useDebounce.ts
`-- pages/
    |-- DashboardPage.tsx
    |-- TicketsPage.tsx
    |-- KnowledgePage.tsx
    |-- ReportsPage.tsx
```

---

## 4. Data Layer

### State Management
- useState + useReducer untuk ticket list management
- Context API untuk auth mock (current agent, role)
- Filter state sebagai URL params (opsional)

### Mock Data Strategy
- 20+ tiket mock dengan berbagai status dan prioritas
- 5+ agent mock dengan role dan skill berbeda
- 10+ message thread mock per tiket
- Data realistic dengan nama dan skenario Indonesia

### Data Flow
- Ticket CRUD → memengaruhi Ticket List, Dashboard Stats, Agent Stats
- Status change → validasi workflow rules → update UI semua komponen terkait
- Filter change → recalculate filtered list → update Ticket Table

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 500KB (gzipped) |

---

## 6. Styling & Theming System

- **Design Token:** CSS custom properties untuk warna, spacing, typography
- **Status Colors:** Cyan untuk Open, Amber untuk In Progress, Purple untuk Waiting, Green untuk Resolved, Slate untuk Closed
- **Priority Colors:** Slate (Low), Blue (Medium), Orange (High), Red (Urgent)
- **Mode:** Light mode default (dengan dark mode opsional)

```css
:root {
  --color-primary: #0891B2;
  --color-status-open: #0891B2;
  --color-status-progress: #D97706;
  --color-status-waiting: #7C3AED;
  --color-status-resolved: #059669;
  --color-status-closed: #64748B;
  --color-priority-urgent: #DC2626;
  /* ... tokens */
}
```

---

## 7. Validation Rules

- Tiket baru: judul required (min 5 char), deskripsi required (min 20 char)
- Email pelanggan: valid email format required
- Status change: validasi urutan workflow (Open→In Progress→Resolved→Closed)
- Priority: tidak boleh empty, default Medium
- Kategori: harus dipilih dari daftar yang tersedia
- Validation messages in Bahasa Indonesia

---

## 8. Error Handling Strategy

- **Ticket load error:** "Gagal memuat daftar tiket. Silakan refresh halaman."
- **Form validation:** Inline error messages pada setiap field
- **Optimistic updates:** Status change langsung tampil, rollback jika error simulasi
- **Global error boundary:** Mencegah crash pada satu panel memengaruhi panel lain
- **Empty states:** Kustom untuk filter tanpa hasil, dashboard tanpa data, knowledge base kosong

---

## 9. Security Considerations

- **Mock only:** Tidak ada autentikasi real, UI siap untuk login
- **Role simulation:** Admin vs Agent view (tambah/hapus tiket only untuk admin)
- **Internal notes:** Hanya visible untuk agent, tidak untuk customer (simulasi)
- **Data isolation:** Data hanya di memori browser, tidak ada transmisi

---

## 10. Deployment & Build

- **Build:** `npm run build` (Vite)
- **Output:** Static files (HTML, CSS, JS)
- **Hosting:** Static hosting siap (Netlify, Vercel, GitHub Pages)
- **CI/CD:** Opsional — GitHub Actions untuk deploy otomatis

---

## 11. Testing Strategy

| Level | Tools | Coverage Target |
|-------|-------|----------------|
| Unit | Vitest | > 70% |
| Component | React Testing Library | Komponen kritis (TicketTable, TicketForm, StatusBadge) |
| Visual | Storybook (opsional) | Komponen UI |
| E2E | Playwright (opsional) | Flow utama (create ticket, change status, filter) |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- Node.js 18+
- NPM / Yarn / PNPM
- Framer Motion 10+
- Lucide React
- Recharts
- shadcn/ui (via Tailwind + Radix UI)

### Constraints
- Tidak ada backend dependency
- Tidak ada API key / autentikasi real
- Semua fitur harus berfungsi offline
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
- Data tidak persist antar session (kecuali menggunakan localStorage opsional)
