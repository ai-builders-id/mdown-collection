# TRD: E-Commerce — Technical Requirements

**Project:** E-Commerce
**Version:** 1.0

---

## 1. Technical Strategy

E-Commerce akan dibangun sebagai **static frontend boilerplate** dengan mock data. Arsitektur berfokus pada multi-page shopping experience dengan state management keranjang yang robust, multi-step checkout form, dan katalog produk interaktif.

```text
[Product Catalog] --> [Product Detail] --> [Cart Drawer]
      |                     |                   |
      v                     v                   v
[Filter/Search]      [Variant Selector]    [Cart State (Reducer)]
                                                  |
                                                  v
                                           [Checkout Steps]
                                                  |
                                                  v
                                           [Order Success]
```

---

## 2. Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Vite + React + TypeScript + Tailwind CSS |
| Styling | Tailwind CSS |
| Icons | Lucide React |
| Animations | Framer Motion |
| Charts | Recharts (admin sales analytics) |
| UI Library | shadcn/ui |
| Data | Mock JSON / TypeScript interfaces |
| Build | Vite |

---

## 3. Component Architecture

```text
src/
|-- components/
|   |-- ui/                    # shadcn/ui base components
|   |-- layout/
|   |   |-- Header.tsx
|   |   |-- Footer.tsx
|   |   |-- Breadcrumb.tsx
|   |   |-- MobileNav.tsx
|   |-- products/
|   |   |-- ProductCard.tsx
|   |   |-- ProductGrid.tsx
|   |   |-- ProductDetail.tsx
|   |   |-- ProductGallery.tsx
|   |   |-- VariantSelector.tsx
|   |   |-- ReviewList.tsx
|   |   |-- ReviewItem.tsx
|   |-- cart/
|   |   |-- CartDrawer.tsx
|   |   |-- CartItem.tsx
|   |   |-- CartSummary.tsx
|   |   |-- QuantityControl.tsx
|   |-- checkout/
|   |   |-- CheckoutForm.tsx
|   |   |-- AddressStep.tsx
|   |   |-- ShippingStep.tsx
|   |   |-- PaymentStep.tsx
|   |   |-- ReviewStep.tsx
|   |   |-- OrderSuccess.tsx
|   |   |-- AddressCard.tsx
|   |-- orders/
|   |   |-- OrderList.tsx
|   |   |-- OrderCard.tsx
|   |   |-- OrderDetail.tsx
|   |   |-- StatusTimeline.tsx
|   |-- filters/
|       |-- FilterSidebar.tsx
|       |-- PriceSlider.tsx
|       |-- CategoryFilter.tsx
|       |-- RatingFilter.tsx
|       |-- SortSelect.tsx
|       |-- SearchBar.tsx
|-- features/
|   |-- products/
|   |   |-- useProducts.ts
|   |   |-- useProductFilter.ts
|   |   |-- useWishlist.ts
|   |-- cart/
|   |   |-- cartReducer.ts
|   |   |-- CartContext.tsx
|   |   |-- useCart.ts
|   |-- checkout/
|   |   |-- useCheckout.ts
|   |   |-- checkoutReducer.ts
|-- lib/
|   |-- format-utils.ts
|   |-- shipping-calculator.ts
|   |-- voucher-validator.ts
|-- data/
|   |-- mockProducts.ts
|   |-- mockCategories.ts
|   |-- mockOrders.ts
|   |-- mockAddresses.ts
|-- types/
|   |-- product.ts
|   |-- cart.ts
|   |-- order.ts
|   |-- checkout.ts
|-- hooks/
|   |-- useLocalStorage.ts
|   |-- useDebounce.ts
`-- pages/
    |-- HomePage.tsx
    |-- CategoryPage.tsx
    |-- ProductDetailPage.tsx
    |-- CartPage.tsx (drawer)
    |-- CheckoutPage.tsx
    |-- OrderSuccessPage.tsx
    |-- MyOrdersPage.tsx
    |-- OrderDetailPage.tsx
    |-- WishlistPage.tsx
```

---

## 4. Data Layer

### State Management
- **useReducer** untuk cart state (items, quantities, subtotal, total)
- **Context API** untuk CartContext — accessible dari seluruh komponen
- **useState** untuk product filtering, search, checkout steps
- **localStorage** untuk cart persistence antar session

### Cart Reducer Actions
- `ADD_ITEM`: Tambah produk + varian; jika sudah ada, increment quantity
- `REMOVE_ITEM`: Hapus item dari cart
- `UPDATE_QUANTITY`: Ubah quantity (dengan validasi stok)
- `APPLY_VOUCHER`: Terapkan kode voucher diskon
- `CLEAR_CART`: Kosongkan cart setelah checkout sukses

### Mock Data Strategy
- 20+ produk di 5+ kategori dengan varian (warna, ukuran)
- 3+ kurir dengan tarif berbeda per kota
- 3+ metode pembayaran (Transfer Bank, E-Wallet, COD)
- Data harga dan stok realistis dalam Rupiah

---

## 5. Performance Requirements

| Kriteria | Target |
|----------|--------|
| First Contentful Paint | < 1.5 detik |
| Time to Interactive | < 3 detik |
| Bundle Size | < 500KB (gzipped) |
| Product image lazy loading | Intersection Observer |

---

## 6. Styling & Theming System

- **Design Token:** CSS custom properties untuk warna, spacing, typography
- **Mode:** Light mode default
- **Brand Colors:** Rose primary (#E11D48), warm gray netrals, emerald untuk success states

```css
:root {
  --color-primary: #E11D48;
  --color-primary-dark: #BE123C;
  --color-primary-light: #FB7185;
  --color-success: #059669;
  --color-warning: #D97706;
  --color-danger: #DC2626;
  --color-bg: #F8FAFC;
  --color-card: #FFFFFF;
  --color-text: #0F172A;
  /* ... tokens */
}
```

---

## 7. Validation Rules

- Produk search: min 2 karakter, debounce 300ms
- Checkout alamat: nama penerima required, telepon required (10-15 digit), alamat required (min 10 char)
- Quantity: min 1, max = stock tersedia
- Voucher code: case-insensitive, validasi daftar mock vouchers
- Pembayaran COD: maksimal order Rp 500.000
- Format currency: Rp (IDR) dengan pemisah ribuan
- Format tanggal: DD/MM/YYYY

---

## 8. Error Handling Strategy

- **Cart errors:** "Stok tidak mencukupi" saat menambah quantity melebihi stok
- **Checkout errors:** Simulasi error pembayaran "Pembayaran gagal. Silakan coba metode lain."
- **Search errors:** "Produk tidak ditemukan. Coba kata kunci lain."
- **Empty states:** Keranjang kosong, pesanan kosong, wishlist kosong — masing-masing dengan ilustrasi dan CTA
- **Global error boundary:** React Error Boundary
- **Network simulation:** Opsional timeout error saat "checkout"

---

## 9. Security Considerations

- **Mock only:** Tidak ada autentikasi real, UI siap untuk login/register
- **Cart data:** Hanya di client-side (localStorage), tidak ada transmisi
- **Payment mock:** Tidak ada data kartu kredit/transfer real
- **Input sanitization:** Untuk form alamat dan search input
- **XSS prevention:** Semua user-generated content di-render dengan React (safe by default)

---

## 10. Deployment & Build

- **Build:** `npm run build` (Vite)
- **Output:** Static files (HTML, CSS, JS)
- **Hosting:** Static hosting siap (Netlify, Vercel, GitHub Pages)
- **CI/CD:** Opsional — GitHub Actions untuk deploy otomatis

---

## 11. Testing Strategy

| Level | Tools | Coverage Target |
|-------|-------|----------------|
| Unit | Vitest | > 70% |
| Component | React Testing Library | Komponen kritis (CartDrawer, CheckoutForm, ProductCard) |
| Integration | Vitest | Cart reducer logic, checkout flow, filter logic |
| E2E | Playwright (opsional) | Browse → Add to cart → Checkout → Order success |

---

## 12. Dependencies & Constraints

### Dependencies
- React 18+
- Node.js 18+
- NPM / Yarn / PNPM
- Framer Motion 10+
- Lucide React
- Recharts
- shadcn/ui (via Tailwind + Radix UI)

### Constraints
- Tidak ada backend dependency
- Tidak ada API key / autentikasi real
- Semua fitur harus berfungsi offline
- Cart tidak persist antar browser (hanya localStorage)
- Checkout tidak memproses pembayaran real
- Browser support: Chrome, Firefox, Safari, Edge (2 versi terakhir)
- Minimum 1 produk di keranjang untuk bisa checkout
