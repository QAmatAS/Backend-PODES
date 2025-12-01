# 📚 Dokumentasi Backend PODES Batu 2024

> **Backend API Server untuk Dashboard Visualisasi Data PODES Batu 2024**

Dokumentasi ini ditujukan untuk developer yang akan melanjutkan pengembangan sistem. Bacalah dengan teliti sebelum melakukan modifikasi apapun.

---

## 📁 Daftar Isi

1. [Gambaran Umum Sistem](#1-gambaran-umum-sistem)
2. [Struktur Folder](#2-struktur-folder)
3. [Cara Menjalankan](#3-cara-menjalankan)
4. [API Endpoints](#4-api-endpoints)
5. [Arsitektur & Alur Data](#5-arsitektur--alur-data)
6. [Penjelasan Setiap File](#6-penjelasan-setiap-file)
7. [Cara Menambah Indikator Baru](#7-cara-menambah-indikator-baru)
8. [Troubleshooting](#8-troubleshooting)

---

## 1. Gambaran Umum Sistem

Backend ini adalah **REST API Server** yang menyediakan data PODES (Potensi Desa) Kota Batu tahun 2024 untuk dikonsumsi oleh aplikasi frontend (React). 

### Teknologi yang Digunakan:
| Teknologi | Versi | Fungsi |
|-----------|-------|--------|
| Node.js | 18+ | Runtime JavaScript |
| Express.js | 4.18.2 | Web framework |
| CORS | 2.8.5 | Mengizinkan akses dari frontend |
| Helmet | 7.0.0 | Security headers |
| Morgan | 1.10.0 | HTTP request logging |
| Compression | 1.7.4 | Kompresi response |

### Cara Kerja Singkat:
1. Saat server dijalankan, data JSON (`data_podes_2024.json`) di-load ke **memory**
2. Setiap request dari frontend akan membaca data dari memory (bukan dari file)
3. Filtering dan kalkulasi dilakukan di backend, lalu hasilnya dikirim ke frontend

---

## 2. Struktur Folder

```
Backend-PODES/
├── server.js              # Entry point utama, konfigurasi Express
├── package.json           # Dependencies dan scripts
├── controllers/
│   └── villageController.js   # Logic bisnis & pemrosesan data
├── routes/
│   └── villages.js        # Definisi endpoint API
├── data/
│   ├── data_podes_2024.json   # Data utama PODES (24 desa)
│   ├── kelurahan.geojson      # Data geospasial polygon desa
│   └── podes_aggregated_per_desa.csv  # Backup data format CSV
└── docs/
    └── README.md          # Dokumentasi ini
```

---

## 3. Cara Menjalankan

### Prasyarat:
- Node.js versi 18 atau lebih baru
- npm (sudah termasuk dengan Node.js)

### Langkah-langkah:

```bash
# 1. Masuk ke folder backend
cd Backend-PODES

# 2. Install dependencies (hanya perlu sekali)
npm install

# 3. Jalankan server (development mode dengan auto-reload)
npm run dev

# ATAU jalankan server (production mode)
npm start
```

### Hasil yang Diharapkan:
```
✅ PODES data loaded successfully: 24 villages
🚀 PODES Batu Server running on port 5001
📍 Health check: http://localhost:5001/api/health
🌐 API Base URL: http://localhost:5001/api
```

### Mengubah Port:
Jika port 5001 sudah terpakai, ubah dengan environment variable:
```bash
PORT=3001 npm run dev
```

---

## 4. API Endpoints

### 4.1 Health Check
**Endpoint:** `GET /api/health`

Untuk mengecek apakah server berjalan dengan baik.

**Response:**
```json
{
  "status": "OK",
  "message": "PODES Batu API Server is running",
  "timestamp": "2024-11-27T10:30:00.000Z",
  "dataCount": 24
}
```

---

### 4.2 Get All Villages (dengan Filter)
**Endpoint:** `GET /api/villages`

Mengambil data semua desa dengan opsi filtering.

**Query Parameters:**
| Parameter | Tipe | Contoh | Deskripsi |
|-----------|------|--------|-----------|
| `kecamatan` | string | `Batu` | Filter berdasarkan kecamatan |
| `desa` | string | `Oro-oro Ombo` | Filter berdasarkan nama desa |
| `category` | string | `Pendidikan` | Kategori indikator |
| `indicator` | string | `jumlah_sd` | Indikator spesifik |

**Contoh Request:**
```
GET /api/villages?kecamatan=Batu&indicator=jumlah_sd
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id_desa": 1,
      "nama_desa": "Oro-oro Ombo",
      "nama_kecamatan": "Batu",
      "jumlah_sd": 5,
      "jumlah_smp": 2,
      ...
    }
  ],
  "count": 8,
  "filters": {
    "kecamatan": "Batu",
    "indicator": "jumlah_sd"
  },
  "kpis": {
    "type": "quantitative",
    "total": 25,
    "average": 3.13,
    "max": 5,
    "min": 1,
    "count": 8
  },
  "rankingData": [...],
  "indicatorType": "quantitative"
}
```

---

### 4.3 Compare Villages
**Endpoint:** `GET /api/villages/compare`

Membandingkan data antar desa berdasarkan ID.

**Query Parameters:**
| Parameter | Tipe | Contoh | Deskripsi |
|-----------|------|--------|-----------|
| `ids` | string | `1,2,3` | ID desa yang dibandingkan (comma-separated) |

**Contoh Request:**
```
GET /api/villages/compare?ids=1,2,5
```

**Response:**
```json
{
  "success": true,
  "data": [
    { "id_desa": 1, "nama_desa": "Oro-oro Ombo", ... },
    { "id_desa": 2, "nama_desa": "Temas", ... },
    { "id_desa": 5, "nama_desa": "Sisir", ... }
  ],
  "count": 3,
  "requestedIds": [1, 2, 5],
  "analysis": {
    "Pendidikan": {
      "jumlah_sd": {
        "label": "Jumlah SD/Sederajat",
        "values": [...],
        "summary": {...}
      }
    }
  }
}
```

---

### 4.4 Get Metadata
**Endpoint:** `GET /api/villages/metadata`

Mengambil metadata untuk filter (daftar kecamatan, desa, kategori, dll).

**Response:**
```json
{
  "success": true,
  "data": {
    "totalVillages": 24,
    "totalKecamatan": 3,
    "kecamatans": ["Semua Kecamatan", "Batu", "Bumiaji", "Junrejo"],
    "desas": ["Beji", "Bulukerto", ...],
    "categoryIndicators": {
      "Pendidikan": {
        "jumlah_tk": "Jumlah TK",
        "jumlah_sd": "Jumlah SD/Sederajat",
        ...
      },
      "Kesehatan": {...},
      ...
    },
    "categories": ["Pendidikan", "Kesehatan", "Infrastruktur & Konektivitas", "Lingkungan & Kebencanaan"]
  }
}
```

---

## 5. Arsitektur & Alur Data

### Diagram Alur:

```
┌─────────────┐     HTTP Request      ┌─────────────────┐
│   Frontend  │ ──────────────────►   │    server.js    │
│   (React)   │                       │   (Express)     │
└─────────────┘                       └────────┬────────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │  routes/        │
                                      │  villages.js    │
                                      └────────┬────────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │  controllers/   │
                                      │  villageController│
                                      └────────┬────────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │  Data di Memory │
                                      │  (podesData)    │
                                      └─────────────────┘
```

### Penjelasan Alur:
1. **Frontend** mengirim HTTP request ke backend (misal: `GET /api/villages?kecamatan=Batu`)
2. **server.js** menerima request dan meneruskan ke middleware (CORS, Helmet, dll)
3. **routes/villages.js** menentukan controller mana yang menangani request
4. **controllers/villageController.js** memproses data:
   - Filtering berdasarkan query parameters
   - Kalkulasi KPI (total, rata-rata, max, min)
   - Generate ranking atau distribusi
5. Response dikirim kembali ke frontend dalam format JSON

---

## 6. Penjelasan Setiap File

### 6.1 `server.js`

**Fungsi:** Entry point aplikasi, konfigurasi Express server.

**Bagian Penting:**

```javascript
// Data di-load ke memory saat server start
let podesData = [];

function loadPodesData() {
  const dataPath = path.join(__dirname, 'data', 'data_podes_2024.json');
  const rawData = fs.readFileSync(dataPath, 'utf8');
  podesData = JSON.parse(rawData);
}

// Middleware untuk inject data ke setiap request
app.use((req, res, next) => {
  req.podesData = podesData;  // Data bisa diakses via req.podesData
  next();
});
```

**Middleware yang Digunakan:**
| Middleware | Fungsi |
|------------|--------|
| `helmet()` | Menambah security headers |
| `cors()` | Mengizinkan request dari origin berbeda (frontend) |
| `compression()` | Kompresi response (gzip) |
| `morgan('combined')` | Logging HTTP requests |

---

### 6.2 `routes/villages.js`

**Fungsi:** Mendefinisikan endpoint URL dan mapping ke controller.

```javascript
// GET /api/villages → getAllVillages
router.get('/', villageController.getAllVillages);

// GET /api/villages/compare → compareVillages
router.get('/compare', villageController.compareVillages);

// GET /api/villages/metadata → getMetadata
router.get('/metadata', villageController.getMetadata);
```

---

### 6.3 `controllers/villageController.js`

**Fungsi:** Semua logic bisnis dan pemrosesan data.

**Fungsi-fungsi Utama:**

| Fungsi | Deskripsi |
|--------|-----------|
| `getCategoryIndicators()` | Mapping kategori ke indikator (Pendidikan → jumlah_tk, jumlah_sd, dll) |
| `getQuantitativeIndicators()` | Daftar indikator numerik (untuk kalkulasi sum, avg, dll) |
| `isQuantitativeIndicator(indicator)` | Cek apakah indikator numerik atau kategorikal |
| `calculateKPIs(data, indicator)` | Hitung KPI (total, avg, max, min untuk numerik; distribusi untuk kategorikal) |
| `generateRankingData(data, indicator)` | Generate top 10 ranking untuk indikator numerik |
| `getAllVillages(req, res)` | Handler untuk GET /api/villages |
| `compareVillages(req, res)` | Handler untuk GET /api/villages/compare |
| `getMetadata(req, res)` | Handler untuk GET /api/villages/metadata |

**Contoh Logic di `calculateKPIs`:**

```javascript
// Untuk indikator NUMERIK (jumlah_sd, jumlah_puskesmas, dll)
if (isQuantitativeIndicator(indicator)) {
  const values = filteredData.map(village => village[indicator] || 0);
  return {
    type: 'quantitative',
    total: values.reduce((sum, val) => sum + val, 0),
    average: total / values.length,
    max: Math.max(...values),
    min: Math.min(...values)
  };
}

// Untuk indikator KATEGORIKAL (status_tps, kekuatan_sinyal, dll)
else {
  const distribution = {};
  filteredData.forEach(village => {
    const value = village[indicator] || 'Tidak Diketahui';
    distribution[value] = (distribution[value] || 0) + 1;
  });
  return {
    type: 'qualitative',
    distribution,  // { "Ada": 15, "Tidak Ada": 9 }
    mostCommon: "Ada"
  };
}
```

---

### 6.4 `data/data_podes_2024.json`

**Fungsi:** Data utama PODES dalam format JSON.

**Struktur Data:**
```json
[
  {
    "id_desa": 1,
    "nama_desa": "Oro-oro Ombo",
    "nama_kecamatan": "Batu",
    
    // Indikator Pendidikan (numerik)
    "jumlah_tk": 3,
    "jumlah_sd": 5,
    "jumlah_smp": 2,
    "jumlah_sma": 1,
    
    // Indikator Kesehatan (numerik)
    "jumlah_rs": 0,
    "jumlah_puskesmas": 1,
    
    // Indikator Infrastruktur (kategorikal)
    "kekuatan_sinyal": "Kuat",
    "jenis_sinyal_internet": "4G",
    "status_penerangan_jalan_surya": "Ada",
    "status_penerangan_jalan_utama": "Ada",
    
    // Indikator Lingkungan (kategorikal)
    "status_peringatan_dini": "Ada",
    "status_tps": "Ada",
    "kebiasaan_bakar_lahan": "Tidak Ada",
    
    // IKG (numerik)
    "ikg_total": 25.5,
    "ikg_pelayanan_dasar": 8.2,
    "ikg_infrastruktur": 10.1,
    "ikg_aksesibilitas": 7.2
  },
  // ... 23 desa lainnya
]
```

---

## 7. Cara Menambah Indikator Baru

### Langkah 1: Tambahkan Data ke JSON
Edit `data/data_podes_2024.json`, tambahkan field baru di setiap objek desa:

```json
{
  "id_desa": 1,
  "nama_desa": "Oro-oro Ombo",
  // ... field existing ...
  "indikator_baru": 10  // Tambahkan field baru
}
```

### Langkah 2: Daftarkan Indikator di Controller
Edit `controllers/villageController.js`:

**Jika NUMERIK**, tambahkan ke `getQuantitativeIndicators()`:
```javascript
const getQuantitativeIndicators = () => {
  return new Set([
    'jumlah_tk', 'jumlah_sd', // ... existing
    'indikator_baru'  // Tambahkan di sini
  ]);
};
```

**Daftarkan ke kategori** di `getCategoryIndicators()`:
```javascript
const getCategoryIndicators = () => {
  return {
    "Pendidikan": {
      // ... existing
      "indikator_baru": "Label Indikator Baru"  // Tambahkan di sini
    }
  };
};
```

### Langkah 3: Restart Server
```bash
npm run dev
```

### Langkah 4: Test via API
```
GET http://localhost:5001/api/villages?indicator=indikator_baru
```

---

## 8. Troubleshooting

### Error: "PODES data loaded: 0 villages"
**Penyebab:** File `data_podes_2024.json` kosong atau format salah.
**Solusi:** Pastikan file berisi array JSON valid dengan minimal 1 objek.

### Error: "EADDRINUSE: port 5001 already in use"
**Penyebab:** Port 5001 sudah digunakan aplikasi lain.
**Solusi:** 
```bash
# Gunakan port berbeda
PORT=3001 npm run dev
```

### Error: "Cannot read property 'filter' of undefined"
**Penyebab:** Data tidak ter-load dengan benar.
**Solusi:** Cek file JSON valid dan restart server.

### CORS Error di Frontend
**Penyebab:** Frontend di-block karena origin berbeda.
**Solusi:** Pastikan middleware `cors()` sudah aktif di server.js.

### Response Lambat (>1 detik)
**Penyebab:** Data terlalu besar atau logic tidak efisien.
**Solusi:** 
1. Pastikan filtering dilakukan di awal
2. Gunakan `compression()` middleware
3. Hindari nested loops yang tidak perlu

---

## 📞 Kontak

Jika ada pertanyaan atau kesulitan, hubungi tim PKL BPS Kota Batu.

---

*Dokumentasi ini terakhir diperbarui: Desember 2025*
