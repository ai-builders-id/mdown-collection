# FRD: CV Extractor — Functional Requirements

**Project:** CV Extractor
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk CV Extractor — aplikasi ekstraksi data CV ke structured data untuk HR Recruiter.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Upload CV

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Drag & drop file area dengan animasi visual | P0 |
| FRD-002 | File picker untuk memilih file dari sistem (PDF, DOCX, TXT) | P0 |
| FRD-003 | Validasi tipe file (hanya PDF/DOCX/TXT, max 10MB) | P0 |
| FRD-004 | Menampilkan nama file setelah dipilih | P0 |
| FRD-005 | Tombol "Proses" untuk memulai ekstraksi | P0 |

### 2.2 Modul Mock CV Library

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-006 | Dropdown untuk memilih CV dari library mock | P0 |
| FRD-007 | Menampilkan informasi singkat CV yang dipilih (nama, posisi) | P1 |
| FRD-008 | Langsung populate hasil ekstraksi tanpa loading | P1 |

### 2.3 Modul Ekstraksi & Preview

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-009 | Menampilkan ringkasan profil kandidat | P0 |
| FRD-010 | Menampilkan data pribadi (nama, email, telepon, alamat, LinkedIn) | P0 |
| FRD-011 | Menampilkan riwayat pengalaman kerja dengan timeline | P0 |
| FRD-012 | Menampilkan riwayat pendidikan | P0 |
| FRD-013 | Menampilkan daftar keahlian sebagai tag/chip | P0 |
| FRD-014 | Menampilkan daftar sertifikasi | P1 |
| FRD-015 | Menampilkan kemampuan bahasa | P1 |

### 2.4 Modul Export

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-016 | Export hasil ekstraksi ke file JSON | P1 |
| FRD-017 | Copy JSON ke clipboard | P2 |
| FRD-018 | Export ke CSV | P2 |
| FRD-019 | Raw JSON preview (collapsible panel) | P1 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Ekstraksi CV dari Upload File

- **Aktor:** HR Recruiter
- **Pre-condition:** User memiliki file CV dalam format PDF/DOCX/TXT
- **Post-condition:** Data CV berhasil diekstrak dan ditampilkan di panel hasil
- **Alur Normal:**
  1. User membuka halaman CV Extractor
  2. User drag & drop file CV ke area upload (atau klik untuk memilih file)
  3. Sistem menampilkan nama file dan tombol "Proses" menjadi aktif
  4. User klik "Proses"
  5. Sistem menampilkan loading state dengan progress bar simulasi (2-3 detik)
  6. Hasil ekstraksi muncul di panel kanan dengan semua field terisi
  7. User dapat scroll dan review hasil
- **Alur Alternatif:**
  - A1: User memilih CV dari mock library dropdown — hasil langsung muncul tanpa loading
  - A2: User klik "Reset" — area upload kembali kosong, panel hasil bersih
- **Exception:**
  - E1: File > 10MB — tampilkan error "Ukuran file maksimal 10MB"
  - E2: Format file tidak didukung — tampilkan error "Format file {{format}} tidak didukung. Gunakan PDF, DOCX, atau TXT"

### UC-02: Export Hasil Ekstraksi

- **Aktor:** HR Recruiter
- **Pre-condition:** Data CV sudah berhasil diekstrak dan ditampilkan
- **Post-condition:** File JSON terdownload ke sistem user
- **Alur Normal:**
  1. User klik tombol "Export" di panel hasil
  2. Dropdown menu muncul dengan opsi: JSON, CSV
  3. User pilih "Export JSON"
  4. File JSON terdownload dengan nama {{nama_kandidat}}_cv_extracted.json
  5. Toast sukses "Data berhasil diexport"
- **Alur Alternatif:**
  - A1: User pilih "Copy JSON" — data tercopy ke clipboard, toast "Data tercopy ke clipboard"
- **Exception:**
  - E1: Belum ada data — tombol export disabled

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | Satu CV per sesi | Hanya satu CV bisa diproses dalam satu waktu. Upload baru mereset hasil sebelumnya |
| BR-002 | Prioritas mock library | Jika user pilih CV dari mock library, hasil langsung tampil tanpa loading |
| BR-003 | Reset otomatis | Upload file baru akan mereset panel hasil dan menampilkan loading baru |
| BR-004 | Validasi file | Hanya PDF, DOCX, dan TXT yang diperbolehkan. Ukuran maksimal 10MB |

---

## 5. Integrasi & Data Flow

```text
[Upload Area] --> [File Validator] --> [Mock Parser Engine]
                                                |
                    [Mock CV Library] --> [Mock Parser Engine]
                                                |
                                          [Result Store]
                                                |
                                    +-----------+-----------+
                                    |                       |
                              [Preview Panel]        [Export Module]
```

---

## 6. Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa loading | Simulasi parsing < 3 detik |
| NFR-002 | Responsive | Desktop + Tablet + Mobile |
| NFR-003 | Aksesibilitas | WCAG 2.1 AA, keyboard navigable |
| NFR-004 | Bahasa | Bahasa Indonesia (id) |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| Upload CV drag & drop | High | Low | P0 |
| Ekstraksi data utama | High | Low | P0 |
| Mock CV Library | High | Low | P0 |
| Preview hasil terstruktur | High | Medium | P0 |
| Export JSON | Medium | Low | P1 |
| Skill radar chart | Low | Medium | P2 |
| Export CSV | Medium | Low | P2 |
