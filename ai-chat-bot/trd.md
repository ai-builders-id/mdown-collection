# TRD: AI Chatbot — Technical Requirements

**Project:** AI Chatbot
**Version:** 1.0

---

## 1. Technical Strategy

AI Chatbot akan dibangun sebagai **static frontend boilerplate** dengan mock data. Arsitektur berbasis komponen dengan fokus pada rapid prototyping untuk simulasi AI chat yang realistis.

```text
[Chat UI Component] --> [Session Manager] --> [Mock AI Engine] --> [Knowledge Base Store]
        |                      |                       |                     |
        v                      v                       v                     v
[Message List]         [Context API]           [Delay Simulator]       [Document Store]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript + Tailwind CSS |
| Styling | Tailwind CSS |
| Icons | Lucide React |
| Animations | Framer Motion |
| Charts | Recharts (untuk knowledge usage analytics opsional) |
| UI Library | shadcn/ui |
| Data | Mock JSON / TypeScript interfaces |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- ui/                    # shadcn/ui base components
|   |-- chat/
|   |   |-- ChatBubble.tsx
|   |   |-- ChatInput.tsx
|   |   |-- ChatHeader.tsx
|   |   |-- TypingIndicator.tsx
|   |   |-- MessageList.tsx
|   |-- sidebar/
|   |   |-- SessionList.tsx
|   |   |-- SessionItem.tsx
|   |   |-- NewChatButton.tsx
|   |-- knowledge/
|       |-- KnowledgePanel.tsx
|       |-- SourceBadge.tsx
|       |-- SourceDetail.tsx
|       |-- DocumentForm.tsx
|       |-- DocumentList.tsx
|       |-- KnowledgeBaseCard.tsx
|-- features/
|   |-- chat/
|   |   |-- useChatSession.ts
|   |   |-- useMockAI.ts
|   |   |-- ChatPage.tsx
|   |-- knowledge/
|       |-- useKnowledgeBase.ts
|       |-- KnowledgePage.tsx
|-- lib/
|   |-- mock-ai.ts             # Simulated AI response engine
|   |-- utils.ts
|-- data/
|   |-- mockSessions.ts
|   |-- mockKnowledgeBases.ts
|   |-- mockDocuments.ts
|-- types/
|   |-- chat.ts
|   |-- knowledge.ts
|-- hooks/
|   |-- useLocalStorage.ts
|   |-- useDebounce.ts
`-- pages/
    |-- ChatPage.tsx
    |-- KnowledgePage.tsx
```

---

## 4. Data Layer

### State Management
- Local state dengan React hooks (useState, useReducer untuk chat messages)
- Context API untuk shared state (active session, knowledge base context)

### Mock AI Strategy
- **Response generation:** Mapping keyword-based responses — input user dicocokkan dengan kata kunci dalam dokumen knowledge base
- **Delay simulation:** SetTimeout acak 1-3 detik untuk simulasi processing
- **Source selection:** Dokumen dengan tag relevan dipilih sebagai sumber jawaban
- **Multi-turn context:** Riwayat pesan dalam sesi digunakan untuk konteks jawaban berikutnya

### Mock Data Strategy
- Semua data di-hardcode dalam file TypeScript
- Struktur data mengikuti interface yang didefinisikan
- Data realistic dengan format id (Indonesia)

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
- **Mode:** Light / Dark mode support
- **Responsive:** Mobile-first dengan breakpoints Tailwind
- **Chat Bubble:** Gradient subtle untuk bot, solid untuk user; border-radius 16px

```css
:root {
  --color-primary: #4F46E5;
  --color-primary-light: #6366F1;
  --color-bg-chat: #F9FAFB;
  --color-bubble-user: #4F46E5;
  --color-bubble-bot: #FFFFFF;
  --color-source-badge: #EEF2FF;
  /* ... tokens */
}
```

---

## 7. Validation Rules

- Form validation menggunakan HTML5 built-in + custom validation
- Judul dokumen: required, min 5 karakter, max 200 karakter
- Konten dokumen: required, min 20 karakter, max 50.000 karakter
- Input chat: max 4000 karakter dengan character counter
- Tags: min 1, max 10 tags per dokumen
- Semantic validation messages in Bahasa Indonesia

---

## 8. Error Handling Strategy

- **Chat errors:** Simulasi "Gagal memproses pertanyaan. Silakan coba lagi." dengan retry button
- **Knowledge base errors:** Gagal muat dokumen dengan opsi reload
- **Global error boundary:** React Error Boundary untuk mencegah crash seluruh app
- **Input validation:** Error message inline pada form di knowledge base
- **Empty states:** Kustom untuk setiap modul (no sessions, no documents, no results)

---

## 9. Security Considerations

- **Mock only:** Tidak ada autentikasi real, tapi UI siap untuk login page
- **Input sanitization:** Text content dibersihkan dari script injection untuk demo
- **Session isolation:** Data sesi hanya di memori/localStorage, tidak dikirim ke server
- **Simulated role-based access:** Tampilkan/menyembunyikan fitur knowledge management berdasarkan role (opsional)

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
| Component | React Testing Library | Komponen kritis (ChatBubble, ChatInput, DocumentForm) |
| Visual | Storybook (opsional) | Komponen UI |
| E2E | Playwright (opsional) | Flow utama (chat, knowledge management) |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- Node.js 18+
- NPM / Yarn / PNPM
- Framer Motion 10+
- Lucide React
- Recharts (opsional)
- shadcn/ui (via Tailwind + Radix UI)

### Constraints
- Tidak ada backend dependency
- Tidak ada API key / autentikasi real
- Semua fitur harus berfungsi offline
- Delay simulasi AI tidak boleh > 5 detik
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
- Tidak ada WebSocket — polling / timeout-based mock
