# CLAUDE.md — Instruksi Generate Dataset CSV & Notebook (.ipynb)

## Konteks Proyek

Ini adalah modul pembelajaran **Pertemuan 2** dari Study Group GWE (Enterprise Data Management Laboratory) dengan judul **"Data Analytics Essentials: SQL & Python Foundation"**. Modul dirancang untuk peserta **from zero** (belum pernah belajar Python maupun SQL).

Pendekatan utama: **Python + SQL terintegrasi dalam satu Jupyter Notebook**, menggunakan SQLite (`sqlite3` bawaan Python) sehingga peserta tidak perlu install database terpisah.

---

## BAGIAN A — GENERATE DATASET CSV

### Skema Database: Sistem Akademik Sederhana (3 Tabel)

Generate **3 file CSV** dengan spesifikasi berikut:

### 1. `mahasiswa.csv`

| Kolom | Tipe | Keterangan |
|-------|------|------------|
| mahasiswa_id | INT (PK) | ID unik, mulai dari 1 |
| nim | STRING | Format: 10 digit angka (misal: "1301213001") |
| nama_lengkap | STRING | Nama lengkap mahasiswa Indonesia |
| kelas | STRING | Pilihan: "IF-A", "IF-B", "SI-A", "SI-B" |
| angkatan | INT | Pilihan: 2022, 2023, 2024 |

**Jumlah data:** 20 baris

**Data kotor yang harus disisipkan (untuk latihan error handling):**
- 2 baris dengan `nama_lengkap` kosong/null
- 1 baris dengan `angkatan` null
- 1 baris duplikat NIM (NIM sama tapi mahasiswa_id berbeda)

---

### 2. `mata_kuliah.csv`

| Kolom | Tipe | Keterangan |
|-------|------|------------|
| mk_id | INT (PK) | ID unik, mulai dari 1 |
| kode_mk | STRING | Format: 2 huruf + 3 angka (misal: "IF201", "SI301") |
| nama_mk | STRING | Nama mata kuliah informatika/SI yang realistis |
| sks | INT | Pilihan: 2, 3, atau 4 |
| dosen_pengampu | STRING | Nama dosen Indonesia |

**Jumlah data:** 12 baris

**Contoh mata kuliah yang harus ada:**
- Basis Data, Data Mining, Statistika, Pemrograman Web, Algoritma dan Struktur Data, Kecerdasan Buatan, Jaringan Komputer, dll.

**Data kotor:**
- 1 baris dengan `dosen_pengampu` null
- 1 baris dengan `sks` berisi string "tiga" (bukan angka) — untuk latihan error handling tipe data

---

### 3. `jadwal.csv`

| Kolom | Tipe | Keterangan |
|-------|------|------------|
| jadwal_id | INT (PK) | ID unik, mulai dari 1 |
| mahasiswa_id | INT (FK) | Referensi ke mahasiswa.mahasiswa_id |
| mk_id | INT (FK) | Referensi ke mata_kuliah.mk_id |
| hari | STRING | Pilihan: "Senin", "Selasa", "Rabu", "Kamis", "Jumat" |
| jam_mulai | STRING | Format: "HH:MM" (misal: "08:00", "10:00", "13:00") |
| jam_selesai | STRING | Format: "HH:MM" |
| ruangan | STRING | Format: "GD-XXX" (misal: "GD-301", "GD-102", "LAB-01") |

**Jumlah data:** 60 baris

**Aturan penting:**
- Satu mahasiswa bisa ambil beberapa mata kuliah (rata-rata 3-5 mk per mahasiswa)
- Satu mata kuliah bisa diambil oleh banyak mahasiswa
- **2-3 mahasiswa TIDAK punya jadwal sama sekali** (untuk demo LEFT JOIN vs INNER JOIN)
- **1-2 mata kuliah TIDAK diambil siapa pun** (untuk demo LEFT JOIN)
- Durasi kuliah sesuai SKS: 2 SKS = 1.5 jam, 3 SKS = 2.5 jam, 4 SKS = 3 jam (kurang lebih)

**Data kotor:**
- 2 baris dengan `ruangan` null
- 1 baris dengan `mk_id` yang TIDAK ADA di tabel mata_kuliah (misal: mk_id = 99) — untuk latihan validasi referential integrity
- 1 baris duplikat (jadwal_id berbeda tapi mahasiswa_id, mk_id, dan hari sama persis)

---

## BAGIAN B — GENERATE JUPYTER NOTEBOOK (.ipynb)

### Instruksi Umum Notebook

- Bahasa pengantar: **Bahasa Indonesia**
- Setiap bagian harus ada **cell Markdown** untuk penjelasan teori, lalu **cell Code** untuk praktik
- Penjelasan ditulis santai tapi informatif, cocok untuk mahasiswa
- Setiap cell code harus ada **komentar** yang menjelaskan setiap baris
- Gunakan **emoji** secukupnya di Markdown untuk bikin menarik (misal: 🎯, 📌, 💡, ⚠️)
- Di akhir setiap bagian, berikan **Latihan Mandiri** (1-2 soal) yang harus dikerjakan peserta

---

### Struktur Notebook

#### Bagian 1 — Pengenalan & Tujuan Modul
- Cell Markdown: Judul modul, deskripsi singkat, apa saja yang akan dipelajari
- Sebutkan alur: Python Dasar → Pandas → SQLite → SQL → Integrasi

#### Bagian 2 — Python Dasar (From Zero)

**2.1 Variabel & Assignment**
- Cara menyimpan data ke variabel
- Aturan penamaan variabel
- Contoh: `nama = "Budi"`, `angkatan = 2023`, `ipk = 3.75`, `aktif = True`

**2.2 Tipe Data**
- String, Integer, Float, Boolean
- Cara cek tipe: `type()`
- Konversi tipe: `int()`, `str()`, `float()`

**2.3 Operasi Dasar**
- Aritmatika: `+`, `-`, `*`, `/`, `//` (floor division), `%` (modulus), `**` (pangkat)
- String: concatenation (`+`), repetition (`*`), `.upper()`, `.lower()`, `len()`
- Comparison: `==`, `!=`, `>`, `<`, `>=`, `<=`
- Contoh kasus: hitung total SKS, hitung rata-rata nilai

**2.4 Print & f-string**
- `print()` dasar
- f-string: `print(f"Mahasiswa {nama} angkatan {angkatan}")`
- Format angka: `f"{ipk:.2f}"`

**2.5 List**
- Cara buat: `daftar_mhs = ["Budi", "Ani", "Citra"]`
- Akses index: `daftar_mhs[0]`, `daftar_mhs[-1]`
- Method: `.append()`, `.remove()`, `.sort()`, `len()`
- Slicing: `daftar_mhs[1:3]`
- List of numbers: operasi pada list nilai

**2.6 Dictionary**
- Cara buat: `mhs = {"nim": "1301213001", "nama": "Budi", "kelas": "IF-A"}`
- Akses value: `mhs["nama"]`, `mhs.get("nama")`
- Tambah/update: `mhs["angkatan"] = 2023`
- Method: `.keys()`, `.values()`, `.items()`
- **List of Dictionaries** — representasi tabel/banyak baris data:
  ```python
  data_mhs = [
      {"nim": "1301213001", "nama": "Budi", "kelas": "IF-A"},
      {"nim": "1301213002", "nama": "Ani", "kelas": "IF-B"},
  ]
  ```

**Latihan Mandiri Bagian 2:**
- Buat dictionary untuk 1 mata kuliah (kode, nama, sks, dosen)
- Buat list berisi 3 dictionary mahasiswa, lalu cetak nama tiap mahasiswa pakai loop

---

#### Bagian 3 — Control Flow

**3.1 If-Elif-Else**
- Sintaks dasar
- Contoh: cek angkatan → cetak "Senior" atau "Junior"
- Contoh: labelisasi SKS → "ringan" (< 12), "normal" (12-20), "berat" (> 20)

**3.2 For Loop**
- Iterasi list
- Iterasi dictionary
- Iterasi list of dictionaries (data mahasiswa)
- `range()` — for i in range(...)
- `enumerate()` — for index, value in enumerate(...)

**3.3 While Loop (singkat)**
- Contoh sederhana: hitung mundur, input validasi

**3.4 Fungsi (def)**
- Cara bikin fungsi dengan parameter dan return
- Contoh: `def hitung_total_sks(list_sks):` → return total
- Contoh: `def label_beban(total_sks):` → return "ringan"/"normal"/"berat"
- Fungsi dengan default parameter

**Latihan Mandiri Bagian 3:**
- Buat fungsi yang menerima list nilai angka dan return rata-rata
- Buat fungsi yang menerima dictionary mahasiswa dan cetak info lengkapnya

---

#### Bagian 4 — Error Handling

**4.1 Apa itu Error?**
- Contoh error umum: `NameError`, `TypeError`, `KeyError`, `ValueError`, `ZeroDivisionError`
- Tunjukkan error terjadi (biarkan cell error, lalu jelaskan)

**4.2 Try-Except**
- Sintaks dasar
- Contoh: konversi `"tiga"` ke `int()` — `ValueError`
- Contoh: akses key dictionary yang tidak ada — `KeyError`

**4.3 Try-Except-Else-Finally**
- Pattern lengkap
- Contoh: baca data, kalau berhasil proses, kalau gagal skip dan catat

**4.4 Validasi Data**
- Contoh fungsi: cek apakah nilai SKS valid (integer, bukan string)
- Contoh fungsi: cek apakah data mahasiswa lengkap (tidak ada null)

**Latihan Mandiri Bagian 4:**
- Buat fungsi yang menerima list campuran (angka dan string), lalu return hanya yang bisa dikonversi ke angka

---

#### Bagian 5 — Pengenalan Pandas & Membuat Dataset CSV

**5.1 Apa itu Pandas?**
- Penjelasan singkat: library untuk olah data tabular
- `import pandas as pd`

**5.2 Membuat DataFrame dari Dictionary**
- Hubungkan ke Bagian 2 (dictionary → DataFrame)
- Buat DataFrame `mahasiswa`, `mata_kuliah`, `jadwal`
- Gunakan data sesuai spesifikasi CSV di BAGIAN A

**5.3 Eksplorasi DataFrame**
- `.head()`, `.tail()`, `.shape`, `.info()`, `.describe()`
- `.dtypes` — cek tipe data tiap kolom
- `.isnull().sum()` — cek data kosong

**5.4 Simpan ke CSV**
- `df.to_csv("mahasiswa.csv", index=False)`
- Lakukan untuk ketiga tabel

**Latihan Mandiri Bagian 5:**
- Tambahkan 2 baris data mahasiswa baru ke DataFrame, lalu simpan ulang

---

#### Bagian 6 — Load CSV ke SQLite

**6.1 Pengenalan SQLite**
- Apa itu SQLite, kenapa cocok untuk pembelajaran
- `import sqlite3`

**6.2 Koneksi & Load Data**
```python
conn = sqlite3.connect("akademik.db")
df_mahasiswa = pd.read_csv("mahasiswa.csv")
df_mahasiswa.to_sql("mahasiswa", conn, if_exists="replace", index=False)
# Ulangi untuk mata_kuliah dan jadwal
```

**6.3 Verifikasi**
- Query sederhana untuk cek data sudah masuk:
```python
pd.read_sql_query("SELECT * FROM mahasiswa LIMIT 5", conn)
```

---

#### Bagian 7 — SQL for Data Analysis

**Semua query dijalankan melalui `pd.read_sql_query()` di dalam notebook.**

**7.1 SELECT & WHERE**
- `SELECT * FROM mahasiswa` — ambil semua
- `SELECT nama_lengkap, kelas FROM mahasiswa WHERE angkatan = 2023`
- `SELECT * FROM mata_kuliah WHERE sks >= 3`
- Operator: `=`, `!=`, `>`, `<`, `LIKE`, `IN`, `BETWEEN`, `IS NULL`

**7.2 ORDER BY & DISTINCT**
- Urutkan mahasiswa berdasarkan nama
- Ambil daftar kelas unik

**7.3 Aggregation**
- `COUNT(*)` — jumlah mahasiswa per kelas
- `SUM(sks)` — total SKS per mahasiswa (butuh JOIN, teaser)
- `AVG(sks)` — rata-rata SKS mata kuliah
- `GROUP BY` — kelompokkan per kelas, per angkatan
- `HAVING` — filter hasil agregasi (misal: kelas yang jumlah mahasiswanya > 5)

**7.4 JOIN**
- **INNER JOIN** — gabungkan jadwal + mahasiswa
  ```sql
  SELECT m.nama_lengkap, mk.nama_mk, j.hari, j.jam_mulai
  FROM jadwal j
  INNER JOIN mahasiswa m ON j.mahasiswa_id = m.mahasiswa_id
  INNER JOIN mata_kuliah mk ON j.mk_id = mk.mk_id
  ```
- **LEFT JOIN** — tampilkan SEMUA mahasiswa termasuk yang belum punya jadwal
  ```sql
  SELECT m.nama_lengkap, j.hari, mk.nama_mk
  FROM mahasiswa m
  LEFT JOIN jadwal j ON m.mahasiswa_id = j.mahasiswa_id
  LEFT JOIN mata_kuliah mk ON j.mk_id = mk.mk_id
  ```
- Jelaskan perbedaan hasil INNER vs LEFT JOIN (ada mahasiswa yang muncul di LEFT tapi tidak di INNER)

**7.5 Studi Kasus KPI**
- Total SKS yang diambil tiap mahasiswa
- Mata kuliah paling banyak diambil
- Dosen dengan beban mengajar terbanyak
- Mahasiswa yang belum mengambil mata kuliah apapun

**Latihan Mandiri Bagian 7:**
- Query: Tampilkan nama mahasiswa angkatan 2023 yang mengambil mata kuliah dengan SKS >= 3
- Query: Hitung jumlah mata kuliah per hari

---

#### Bagian 8 — Integrasi: SQL + Python

**8.1 Tampung Hasil Query ke Variabel Python**
```python
result = pd.read_sql_query("SELECT ...", conn)
list_nama = result["nama_lengkap"].tolist()  # → List
dict_mhs = result.to_dict("records")          # → List of Dictionaries
```

**8.2 Labelisasi dengan Control Flow**
- Query total SKS per mahasiswa
- Loop hasilnya, pakai if-else untuk labeli: "ringan", "normal", "berat"

**8.3 Validasi Data dengan Error Handling**
- Cek data null dari hasil query
- Cek referential integrity: apakah semua mk_id di jadwal ada di mata_kuliah?
- Tangani data kotor (SKS yang berisi string "tiga")
- Buat fungsi `validasi_jadwal()` yang cek dan laporkan masalah

**8.4 Fungsi Lengkap: Query + Proses + Return**
```python
def laporan_mahasiswa(mahasiswa_id, conn):
    # 1. Query data mahasiswa + jadwal + mk (JOIN)
    # 2. Validasi (error handling)
    # 3. Hitung total SKS
    # 4. Labelisasi beban
    # 5. Return dictionary laporan lengkap
```

---

#### Bagian 9 — Mini Project (Latihan Akhir)

**Judul: "Laporan Akademik Otomatis"**

Instruksi untuk peserta:
1. Query seluruh data jadwal lengkap (JOIN 3 tabel)
2. Tampung ke variabel Python
3. Untuk setiap mahasiswa:
   - Hitung total SKS
   - Beri label beban SKS
   - Cek apakah ada data tidak valid
4. Cetak laporan ringkasan dalam format yang rapi
5. Handle semua kemungkinan error (null, tipe data salah, FK tidak valid)

---

## CATATAN TEKNIS

- **Python version:** 3.x
- **Library yang digunakan:** `pandas`, `sqlite3` (keduanya sudah built-in atau sangat umum)
- **TIDAK perlu install database eksternal** — semua pakai SQLite in-memory atau file `.db`
- **Semua dilakukan di dalam Jupyter Notebook** — peserta tidak perlu buka terminal atau tools lain
- **File CSV dibuat di dalam notebook** menggunakan Pandas, BUKAN file terpisah yang di-upload
- Notebook harus bisa dijalankan dari atas ke bawah tanpa error (kecuali cell yang memang sengaja untuk demo error)