# FRD: Chat AI Prompt Generator — Functional Requirements

**Project:** Chat AI Prompt Generator
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk {{PROJECT_NAME}} — aplikasi generator prompt untuk AI chatbot yang membantu pengguna membuat, mengelola, dan menggunakan template prompt berkualitas.

---

## 2. Modul & Fitur

### 2.1 Modul Template Library

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-LIB-001 | Menampilkan daftar template prompt dalam grid/list | P0 |
| FRD-LIB-002 | Filter template berdasarkan kategori/model AI | P0 |
| FRD-LIB-003 | Search template berdasarkan judul, tags, konten | P0 |
| FRD-LIB-004 | Menampilkan detail template (judul, variable, tags) | P0 |
| FRD-LIB-005 | Sorting template (popular, terbaru, most used) | P1 |
| FRD-LIB-006 | Pagination/infinite scroll untuk template list | P1 |

### 2.2 Modul Prompt Editor

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-EDIT-001 | Text editor dengan syntax highlighting untuk `{{variable}}` | P0 |
| FRD-EDIT-002 | Auto-detect variable dari teks prompt | P0 |
| FRD-EDIT-003 | Real-time preview prompt hasil isi variable | P0 |
| FRD-EDIT-004 | Word count dan character count | P1 |
| FRD-EDIT-005 | Token estimation untuk berbagai model AI | P1 |
| FRD-EDIT-006 | Undo/redo dalam editor | P2 |

### 2.3 Modul Variable Manager

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-VAR-001 | Generate form input otomatis dari variable terdeteksi | P0 |
| FRD-VAR-002 | Validasi input required variable | P0 |
| FRD-VAR-003 | Default value untuk variable opsional | P1 |
| FRD-VAR-004 | Type-specific input (text, number, select, textarea) | P1 |
| FRD-VAR-005 | Variable history (nilai yang pernah diisi sebelumnya) | P2 |

### 2.4 Modul Copy & Export

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-COPY-001 | Copy prompt ke clipboard dengan satu klik | P0 |
| FRD-COPY-002 | Notifikasi sukses/gagal copy | P0 |
| FRD-COPY-003 | Export prompt ke file .txt | P1 |
| FRD-COPY-004 | Copy sebagai plain text atau markdown | P2 |

### 2.5 Modul History

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-HIST-001 | Menyimpan otomatis prompt yang sudah diisi | P1 |
| FRD-HIST-002 | Menampilkan riwayat prompt dengan timestamp | P1 |
| FRD-HIST-003 | Filter dan search riwayat prompt | P1 |
| FRD-HIST-004 | Favorite/unfavorite prompt | P1 |
| FRD-HIST-005 | Hapus riwayat individual atau bulk | P2 |

### 2.6 Modul Collections

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-COL-001 | Membuat koleksi prompt kustom | P1 |
| FRD-COL-002 | Menambah/menghapus prompt dari koleksi | P1 |
| FRD-COL-003 | Rename dan delete koleksi | P1 |
| FRD-COL-004 | Share koleksi (copy list) | P2 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Menggunakan Template Prompt

- **Aktor:** Pengguna
- **Pre-condition:** Template library sudah dimuat
- **Post-condition:** Prompt siap digunakan
- **Alur Normal:**
  1. Pengguna membuka halaman utama
  2. Sistem menampilkan template library
  3. Pengguna memilih kategori atau mencari template
  4. Sistem menampilkan daftar template yang sesuai
  5. Pengguna klik template yang diinginkan
  6. Sistem memuat template ke editor
  7. Sistem auto-detect variable dan generate form input
  8. Pengguna mengisi variable
  9. Preview prompt terupdate real-time
  10. Pengguna klik Copy
  11. Sistem copy ke clipboard dan tampilkan notifikasi
  12. Riwayat otomatis tersimpan

### UC-02: Membuat Template Baru

- **Aktor:** Power user
- **Pre-condition:** User login (mock)
- **Alur Normal:**
  1. User klik "Buat Template Baru"
  2. Editor kosong terbuka
  3. User menulis prompt dengan `{{variable}}`
  4. User mengisi metadata (judul, kategori, tags)
  5. User klik "Simpan Template"
  6. Template muncul di library

---

## 4. Aturan Bisnis

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | Variable Format | Variable menggunakan `{{double_curly}}` syntax |
| BR-002 | Default Category | Template tanpa kategori masuk "Uncategorized" |
| BR-003 | History Limit | Riwayat maksimal 100 entri (circular buffer) |
| BR-004 | Copy Validation | Prompt hanya bisa di-copy jika semua required variable terisi |

---

## 5. Integrasi & Data Flow

```text
[Template Library] --> [Editor] --> [Variable Form] --> [Preview] --> [Copy/Export]
       |                                                              |
       +--> [History] <-----------------------------------------------+
```

---

## 6. Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa load library | < 1 detik |
| NFR-002 | Editor response | < 100ms typing latency |
| NFR-003 | Responsive | Desktop + Tablet + Mobile |
| NFR-004 | Offline support | PWA dengan service worker |
| NFR-005 | Bahasa | Bahasa Indonesia |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| Template library | High | Low | P0 |
| Prompt editor | High | Medium | P0 |
| Variable system | High | Medium | P0 |
| Copy to clipboard | High | Low | P0 |
| History | Medium | Medium | P1 |
| Collections | Medium | Medium | P1 |
| Export | Low | Low | P2 |
