<img src="https://img.shields.io/badge/status-active-success?style=for-the-badge" alt="Status: Active"/>
<img src="https://img.shields.io/badge/domains-25-blue?style=for-the-badge" alt="25 Domains"/>
<img src="https://img.shields.io/badge/templates-100%2B-orange?style=for-the-badge" alt="100+ Templates"/>
<img src="https://img.shields.io/badge/language-Indonesia-brightgreen?style=for-the-badge" alt="Bahasa Indonesia"/>

<br/>

# 📋 Koleksi Template PRD · BRD · FRD · TRD

### _Blueprint aplikasi bisnis siap pakai — tinggal ganti `{{variable}}`, langsung prototipe._

<br/>

```text
╔══════════════════════════════════════════════════════════════╗
║                                                              ║
║    25 DOMAIN   100+ FILE   BAHASA INDONESIA   {{VARIABLE}}   ║
║                                                              ║
║    Format: PRD - Domain.md · BRD - Domain.md                 ║
║            FRD - Domain.md · TRD - Domain.md                 ║
║                                                              ║
║    Stack: Vite + React + TypeScript + Tailwind CSS           ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

<br/>

---

## 🚀 Mulai Dalam 30 Detik

```bash
# 1. Salin folder domain yang kamu butuh
cp -r e-commerce/ project-kamu/

# 2. Ganti variabel (PowerShell)
Get-ChildItem -Recurse -Filter *.md | ForEach-Object {
  (Get-Content $_.FullName -Raw) `
    -replace '\{\{PROJECT_NAME\}\}','TokoImpian' `
    -replace '\{\{PROJECT_SLUG\}\}','toko-impian'
  | Set-Content $_.FullName -Encoding utf8
}

# 3. Selesai! Mulai prototipe 🎉
```

> 🔍 **Cari variabel lain?** Lihat panduan lengkap di [`_TEMPLATE/VARIABLE_REGISTRY.md`](_TEMPLATE/VARIABLE_REGISTRY.md)

---

## 📦 Semua Domain

<br/>

### 💰 Accounting & Finance

| Folder | Tentang | File |
|--------|---------|:----:|
| [`accounting/`](accounting/) | Akuntansi double-entry — chart of accounts, jurnal, buku besar, laporan keuangan | 🅿 🅱 🅵 🆃 |
| [`erp/`](erp/) | ERP all-in-one — finance, inventory, HR, operasional | 🅿 🅱 🅵 🆃 |
| [`personal-finance-tracker/`](personal-finance-tracker/) | Keuangan pribadi — budgeting, income, expenses, savings | 🅿 🅱 🅵 🆃 |
| [`point-of-sales/`](point-of-sales/) | POS toko retail — kasir, diskon, receipt, stok | 🅿 🅱 🅵 🆃 |
| [`procurement/`](procurement/) | Pengadaan barang — PO, approvals, supplier tracking | 🅿 🅱 🅵 🆃 |

### 🤖 AI & Communication

| Folder | Tentang | File |
|--------|---------|:----:|
| [`ai-chat-bot/`](ai-chat-bot/) | AI Chatbot dengan knowledge base & custom knowledge | 🅿 🅱 🅵 🆃 |
| [`customer-support-helpdesk/`](customer-support-helpdesk/) | Helpdesk & tiket — prioritas, workflow agent | 🅿 🅱 🅵 🆃 |
| [`chat-ai-generator/`](chat-ai-generator/) | Prompt generator AI — template, kategori, riwayat | 🅿 🅱 🅵 🆃 |

### 📊 Business & CRM

| Folder | Tentang | File |
|--------|---------|:----:|
| [`crm/`](crm/) | CRM — kontak, deals, pipeline penjualan, aktivitas | 🅿 🅱 🅵 🆃 |
| [`business-intelligence/`](business-intelligence/) | BI Dashboard — visualisasi data, laporan interaktif | 🅿 🅱 🅵 🆃 |
| [`e-commerce/`](e-commerce/) | Toko online — produk, keranjang, checkout, pesanan | 🅿 🅱 🅵 🆃 |
| [`social-media-analytics/`](social-media-analytics/) | Analitik medsos — follower growth, engagement, reach | 🅿 🅱 🅵 🆃 |

### 👥 HR & Education

| Folder | Tentang | File |
|--------|---------|:----:|
| [`hrm/`](hrm/) | HRM — rekrutmen, payroll, onboarding, offboarding | 🅿 🅱 🅵 🆃 |
| [`cv-extractor/`](cv-extractor/) | Ekstraksi CV ke structured data otomatis | 🅿 🅱 🅵 🆃 |
| [`learning-management-system/`](learning-management-system/) | LMS — kursus, progress, sertifikasi | 🅿 🅱 🅵 🆃 |
| [`notes-app/`](notes-app/) | Catatan markdown — organisasi folder, tag, search | 🅿 🅱 🅵 🆃 |

### 🏥 Healthcare & Compliance

| Folder | Tentang | File |
|--------|---------|:----:|
| [`healthcare-emr/`](healthcare-emr/) | Rekam medis — pasien, diagnosis, resep, jadwal | 🅿 🅱 🅵 🆃 |
| [`grc/`](grc/) | Governance, Risk & Compliance tracking | 🅿 🅱 🅵 🆃 |

### 📦 Operations & Supply Chain

| Folder | Tentang | File |
|--------|---------|:----:|
| [`supply-chain-management/`](supply-chain-management/) | SCM — inventory, pengiriman, tracking status | 🅿 🅱 🅵 🆃 |
| [`enterprise-asset-management/`](enterprise-asset-management/) | Manajemen aset fisik perusahaan | 🅿 🅱 🅵 🆃 |
| [`vehicle-fleet-management/`](vehicle-fleet-management/) | Armada kendaraan — fuel, maintenance, driver | 🅿 🅱 🅵 🆃 |
| [`document-management-system/`](document-management-system/) | Dokumen digital — filing, search, versioning | 🅿 🅱 🅵 🆃 |
| [`real-estate-management/`](real-estate-management/) | Properti — penyewa, kontrak sewa, maintenance | 🅿 🅱 🅵 🆃 |
| [`project-management-collab/`](project-management-collab/) | Manajemen proyek — tasks, deadlines, kolaborasi tim | 🅿 🅱 🅵 🆃 |
| [`content-management-system/`](content-management-system/) | CMS — blog, pages, publishing tanpa coding | 🅿 🅱 🅵 🆃 |

> 🅿 = `PRD - Domain.md` · 🅱 = `BRD - Domain.md` · 🅵 = `FRD - Domain.md` · 🆃 = `TRD - Domain.md`

---

## 🧩 Yang Ada di Setiap Folder

```
📂 domain-kamu/
├── 📄 PRD - Domain.md  →  Product Requirements Document
│                           Layout, data structure (TypeScript),
│                           UI components, mock data, success criteria
│
├── 📄 BRD - Domain.md  →  Business Requirements Document
│                           Tujuan bisnis, target pasar, KPI,
│                           analisis kompetitor, rencana implementasi
│
├── 📄 FRD - Domain.md  →  Functional Requirements Document
│                           Modul & fitur (tabel prioritas P0/P1/P2),
│                           use case, aturan bisnis, data flow
│
└── 📄 TRD - Domain.md  →  Technical Requirements Document
                            Tech stack, component architecture,
                            CSS tokens, deployment, testing
```

---

## ✨ Kenapa Pake Template Ini?

| | |
|---|---|
| ✅ **25 domain** | Dari akuntansi sampai fleet management — semua sektor ter-cover |
| ✅ **Bahasa Indonesia** | Konten & mock data pake nama/value lokal (Budi, Sari, PT Maju Jaya, Rp) |
| ✅ **{{Variable}}** | Tinggal search-and-replace, langsung jadi |
| ✅ **TypeScript** | Data structure pake interface — siap implementasi |
| ✅ **Mock data** | Contoh konkret, tinggal pakai untuk prototipe |
| ✅ **100% frontend** | Vite + React + Tailwind — no backend required |

---

## 🛠️ Cara Pakai

| Langkah | Perintah |
|---------|----------|
| **Clone repo** | `git clone https://github.com/ai-builders-id/mdown-collection.git` |
| **Pilih domain** | `cp -r accounting/ proyek-baru/` |
| **Ganti variabel** | Search `{{` di VS Code → Replace All |
| **Build prototipe** | `npm create vite@latest` + paste template |

---

## 📖 Juga Tersedia

| Link | Untuk |
|------|-------|
| [`_TEMPLATE/`](_TEMPLATE/) | Template kosong — bikin domain kustom |
| [`_TEMPLATE/VARIABLE_REGISTRY.md`](_TEMPLATE/VARIABLE_REGISTRY.md) | Daftar semua variable + mapping warna |
| [`landing page/`](landing%20page/) | 12 design brief landing page siap build |

---

<br/>

<div align="center">

**Dibuat dengan ❤️ oleh [AI Builders ID](https://github.com/ai-builders-id)** · _Bangun produk impian, tanpa mulai dari nol._

</div>
