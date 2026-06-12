# Variable Registry — {{VARIABLES}} Reference

Setiap dokumen template di repositori ini menggunakan **{{PLACEHOLDER}}** tokens yang harus diganti sesuai proyek kamu.  
Gunakan tabel di bawah untuk mencari dan mengganti semua variabel.

---

## Wajib Diisi (Minimum)

Daftar variabel yang HARUS kamu ganti sebelum menggunakan template ini:

| Token | Yang Harus Diisi | Contoh |
|-------|------------------|--------|
| `{{PROJECT_NAME}}` | Nama produk / aplikasi | `SiAkuntan` |
| `{{PROJECT_SLUG}}` | Slug untuk CLI / package / URL | `siakuntan` |
| `{{PROJECT_DESCRIPTION}}` | Deskripsi singkat (1 kalimat) | `Aplikasi akuntansi berbasis web untuk UKM` |
| `{{TARGET_USER}}` | User persona utama | `Akuntan UKM` / `Manajer HR` / `Admin` |
| `{{AESTHETIC}}` | Arah visual / desain | `Professional, clean, modern SaaS` |
| `{{PRIMARY_COLOR}}` | Warna primer (CSS atau hex) | `#4F46E5 (Indigo)` |
| `{{STACK_FRONTEND}}` | Framework / library frontend | `Vite + React + TypeScript + Tailwind CSS` |
| `{{STACK_BACKEND}}` | Backend (opsional, default "mock") | `Mock data, no backend` |
| `{{LANGUAGE}}` | Bahasa lokal | `Bahasa Indonesia` |
| `{{LOCALE}}` | Locale code | `id` / `en-US` / `ms-MY` |

## Opsional (Sesuai Kebutuhan)

| Token | Yang Harus Diisi | Contoh |
|-------|------------------|--------|
| `{{COMPANY_NAME}}` | Nama perusahaan / klien | `PT Maju Jaya` |
| `{{CURRENCY}}` | Mata uang utama | `IDR` / `USD` / `MYR` |
| `{{CURRENCY_SYMBOL}}` | Simbol mata uang | `Rp` / `$` / `RM` |
| `{{DATE_FORMAT}}` | Format tanggal | `DD/MM/YYYY` |
| `{{ICON_PACK}}` | Library ikon | `Lucide React` / `Phosphor Icons` |
| `{{CHART_LIBRARY}}` | Library chart | `Recharts` / `Chart.js` / `ApexCharts` |
| `{{UI_LIBRARY}}` | Component library | `shadcn/ui` / `Mantine` / `Ant Design` |
| `{{ANIMATION_LIB}}` | Library animasi | `Framer Motion` / `GSAP` |

## Tabel Mapping Per Domain

| Domain Folder | `{{PROJECT_NAME}}` | `{{PROJECT_SLUG}}` | `{{PRIMARY_COLOR}}` |
|--------------|-------------------|--------------------|--------------------|
| `accounting/` | Accounting | accounting | `#2563EB` (Blue) |
| `ai-chat-bot/` | AI Chatbot | ai-chatbot | `#4F46E5` (Indigo) |
| `business-intelligence/` | Business Intelligence | bi-dashboard | `#059669` (Emerald) |
| `content-management-system/` | Content Management System | cms | `#DC2626` (Red) |
| `crm/` | CRM | crm | `#2563EB` (Blue) |
| `cv-extractor/` | CV Extractor | cv-extractor | `#7C3AED` (Violet) |
| `customer-support-helpdesk/` | Customer Support & Helpdesk | helpdesk | `#0891B2` (Cyan) |
| `document-management-system/` | Document Management System | dms | `#D97706` (Amber) |
| `e-commerce/` | E-Commerce | ecommerce | `#E11D48` (Rose) |
| `enterprise-asset-management/` | Enterprise Asset Management | eam | `#65A30D` (Lime) |
| `erp/` | ERP | erp | `#1D4ED8` (Blue) |
| `grc/` | GRC (Governance, Risk, Compliance) | grc | `#78716C` (Stone) |
| `healthcare-emr/` | Healthcare EMR | healthcare-emr | `#0D9488` (Teal) |
| `hrm/` | Human Resource Management | hrm | `#8B5CF6` (Purple) |
| `learning-management-system/` | Learning Management System | lms | `#F59E0B` (Amber) |
| `personal-finance-tracker/` | Personal Finance Tracker | finance-tracker | `#10B981` (Emerald) |
| `point-of-sales/` | Point of Sales | pos | `#F97316` (Orange) |
| `procurement/` | Procurement | procurement | `#6366F1` (Indigo) |
| `project-management-collab/` | Project Management & Collab | pm | `#EC4899` (Pink) |
| `real-estate-management/` | Real Estate Management | real-estate | `#84CC16` (Lime) |
| `social-media-analytics/` | Social Media Analytics | social-analytics | `#F43F5E` (Rose) |
| `supply-chain-management/` | Supply Chain Management | scm | `#14B8A6` (Teal) |
| `vehicle-fleet-management/` | Vehicle Fleet Management | fleet | `#EAB308` (Yellow) |
| `chat-ai-generator/` | Chat AI Prompt Generator | chat-ai-gen | `#A855F7` (Purple) |

---

## Cara Penggunaan

### 1. Copy folder domain yang kamu butuhkan
```bash
cp -r accounting/ project-anda/
```

### 2. Ganti semua {{VARIABLES}}
Gunakan search-and-replace di seluruh folder:

**PowerShell:**
```powershell
Get-ChildItem -Recurse -Filter *.md | ForEach-Object {
  (Get-Content $_.FullName -Raw) `
    -replace '\{\{PROJECT_NAME\}\}','SiAkuntan' `
    -replace '\{\{PROJECT_SLUG\}\}','siakuntan' `
    -replace '\{\{TARGET_USER\}\}','Akuntan UKM' |
  Set-Content $_.FullName -Encoding utf8
}
```

**VS Code:** Cari `\{\{(.*?)\}\}` dengan regex → Replace All dengan nilai yang sesuai.

### 3. Hapus template yang tidak diperlukan

### 4. Mulai build prototipe
