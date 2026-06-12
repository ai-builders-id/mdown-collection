# PRD: E-Commerce — Produk, Keranjang, Checkout, Pesanan

**Project:** E-Commerce
**Aesthetic:** Modern, vibrant, consumer-friendly dengan warna rose yang menarik dan clean shopping experience

---

## 1. Core Layout

Aplikasi menggunakan layout toko online standar:

- **Top Header (8%):** Logo, search bar (lebar), ikon keranjang dengan badge jumlah, user menu (login/register), kategori dropdown.
- **Main Content:** Halaman bervariasi — Home (hero + grid produk), Kategori (grid/filter), Detail Produk, Keranjang, Checkout, Pesanan Saya.
- **Sidebar Filter (20%):** Pada halaman kategori/daftar produk — filter harga, kategori, rating, brand.
- **Footer (15%):** Informasi toko, links, pembayaran, sosial media.

Layout responsif: mobile — hamburger menu, filter sebagai bottom sheet, keranjang sebagai overlay dari kanan.

---

## 2. Data Structure (Mock Only)

```typescript
export interface Product {
  id: string;
  name: string;
  slug: string;
  description: string;
  shortDescription: string;
  price: number;
  originalPrice?: number; // untuk diskon
  discountPercentage?: number;
  categoryId: string;
  categoryName: string;
  brand: string;
  images: string[];
  thumbnail: string;
  rating: number;
  reviewCount: number;
  stock: number;
  soldCount: number;
  tags: string[];
  variants: ProductVariant[];
  specifications: Record<string, string>;
  createdAt: string;
  isActive: boolean;
}

export interface ProductVariant {
  id: string;
  name: string; // misal: "Hitam", "XL"
  type: 'color' | 'size' | 'material' | 'other';
  additionalPrice: number;
  stock: number;
  image?: string;
}

export interface CartItem {
  id: string;
  productId: string;
  productName: string;
  productImage: string;
  variant: {
    name: string;
    type: string;
    value: string;
  } | null;
  quantity: number;
  unitPrice: number;
  subtotal: number;
  stockAvailable: number;
}

export interface Cart {
  id: string;
  items: CartItem[];
  totalItems: number;
  subtotal: number;
  shippingCost: number;
  discount: number;
  voucherCode: string | null;
  total: number;
  updatedAt: string;
}

export interface Order {
  id: string;
  orderNumber: string;
  status: 'pending_payment' | 'processing' | 'shipped' | 'delivered' | 'cancelled' | 'refunded';
  items: CartItem[];
  shippingAddress: Address;
  paymentMethod: string;
  subtotal: number;
  shippingCost: number;
  discount: number;
  total: number;
  trackingNumber: string | null;
  courier: string;
  estimatedDelivery: string;
  paidAt: string | null;
  shippedAt: string | null;
  deliveredAt: string | null;
  createdAt: string;
  updatedAt: string;
}

export interface Address {
  id: string;
  label: string; // "Rumah", "Kantor"
  recipientName: string;
  phone: string;
  street: string;
  city: string;
  district: string;
  province: string;
  postalCode: string;
  isDefault: boolean;
}

export interface ProductCategory {
  id: string;
  name: string;
  slug: string;
  icon: string;
  image: string;
  parentId: string | null;
  productCount: number;
}

export interface Review {
  id: string;
  productId: string;
  userId: string;
  userName: string;
  userAvatar: string;
  rating: number;
  title: string;
  content: string;
  images: string[];
  createdAt: string;
  likes: number;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS dengan kustomisasi untuk e-commerce
- **Icons:** Lucide React
- **Animation:** Framer Motion (animasi tambah keranjang, transisi halaman, modal)
- **Chart:** Recharts (sales analytics di dashboard admin)
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary `#E11D48` (Rose), secondary warm gray, accent emerald untuk success (checkout berhasil).
- **Typography:** Inter (sans-serif), ukuran 16px untuk body, 24px untuk judul produk, 18px untuk harga.
- **Spacing:** Tailwind scale, card produk dengan shadow-md, border-radius 12px, gap grid 24px.
- **Wishlist heart:** Animasi scaling saat di-click.

---

## 5. UI Components Specification

```text
+-------------------------------------------------------------------------------------------+
| HEADER: [LOGO] [Cari produk...         ] [Kategori v] [Keranjang (3)] [Masuk/Daftar]      |
+-------------------------------------------------------------------------------------------+
|                                                                                           |
|  +-- BREADCRUMB: Home > Elektronik > Headphone                                            |
|                                                                                           |
|  +------------------------------------------------------------------+  +---------------+ |
|  |  PRODUCT GRID                                                    |  | FILTER        | |
|  |                                                                  |  | Harga:        | |
|  |  +------+  +------+  +------+  +------+                         |  | [====slider==]| |
|  |  |IMG   |  |IMG   |  |IMG   |  |IMG   |                         |  | Kategori:     | |
|  |  |ProdukA|  |ProdukB|  |ProdukC|  |ProdukD|                      |  | [x] Elektro   | |
|  |  |Rp150rb|  |Rp89rb |  |Rp250rb|  |Rp45rb |                      |  | [ ] Fashion   | |
|  |  | ★4.5 |  | ★4.2 |  | ★4.8 |  | ★4.0 |                      |  | [ ] Makanan   | |
|  |  |[Tambah]|  |[Tambah]|  |[Tambah]|  |[Tambah]|                   |  | Rating:       | |
|  |  +------+  +------+  +------+  +------+                         |  | [★★★↑]       | |
|  |                                                                  |  | Brand:        | |
|  |  [1] [2] [3] ... [10]  (Pagination)                             |  | [x] Sony      | |
|  +------------------------------------------------------------------+  | [ ] Samsung   | |
|                                                                        +---------------+ |
|                                                                                           |
+-------------------------------------------------------------------------------------------+
| FOOTER: [Tentang] [Kebijakan] [Pembayaran] [Sosial Media] [Copyright 2026]                |
+-------------------------------------------------------------------------------------------+
```

Komponen utama:
- **ProductCard:** Kartu produk dengan gambar, harga, rating, badge diskon, wishlist button, tombol tambah keranjang.
- **ProductGrid:** Grid responsif (4 kolom desktop, 2 tablet, 1 mobile) dari ProductCard.
- **FilterSidebar:** Range slider harga, checkbox kategori, rating filter, brand filter.
- **CartDrawer:** Slide-over dari kanan menampilkan item keranjang, subtotal, tombol checkout.
- **CheckoutForm:** Multi-step form (Alamat > Pengiriman > Pembayaran > Konfirmasi).
- **OrderCard:** Kartu ringkasan pesanan di halaman "Pesanan Saya".
- **SearchBar:** Input dengan autocomplete dropdown, debounce 300ms.
- **ProductDetail:** Halaman detail produk — galeri gambar, info, varian, review.

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton produk (4 kartu abu-abu dengan shimmer); skeleton detail produk; spinner saat checkout.
- **Empty:** Keranjang kosong: "Keranjang belanja Anda masih kosong. Yuk, belanja sekarang!" Ilustrasi empty cart dengan tombol "Mulai Belanja".
- **Error:** "Gagal memuat produk. Silakan refresh halaman." Banner error dengan tombol retry. Checkout gagal: simulasi error pembayaran.
- **Edge cases:** Stok habis (tombol "Stok Habis" disabled); harga Rp 0; checkout dengan keranjang kosong; filter tanpa hasil; produk duplikat di keranjang (increment qty).

**Flow Interaksi Utama:**
1. User membuka halaman Home → melihat hero banner dan produk unggulan → klik kategori → filter produk.
2. User klik produk → halaman detail → pilih varian (warna/ukuran) → klik "Tambah ke Keranjang" → animasi notifikasi + badge keranjang bertambah.
3. User buka keranjang (slide dari kanan) → lihat item → ubah quantity → klik "Checkout".
4. User masuk ke Checkout → isi alamat (atau pilih alamat tersimpan) → pilih kurir → pilih pembayaran → konfirmasi → halaman sukses pesanan.
5. User buka "Pesanan Saya" → lihat daftar pesanan → klik salah satu → detail pesanan dengan status tracking.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- **Mata uang:** IDR (Rp)
- Label: "Beranda", "Kategori", "Keranjang", "Pesanan Saya", "Masuk", "Daftar".
- Tombol: "Tambah ke Keranjang", "Beli Sekarang", "Checkout", "Lanjutkan Belanja", "Simpan", "Hapus".
- Status Pesanan: "Menunggu Pembayaran", "Diproses", "Dikirim", "Telah Tiba", "Dibatalkan", "Dikembalikan".
- Notifikasi: "Produk ditambahkan ke keranjang", "Pesanan berhasil dibuat", "Stok tidak mencukupi".

---

## 8. Example Mock Data

```typescript
export const mockProducts: Product[] = [
  {
    id: 'product-001',
    name: 'Headphone Wireless Pro Max',
    slug: 'headphone-wireless-pro-max',
    description: 'Headphone nirkabel dengan noise cancellation aktif, battery life 40 jam, dan kualitas suara Hi-Res Audio.',
    shortDescription: 'Wireless headphone dengan ANC dan Hi-Res Audio',
    price: 1499000,
    originalPrice: 1999000,
    discountPercentage: 25,
    categoryId: 'cat-001',
    categoryName: 'Elektronik',
    brand: 'Sony',
    images: ['/images/headphone-1.jpg', '/images/headphone-2.jpg'],
    thumbnail: '/images/headphone-thumb.jpg',
    rating: 4.5,
    reviewCount: 234,
    stock: 45,
    soldCount: 1280,
    tags: ['headphone', 'wireless', 'premium'],
    variants: [
      { id: 'var-001', name: 'Hitam', type: 'color', additionalPrice: 0, stock: 20, image: '/images/headphone-black.jpg' },
      { id: 'var-002', name: 'Putih', type: 'color', additionalPrice: 0, stock: 15, image: '/images/headphone-white.jpg' },
      { id: 'var-003', name: 'Biru', type: 'color', additionalPrice: 100000, stock: 10, image: '/images/headphone-blue.jpg' },
    ],
    specifications: { 'Driver': '40mm', 'Battery': '40 jam', 'Bluetooth': '5.2', 'Weight': '250g' },
    createdAt: '2026-01-01T08:00:00Z',
    isActive: true,
  },
  {
    id: 'product-002',
    name: 'Kaos Polos Premium Cotton',
    slug: 'kaos-polos-premium-cotton',
    description: 'Kaos polos bahan cotton combed 30s, nyaman dipakai sehari-hari dengan jahitan rapi.',
    shortDescription: 'Kaos polos cotton combed 30s',
    price: 89000,
    categoryId: 'cat-002',
    categoryName: 'Fashion',
    brand: 'Local Brand',
    images: ['/images/kaos-1.jpg'],
    thumbnail: '/images/kaos-thumb.jpg',
    rating: 4.2,
    reviewCount: 567,
    stock: 200,
    soldCount: 3450,
    tags: ['kaos', 'polos', 'casual'],
    variants: [
      { id: 'var-004', name: 'S', type: 'size', additionalPrice: 0, stock: 50 },
      { id: 'var-005', name: 'M', type: 'size', additionalPrice: 0, stock: 80 },
      { id: 'var-006', name: 'L', type: 'size', additionalPrice: 0, stock: 70 },
    ],
    specifications: { 'Bahan': 'Cotton Combed 30s', 'Ukuran': 'S, M, L, XL', 'Perawatan': 'Cuci dingin' },
    createdAt: '2026-02-15T08:00:00Z',
    isActive: true,
  },
];

export const mockCart: Cart = {
  id: 'cart-001',
  items: [
    {
      id: 'cart-item-001',
      productId: 'product-001',
      productName: 'Headphone Wireless Pro Max',
      productImage: '/images/headphone-thumb.jpg',
      variant: { name: 'Warna', type: 'color', value: 'Hitam' },
      quantity: 1,
      unitPrice: 1499000,
      subtotal: 1499000,
      stockAvailable: 20,
    },
  ],
  totalItems: 1,
  subtotal: 1499000,
  shippingCost: 15000,
  discount: 0,
  voucherCode: null,
  total: 1514000,
  updatedAt: '2026-06-13T10:00:00Z',
};

export const mockOrders: Order[] = [
  {
    id: 'order-001',
    orderNumber: 'INV-20260601-001',
    status: 'shipped',
    items: [
      {
        id: 'order-item-001',
        productId: 'product-001',
        productName: 'Headphone Wireless Pro Max',
        productImage: '/images/headphone-thumb.jpg',
        variant: { name: 'Warna', type: 'color', value: 'Hitam' },
        quantity: 1,
        unitPrice: 1499000,
        subtotal: 1499000,
        stockAvailable: 0,
      },
    ],
    shippingAddress: {
      id: 'addr-001',
      label: 'Rumah',
      recipientName: 'Andi Pratama',
      phone: '0812-3456-7890',
      street: 'Jl. Merdeka No. 45, RT 03 RW 08',
      city: 'Bandung',
      district: 'Coblong',
      province: 'Jawa Barat',
      postalCode: '40132',
      isDefault: true,
    },
    paymentMethod: 'Transfer Bank BCA',
    subtotal: 1499000,
    shippingCost: 15000,
    discount: 0,
    total: 1514000,
    trackingNumber: 'JNE-TRK-1234567890',
    courier: 'JNE REG',
    estimatedDelivery: '2026-06-15',
    paidAt: '2026-06-01T10:30:00Z',
    shippedAt: '2026-06-02T08:00:00Z',
    deliveredAt: null,
    createdAt: '2026-06-01T10:00:00Z',
    updatedAt: '2026-06-02T08:00:00Z',
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] CRUD produk di admin panel berfungsi
- [ ] Keranjang berfungsi (tambah, ubah quantity, hapus item)
- [ ] Checkout flow multi-step berfungsi (alamat > pengiriman > pembayaran > konfirmasi)
- [ ] Filter produk (kategori, harga, rating, brand) bekerja
- [ ] Search produk dengan debounce berfungsi
- [ ] Responsive design (desktop, tablet, mobile)
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
