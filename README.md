# UAS Aplikasi Mobile Lanjutan

**Nama:** Helen Oktaviani  
**Kelas:** TI SE 23 M  
**NIM:** 1123150205  
**GitHub:** [@helenoktaa](https://github.com/helenoktaa)

---

## Daftar Aplikasi

| Aplikasi | Deskripsi | Repository |
|----------|-----------|------------|
| Urban Plant | Aplikasi e-commerce tanaman hias | [urban_plant](https://github.com/helenoktaa/urban_plant) |
| Wallt | Aplikasi e-money / dompet digital | [dompet_kampus_global](https://github.com/helenoktaa/dompet_kampus_global) |

Kedua aplikasi terintegrasi melalui mekanisme **Deep Link** — pembayaran dari Urban Plant diproses langsung oleh Wallt secara real-time.

---

## Video Demo

[Tonton di YouTube](#) *(link akan diupdate setelah upload)*

---

## Urban Plant — E-Commerce

### Deskripsi

Urban Plant adalah aplikasi e-commerce berbasis Flutter untuk jual beli tanaman hias. Pengguna dapat browse produk, menambahkan ke keranjang, checkout, dan membayar menggunakan Wallt melalui deep link.

### Fitur Utama

- Login dengan Google / email dan password
- Browse dan search produk tanaman
- Keranjang belanja dan checkout
- Pembayaran via Wallt menggunakan deep link
- Riwayat pesanan dan status tracking
- Wishlist produk

### Arsitektur

```
urban_plant/
├── lib/
│   ├── core/
│   │   ├── services/       # ApiService, DeeplinkService
│   │   ├── theme/          # AppColors, AppTheme
│   │   └── utils/          # Formatter, Helper
│   ├── features/
│   │   ├── auth/           # Login, Register
│   │   ├── home/           # Halaman utama
│   │   ├── product/        # Detail produk
│   │   ├── cart/           # Keranjang belanja
│   │   ├── order/          # Riwayat pesanan
│   │   └── checkout/       # Proses checkout
│   └── widgets/            # Komponen reusable
```

**Architectural Pattern:** BLoC (Business Logic Component)  
**Navigation:** Go Router  
**State Management:** flutter_bloc  
**Dependency Injection:** GetIt

### Cara Menjalankan

```bash
git clone https://github.com/helenoktaa/urban_plant.git
cd urban_plant
flutter pub get
flutter run
```

### Dependensi Utama

| Package | Versi | Kegunaan |
|---------|-------|----------|
| flutter_bloc | ^9.0.0 | State management |
| go_router | ^14.8.1 | Navigation dan deep link |
| dio | ^5.7.0 | HTTP client |
| get_it | ^8.0.2 | Dependency injection |
| firebase_auth | ^5.5.2 | Autentikasi pengguna |
| google_sign_in | ^6.2.2 | Login dengan Google |
| shared_preferences | ^2.3.4 | Local storage |
| cached_network_image | ^3.4.1 | Caching gambar |

### Screenshot

| Home | Detail Produk | Cart | Riwayat Pesanan |
|------|--------------|------|-----------------|
| ![Home](assets/screenshots/urban_home.png) | ![Detail](assets/screenshots/urban_detail.png) | ![Cart](assets/screenshots/urban_cart.png) | ![Order](assets/screenshots/urban_order.png) |

---

## Wallt — E-Money

### Deskripsi

Wallt adalah aplikasi dompet digital berbasis Flutter yang terintegrasi dengan Firebase. Mendukung transaksi pembayaran melalui deep link dari merchant eksternal (Urban Plant), dilengkapi verifikasi PIN dan Two-Factor Authentication (2FA).

### Fitur Utama

- Login dengan Google / email dan password
- Dashboard saldo dan riwayat transaksi
- Pembayaran via deep link dari merchant
- Verifikasi PIN sebelum transaksi
- Two-Factor Authentication (2FA) — SMTP dan biometric
- QR Code scanner untuk pembayaran
- Push notification via Firebase Messaging

### Arsitektur

```
dompet_kampus_global/
├── lib/
│   ├── core/
│   │   ├── services/       # DeeplinkService, DeeplinkCallbackService
│   │   ├── theme/          # AppColors, AppTheme
│   │   └── utils/          # CurrencyFormatter
│   ├── features/
│   │   ├── auth/           # Login, Register, 2FA
│   │   ├── home/           # Dashboard saldo
│   │   ├── payment/        # Konfirmasi pembayaran deeplink
│   │   ├── pin/            # Verifikasi PIN
│   │   ├── transaction/    # Riwayat transaksi
│   │   └── scan/           # QR Scanner
│   └── widgets/            # AppButton, AppField, AppLogo
```

**Architectural Pattern:** BLoC (Business Logic Component)  
**Navigation:** Go Router dengan Deep Link support  
**State Management:** flutter_bloc  
**Dependency Injection:** GetIt  
**Authentication:** Firebase Auth + Google Sign In  
**Security:** PIN + 2FA (SMTP / Biometric)

### Cara Menjalankan

```bash
git clone https://github.com/helenoktaa/dompet_kampus_global.git
cd dompet_kampus_global
flutter pub get
flutter run
```

### Dependensi Utama

| Package | Versi | Kegunaan |
|---------|-------|----------|
| flutter_bloc | ^9.0.0 | State management |
| go_router | ^14.8.1 | Navigation dan deep link |
| dio | ^5.7.0 | HTTP client |
| get_it | ^8.0.2 | Dependency injection |
| firebase_auth | ^5.5.2 | Autentikasi pengguna |
| firebase_messaging | ^15.2.4 | Push notification |
| google_sign_in | ^6.2.2 | Login dengan Google |
| flutter_secure_storage | ^9.2.2 | Penyimpanan PIN dan token |
| mobile_scanner | ^7.0.0 | QR Code scanner |
| flutter_biometric_kit | local | Biometric 2FA |

### Screenshot

| Login | Home | Konfirmasi Pembayaran | Verifikasi PIN |
|-------|------|----------------------|----------------|
| ![Login](assets/screenshots/wallt_login.png) | ![Home](assets/screenshots/wallt_home.png) | ![Payment](assets/screenshots/wallt_payment.png) | ![PIN](assets/screenshots/wallt_pin.png) |

---

## Deep Link Integration

Integrasi antar aplikasi menggunakan URL scheme deep link dengan format berikut:

```
dompetkampus://pay?amount=25000&merchant=Urban+Plant&description=Pembayaran+%2327&reference=INV-27&callback=urbanplant://callback
```

### Alur Pembayaran

1. Pengguna checkout di Urban Plant dan memilih Wallt sebagai metode pembayaran
2. Urban Plant membuka Wallt via deep link beserta data transaksi
3. Wallt menampilkan halaman konfirmasi pembayaran dengan detail transaksi
4. Pengguna memverifikasi transaksi menggunakan PIN dan 2FA
5. Wallt mengirimkan callback ke Urban Plant
6. Status pesanan di Urban Plant diperbarui menjadi **Paid**

---

## Two-Factor Authentication (2FA)

Wallt mengimplementasikan dua lapisan verifikasi keamanan sebelum setiap transaksi diproses:

| Layer | Metode | Keterangan |
|-------|--------|------------|
| Layer 1 | PIN | 6 digit PIN yang disimpan secara aman menggunakan flutter_secure_storage |
| Layer 2 | SMTP / Biometric | Kode OTP via email atau verifikasi fingerprint via flutter_biometric_kit |

---

## Download APK

| Aplikasi | Link |
|----------|------|
| Urban Plant (debug) | [Download](#) *(coming soon)* |
| Wallt (debug) | [Download](#) *(coming soon)* |
