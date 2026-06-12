# BRD: DocuVault — Business Requirements

## 1. Latar Belakang & Konteks Bisnis
Perusahaan di Indonesia masih banyak yang menyimpan dokumen secara terpisah — di Google Drive, email, laptop pribadi, dan lemari fisik. Akibatnya, dokumen sulit ditemukan, versi kacau, dan kolaborasi tidak efektif. DocuVault hadir sebagai DMS yang terstruktur dengan folder tree, versioning, dan pencarian cepat, khusus untuk tim yang butuh organisasi dokumen yang rapi.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target |
|--------|--------|--------|
| Mempercepat pencarian dokumen | Rata-rata waktu menemukan dokumen | < 10 detik |
| Mengurangi duplikasi | % dokumen duplikat terdeteksi | < 2% |
| Meningkatkan kolaborasi | % dokumen dengan multi-kontributor | > 30% |
| Version control kepatuhan | % dokumen memiliki riwayat versi | 100% |
| Digitalisasi arsip | % dokumen fisik yang sudah digital | > 80% dalam 6 bulan |

## 3. Value Proposition
- **Folder Tree Hierarkis**: Organisasi dokumen intuitif seperti file explorer
- **Versioning Otomatis**: Setiap perubahan tersimpan sebagai versi, tidak perlu "Laporan_Final_v3_revisi2.docx"
- **Preview Tanpa Download**: Lihat PDF, markdown, gambar langsung di browser
- **Search Cepat**: Full-text search + filter tipe, tag, author, tanggal
- **Kolaborasi**: Comments, mentions, shared link

## 4. Target Pengguna

| Persona | Peran | Kebutuhan Utama |
|---------|-------|-----------------|
| Rina Amalia | Office Manager (35 th) | Arsip kontrak, SOP, surat menyurat |
| Dimas Aditya | Project Manager (38 th) | Dokumen project, notulensi, timeline |
| Nina Kusuma | Legal Staff (30 th) | Kontrak client, version control, expiry tracking |
| Pak Hartono | Direktur (60 th) | Akses cepat ke laporan tanpa bantuan staff |

## 5. Fitur Prioritas MVP

| Prioritas | Fitur |
|-----------|-------|
| P0 | Folder tree + CRUD folder |
| P0 | Upload & download dokumen |
| P0 | Grid/List view, sort, search |
| P1 | Preview inline (PDF, markdown, image) |
| P1 | Version history |
| P1 | Tag management |
| P1 | Star / pin favorites |
| P2 | Comments & mentions |
| P2 | Shared link (external access) |
| P2 | Full-text search |
| P2 | Drag-drop antar folder |

## 6. Model Distribusi
- **Web App**: Full-featured dashboard
- **PWA**: Akses mobile untuk upload via HP
- **Deployment**: Cloud (SaaS)
- **Pricing**:
  - Free: 100 dokumen, 5 folder, 2 user
  - Pro: 10.000 dokumen, unlimited folder, 20 user, versioning
  - Enterprise: Unlimited, SSO, custom retention policy

## 7. KPI & Metrik
- Time-to-find document: Target < 10 detik
- Document upload rate: > 200 dokumen/bulan/user aktif
- Version adoption: > 50% dokumen memiliki >1 versi
- User active rate: 70% login dalam 7 hari terakhir
- Storage efficiency: % pengurangan duplikasi

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan |
|------------|-----------|------------|
| **Google Drive** | Gratis, kolaborasi mudah | Tidak ada versioning proper, folder terbatas, tidak ada tag |
| **Dropbox** | Syncing bagus, file recovery | Harga mahal untuk storage besar |
| **Zoho Docs** | Fitur lengkap, harga murah | UI rumit, kadang lambat |
| **G-Drive / file server lokal** | Cepat, offline | Tidak ada search indexing, risk data loss |

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Data loss saat migrasi | Tinggi | Backup berkala, dry-run migrasi |
| User upload file berbahaya | Tinggi | Scan antivirus, validasi tipe file |
| Storage overflow | Sedang | Limit per user, upgrade plan, kompresi otomatis |
| Kehilangan akses karena lupa password | Rendah | Password reset via email, SSO option |

## 10. Rencana Implementasi
- **Phase 1 (Minggu 1-3)**: Folder tree, upload, list/grid view, search
- **Phase 2 (Minggu 4-6)**: Preview inline, version history, tag management
- **Phase 3 (Minggu 7-9)**: Comments, mentions, shared link, drag-drop
- **Phase 4 (Minggu 10-12)**: Full-text search, trash, restore, export
