# FRD: AI Chatbot — Functional Requirements

**Project:** AI Chatbot
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk AI Chatbot — aplikasi AI Chatbot dengan Knowledge Base dan Custom Knowledge untuk Karyawan Perusahaan.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Chat Interface

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Menampilkan daftar percakapan/sesi chat di sidebar | P0 |
| FRD-002 | Membuat sesi chat baru dengan satu klik | P0 |
| FRD-003 | Mengirim pesan teks melalui input chat | P0 |
| FRD-004 | Menampilkan gelembung pesan user (kanan) dan bot (kiri) | P0 |
| FRD-005 | Menampilkan indikator "sedang mengetik" dari bot | P0 |
| FRD-006 | Menampilkan sumber dokumen yang dirujuk dalam setiap jawaban bot | P0 |
| FRD-007 | Mencari riwayat percakapan berdasarkan judul | P1 |
| FRD-008 | Mengarsipkan sesi chat | P1 |
| FRD-009 | Menghapus sesi chat | P1 |
| FRD-010 | Mengekspor percakapan ke format teks | P2 |

### 2.2 Modul Knowledge Base Management

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-011 | Menampilkan daftar knowledge base yang tersedia | P0 |
| FRD-012 | Membuat knowledge base baru (nama, deskripsi, tipe) | P0 |
| FRD-013 | Menambah dokumen ke knowledge base (judul, konten, tags, kategori) | P0 |
| FRD-014 | Mengedit dokumen yang sudah ada | P0 |
| FRD-015 | Menghapus dokumen dari knowledge base | P0 |
| FRD-016 | Mencari dokumen dalam knowledge base berdasarkan judul/tags | P1 |
| FRD-017 | Menampilkan statistik knowledge base (jumlah dokumen, update terakhir) | P1 |
| FRD-018 | Mengelompokkan dokumen berdasarkan kategori | P1 |

### 2.3 Modul Konteks & Sumber

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-019 | Panel kanan menampilkan daftar sumber yang dirujuk oleh bot | P0 |
| FRD-020 | Setiap sumber menampilkan judul, skor relevansi, dan cuplikan | P0 |
| FRD-021 | Klik sumber untuk melihat detail konten dokumen | P1 |
| FRD-022 | Badge jumlah sumber pada setiap jawaban bot | P1 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Bertanya ke Knowledge Base

- **Aktor:** Karyawan Perusahaan
- **Pre-condition:** User sudah login, knowledge base sudah berisi dokumen
- **Post-condition:** User mendapat jawaban dengan sumber rujukan
- **Alur Normal:**
  1. User membuka aplikasi dan memulai sesi chat baru
  2. User mengetik pertanyaan di input chat dan menekan Send
  3. Sistem menampilkan typing indicator
  4. Sistem menampilkan jawaban bot lengkap dengan badge sumber
  5. Panel kanan menampilkan daftar sumber yang dirujuk
- **Alur Alternatif:**
  - A1: User memilih sesi chat lama dari history untuk melanjutkan percakapan
  - A2: User mengklik sumber untuk melihat cuplikan lengkap
- **Exception:**
  - E1: Knowledge base kosong — bot membalas "Belum ada dokumen dalam knowledge base. Silakan hubungi administrator."

### UC-02: Mengelola Knowledge Base

- **Aktor:** Karyawan Perusahaan (dengan akses admin/knowledge manager)
- **Pre-condition:** Knowledge base sudah ada
- **Post-condition:** Dokumen baru tersimpan dan bisa dirujuk oleh bot
- **Alur Normal:**
  1. User membuka halaman Knowledge Base dari sidebar
  2. User melihat daftar knowledge base dan memilih salah satu
  3. User mengklik "Tambah Dokumen"
  4. User mengisi form (judul, konten, tags, kategori)
  5. User menyimpan — dokumen muncul di daftar dokumen
- **Alur Alternatif:**
  - A1: User mengedit dokumen yang sudah ada
  - A2: User menghapus dokumen dengan konfirmasi
- **Exception:**
  - E1: Judul dokumen sudah ada — notifikasi duplikasi

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | Maksimum sesi chat | User dapat memiliki maksimal 50 sesi chat aktif. Sesi >50 akan mengarsipkan yang terlama. |
| BR-002 | Relevansi sumber | Bot hanya merujuk dokumen yang memiliki skor relevansi > 70% |
| BR-003 | Batas konten | Pesan maksimal 4000 karakter. Dokumen maksimal 50.000 karakter. |
| BR-004 | Simulasi respons | Bot menggunakan delay acak 1-3 detik sebagai simulasi pemrosesan AI |

---

## 5. Integrasi & Data Flow

```text
[Chat Interface] <--> [Chat Session Manager] <--> [Mock Data Layer]
       |
       v
[Knowledge Base Manager] <--> [Document Store (Mock)]
       |
       v
[Source References] <--> [Document Content]
```

---

## 6. Non-Functional Requirements (dari FRD)

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa loading | < 2 detik |
| NFR-002 | Responsive | Desktop + Tablet + Mobile |
| NFR-003 | Aksesibilitas | WCAG 2.1 AA |
| NFR-004 | Bahasa | Bahasa Indonesia (id) |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| Chat Interface dengan KB | High | Medium | P0 |
| CRUD Knowledge Base | High | Medium | P0 |
| Source References | High | Low | P0 |
| Riwayat Sesi Chat | Medium | Medium | P1 |
| Search Chat History | Medium | Low | P1 |
| Ekspor Percakapan | Low | Low | P2 |
| Dark Mode | Low | Medium | P2 |
