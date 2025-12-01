# 🔧 Troubleshooting Guide - Backend PODES Batu 2024

Panduan lengkap untuk mengatasi masalah umum pada Backend API.

---

## Quick Diagnostics

### Health Check

```bash
# Cek apakah server running
curl http://localhost:5001/api/health

# Expected response:
{
  "status": "OK",
  "timestamp": "2024-12-02T10:30:00.000Z",
  "dataCount": 24
}
```

### Log Check

```bash
# Lihat log saat menjalankan server
cd Backend-PODES
npm start

# Perhatikan output:
# ✓ Data loaded: 24 villages
# ✓ Server running on port 5001
```

---

## Common Issues

### 🔴 Error: Cannot find module 'X'

**Gejala:**
```
Error: Cannot find module 'express'
    at Function.Module._resolveFilename
```

**Penyebab:** Dependencies belum terinstall.

**Solusi:**
```bash
cd Backend-PODES
npm install
```

Jika masih error:
```bash
rm -rf node_modules
rm package-lock.json
npm install
```

---

### 🔴 Error: EADDRINUSE (Port already in use)

**Gejala:**
```
Error: listen EADDRINUSE: address already in use :::5001
```

**Penyebab:** Port 5001 sudah dipakai proses lain.

**Solusi Windows (PowerShell):**
```powershell
# Cari proses yang menggunakan port 5001
netstat -ano | Select-String ":5001"

# Output: TCP    0.0.0.0:5001    LISTENING    12345
# 12345 adalah PID

# Kill proses
taskkill /PID 12345 /F

# Atau gunakan port berbeda
$env:PORT=5002; npm start
```

**Solusi Linux/Mac:**
```bash
# Cari proses
lsof -i :5001

# Kill proses
kill -9 <PID>

# Atau gunakan port berbeda
PORT=5002 npm start
```

---

### 🔴 Error: ENOENT (File not found)

**Gejala:**
```
Error: ENOENT: no such file or directory, open './data/data_podes_2024.json'
```

**Penyebab:** File data tidak ditemukan.

**Solusi:**

1. Pastikan struktur folder benar:
```
Backend-PODES/
├── data/
│   ├── data_podes_2024.json    ← Harus ada
│   └── kelurahan.geojson
├── server.js
└── ...
```

2. Cek path di server.js:
```javascript
// Path harus relatif terhadap lokasi server.js
const DATA_PATH = path.join(__dirname, 'data', 'data_podes_2024.json');
```

3. Jika menjalankan dari folder berbeda:
```bash
# Selalu cd ke folder Backend-PODES dulu
cd Backend-PODES
npm start
```

---

### 🔴 Error: JSON Parse Error

**Gejala:**
```
SyntaxError: Unexpected token X in JSON at position Y
```

**Penyebab:** File JSON tidak valid.

**Debug:**
```bash
# Validate JSON file
node -e "JSON.parse(require('fs').readFileSync('./data/data_podes_2024.json', 'utf8'))"
```

**Masalah umum JSON:**
```json
// ❌ Trailing comma
{ "nama": "Batu", }

// ❌ Single quotes
{ 'nama': 'Batu' }

// ❌ Unquoted keys
{ nama: "Batu" }

// ❌ Comments
{ /* comment */ "nama": "Batu" }

// ✅ Correct
{ "nama": "Batu" }
```

**Solusi:**
1. Gunakan online validator: https://jsonlint.com/
2. Fix syntax errors
3. Save dengan encoding UTF-8

---

### 🔴 API Returns Empty Data

**Gejala:** `/api/villages` returns `{ data: [] }`

**Debug:**

```javascript
// Tambahkan log di server.js
const loadData = () => {
  try {
    const rawData = fs.readFileSync(DATA_PATH, 'utf8');
    console.log('Raw data size:', rawData.length, 'bytes');
    
    villagesData = JSON.parse(rawData);
    console.log('Villages loaded:', villagesData.length);
    
    if (villagesData.length > 0) {
      console.log('First village:', villagesData[0].nama_desa);
    }
  } catch (err) {
    console.error('Load failed:', err.message);
  }
};
```

**Penyebab umum:**
1. File JSON kosong
2. JSON adalah object bukan array
3. File tidak terbaca

**Cek format JSON:**
```javascript
// Harus array di root level
[
  { "id_desa": 1, "nama_desa": "Oro-oro Ombo", ... },
  { "id_desa": 2, "nama_desa": "Temas", ... },
  ...
]

// BUKAN object
{
  "data": [...]  // Salah jika code expect array
}
```

---

### 🔴 CORS Error

**Gejala (di browser console):**
```
Access to XMLHttpRequest at 'http://localhost:5001/api/villages' 
from origin 'http://localhost:5173' has been blocked by CORS policy
```

**Penyebab:** CORS middleware tidak dikonfigurasi dengan benar.

**Solusi:**

```javascript
// server.js
const cors = require('cors');

// Development - allow all
app.use(cors());

// Production - specific origins
app.use(cors({
  origin: [
    'http://localhost:5173',
    'http://localhost:3000',
    'https://yourdomain.com'
  ],
  credentials: true
}));
```

---

### 🔴 Request Timeout

**Gejala:** Request hangs dan timeout.

**Penyebab:**
1. Server overloaded
2. Data terlalu besar
3. Infinite loop di code

**Debug:**
```javascript
// Tambahkan timing log
app.get('/api/villages', (req, res) => {
  console.time('villages-request');
  // ... process
  console.timeEnd('villages-request');
  res.json(result);
});
```

**Solusi:**
```javascript
// Set timeout untuk request
app.use((req, res, next) => {
  res.setTimeout(30000, () => {
    res.status(408).json({ error: 'Request timeout' });
  });
  next();
});
```

---

### 🔴 Memory Issues

**Gejala:**
```
FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - JavaScript heap out of memory
```

**Penyebab:** Data terlalu besar untuk memory.

**Solusi:**
```bash
# Increase Node.js memory limit
node --max-old-space-size=4096 server.js

# Atau set via environment
NODE_OPTIONS=--max-old-space-size=4096 npm start
```

---

### 🔴 Encoding Issues (Karakter Aneh)

**Gejala:** Nama desa muncul sebagai karakter aneh: `Oro-oro Ombo` → `Oro-oro Omboï¿½`

**Penyebab:** File tidak di-save dengan UTF-8 encoding.

**Solusi:**

1. Di VS Code, cek encoding di status bar (kanan bawah)
2. Jika bukan UTF-8, klik dan pilih "Reopen with Encoding" → UTF-8
3. Save ulang

```bash
# Convert file ke UTF-8 (Linux/Mac)
iconv -f ISO-8859-1 -t UTF-8 data_podes_2024.json > data_podes_2024_utf8.json
```

---

## API Endpoint Issues

### 🔴 404 Not Found

**Gejala:** API returns 404.

**Debug:**
```bash
# Cek route yang benar
curl http://localhost:5001/api/villages        # ✓
curl http://localhost:5001/villages           # ✗ Missing /api
curl http://localhost:5001/api/village        # ✗ Typo (singular)
```

**Cek routing:**
```javascript
// server.js harus ada:
app.use('/api', villagesRouter);

// routes/villages.js harus ada:
router.get('/villages', controller.getAllVillages);
router.get('/villages/compare', controller.getComparison);
router.get('/villages/metadata', controller.getMetadata);
```

---

### 🔴 Query Parameters Not Working

**Gejala:** Filter tidak bekerja: `/api/villages?kecamatan=Batu` returns all data.

**Debug di controller:**
```javascript
exports.getAllVillages = async (req, res) => {
  console.log('Query params:', req.query);
  console.log('Kecamatan filter:', req.query.kecamatan);
  
  // ...
};
```

**Penyebab umum:**
1. Parameter name typo
2. Case mismatch
3. Filter logic salah

**Fix:**
```javascript
// Case-insensitive comparison
const kecamatan = req.query.kecamatan;
if (kecamatan && kecamatan !== 'Semua Kecamatan') {
  filtered = filtered.filter(v => 
    v.kecamatan.toLowerCase() === kecamatan.toLowerCase()
  );
}
```

---

### 🔴 Comparison Endpoint Returns Empty

**Gejala:** `/api/villages/compare?ids=1,2,3` returns `[]`.

**Debug:**
```javascript
exports.getComparison = async (req, res) => {
  const idsParam = req.query.ids;
  console.log('Raw ids param:', idsParam);
  
  const ids = idsParam.split(',').map(id => parseInt(id.trim()));
  console.log('Parsed ids:', ids);
  
  const results = villagesData.filter(v => ids.includes(v.id_desa));
  console.log('Results count:', results.length);
  
  // ...
};
```

**Penyebab umum:**
1. ID format mismatch (string vs number)
2. Field name salah (`id_desa` vs `id`)
3. Data tidak punya field ID

**Fix:**
```javascript
// Flexible ID matching
const ids = idsParam.split(',').map(id => {
  const num = parseInt(id.trim());
  return isNaN(num) ? id.trim() : num;
});

const results = villagesData.filter(v => 
  ids.includes(v.id_desa) || ids.includes(String(v.id_desa))
);
```

---

## Debugging Tips

### Enable Detailed Logging

```javascript
// Di server.js
const morgan = require('morgan');

// Detailed request logging
app.use(morgan('dev'));

// Atau custom format
app.use(morgan(':method :url :status :response-time ms - :res[content-length]'));
```

### Console Log Strategy

```javascript
// Request lifecycle
app.use((req, res, next) => {
  console.log('→ Request:', req.method, req.url);
  console.log('  Query:', JSON.stringify(req.query));
  console.log('  Body:', JSON.stringify(req.body));
  
  const start = Date.now();
  res.on('finish', () => {
    console.log('← Response:', res.statusCode, `${Date.now() - start}ms`);
  });
  
  next();
});
```

### Error Handling

```javascript
// Global error handler
app.use((err, req, res, next) => {
  console.error('Error:', err.message);
  console.error('Stack:', err.stack);
  
  res.status(500).json({
    success: false,
    error: process.env.NODE_ENV === 'development' 
      ? err.message 
      : 'Internal server error'
  });
});

// Unhandled promise rejections
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection:', reason);
});
```

---

## Testing API

### Using curl

```bash
# Health check
curl http://localhost:5001/api/health

# Get all villages
curl http://localhost:5001/api/villages

# Get with filter
curl "http://localhost:5001/api/villages?kecamatan=Batu"

# Get metadata
curl http://localhost:5001/api/villages/metadata

# Compare villages
curl "http://localhost:5001/api/villages/compare?ids=1,2,3"

# Pretty print JSON
curl http://localhost:5001/api/villages | python -m json.tool
```

### Using Postman/Insomnia

1. Import collection atau buat manual
2. Set base URL: `http://localhost:5001/api`
3. Test setiap endpoint

### Using Browser

Langsung buka di browser:
- http://localhost:5001/api/health
- http://localhost:5001/api/villages
- http://localhost:5001/api/villages/metadata

---

## Production Issues

### 🔴 Server Crashes dan Tidak Restart

**Solusi:** Gunakan PM2

```bash
# Install PM2
npm install -g pm2

# Start dengan PM2
pm2 start server.js --name "podes-api"

# Auto restart on crash
pm2 startup
pm2 save

# Monitor
pm2 logs
pm2 monit
```

### 🔴 Environment Variables Not Loading

```javascript
// Pastikan dotenv di load paling awal
require('dotenv').config();

// SEBELUM line lain
const express = require('express');
```

---

## Getting Help

Jika masih stuck:

1. **Read error message carefully** - usually tells you what's wrong
2. **Check logs** - add more console.log if needed  
3. **Google the error** - someone else probably had same issue
4. **Isolate the problem** - comment out code to find the issue
5. **Ask with context**:
   - Full error message
   - Relevant code snippet
   - What you've tried
   - Environment (Node version, OS)

---

*Troubleshooting Guide Backend terakhir diperbarui: Desember 2024*
