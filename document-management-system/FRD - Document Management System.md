# FRD: DocuVault — Functional Requirements

## 1. Scope
DocuVault adalah Document Management System untuk menyimpan, mengelola, dan mencari dokumen digital. Mencakup folder hierarkis, versioning, preview inline, tagging, dan pencarian. Target pengguna adalah tim dan perusahaan yang butuh organisasi dokumen yang rapi dan mudah diakses.

## 2. Modul & Fitur

| Modul | Fitur | Deskripsi | Prioritas |
|-------|-------|-----------|-----------|
| **Folder** | Tree View | Tampilan folder hierarkis expandable | P0 |
| **Folder** | CRUD Folder | Buat, rename, move, delete folder | P0 |
| **Folder** | Color Coding | Warna folder untuk identifikasi visual | P1 |
| **Dokumen** | Upload | Upload via button + drag-drop | P0 |
| **Dokumen** | Download | Download file original | P0 |
| **Dokumen** | Delete (Soft) | Pindah ke trash, restore dalam 30 hari | P1 |
| **Dokumen** | Move | Pindah folder via dropdown atau drag-drop | P1 |
| **View** | Grid/List Toggle | Beralih tampilan card grid atau tabel list | P0 |
| **View** | Sort | Sort by nama, tanggal, ukuran, tipe | P1 |
| **Preview** | PDF Preview | Render PDF inline | P1 |
| **Preview** | Markdown Preview | Render markdown ke HTML | P1 |
| **Preview** | Image Preview | Lightbox untuk image | P1 |
| **Version** | Version History | List versi, siapa upload, kapan, notes | P1 |
| **Version** | Upload New Version | Upload versi baru, otomatis increment | P1 |
| **Tag** | CRUD Tag | Buat/edit/hapus tag | P1 |
| **Tag** | Assign Tag | Tag dokumen, multi-tag per dokumen | P1 |
| **Tag** | Filter by Tag | Klik tag untuk filter | P1 |
| **Search** | Global Search | Pencarian judul, deskripsi, tag | P0 |
| **Search** | Advanced Filter | Filter tipe, folder, author, date range | P1 |
| **Favorites** | Star/Pin | Tandai dokumen favorit | P1 |
| **Trash** | Trash Folder | Lihat file terhapus, restore, permanent delete | P1 |

## 3. Use Case

**UC-01: Upload Dokumen Versi Baru**
1. User membuka dokumen yang sudah ada (misal: "Laporan Keuangan Q1 2025")
2. Klik tombol "Upload Versi Baru"
3. Sistem tampilkan form: pilih file, catatan perubahan (required)
4. User pilih file dari komputer, isi catatan "Update data Maret, perbaiki typo"
5. Sistem validasi: tipe file diizinkan, ukuran < 50MB
6. Sistem increment versi dari 2 ke 3, simpan file baru
7. Versi lama tetap tersimpan di history
8. Metadata updatedAt berubah, author tercatat sebagai pengupload
9. Notifikasi ke user yang pernah comment di dokumen tersebut (Phase 2)

**UC-02: Mencari Dokumen dengan Filter**
1. User klik search bar di header
2. Ketik kata kunci (misal: "kontrak")
3. Sistem tampilkan hasil real-time (debounce 300ms): judul, snippet, folder path
4. User klik "Filter" untuk refine: pilih tipe file = "PDF"
5. Filter lagi: pilih folder "Kontrak" dan tag "vendor"
6. Sistem update hasil dengan kombinasi filter
7. User klik dokumen hasil pencarian
8. Sistem navigasi ke folder lokasi dokumen + buka preview

## 4. Aturan Bisnis
- Satu folder tidak boleh memiliki nama duplikat di level yang sama (parent sama)
- Maksimum kedalaman folder: 10 level
- Tipe file yang diizinkan: pdf, docx, xlsx, pptx, txt, md, png, jpg, gif, svg
- Ukuran maksimum upload: 50MB (dapat dikonfigurasi)
- Soft delete: dokumen di trash otomatis permanent delete setelah 30 hari
- Hanya admin yang bisa permanent delete dari trash
- Versi tidak bisa dihapus — selalu tersimpan di history
- Tag bersifat global (bisa dipakai banyak dokumen)
- Satu dokumen bisa memiliki banyak tag

## 5. Integrasi & Data Flow
```
[Upload Drag-Drop] ──┐
[Button Upload] ─────┤──> DocuVault API ──> Object Storage ──> PostgreSQL Metadata
[Import Google Drive] ┘                          │
                                                 v
                                          [Search Indexer]
                                                 │
                                          [ElasticSearch / Fuse.js]
```

- **Storage**: File disimpan di object storage (S3/MinIO), metadata di PostgreSQL
- **Search**: Fuse.js untuk client-side search MVP, ElasticSearch untuk production
- **Preview**: PDF.js untuk PDF, react-markdown untuk markdown
- **Thumbnail**: Generate thumbnail otomatis untuk image

## 6. Non-Functional Requirements
- **Performance**: Folder tree load < 1 detik untuk 500 folder, search < 500ms
- **Availability**: 99.5% uptime
- **Storage**: Support hingga 10TB total storage (tergantung plan)
- **Concurrency**: 100 user upload simultan
- **Preview size**: PDF preview maks 50 halaman
- **Upload speed**: Chunked upload untuk file > 10MB

## 7. Prioritization Matrix

| Fitur | Value Bisnis | Effort | Priority |
|-------|-------------|--------|----------|
| Folder Tree | High | Medium | P0 |
| Upload | High | Low | P0 |
| Search | High | Medium | P0 |
| Preview | Medium | Medium | P1 |
| Versioning | High | Medium | P1 |
| Tags | Medium | Low | P1 |
| Comments | Low | Medium | P2 |
