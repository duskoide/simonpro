# SiMonPro

Sistem Monitoring Produksi — repo tubes IF2050 DRPL

## Prasyarat

- [Docker Desktop](https://docs.docker.com/get-docker/) (sudah termasuk Docker Compose)

## Menjalankan Database

Di terminal jalankan:

```bash
# Mulai PostgreSQL
docker compose up -d

# Hentikan
docker compose down

# Hentikan + hapus data
docker compose down -v
```

### Mengakses Database

**Linux/macOS** (jika `psql` sudah terinstall):

```bash
psql -U postgres -h localhost -d simonpro
```

**Windows** (lewat Docker):

```bash
docker compose exec postgres psql -U postgres -d simonpro
```

Atau gunakan GUI seperti [pgAdmin](https://www.pgadmin.org/) atau DBeaver dengan koneksi:

| Parameter | Nilai |
|-----------|-------|
| Host | `localhost` |
| Port | `5432` |
| User | `postgres` |
| Password | `secret` |
| Database | `simonpro` |

Konfigurasi bawaan (cocok dengan `backend/internal/config/config.go`):

| Parameter | Nilai |
|-----------|-------|
| Host | `localhost` |
| Port | `5432` |
| User | `postgres` |
| Password | `secret` |
| Database | `simonpro` |

Skema database (`database/init.sql`) dijalankan otomatis saat container pertama kali dibuat.
