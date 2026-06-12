# PRD: {{PROJECT_NAME}} — {{PROJECT_DESCRIPTION}}

**Project:** {{PROJECT_NAME}}
**Aesthetic:** {{AESTHETIC}}

---

## 1. Core Layout

Deskripsikan layout utama aplikasi. Struktur panel/kontainer dan bagaimana halaman tersusun.

*Contoh: Tiga panel — sidebar kiri (20%) untuk navigasi, panel tengah (55%) untuk konten utama, panel kanan (25%) untuk detail/aksi.*

---

## 2. Data Structure (Mock Only)

Tipe dan interface data yang digunakan untuk mock data. Gunakan TypeScript interfaces.

```typescript
// Contoh
export interface Item {
  id: string;
  name: string;
  description?: string;
  status: 'active' | 'inactive';
  createdAt: string;
  updatedAt: string;
}
```

---

## 3. Implementation Details

- **Frontend:** {{STACK_FRONTEND}}
- **Styling:** Tailwind CSS / CSS Modules
- **Icons:** {{ICON_PACK}}
- **Animation:** {{ANIMATION_LIB}}
- **Chart:** {{CHART_LIBRARY}}
- **UI Library:** {{UI_LIBRARY}}
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary {{PRIMARY_COLOR}}, dengan aksen dan netral yang sesuai.
- **Typography:** Font system-native atau font khusus.
- **Spacing:** Konsisten, gunakan utility classes.

---

## 5. UI Components Specification

Spesifikasi komponen UI utama beserta layout ASCII / notes.

```text
+--+------------------+--+
|  |                  |  |
|  |   Main Content   |  |
|S |                  |D |
|i |                  |e |
|d |                  |t |
|e |                  |a |
|b |                  |i |
|a |                  |l |
|r |                  |  |
|  |                  |  |
+--+------------------+--+
```

---

## 6. Interactions & States

Jelaskan flow interaksi utama dan semua state yang harus di-handle:

**States:**
- **Loading:** Skeleton/spinner saat memuat data
- **Empty:** Tampilan ketika belum ada data (sertakan copy)
- **Error:** Pesan error jika gagal memuat
- **Edge cases:** Data kosong, pencarian tanpa hasil, duplikasi

**Flow Interaksi utama:**
1. User membuka halaman → ...
2. User menekan tombol tambah → ...
3. User mengisi form → ...

---

## 7. Language & Localization

- **Bahasa:** {{LANGUAGE}} ({{LOCALE}})
- Semua UI labels, tombol, notifikasi, placeholder dalam Bahasa Indonesia.

---

## 8. Example Mock Data

```typescript
export const mockData = [
  {
    id: '1',
    // ... field sesuai tipe data
  },
];
```

---

## 9. Success Criteria

- [ ] ✅ Semua UI komponen render dengan mock data
- [ ] ✅ CRUD operations berfungsi (create, read, update, delete)
- [ ] ✅ Semantic search / filter bekerja
- [ ] ✅ Responsive design (desktop & mobile)
- [ ] ✅ Semua state handling (loading, empty, error) terimplementasi
- [ ] ✅ Bahasa Indonesia digunakan untuk semua label UI
- [ ] ✅ TIDAK ADA backend calls, TIDAK ADA API integration
