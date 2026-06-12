# FRD: GRC — Functional Requirements

**Project:** GRC (Governance Risk Compliance)
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk GRC (Governance Risk Compliance) — aplikasi manajemen tata kelola, risiko, dan kepatuhan untuk Risk & Compliance Officer.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Risk Register

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Tambah risiko baru dengan form lengkap (nama, deskripsi, kategori, dampak, probabilitas) | P0 |
| FRD-002 | Perhitungan skor risiko otomatis (dampak x probabilitas) | P0 |
| FRD-003 | Tampilan kanban 5 kolom (teridentifikasi, dinilai, dimitigasi, dimonitor, ditutup) | P0 |
| FRD-004 | Drag & drop risk card antar kolom kanban | P0 |
| FRD-005 | Edit detail risiko dari card | P0 |
| FRD-006 | Filter risiko berdasarkan departemen, kategori, skor minimal | P0 |
| FRD-007 | Risk heatmap matriks 5x5 untuk visualisasi | P0 |
| FRD-008 | Search risiko berdasarkan kode atau nama | P1 |
| FRD-009 | Riwayat perubahan status risiko | P2 |

### 2.2 Modul Compliance Tracker

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-010 | Daftar compliance items dengan tabel (kode, nama, regulasi, status, tenggat) | P0 |
| FRD-011 | Tambah compliance item baru | P0 |
| FRD-012 | Status badge: patuh (hijau), sebagian (kuning), tidak patuh (merah) | P0 |
| FRD-013 | Filter berdasarkan departemen, kategori (internal/eksternal), frekuensi | P0 |
| FRD-014 | Update status kepatuhan | P0 |
| FRD-015 | Notifikasi tenggat compliance dalam 30 hari | P1 |
| FRD-016 | Grafik kepatuhan per departemen (pie chart) | P1 |

### 2.3 Modul Audit Temuan

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-017 | Tambah temuan audit baru (judul, deskripsi, severity, area, auditee) | P0 |
| FRD-018 | Tentukan severity: rendah, sedang, tinggi, kritis (dengan warna) | P0 |
| FRD-019 | Track tindak lanjut dan tenggat perbaikan | P0 |
| FRD-020 | Update status temuan (terbuka, ditindaklanjuti, selesai, ditutup) | P0 |
| FRD-021 | Daftar temuan dengan filter severity dan status | P1 |

### 2.4 Modul Kebijakan

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-022 | Daftar kebijakan dengan nomor dokumen, versi, status, departemen | P1 |
| FRD-023 | Upload dokumen kebijakan (mock file) | P2 |
| FRD-024 | Riwayat versi kebijakan | P2 |
| FRD-025 | Status kadaluarsa alert untuk kebijakan yang perlu direview | P2 |

### 2.5 Modul Dashboard

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-026 | Total risiko, risiko kritis, risiko tinggi | P0 |
| FRD-027 | Total compliance items, tingkat kepatuhan % | P0 |
| FRD-028 | Temuan audit terbuka per severity | P0 |
| FRD-029 | Grafik tren risiko per kuartal | P1 |
| FRD-030 | Notifikasi: tenggat compliance, temuan baru | P1 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Melakukan Risk Assessment

- **Aktor:** Risk & Compliance Officer
- **Pre-condition:** User memiliki akses ke Risk Register
- **Post-condition:** Risiko baru tercatat dengan skor dan status teridentifikasi
- **Alur Normal:**
  1. User buka halaman Risk Register → melihat kanban board
  2. User klik "Tambah Risiko" → modal form muncul
  3. User isi: nama risiko, deskripsi, kategori, departemen, pemilik
  4. User pilih tingkat dampak dan probabilitas dari dropdown (5 level)
  5. Skor otomatis terhitung dan ditampilkan (misal: dampak=tinggi(4) x probabilitas=sedang(3) = 12)
  6. User isi rencana mitigasi dan tenggat
  7. User klik "Simpan" → risk card muncul di kolom "Teridentifikasi"
- **Alur Alternatif:**
  - A1: User drag card ke kolom "Dinilai" → status berubah, history tercatat
  - A2: User klik card → panel detail terbuka (modal/overlay)
- **Exception:**
  - E1: Field wajib kosong — validasi error, field di-highlight
  - E2: Tenggat sudah lewat — peringatan kuning "Tenggat sudah terlewati"

### UC-02: Update Compliance Status

- **Aktor:** Risk & Compliance Officer
- **Pre-condition:** Compliance item sudah terdaftar
- **Post-condition:** Status compliance terupdate
- **Alur Normal:**
  1. User buka Compliance Tracker
  2. User cari compliance item via filter atau search
  3. User klik tombol "Update Status" pada baris item
  4. Dropdown muncul: patuh, sebagian, tidak patuh
  5. User pilih status dan tambahkan catatan
  6. Sistem update, badge berubah warna sesuai status
- **Alur Alternatif:**
  - A1: User buka detail compliance item → melihat riwayat update

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | Skor risiko | Skor = dampak x probabilitas. Range: 1-25 (5x5 matrix) |
| BR-002 | Risk critical threshold | Risiko dengan skor >= 15 otomatis masuk kategori "high priority" |
| BR-003 | Status compliance | Jika tenggat terlewat dan status bukan "patuh", otomatis menjadi "tidak_patuh" |
| BR-004 | Audit severity escalation | Temuan severity "kritis" harus memiliki tenggat perbaikan <= 30 hari |
| BR-005 | Duplicate risk check | Sistem memeriksa apakah risiko serupa sudah ada (berdasarkan nama/kategori/departemen) |

---

## 5. Integrasi & Data Flow

```text
[Dashboard] <---> [Risk Register] <---> [Compliance Tracker]
                       ^                       ^
                       |                       |
                 [Audit Temuan]          [Kebijakan]
                       |                       |
                       +-----------+-----------+
                                   |
                            [Laporan Module]

Data Flow:
1. Risk baru -> Update dashboard statistik
2. Compliance update -> Notifikasi jika tidak patuh
3. Temuan audit -> Dapat memicu identifikasi risiko baru
4. Dashboard -> Agregasi dari semua modul
```

---

## 6. Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa loading | < 2 detik untuk kanban dengan 25+ risk cards |
| NFR-002 | Responsive | Desktop + Tablet |
| NFR-003 | Aksesibilitas | WCAG 2.1 AA |
| NFR-004 | Bahasa | Bahasa Indonesia (id) |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| Risk Register | High | Medium | P0 |
| Risk Heatmap | High | Low | P0 |
| Kanban Board | High | Medium | P0 |
| Compliance Tracker | High | Medium | P0 |
| Dashboard GRC | High | Low | P0 |
| Audit Temuan | Medium | Medium | P1 |
| Laporan Export | Medium | Medium | P1 |
| Kebijakan Repository | Low | Medium | P2 |
