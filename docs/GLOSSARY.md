# 📖 Glossary - Dashboard PODES Batu 2024

Daftar istilah dan terminologi yang digunakan dalam proyek ini.

---

## A

### Adapter
Pattern desain software yang mengubah interface dari satu sistem ke interface yang diharapkan sistem lain. Dalam proyek ini, adapter digunakan untuk memformat data dari API agar sesuai dengan format yang diharapkan komponen chart.

### API (Application Programming Interface)
Sekumpulan aturan dan protokol yang memungkinkan aplikasi berbeda berkomunikasi. Backend menyediakan REST API untuk frontend.

### Axios
Library JavaScript untuk melakukan HTTP requests. Digunakan di frontend untuk komunikasi dengan backend API.

---

## B

### Backend
Server-side aplikasi yang menangani logika bisnis, penyimpanan data, dan menyediakan API. Menggunakan Node.js + Express.js.

### Binary Indicator
Indikator dengan dua kemungkinan nilai: Ya/Tidak, Ada/Tidak Ada, 1/0. Contoh: `ada_sd`, `ada_puskesmas`.

---

## C

### Category (Kategori)
Pengelompokan indikator berdasarkan tema/bidang. Contoh: Pendidikan, Kesehatan, Infrastruktur, Lingkungan.

### Choropleth Map
Peta tematik yang menggunakan gradasi warna untuk menunjukkan nilai statistik per wilayah. Digunakan untuk visualisasi data per desa.

### Component (Komponen)
Bagian UI yang dapat digunakan kembali (reusable) di React. Contoh: `FilterSidebar`, `DistributionChart`, `DataTable`.

### Config-driven Design
Pendekatan desain di mana perilaku aplikasi dikontrol oleh file konfigurasi, bukan hardcoded. Memudahkan penambahan fitur tanpa mengubah code.

### Controller
Bagian dari pattern MVC yang menangani logika bisnis. Menerima request, memproses, dan mengembalikan response.

### CORS (Cross-Origin Resource Sharing)
Mekanisme keamanan browser yang mengontrol request dari domain berbeda. Backend harus mengizinkan request dari frontend domain.

---

## D

### Dashboard
Tampilan visual yang menyajikan informasi penting dalam bentuk ringkas dan mudah dipahami. Proyek ini adalah dashboard data statistik.

### Data Binding
Proses menghubungkan data dengan elemen UI sehingga perubahan data otomatis memperbarui tampilan.

### Desa/Kelurahan
Unit administrasi tingkat terbawah dalam pembagian wilayah Indonesia. Kota Batu memiliki 24 desa/kelurahan.

---

## E

### Endpoint
URL spesifik pada API yang menerima request. Contoh: `/api/villages`, `/api/villages/metadata`.

### Environment Variable
Variabel yang di-set di level sistem operasi untuk mengkonfigurasi aplikasi. Contoh: `PORT`, `VITE_API_URL`.

### Express.js
Framework web minimalis untuk Node.js yang digunakan untuk membuat REST API di backend.

---

## F

### Frontend
Client-side aplikasi yang berjalan di browser pengguna. Menggunakan React.js.

### Filter
Fitur untuk menyaring/memilih subset data berdasarkan kriteria tertentu. Contoh: filter by kecamatan, filter by indicator.

---

## G

### GeoJSON
Format data berbasis JSON untuk merepresentasikan struktur geografis (titik, garis, polygon). Digunakan untuk data batas desa.

### Geospatial
Berkaitan dengan lokasi geografis dan data yang memiliki komponen lokasi.

---

## H

### Hook
Fitur React untuk menggunakan state dan lifecycle dalam functional components. Contoh: `useState`, `useEffect`, `useMemo`.

---

## I

### Indicator (Indikator)
Variabel/metrik yang diukur dan divisualisasikan. Contoh: `jumlah_sd` (jumlah SD/Sederajat), `kepadatan_penduduk`.

### Infrastructure (Infrastruktur)
Fasilitas fisik dasar yang mendukung fungsi masyarakat. Kategori indikator meliputi jalan, jembatan, penerangan.

---

## J

### JSON (JavaScript Object Notation)
Format pertukaran data yang ringan dan mudah dibaca. Digunakan untuk menyimpan data PODES dan komunikasi API.

### JSX
Ekstensi sintaks untuk JavaScript yang memungkinkan penulisan HTML-like code di dalam JavaScript. Digunakan di React.

---

## K

### Kecamatan
Unit administrasi di bawah Kota/Kabupaten. Kota Batu memiliki 3 kecamatan: Batu, Bumiaji, Junrejo.

### KPI (Key Performance Indicator)
Metrik utama yang menunjukkan kinerja atau kondisi. Ditampilkan di bagian atas dashboard.

---

## L

### Leaflet
Library JavaScript open-source untuk peta interaktif. Digunakan via react-leaflet di komponen GeospatialMap.

### Legend
Keterangan pada visualisasi yang menjelaskan arti warna, simbol, atau ukuran.

---

## M

### Material-UI (MUI)
Library komponen React yang mengimplementasikan Material Design dari Google. Menyediakan komponen UI siap pakai.

### Middleware
Function yang dieksekusi dalam pipeline request-response di Express. Contoh: cors(), helmet(), morgan().

### MVC (Model-View-Controller)
Pattern arsitektur yang memisahkan aplikasi menjadi tiga komponen: Model (data), View (tampilan), Controller (logika).

---

## N

### Node.js
Runtime JavaScript yang memungkinkan JavaScript berjalan di server (bukan hanya browser).

### npm (Node Package Manager)
Manajer paket untuk JavaScript/Node.js. Digunakan untuk menginstall dependencies.

---

## O

### Object Destructuring
Fitur JavaScript untuk mengekstrak properti dari object ke variabel terpisah. `const { nama, umur } = person;`

---

## P

### PODES (Potensi Desa)
Survei yang dilakukan BPS untuk mengumpulkan data potensi desa di seluruh Indonesia. Sumber data utama proyek ini.

### Prop (Properties)
Cara untuk mengirim data dari parent component ke child component di React.

---

## Q

### Qualitative Indicator (Indikator Kualitatif)
Indikator yang bersifat deskriptif atau kategorikal, bukan numerik. Contoh: jenis sumber air, kondisi jalan.

### Quantitative Indicator (Indikator Kuantitatif)
Indikator berbentuk angka/numerik yang dapat dihitung. Contoh: jumlah sekolah, jumlah penduduk.

### Query Parameter
Parameter yang ditambahkan ke URL untuk mengirim data. Contoh: `/api/villages?kecamatan=Batu`.

---

## R

### React
Library JavaScript untuk membangun user interface, dikembangkan oleh Facebook/Meta.

### React Router
Library untuk navigasi/routing di aplikasi React single-page.

### REST API (Representational State Transfer)
Arsitektur API yang menggunakan HTTP methods (GET, POST, PUT, DELETE) untuk operasi data.

### Route
Definisi mapping antara URL path dengan handler function di Express.

---

## S

### SPA (Single Page Application)
Aplikasi web yang memuat satu halaman HTML dan memperbarui konten secara dinamis tanpa reload.

### State
Data yang disimpan dan dikelola dalam komponen React. Perubahan state memicu re-render.

### Service Layer
Abstraksi yang memisahkan logika komunikasi API dari komponen UI.

---

## T

### Theme
Konfigurasi visual yang mendefinisikan warna, font, spacing konsisten di seluruh aplikasi.

### Tooltip
Informasi tambahan yang muncul saat hover di atas elemen UI.

---

## U

### UI (User Interface)
Tampilan visual dan elemen interaktif yang dilihat dan digunakan pengguna.

### UX (User Experience)
Keseluruhan pengalaman pengguna saat berinteraksi dengan aplikasi.

---

## V

### Vite
Build tool modern untuk development frontend yang sangat cepat. Menggantikan Create React App.

### Visualization (Visualisasi)
Representasi visual data dalam bentuk chart, map, tabel untuk memudahkan pemahaman.

---

## W

### Webpack
Bundler JavaScript yang menggabungkan dan mengoptimasi file untuk production. Vite menggunakan Rollup untuk build.

---

## Akronim & Singkatan

| Singkatan | Kepanjangan |
|-----------|-------------|
| API | Application Programming Interface |
| BPS | Badan Pusat Statistik |
| CORS | Cross-Origin Resource Sharing |
| CSS | Cascading Style Sheets |
| DOM | Document Object Model |
| GeoJSON | Geographic JSON |
| HTML | HyperText Markup Language |
| HTTP | HyperText Transfer Protocol |
| JSON | JavaScript Object Notation |
| JSX | JavaScript XML |
| KPI | Key Performance Indicator |
| MUI | Material-UI |
| MVC | Model-View-Controller |
| NPM | Node Package Manager |
| PODES | Potensi Desa |
| REST | Representational State Transfer |
| SDK | Software Development Kit |
| SPA | Single Page Application |
| UI | User Interface |
| URL | Uniform Resource Locator |
| UX | User Experience |

---

*Glossary terakhir diperbarui: Desember 2024*
