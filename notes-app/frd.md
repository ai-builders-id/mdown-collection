# FRD: Catat.in — Functional Requirements

## 1. Scope
Catat.in adalah aplikasi catatan pribadi berbasis markdown dengan organisasi folder hierarkis dan tag system. Fitur utama: menulis catatan dalam markdown, mengorganisir dalam folder dan tag, mencari dengan cepat, dan akses offline. Target pengguna adalah individu yang membutuhkan catatan digital yang rapi, cepat, dan minimal.

## 2. Modul & Fitur

| Modul | Fitur | Deskripsi | Prioritas |
|-------|-------|-----------|-----------|
| **Notes** | Create Note | Buat catatan baru dengan judul + markdown konten | P0 |
| **Notes** | Edit Note | Edit konten markdown dengan live preview | P0 |
| **Notes** | Delete Note | Soft delete + trash (restore 7 hari) | P0 |
| **Notes** | Auto-save | Simpan otomatis setiap 30 detik + manual Ctrl+S | P0 |
| **Editor** | Markdown Editor | Textarea/CodeMirror dengan syntax | P0 |
| **Editor** | Live Preview | Side-by-side preview rendered HTML | P0 |
| **Editor** | Floating Toolbar | Toolbar mini muncul saat select text | P1 |
| **Editor** | Keyboard Shortcuts | Ctrl+B/I/H/K, Ctrl+P toggle preview | P1 |
| **Folder** | CRUD Folder | Buat, rename, delete folder + nested | P0 |
| **Folder** | Move Note | Pindah note antar folder (drag-drop di tree) | P1 |
| **Folder** | Emoji Icon | Pilih emoji untuk folder | P1 |
| **Tag** | CRUD Tag | Buat/edit/hapus tag dengan warna | P1 |
| **Tag** | Assign Tag | Assign/remove tag dari note (multi-tag) | P1 |
| **Tag** | Filter by Tag | Klik tag → filter notes | P1 |
| **Search** | Full-text Search | Cari judul + konten, highlight hasil | P0 |
| **Search** | Filter | Filter by folder + tag kombinasi | P1 |
| **Pin/Fav** | Pin Note | Pin note ke atas list | P1 |
| **Pin/Fav** | Favorite | Tandai favorit, tampilkan di tab terpisah | P1 |
| **Archive** | Archive | Arsipkan note, tidak muncul di main list | P1 |
| **Theme** | Dark Mode | Toggle dark/light/sepia | P1 |
| **History** | Version History | Simpan 10 versi terakhir per note | P2 |

## 3. Use Case

**UC-01: Membuat Catatan Baru dengan Markdown**
1. User klik tombol "+" atau tekan `Ctrl+N`
2. Sistem buka note baru di editor dengan judul "Catatan Baru" (placeholder)
3. User ketik judul di field judul
4. User mulai menulis konten dengan markdown:
   - `# Heading` untuk judul section
   - `- list item` untuk bullet list
   - `**bold**` dan `*italic*`
   - `` `code` `` untuk inline code
5. Panel preview di sebelah kanan langsung ter-render secara real-time
6. Setelah 30 detik, sistem auto-save (indicator "Tersimpan" muncul)
7. User dapat menambahkan tag dari sidebar kanan
8. User dapat memindahkan note ke folder dari dropdown folder di header

**UC-02: Mencari Catatan dengan Keyword**
1. User klik search bar atau tekan `Ctrl+F`
2. Ketik kata kunci (misal: "sprint planning")
3. Sistem lakukan full-text search di judul dan konten (Fuse.js)
4. Hasil muncul dalam < 200ms dengan judul, preview, dan kata kunci di-highlight
5. User dapat filter hasil dengan memilih folder tertentu atau tag
6. User klik salah satu hasil → sistem navigasi ke note tersebut
7. Kata kunci tetap ter-highlight di editor/preview setelah navigasi

## 4. Aturan Bisnis
- Judul note: 1-200 karakter, wajib diisi
- Satu folder tidak boleh memiliki nama yang sama di level parent yang sama
- Maksimum kedalaman folder: 5 level
- Tags bersifat global — satu tag bisa dipakai banyak note
- Satu note bisa memiliki maksimum 10 tags
- Note yang di-archive tidak muncul di main list, tapi tetap bisa dicari
- Soft delete: note di trash otomatis permanent delete setelah 7 hari
- Hanya 10 versi terakhir yang disimpan per note (FIFO)
- Auto-save hanya aktif saat ada perubahan

## 5. Integrasi & Data Flow
```
[Editor] ──> Auto-save timer (30s) ──> Zustand Store ──> localStorage
                                                              │
                                                              v
                                                        [IndexedDB Backup]
                                                              │
                                                     (Phase 2: Cloud Sync)
```

- **Storage**: Zustand persist ke localStorage untuk state cepat; IndexedDB untuk backup dan version history
- **Search**: Fuse.js di-client dengan seluruh array notes — inisialisasi saat load
- **Export**: `navigator.clipboard` untuk copy markdown; Blob download untuk .md file
- **No backend**: 100% client-side untuk MVP

## 6. Non-Functional Requirements
- **Performance**: App load < 2 detik, new note instant, search < 200ms (10.000 notes)
- **Offline**: 100% functional tanpa internet
- **Storage**: Local storage quota ~5-10MB untuk 10.000 notes (markdown ringan)
- **Concurrency**: Single user (personal app)
- **Responsive**: Desktop optimal, tablet support, mobile basic readable

## 7. Prioritization Matrix

| Fitur | Value User | Effort | Priority |
|-------|-----------|--------|----------|
| CRUD Note + Markdown | High | Medium | P0 |
| Live Preview | High | Low | P0 |
| Folder Tree | High | Medium | P0 |
| Search | High | Medium | P0 |
| Tags | Medium | Low | P1 |
| Dark Mode | Medium | Low | P1 |
| Version History | Low | Medium | P2 |
| Cloud Sync | Low | High | P2 |
