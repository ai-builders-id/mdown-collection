# PRD: Chat AI Prompt Generator — Generator Prompt untuk AI Chat

**Project:** Chat AI Prompt Generator
**Aesthetic:** Futurist, clean, dark/light mode, dengan nuansa AI/tech

---

## 1. Core Layout

Tiga panel utama:

- **Sidebar Kiri (25%):** Daftar kategori prompt (ChatGPT, Claude, Gemini, Midjourney, DALL-E, Stable Diffusion), folder koleksi, dan riwayat prompt.
- **Panel Tengah (50%):** Prompt editor dengan syntax highlighting, parameter variables, dan preview.
- **Panel Kanan (25%):** Template library, prompt library, quick snippets.

---

## 2. Data Structure (Mock Only)

```typescript
export interface PromptCategory {
  id: string;
  name: string; // "ChatGPT", "Claude", "Midjourney", dll
  icon: string;
  color: string;
  description: string;
  promptCount: number;
}

export interface PromptTemplate {
  id: string;
  title: string;
  content: string;
  categoryId: string;
  tags: string[];
  variables: PromptVariable[];
  author: string;
  version: string;
  isPublic: boolean;
  usageCount: number;
  createdAt: string;
  updatedAt: string;
}

export interface PromptVariable {
  key: string;
  label: string;
  type: 'text' | 'number' | 'select' | 'textarea';
  defaultValue: string;
  options?: string[];
  required: boolean;
  placeholder: string;
}

export interface PromptHistory {
  id: string;
  templateId?: string;
  title: string;
  content: string;
  filledContent: string;
  modelType: string;
  tokens: number;
  createdAt: string;
  isFavorite: boolean;
}

export interface PromptCollection {
  id: string;
  name: string;
  description: string;
  promptIds: string[];
  createdAt: string;
  updatedAt: string;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Syntax Highlighting:** CodeMirror atau Monaco Editor mini
- **Icons:** Lucide React
- **Animation:** Framer Motion
- **State:** React Context + useReducer
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary `#A855F7` (Purple), accent `#06B6D4` (Cyan), dark bg `#0F172A`
- **Typography:** Inter / system sans-serif untuk UI, JetBrains Mono untuk code/editor
- **Mode:** Dark mode default, light mode toggle
- **Spacing:** Konsisten dengan utility classes

---

## 5. UI Components Specification

```text
+------------------------------------------+
| [Kategori]  | [Editor Area]     | [Panel] |
|             |                   |         |
|  🔹ChatGPT  |  Title: [...]     | 📋Temp  |
|  🔸Claude   |                   |   lib   |
|  🔹Gemini   |  [Editor dengan   |         |
|  🔸MJ       |   syntax highlight]| Variab  |
|  🔹DALL-E   |                   |  le     |
|             |  Variables:       |  list   |
|  [Search]   |  {nama} [___]     |         |
|             |  {topik} [___]    |         |
| [Koleksi 1] |                   |         |
| [Koleksi 2] |  [Copy] [Use]     |         |
+------------------------------------------+
```

### Komponen Utama:

1. **PromptEditor:** Editor dengan syntax highlighting untuk `{{variable}}`, line numbers, word count, token counter
2. **VariableForm:** Input dinamis berdasarkan variabel template (text, select, textarea)
3. **PromptCard:** Kartu preview prompt dengan judul, kategori, tags, copy button
4. **CategoryTree:** Tree navigasi kategori dengan jumlah prompt
5. **TemplateLibrary:** Grid template prompt yang bisa difilter dan dicari
6. **HistoryPanel:** Riwayat prompt yang sudah digunakan dengan pencarian

---

## 6. Interactions & States

**Flow Utama:**
1. User pilih kategori atau cari template
2. Template dimuat ke editor
3. User isi variable (auto-detect dari `{{...}}`)
4. Preview real-time prompt yang sudah diisi
5. Copy ke clipboard → notifikasi sukses
6. Otomatis tersimpan ke history

**States:**
- **Loading:** Skeleton untuk template list
- **Empty:** "Belum ada template. Buat template baru!" untuk kategori kosong
- **Error:** Gagal memuat template — "Gagal memuat template. Coba refresh."
- **Edge Cases:** Template tanpa variabel, copy fail, history kosong, pencarian tanpa hasil

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- **UI Labels:** Semua dalam Bahasa Indonesia
- **Model Names:** Tetap menggunakan nama asli (ChatGPT, Claude, Gemini, dll)
- **Format:** Tanggal DD/MM/YYYY, waktu 24h

---

## 8. Example Mock Data

```typescript
export const mockCategories: PromptCategory[] = [
  { id: 'chatgpt', name: 'ChatGPT', icon: 'sparkles', color: '#10A37F', description: 'Template untuk ChatGPT', promptCount: 24 },
  { id: 'claude', name: 'Claude', icon: 'bot', color: '#A855F7', description: 'Template untuk Claude AI', promptCount: 18 },
  { id: 'midjourney', name: 'Midjourney', icon: 'image', color: '#0D9488', description: 'Template prompt gambar', promptCount: 15 },
];

export const mockTemplates: PromptTemplate[] = [
  {
    id: 't1',
    title: 'Analisis SWOT',
    content: 'Analisis SWOT untuk {{company}} di industri {{industry}}.\n\nKekuatan:\n- \nKelemahan:\n- \nPeluang:\n- \nAncaman:\n- ',
    categoryId: 'chatgpt',
    tags: ['bisnis', 'analisis', 'strategi'],
    variables: [
      { key: 'company', label: 'Nama Perusahaan', type: 'text', defaultValue: '', required: true, placeholder: 'PT Maju Jaya' },
      { key: 'industry', label: 'Industri', type: 'text', defaultValue: '', required: true, placeholder: 'Teknologi' },
    ],
    author: 'Admin',
    version: '1.0',
    isPublic: true,
    usageCount: 142,
    createdAt: '2025-01-15',
    updatedAt: '2025-06-01',
  },
];
```

---

## 9. Success Criteria

- [ ] ✅ Prompt editor dengan syntax highlighting untuk {{variable}}
- [ ] ✅ Template library dengan filter kategori dan search
- [ ] ✅ Variable auto-detect dan form input dinamis
- [ ] ✅ Preview real-time prompt yang sudah diisi
- [ ] ✅ Copy to clipboard dengan notifikasi
- [ ] ✅ History prompt yang sudah digunakan
- [ ] ✅ Kategori prompt untuk berbagai model AI
- [ ] ✅ Dark mode default
- [ ] ✅ Responsive design (desktop + tablet)
- [ ] ✅ TIDAK ADA backend calls
