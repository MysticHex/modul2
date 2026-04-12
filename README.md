# modul2

Repo ini sudah siap dipakai untuk versioning dan deployment dasar project kamu (notebook + dataset akademik).

## Struktur

- `modul2_akademik.ipynb` — notebook utama
- `mahasiswa.csv`, `mata_kuliah.csv`, `jadwal.csv` — dataset
- `requirements.txt` — dependency Python
- `Dockerfile` — container runtime
- `.github/workflows/ci.yml` — validasi otomatis di GitHub Actions

## Jalankan lokal

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

## Build container (opsional)

```bash
docker build -t modul2-akademik .
docker run -p 8888:8888 modul2-akademik
```

## Push ke GitHub

```bash
git add .
git commit -m "Initial repo setup for deployment"
git remote add origin <URL_REPO_GITHUB_KAMU>
git push -u origin main
```

