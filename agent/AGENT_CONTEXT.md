# SiMonPro — Repo Context

**Updated**: 2026-04-30  
**Project**: SiMonPro — **Si**stem **Mon**itoring **Pro**duksi (Production Monitoring System)  
**Course**: IF2050 DRPL (Software Engineering), STEI-ITB  
**Team**: K01_G08 — 5 members (Yumna, Benedicta, Illona, Rafi, Lukman) + Najwa Kahani Fatima as client  

---

## What This Repo Is

A **desktop application** for small businesses to record, manage, and evaluate daily production. It runs **offline** with a local PostgreSQL 15 database. Two user roles:

| Role | Capabilities |
|------|-------------|
| **Owner** (Pemilik Usaha) | View-only: dashboard, insights, graphs, PDF reports |
| **Admin Produksi** | Full CRUD: products, categories, targets, daily production input, PDF reports |

---

## Tech Stack (from DPPL)

| Layer | Tech |
|-------|------|
| Frontend | JavaScript, Figma (for design) |
| Backend | Go |
| Database | PostgreSQL 15 (local) |
| Target OS | Windows 11, Linux (Debian/RHEL) |
| Reports | PDF generation |

---

## Functional Requirements (SKPL F01–F12)

| ID | Requirement |
|----|-------------|
| F01 | CRUD products (name, code, category, unit, image, description) |
| F02 | CRUD product categories |
| F03 | Input production targets per product per period (daily/weekly/monthly) |
| F04 | Record daily production: date, product code, actual output, defect count, PIC, issues |
| F05 | Validate data: output cannot be negative, defects ≤ output |
| F06 | Calculate target achievement percentage |
| F07 | Calculate defect rate per product |
| F08 | Display performance insights: non-defect trend graph, highest-output products, highest-defect products |
| F09 | Filter production data by product, date range, or period |
| F10 | Generate PDF production reports by period |
| F11 | Display average target achievement percentage per period |
| F12 | Authentication (login/logout) with role-based access |

---

## Non-Functional Requirements

| ID | Parameter | Requirement |
|----|-----------|-------------|
| NF01 | Availability | 99% uptime during operational hours |
| NF02 | Reliability | 99.9% data save success rate |
| NF03 | Ergonomy | New user can input data in ≤ 3 attempts without training |
| NF04 | Portability | Desktop offline operation |
| NF05 | Response time | Save data < 5 seconds |
| NF06 | Security | Role-based access control |
| NF07 | Data Integrity | Auto-validate negative values and defect ≤ production |
| NF08 | Scalability | Handle ≥ 100 production records |
| NF09 | Performance Report | PDF generation < 5 seconds |
| NF10 | Localization | All UI in Bahasa Indonesia |

---

## Use Cases (9 Total)

| UC | Name | Actor(s) |
|----|------|----------|
| UC01 | Login & Logout | Both |
| UC02 | Manage Products (CRUD) | Admin Produksi |
| UC03 | Manage Categories (CRUD) | Admin Produksi |
| UC04 | Manage Production Targets | Admin Produksi |
| UC05 | Input Daily Production | Admin Produksi |
| UC06 | View Target Achievement Insights | Both |
| UC07 | View Defect Rate Insights | Both |
| UC08 | View Performance Summary (Dashboard) | Both |
| UC09 | Generate Production Reports (PDF) | Both |

---

## Architecture (DPPL — 6 Modules)

1. **Auth Module** — login, logout, session, role-based access (CD-02 AuthService, CD-03 Session)  
2. **Product Module** — product and category CRUD (CD-10 ProdukService, CD-15 KategoriService)  
3. **Production Module** — targets + daily production input (CD-19 TargetService, CD-23 ProduksiService)  
4. **Analytics Module** — target achievement %, defect rate, performance insights (CD-26 PencapaianService, CD-29 DefectService, CD-32 PerformaService)  
5. **Report Module** — PDF generation (CD-35 LaporanService)  
6. **Local Database** — PostgreSQL 15

All modules interact through the local database as the central data store.

---

## Design Classes (DPPL — 37 Total)

| ID | Class | Type | Key Attributes / Ops |
|----|-------|------|----------------------|
| CD-01 | User | Entity | userId, username, password, role; checkPassword() |
| CD-02 | AuthService | Service | login(), logout(), validateUser(), hashPassword() |
| CD-03 | Session | Entity | sessionId, userId, loginTime, status; createSession(), endSession() |
| CD-04 | UserDataLocal | Entity | (local user data store) |
| CD-05 | DashboardController | Controller | |
| CD-06 | LoginPage | View | |
| CD-07 | DashboardView | View | Cards: Total Produksi, Pencapaian Target, Tingkat Defect, Produk Aktif; Charts: Pencapaian, Defect |
| CD-08 | Produk | Entity | kodeProduk, namaProduk, deskripsiProduk, satuan, gambar, statusAktif, kategori |
| CD-09 | KategoriProduk | Entity | kategoriId, namaKategori; cekDuplikasi(), simpanPerubahan() |
| CD-10 | ProdukService | Service | tambahProduk(), ubahProduk(), hapusProduk(), validasiProduk(), cekKategoriTersedia() |
| CD-11 | ProdukController | Controller | onTambah(), onEdit(), onHapus(), onKonfirmasiHapus() |
| CD-12 | ProdukListView | View | renderDaftarProduk(), renderKategoriFilter(), displayPesan() |
| CD-13 | ProdukFormView | View | renderFormTambah(), renderFormEdit(), displayError(), displaySuccess() |
| CD-14 | KonfirmasiHapusView | View | show(), onKonfirmasi(), onBatal() |
| CD-15 | KategoriService | Service | tambahKategori(), hapusKategori(), validasiKategori(), updateKategori() |
| CD-16 | KategoriController | Controller | requestDaftarProduk(), requestEditKategori(), submitKategori() |
| CD-17 | KategoriViewer | View | tampilkanDaftarProduk(), tampilkanFormEdit(), tampilkanSuccess/Error() |
| CD-18 | TargetProduksi | Entity | targetId, periode, jumlahTarget, tanggalMulai, tanggalSelesai; cekTarget(), simpanTarget(), updateData() |
| CD-19 | TargetService | Service | tambahTarget(), updateTarget(), hapusTarget(), validasiTarget(), validasiInput(), prosesTarget() |
| CD-20 | TargetController | Controller | requestFormTarget(), submitTarget(), konfirmasiOverwrite() |
| CD-21 | TargetViewer | View | tampilkanForm(), tampilkanSuccess/Error(), tampilkanKonfirmasiOverwrite() |
| CD-22 | ProduksiHarian | Entity | produksiId, tanggalProduksi, jumlahAktual, jumlahDefect, kendalaProduksi, produk |
| CD-23 | ProduksiService | Service | tambahProduksi(), ubahProduksi(), validasiProduksi() |
| CD-24 | ProduksiController | Controller | |
| CD-25 | ProduksiViewer | View | |
| CD-26 | PencapaianService | Service | hitungPersentasePencapaian(), generateGrafikPencapaian() |
| CD-27 | PencapaianController | Controller | |
| CD-28 | PencapaianViewer | View | |
| CD-29 | DefectService | Service | hitungTingkatDefect() (two overloads), getByPeriode(), getById() |
| CD-30 | DefectController | Controller | requestDefectInsight() |
| CD-31 | DefectViewer | View | muatFormPilihPeriode(), showDefectInsight(), tampilPesanError() |
| CD-32 | PerformaService | Service | hitungRingkasanPerforma(), getTopProducer(), getTopDefect() |
| CD-33 | PerformaController | Controller | getRingkasanPerforma() |
| CD-34 | PerformaViewer | View | loadDashboard(), renderRingkasan(), tampilPesanError() |
| CD-35 | LaporanService | Service | generateLaporan(), exportPDF() |
| CD-36 | LaporanController | Controller | generateLaporan() |
| CD-37 | LaporanViewer | View | openLaporanPage(), requestLaporan(), showDownloadReady(), tampilPesanError() |

---

## Data Relationships

```
User (1) ──< (N) Session
KategoriProduk (1) ──< (N) Produk
Produk (1) ──< (N) TargetProduksi
Produk (1) ──< (N) ProduksiHarian
```

---

## UI Screens (from DPPL)

1. **Login** — username/password form, "Masuk" button
2. **Dashboard** — sidebar menu (7 items: Dashboard, Produk, Target, Input Produksi, Performa, Defect, Laporan), 4 summary cards, 2 charts
3. Remaining screens (Produk list/form, Kategori, Target, Produksi, Performa, Defect, Laporan) — defined by class specs but wireframes not yet designed in the DPPL

---

## What Exists Now

### Backend (`backend/`)
- `internal/config/config.go` — ENV-based config loader (DB + JWT + port only)
- `cmd/api/main.go` — empty
- `go.mod` — empty (no dependencies)
- `internal/` — empty scaffold dirs: database, handlers, middleware, models, repository, services

### Database (`database/`)
- `init.sql` — full schema with 6 tables, 2 enums, CHECK constraints
- `schema.html` — visual ER diagram
- `dummy.sql` — empty (no seed data)

### Frontend (`frontend/`)
- Empty `README.md` only — no framework or code yet

### Docs (`docs/`)
- `K01_G08_Draft_CD.pdf` — SKPL (Software Requirements Spec), 39 pages, **complete and polished**
- `K01_G08_Draft_DPPL2.pdf` — DPPL (Software Design Spec), 53 pages, **partially complete**:
  - ✅ Architecture, sequence diagrams, design class specs, Login + Dashboard wireframes
  - ❌ Algorithm/pseudocode sections (3.4) — empty templates
  - ❌ Statechart diagrams (3.5) — placeholder only
  - ❌ Most UI wireframes (3.6) — Login repeated 7x, other screens not designed
  - ❌ Persistence mapping (3.7) — using placeholder names
  - ❌ Traceability matrix (section 4) — only first row filled
- `K01_G08_Final_RG.docx` — Final report template

### Root
- `README.md` — basic Docker usage instructions
- `docker-compose.yml` — PostgreSQL 15 setup with auto-init
- `.gitignore` — standard Go + env

---

## Config Loader (`backend/internal/config/config.go`)

Reads environment variables with defaults:

| Env Var | Default | Purpose |
|---------|---------|---------|
| DB_HOST | localhost | PostgreSQL host |
| DB_PORT | 5432 | PostgreSQL port |
| DB_USER | postgres | PostgreSQL user |
| DB_PASSWORD | secret | PostgreSQL password |
| DB_NAME | simonpro | Database name |
| JWT_SECRET | your-secret-key | JWT signing key |
| SERVER_PORT | 8080 | API server port |

---

## What Is Missing (Implementation TODO)

1. `go.mod` initialization with dependencies (chi/gorilla/echo router, pgx/sqlx driver, jwt-go, etc.)
2. `main.go` application bootstrap and router setup
3. All backend layers: models → repository → services → handlers → middleware
4. Database connection and migration code
5. All frontend pages and views (framework not yet chosen)
6. PDF report generation implementation
7. Authentication & session management
8. Data validation logic (service-layer)
9. Seed/dummy data for development and testing
10. Tests

---

## Important Files to Reference

| File | Role |
|------|------|
| `docs/K01_G08_Draft_CD.pdf` | Authoritative SKPL: functional reqs, use cases, class diagrams |
| `docs/K01_G08_Draft_DPPL2.pdf` | Authoritative DPPL: architecture, design classes, sequence diagrams |
| `docs/K01_G08_Final_RG.docx` | Final report template |
| `backend/internal/config/config.go` | Only existing Go code |
| `database/init.sql` | Complete database schema |
| `database/schema.html` | Visual ER diagram |

---

## Design Decisions (from DPPL)

- **MVC architecture** with controllers (CD-11,16,20,24,27,30,33,36), views (CD-06,07,12–14,17,21,25,28,31,34,37), and services (CD-02,10,15,19,23,26,29,32,35)
- **Session-based auth** — Session object created on login, invalidated on logout, checked for page access
- **Validation in services** — ProdukService validates (name not empty, category required, code unique), ProduksiHarianService validates (non-negative, defect ≤ output)
- **PDF generation** — LaporanService generates PDF by period via exportPDF()
- **Local/offline DB** — No internet dependency, PostgreSQL runs locally
- **All UI in Bahasa Indonesia** (NF10)