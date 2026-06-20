<p align="center">
  <img src="D-Voyager-noBG-Transparent.png" alt="D-Voyager Logo" width="250"/>
</p>

# D-Voyager Front-End (Ionic Angular)

Frontend aplikasi pemesanan tiket shuttle **D-Voyager** yang dibangun menggunakan **Ionic Angular** dan **Capacitor** untuk mendukung platform Web dan Android (Mobile).

Aplikasi ini memiliki 2 peran utama:
1. **Customer (Penumpang)**: Memesan tiket, memilih kursi, memantau posisi driver secara real-time, mengobrol dengan driver/chatbot, dan melakukan pembayaran.
2. **Driver (Supir)**: Melihat jadwal keberangkatan, melihat manifest penumpang, navigasi rute Mapbox, dan membagikan lokasi GPS secara real-time.

---

## 🚀 Fitur Utama

### 1. Panel Customer (Penumpang)
* **Pemesanan Tiket & Kursi**: Alur pemesanan interaktif dengan pemilihan kursi (*seat selection*) secara visual.
* **Integrasi Pembayaran**: Halaman verifikasi pembayaran dengan tampilan status berhasil/gagal.
* **Live Driver Tracking**: Menampilkan posisi supir secara real-time di peta Mapbox menggunakan Laravel Reverb (WebSockets).
* **Real-time Chat**: Obrolan langsung antara supir dan penumpang.
* **Chatbot & Customer Service**: Layanan bantuan pelanggan instan.
* **Riwayat Tiket & Rating**: Ulasan dan rating bintang untuk supir setelah perjalanan selesai.

### 2. Panel Driver (Supir)
* **Jadwal Perjalanan**: Daftar perjalanan aktif dan mendatang untuk supir.
* **Live GPS Broadcasting**: Mengirimkan koordinat lokasi GPS secara berkala saat perjalanan dimulai agar penumpang dapat melacak posisi shuttle.
* **Manifest Penumpang**: Informasi lengkap daftar penumpang untuk keberangkatan shuttle.
* **Peta Navigasi**: Rute perjalanan di peta Mapbox lengkap dengan penanda titik asal dan tujuan.

---

## 🛠️ Tech Stack & Dependensi

* **Framework Core**: [Ionic Angular v8+](https://ionicframework.com/) & [Angular v20](https://angular.dev/)
* **Mobile Engine**: [Capacitor v8](https://capacitorjs.com/)
* **Peta & Rute**: [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/api/) (melalui API Mapbox Directions)
* **Real-Time Connection**: [Laravel Echo](https://laravel.com/docs/broadcasting) & [Pusher JS](https://github.com/pusher/pusher-js) (menggunakan Laravel Reverb di backend)
* **Lokasi & Tracking**: `@capacitor/geolocation` untuk melacak posisi GPS perangkat mobile.

---

## ⚙️ Persyaratan Sistem

Sebelum menjalankan aplikasi, pastikan Anda telah menginstal tools berikut:
* **Node.js** (versi 18 atau lebih baru)
* **Ionic CLI** (`npm install -g @ionic/cli`)
* **Android Studio** (untuk build platform Android)
* **JDK 17 / Gradle** (untuk compile aplikasi Android)

---

## 🚀 Cara Menjalankan Aplikasi

### 1. Clone & Install Dependensi
Masuk ke direktori frontend dan jalankan command berikut:
```bash
git clone <url-repo-frontend> D-Voyager-FrontEnd
cd D-Voyager-FrontEnd
npm install
```

### 2. Konfigurasi Environment
Buka file `src/environments/environment.ts` untuk mengonfigurasi endpoint API backend:
```typescript
export const environment = {
  production: false,
  apiUrl: 'http://127.0.0.1:8000/api', // Ubah sesuai dengan IP backend Anda
  serviceFee: 0
};
```
*Untuk WebSocket (Real-time chat/tracking)*: Sesuaikan IP host `wsHost` di dalam file [echo.service.ts](file:///C:/laragon/www/D-Voyager-FrontEnd/src/app/services/echo.service.ts#L42) jika backend di-host di komputer lain atau di server production.

### 3. Menjalankan di Mode Web (Development)
Untuk menjalankan aplikasi di browser lokal:
```bash
ionic serve
# atau
npm start
```
Aplikasi akan otomatis terbuka di browser pada alamat `http://localhost:8100`.

### 4. Build dan Menjalankan di Android (Mobile)
Aplikasi ini mendukung cross-platform mobile menggunakan Capacitor. Ikuti langkah berikut untuk build ke file APK Android:

1. **Build bundle web**:
   ```bash
   ionic build
   ```
2. **Sinkronisasi Capacitor dengan Android**:
   ```bash
   npx cap sync
   ```
3. **Membuka Android Studio**:
   ```bash
   npx cap open android
   ```
   *Dari Android Studio, hubungkan device Android fisik atau gunakan Emulator, lalu jalankan/run aplikasi.*
4. **Menjalankan langsung via terminal**:
   ```bash
   npx cap run android
   ```

---

## 📁 Struktur Folder Utama
```text
D-Voyager-FrontEnd/
├── android/                   # Source code native Android (Capacitor)
├── src/
│   ├── app/
│   │   ├── booking-summary/   # Ringkasan pemesanan tiket
│   │   ├── chatbot/           # Fitur chat otomatis (AI Chatbot)
│   │   ├── driver-home/       # Beranda utama untuk supir + Peta Mapbox
│   │   ├── driver-chat/       # Chat di sisi supir
│   │   ├── home/              # Halaman beranda customer (pemilihan rute/tujuan)
│   │   ├── tickets/           # Riwayat tiket customer & tracking real-time
│   │   ├── select-seat/       # Pemilihan kursi shuttle
│   │   ├── payment/           # Halaman status pembayaran
│   │   ├── services/
│   │   │   ├── api.service.ts # Kumpulan HTTP Requests ke Laravel API
│   │   │   └── echo.service.ts# Konfigurasi Laravel Echo untuk WebSocket
│   │   └── welcome/           # Welcome/Onboarding screen
│   ├── assets/                # Gambar, logo, ikon, dan resource static
│   ├── theme/
│   │   └── variables.scss     # Konfigurasi warna & tema (premium/dark style)
│   └── index.html
├── capacitor.config.ts        # Konfigurasi nama app & package name
└── package.json
```

---

## 🔒 Catatan Keamanan Mapbox
* Token Mapbox di-inisialisasi langsung di controller halaman (`driver-home.page.ts` dan `tickets.page.ts`).
* Token ini adalah **Public Token** (`pk.***`), sehingga aman disimpan di repositori frontend dan bisa dibaca oleh client untuk merender peta.
