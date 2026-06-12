# {{PROJECT_NAME}} — Prompt Collection

Koleksi prompt dokumentasi PRD, BRD, FRD, dan TRD untuk berbagai domain aplikasi bisnis.
Setiap folder berisi template lengkap dengan `{{VARIABLES}}` yang bisa kamu kustomisasi.

---

## Daftar Domain

### 💰 Accounting & Finance
| Folder | Deskripsi |
|--------|-----------|
| [accounting/](./accounting/) | Aplikasi akuntansi double-entry — chart of accounts, jurnal, buku besar, laporan keuangan |
| [erp/](./erp/) | Enterprise Resource Planning — finance, inventory, HR, operasional |
| [personal-finance-tracker/](./personal-finance-tracker/) | Pencatatan keuangan pribadi — budgeting, income, expenses |
| [point-of-sales/](./point-of-sales/) | Point of Sales untuk toko retail — kasir, diskon, receipt, stok |
| [procurement/](./procurement/) | Procurement — purchase orders, approvals, supplier tracking |

### 🤖 AI & Communication
| Folder | Deskripsi |
|--------|-----------|
| [ai-chat-bot/](./ai-chat-bot/) | AI Chatbot dengan knowledge base dan custom knowledge |
| [customer-support-helpdesk/](./customer-support-helpdesk/) | Customer Support & Helpdesk — tiket, prioritas, workflow agent |
| [chat-ai-generator/](./chat-ai-generator/) | Chat AI Prompt Generator — template, kategori, riwayat prompt |

### 📊 Business & CRM
| Folder | Deskripsi |
|--------|-----------|
| [crm/](./crm/) | CRM — kontak, deals, pipeline penjualan, aktivitas |
| [business-intelligence/](./business-intelligence/) | Business Intelligence — dashboard, visualisasi, laporan interaktif |
| [e-commerce/](./e-commerce/) | E-Commerce — produk, keranjang, checkout, pesanan |
| [social-media-analytics/](./social-media-analytics/) | Social Media Analytics — follower growth, engagement, reach |

### 👥 HR & Education
| Folder | Deskripsi |
|--------|-----------|
| [hrm/](./hrm/) | Human Resource Management — rekrutmen, payroll, onboarding |
| [cv-extractor/](./cv-extractor/) | CV Extractor — parsing CV ke structured data |
| [learning-management-system/](./learning-management-system/) | Learning Management System — kursus, progress, sertifikasi |
| [notes-app/](./notes-app/) | Notes App — catatan markdown, organisasi folder, tag |

### 🏥 Healthcare & Compliance
| Folder | Deskripsi |
|--------|-----------|
| [healthcare-emr/](./healthcare-emr/) | Healthcare EMR — catatan pasien, diagnosis, resep |
| [grc/](./grc/) | GRC — Governance, Risk, Compliance tracking |

### 📦 Operations & Supply Chain
| Folder | Deskripsi |
|--------|-----------|
| [supply-chain-management/](./supply-chain-management/) | Supply Chain Management — inventory, pengiriman, tracking |
| [enterprise-asset-management/](./enterprise-asset-management/) | Enterprise Asset Management — tracking aset fisik |
| [vehicle-fleet-management/](./vehicle-fleet-management/) | Vehicle Fleet Management — armada, fuel, maintenance |
| [document-management-system/](./document-management-system/) | Document Management System — filing, search, versioning |
| [real-estate-management/](./real-estate-management/) | Real Estate Management — properti, penyewa, kontrak sewa |
| [project-management-collab/](./project-management-collab/) | Project Management & Collab — tasks, deadlines, tim |
| [content-management-system/](./content-management-system/) | Content Management System — blog, pages, publishing |

---

## Cara Menggunakan

### 1. Pilih folder domain yang sesuai

Copy folder domain ke project kamu:
```bash
cp -r accounting/ project-anda/
```

### 2. Ganti semua `{{VARIABLES}}`

Lihat daftar lengkap variabel di [_TEMPLATE/VARIABLE_REGISTRY.md](./_TEMPLATE/VARIABLE_REGISTRY.md).

**Quick replace dengan PowerShell:**
```powershell
Get-ChildItem -Recurse -Filter *.md | ForEach-Object {
  (Get-Content $_.FullName -Raw) `
    -replace '\{\{PROJECT_NAME\}\}','NamaProjectAnda' `
    -replace '\{\{PROJECT_SLUG\}\}','nama-project' |
  Set-Content $_.FullName -Encoding utf8
}
```

### 3. Setiap folder berisi 4 file

| File | Isi |
|------|-----|
| `prd.md` | Product Requirements Document — deskripsi produk, fitur, data structure, UI spec |
| `brd.md` | Business Requirements Document — tujuan bisnis, target pasar, KPI |
| `frd.md` | Functional Requirements Document — requirement detail, use case, aturan bisnis |
| `trd.md` | Technical Requirements Document — tech stack, arsitektur, deployment |

### 4. Stack default

Semua template menggunakan **Vite + React + TypeScript + Tailwind CSS** dengan mock data (no backend).

---

## Template Referensi

- [_TEMPLATE/](./_TEMPLATE/) — Template kosong untuk semua dokumen
- [_TEMPLATE/VARIABLE_REGISTRY.md](./_TEMPLATE/VARIABLE_REGISTRY.md) — Daftar semua variabel
- [landing page/](./landing%20page/) — Landing page design briefs

---

## Lisensi

Template ini bebas digunakan dan dimodifikasi untuk project apapun.
