# Data Schema - PODES Batu 2024

Dokumentasi struktur data yang digunakan dalam sistem.

---

## Data Source

**File:** `data/data_podes_2024.json`

Data bersumber dari Survei Potensi Desa (PODES) 2024 yang dikumpulkan oleh BPS Kota Batu.

---

## Schema Overview

```json
{
  "id_desa": number,           // ID unik desa
  "nama_desa": string,         // Nama desa/kelurahan
  "nama_kecamatan": string,    // Nama kecamatan
  
  // === PENDIDIKAN (Numerik) ===
  "jumlah_tk": number,
  "jumlah_sd": number,
  "jumlah_smp": number,
  "jumlah_sma": number,
  
  // === KESEHATAN (Numerik) ===
  "jumlah_rs": number,
  "jumlah_puskesmas": number,
  
  // === INFRASTRUKTUR & KONEKTIVITAS (Kategorikal) ===
  "kekuatan_sinyal": string,
  "jenis_sinyal_internet": string,
  "status_penerangan_jalan_surya": string,
  "status_penerangan_jalan_utama": string,
  
  // === LINGKUNGAN & KEBENCANAAN (Kategorikal) ===
  "status_peringatan_dini": string,
  "status_alat_keselamatan": string,
  "status_rambu_evakuasi": string,
  "status_tps": string,
  "status_tps3r": string,
  "status_dilakukan_pemilahan_sampah": string,
  "kebiasaan_pemilahan_sampah": string,
  "warga_terlibat_olah_sampah": string,
  "kebiasaan_bakar_lahan": string,
  
  // === IKG - Indeks Kesulitan Geografis (Numerik) ===
  "ikg_total": number,
  "ikg_pelayanan_dasar": number,
  "ikg_infrastruktur": number,
  "ikg_aksesibilitas": number
}
```

---

## Field Details

### Identitas Desa

| Field | Type | Example | Description |
|-------|------|---------|-------------|
| `id_desa` | number | `1` | ID unik desa (1-24) |
| `nama_desa` | string | `"Oro-oro Ombo"` | Nama desa/kelurahan |
| `nama_kecamatan` | string | `"Batu"` | Nama kecamatan |

### Kecamatan di Kota Batu

| Kecamatan | Jumlah Desa |
|-----------|-------------|
| Batu | 8 |
| Bumiaji | 9 |
| Junrejo | 7 |
| **Total** | **24** |

---

### Kategori: Pendidikan

**Tipe Data:** Numerik (untuk kalkulasi sum, avg, max, min)

| Field | Type | Range | Description |
|-------|------|-------|-------------|
| `jumlah_tk` | number | 0-10 | Jumlah Taman Kanak-kanak |
| `jumlah_sd` | number | 0-10 | Jumlah Sekolah Dasar |
| `jumlah_smp` | number | 0-5 | Jumlah Sekolah Menengah Pertama |
| `jumlah_sma` | number | 0-5 | Jumlah Sekolah Menengah Atas |

---

### Kategori: Kesehatan

**Tipe Data:** Numerik

| Field | Type | Range | Description |
|-------|------|-------|-------------|
| `jumlah_rs` | number | 0-3 | Jumlah Rumah Sakit |
| `jumlah_puskesmas` | number | 0-2 | Jumlah Puskesmas |

---

### Kategori: Infrastruktur & Konektivitas

**Tipe Data:** Kategorikal (untuk distribusi)

| Field | Type | Possible Values | Description |
|-------|------|-----------------|-------------|
| `kekuatan_sinyal` | string | `"Kuat"`, `"Sedang"`, `"Lemah"` | Kualitas sinyal seluler |
| `jenis_sinyal_internet` | string | `"4G"`, `"3G"`, `"2G"` | Jenis sinyal internet tersedia |
| `status_penerangan_jalan_surya` | string | `"Ada"`, `"Tidak Ada"` | Ketersediaan penerangan jalan tenaga surya |
| `status_penerangan_jalan_utama` | string | `"Ada"`, `"Tidak Ada"` | Ketersediaan penerangan jalan utama |

---

### Kategori: Lingkungan & Kebencanaan

**Tipe Data:** Kategorikal

| Field | Type | Possible Values | Description |
|-------|------|-----------------|-------------|
| `status_peringatan_dini` | string | `"Ada"`, `"Tidak Ada"` | Sistem peringatan dini bencana |
| `status_alat_keselamatan` | string | `"Ada"`, `"Tidak Ada"` | Ketersediaan alat keselamatan |
| `status_rambu_evakuasi` | string | `"Ada"`, `"Tidak Ada"` | Ketersediaan rambu evakuasi |
| `status_tps` | string | `"Ada"`, `"Tidak Ada"` | Tempat Penampungan Sampah |
| `status_tps3r` | string | `"Tidak ada"`, `"Ada, digunakan"`, `"Ada, tidak digunakan"` | TPS 3R (Reduce, Reuse, Recycle) |
| `status_dilakukan_pemilahan_sampah` | string | `"Ada"`, `"Tidak Ada"` | Pemilahan sampah dilakukan |
| `kebiasaan_pemilahan_sampah` | string | `"Semua Keluarga"`, `"Sebagian Besar Keluarga"`, `"Sebagian Kecil Keluarga"` | Tingkat kebiasaan pemilahan |
| `warga_terlibat_olah_sampah` | string | `"Ada, semua warga terlibat"`, `"Ada, sebagian warga terlibat"`, `"Tidak Ada"` | Partisipasi warga |
| `kebiasaan_bakar_lahan` | string | `"Ada"`, `"Tidak Ada"` | Kebiasaan membakar lahan |

---

### Kategori: IKG (Indeks Kesulitan Geografis)

**Tipe Data:** Numerik (desimal)

| Field | Type | Range | Description |
|-------|------|-------|-------------|
| `ikg_total` | number | 0-100 | Skor IKG Total |
| `ikg_pelayanan_dasar` | number | 0-100 | Skor IKG Pelayanan Dasar |
| `ikg_infrastruktur` | number | 0-100 | Skor IKG Infrastruktur |
| `ikg_aksesibilitas` | number | 0-100 | Skor IKG Aksesibilitas |

**Catatan:** Semakin tinggi nilai IKG, semakin sulit aksesibilitas geografis desa tersebut.

---

## Indicator Classification

### Quantitative Indicators (Numerik)

Indikator ini dihitung dengan operasi matematika (sum, average, max, min):

```javascript
[
  'jumlah_tk',
  'jumlah_sd', 
  'jumlah_smp',
  'jumlah_sma',
  'jumlah_rs',
  'jumlah_puskesmas',
  'jumlah_bts',
  'jumlah_keluarga_pengguna_kayu_bakar',
  'ikg_total',
  'ikg_pelayanan_dasar',
  'ikg_infrastruktur',
  'ikg_aksesibilitas'
]
```

### Qualitative Indicators (Kategorikal)

Indikator ini dihitung distribusinya (count per kategori):

```javascript
[
  'kekuatan_sinyal',
  'jenis_sinyal_internet',
  'status_penerangan_jalan_surya',
  'status_penerangan_jalan_utama',
  'status_peringatan_dini',
  'status_alat_keselamatan',
  'status_rambu_evakuasi',
  'status_tps',
  'status_tps3r',
  'status_dilakukan_pemilahan_sampah',
  'kebiasaan_pemilahan_sampah',
  'warga_terlibat_olah_sampah',
  'kebiasaan_bakar_lahan'
]
```

---

## Sample Data

```json
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
  "jenis_sinyal_internet": "4G",
  "status_penerangan_jalan_surya": "Ada",
  "status_penerangan_jalan_utama": "Ada",
  "status_peringatan_dini": "Ada",
  "status_alat_keselamatan": "Ada",
  "status_rambu_evakuasi": "Ada",
  "status_tps": "Ada",
  "status_tps3r": "Ada, digunakan",
  "status_dilakukan_pemilahan_sampah": "Ada",
  "kebiasaan_pemilahan_sampah": "Sebagian Besar Keluarga",
  "warga_terlibat_olah_sampah": "Ada, sebagian warga terlibat",
  "kebiasaan_bakar_lahan": "Tidak Ada",
  "ikg_total": 25.5,
  "ikg_pelayanan_dasar": 8.2,
  "ikg_infrastruktur": 10.1,
  "ikg_aksesibilitas": 7.2
}
```

---

## GeoJSON Data

**File:** `data/kelurahan.geojson`

Data geospasial untuk visualisasi peta berisi polygon batas wilayah setiap desa.

### Schema

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "nama_desa": "Oro-oro Ombo",
        "nama_kecamatan": "Batu",
        "id_desa": 1
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[lng, lat], [lng, lat], ...]]
      }
    }
  ]
}
```

---

## Adding New Data

### Menambah Desa Baru

1. Tambahkan objek baru di array `data_podes_2024.json`
2. Pastikan `id_desa` unik dan berurutan
3. Isi semua field yang required
4. Restart backend server

### Menambah Field Baru

1. Tambahkan field di setiap objek desa
2. Update schema dokumentasi ini
3. Jika numerik, daftarkan di `getQuantitativeIndicators()`
4. Daftarkan di `getCategoryIndicators()` dengan kategori yang sesuai
5. Update frontend config jika diperlukan

---

*Schema documentation last updated: December 2025*
