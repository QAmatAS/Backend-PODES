# 🖥️ Dashboard PODES Batu 2024 - Backend

REST API untuk Dashboard Potensi Desa (PODES) Kota Batu tahun 2024.

## 🚀 Quick Start

```bash
# Install dependencies
npm install

# Run development server
npm run dev
```

Server berjalan di: http://localhost:5001

## 🛠️ Tech Stack

- **Node.js** + **Express.js**
- **CORS, Helmet, Morgan** - Middleware
- **JSON** - Data storage

## 📡 API Endpoints

| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/api/villages` | Data semua desa (dengan filter) |
| GET | `/api/villages/compare?ids=1,2,3` | Perbandingan desa |
| GET | `/api/villages/metadata` | Metadata untuk filter |
| GET | `/api/health` | Health check |

## 📁 Struktur Utama

```
├── server.js           # Entry point
├── controllers/        # Business logic
├── routes/             # API routes
└── data/               # JSON & GeoJSON data
```

## 📖 Dokumentasi Lengkap

Lihat folder `docs/` untuk dokumentasi detail:
- `docs/README.md` - Overview lengkap
- `docs/API_REFERENCE.md` - Referensi API
- `docs/DATA_SCHEMA.md` - Skema data