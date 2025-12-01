# API Reference - Backend PODES Batu 2024

Dokumentasi lengkap semua endpoint API yang tersedia.

---

## Base URL

```
Development: http://localhost:5001/api
Production:  https://[your-domain]/api
```

---

## Endpoints

### 1. Health Check

```http
GET /api/health
```

#### Response
```json
{
  "status": "OK",
  "message": "PODES Batu API Server is running",
  "timestamp": "2024-12-02T10:30:00.000Z",
  "dataCount": 24
}
```

#### Status Codes
| Code | Description |
|------|-------------|
| 200 | Server berjalan normal |
| 500 | Server error |

---

### 2. Get All Villages

```http
GET /api/villages
```

Mengambil data semua desa dengan opsi filtering.

#### Query Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `kecamatan` | string | No | - | Filter by kecamatan name |
| `desa` | string | No | - | Filter by desa name |
| `category` | string | No | - | Filter category (Pendidikan, Kesehatan, dll) |
| `indicator` | string | No | - | Specific indicator key |

#### Example Requests

```bash
# Get all villages
curl http://localhost:5001/api/villages

# Filter by kecamatan
curl http://localhost:5001/api/villages?kecamatan=Batu

# Filter by indicator
curl "http://localhost:5001/api/villages?indicator=jumlah_sd"

# Combined filters
curl "http://localhost:5001/api/villages?kecamatan=Batu&indicator=jumlah_sd"
```

#### Response

```json
{
  "success": true,
  "data": [
    {
      "id_desa": 1,
      "nama_desa": "Oro-oro Ombo",
      "nama_kecamatan": "Batu",
      "jumlah_tk": 3,
      "jumlah_sd": 5,
      "jumlah_smp": 2,
      "jumlah_sma": 1,
      "jumlah_rs": 0,
      "jumlah_puskesmas": 1,
      "kekuatan_sinyal": "Kuat",
      "status_tps": "Ada"
    }
  ],
  "count": 24,
  "filters": {
    "kecamatan": null,
    "category": null,
    "indicator": null,
    "desa": null
  },
  "kpis": null,
  "rankingData": null,
  "chartData": null,
  "indicatorType": null
}
```

#### Response with Indicator (Quantitative)

```json
{
  "success": true,
  "data": [...],
  "count": 24,
  "filters": {
    "indicator": "jumlah_sd"
  },
  "kpis": {
    "type": "quantitative",
    "total": 75,
    "average": 3.13,
    "max": 7,
    "min": 1,
    "count": 24,
    "topVillage": {
      "name": "Sisir",
      "kecamatan": "Batu",
      "value": 7
    }
  },
  "rankingData": [
    { "nama_desa": "Sisir", "nama_kecamatan": "Batu", "value": 7 },
    { "nama_desa": "Temas", "nama_kecamatan": "Batu", "value": 6 }
  ],
  "indicatorType": "quantitative"
}
```

#### Response with Indicator (Qualitative)

```json
{
  "success": true,
  "data": [...],
  "count": 24,
  "filters": {
    "indicator": "status_tps"
  },
  "kpis": {
    "type": "qualitative",
    "distribution": {
      "Ada": 18,
      "Tidak Ada": 6
    },
    "percentages": {
      "Ada": 75,
      "Tidak Ada": 25
    },
    "total": 24,
    "mostCommon": "Ada",
    "categories": 2
  },
  "chartData": [
    { "name": "Ada", "value": 18, "percentage": 75 },
    { "name": "Tidak Ada", "value": 6, "percentage": 25 }
  ],
  "indicatorType": "qualitative"
}
```

---

### 3. Compare Villages

```http
GET /api/villages/compare
```

Membandingkan data beberapa desa berdasarkan ID.

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `ids` | string | **Yes** | Comma-separated village IDs (min 2) |

#### Example Request

```bash
curl "http://localhost:5001/api/villages/compare?ids=1,2,5"
```

#### Response

```json
{
  "success": true,
  "data": [
    {
      "id_desa": 1,
      "nama_desa": "Oro-oro Ombo",
      "nama_kecamatan": "Batu",
      "jumlah_sd": 5
    },
    {
      "id_desa": 2,
      "nama_desa": "Temas",
      "nama_kecamatan": "Batu",
      "jumlah_sd": 6
    }
  ],
  "count": 2,
  "requestedIds": [1, 2, 5],
  "analysis": {
    "Pendidikan": {
      "jumlah_tk": {
        "label": "Jumlah TK",
        "values": [
          { "desa": "Oro-oro Ombo", "kecamatan": "Batu", "value": 3 },
          { "desa": "Temas", "kecamatan": "Batu", "value": 4 }
        ],
        "summary": {
          "type": "quantitative",
          "total": 7,
          "average": 3.5,
          "max": 4,
          "min": 3
        }
      }
    },
    "Kesehatan": {...},
    "Infrastruktur & Konektivitas": {...},
    "Lingkungan & Kebencanaan": {...}
  }
}
```

#### Error Responses

```json
// Missing ids parameter
{
  "success": false,
  "error": "Village IDs are required",
  "message": "Please provide ids parameter with comma-separated village IDs"
}

// Less than 2 villages
{
  "success": false,
  "error": "At least 2 villages required for comparison",
  "message": "Please provide at least 2 village IDs"
}

// No villages found
{
  "success": false,
  "error": "No villages found",
  "message": "None of the provided IDs match existing villages"
}
```

---

### 4. Get Metadata

```http
GET /api/villages/metadata
```

Mengambil metadata untuk membangun UI filter.

#### Example Request

```bash
curl http://localhost:5001/api/villages/metadata
```

#### Response

```json
{
  "success": true,
  "data": {
    "totalVillages": 24,
    "totalKecamatan": 3,
    "kecamatans": [
      "Semua Kecamatan",
      "Batu",
      "Bumiaji",
      "Junrejo"
    ],
    "desas": [
      "Beji",
      "Bulukerto",
      "Bumiaji",
      "Giripurno",
      "Gunungsari",
      "Junrejo",
      "Mojorejo",
      "Ngaglik",
      "Oro-oro Ombo",
      "Pandanrejo",
      "Pendem",
      "Pesanggrahan",
      "Punten",
      "Sidomulyo",
      "Sisir",
      "Sumberejo",
      "Sumbergondo",
      "Temas",
      "Tlekung",
      "Torongrejo",
      "Tulungrejo"
    ],
    "categoryIndicators": {
      "Pendidikan": {
        "jumlah_tk": "Jumlah TK",
        "jumlah_sd": "Jumlah SD/Sederajat",
        "jumlah_smp": "Jumlah SMP/Sederajat",
        "jumlah_sma": "Jumlah SMA/Sederajat"
      },
      "Kesehatan": {
        "jumlah_rs": "Jumlah Rumah Sakit",
        "jumlah_puskesmas": "Jumlah Puskesmas"
      },
      "Infrastruktur & Konektivitas": {
        "kekuatan_sinyal": "Kualitas Sinyal Internet",
        "jenis_sinyal_internet": "Jenis Sinyal Internet",
        "status_penerangan_jalan_surya": "Penerangan Jalan Tenaga Surya",
        "status_penerangan_jalan_utama": "Penerangan Jalan Utama"
      },
      "Lingkungan & Kebencanaan": {
        "status_peringatan_dini": "Sistem Peringatan Dini",
        "status_alat_keselamatan": "Alat Keselamatan",
        "status_rambu_evakuasi": "Rambu Keselamatan",
        "status_tps": "Tempat Penampungan Sampah (TPS)",
        "status_tps3r": "Tempat Penampungan Sampah 3R (TPS3R)",
        "status_dilakukan_pemilahan_sampah": "Pemilahan Sampah",
        "kebiasaan_pemilahan_sampah": "Kebiasaan Pemilahan Sampah",
        "warga_terlibat_olah_sampah": "Partisipasi Warga Pengolahan Sampah",
        "kebiasaan_bakar_lahan": "Kebiasaan Bakar Lahan"
      }
    },
    "categories": [
      "Pendidikan",
      "Kesehatan",
      "Infrastruktur & Konektivitas",
      "Lingkungan & Kebencanaan"
    ],
    "villages": [...],
    "dataFields": [
      "id_desa",
      "nama_desa",
      "nama_kecamatan",
      "jumlah_tk",
      "jumlah_sd",
      ...
    ]
  }
}
```

---

## Error Handling

### Standard Error Response

```json
{
  "success": false,
  "error": "Error Type",
  "message": "Detailed error message"
}
```

### HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 400 | Bad Request (missing/invalid parameters) |
| 404 | Not Found (invalid endpoint or no data) |
| 500 | Internal Server Error |

---

## Rate Limiting

Saat ini tidak ada rate limiting. Untuk production, pertimbangkan menggunakan:
- `express-rate-limit` package
- Nginx rate limiting
- Cloud provider rate limiting

---

## CORS Configuration

Backend mengizinkan semua origins secara default. Untuk production, ubah konfigurasi CORS di `server.js`:

```javascript
// Restrict to specific origins
app.use(cors({
  origin: ['https://yourdomain.com', 'http://localhost:5173']
}));
```

---

*Dokumentasi API terakhir diperbarui: Desember 2025*
