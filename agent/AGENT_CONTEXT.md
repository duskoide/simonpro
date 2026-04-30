# SiMonPro — Repo Context

**Generated**: 2026-04-29  
**Project**: SiMonPro — **Si**stem **Mon**itoring **Pro**duksi (Production Monitoring System)  
**Course**: IF2050 DRPL (Software Engineering)  
**Team**: K01_G08 — 6 members (Yumna, Benedicta, Illona, Rafi, Lukman, + Najwa as client)  

---

## What This Repo Is

A **desktop application** for small businesses to record, manage, and evaluate daily production. It is NOT a security scanner — earlier summary/plan files were incorrect and have been deleted.

The app runs **offline** with a local PostgreSQL database. Two user roles:

| Role | Capabilities |
|------|-------------|
| **Owner** (Pemilik Usaha) | View-only: dashboard, insights, graphs, PDF reports |
| **Admin Produksi** | Full CRUD: products, categories, targets, daily production input, PDF reports |

---

## Tech Stack

| Layer | Tech | State |
|-------|------|-------|
| Frontend | JavaScript + Figma designs | Empty directory |
| Backend | Go (no framework specified yet) | Scaffolded, minimal code |
| Database | PostgreSQL 15 (local) | SQL files empty |
| Target OS | Windows 11, Linux (Debian/RHEL) | N/A |
| Reports | PDF generation | Not implemented |

---

## What Exists Now

### Backend (`backend/`)
- `internal/config/config.go` — ENV-based config loader (only implemented Go code)
- `cmd/api/main.go` — empty
- `go.mod` — empty
- `go_cheatsheet.md` — Go language reference for the team
- `pkg/` — empty

### Database (`database/`)
- `init.sql` — full schema with 6 tables, 2 enums, CHECK constraints
- `schema.html` — visual ER diagram
- `dummy.sql` — empty
- `dummy.sql` — empty

### Frontend (`frontend/`)
- Empty `README.md` only

### Docs (`docs/`)
- `README.md` — documentation index (references some now-deleted files)
- `K01_G08_Draft_CD.pdf` — SKPL (Software Requirements Specification)
- `K01_G08_Draft_DPPL2.pdf` — DPPL (Software Design & Architecture)
- `K01_G08_Final_RG.docx` — Final Report template
- ~~`PROJECT_PLAN.md`~~ — deleted (was incorrect)
- ~~`SERVICE_LAYER_ALGORITHMS.md`~~ — deleted (was incorrect)

### Root
- `README.md` — project title only
- `.gitignore` — standard Go + env ignore
- `agent/REPO_CONTEXT.md` — this file

---

## Functional Requirements (From SKPL PDF)

| ID | Requirement |
|----|-------------|
| F01 | CRUD products (name, code, category, unit, image, description) |
| F02 | CRUD product categories |
| F03 | Input production targets per product per period (daily/weekly/monthly) |
| F04 | Record daily production: date, product code, actual output, defect count, PIC, issues |
| F05 | Validate data: output cannot be negative, defects <= output |
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
| NF07 | Data Integrity | Auto-validate negative values and defect <= production |
| NF08 | Scalability | Handle ≥ 100 production records |
| NF09 | Performance Report | PDF generation < 5 seconds |
| NF10 | Localization | All UI in Bahasa Indonesia |

---

## Use Cases (9 Total)

| UC | Name | Actor(s) |
|----|------|----------|
| UC01 | Login & Logout | Both |
| UC02 | Manage Products | Admin Produksi |
| UC03 | Manage Categories | Admin Produksi |
| UC04 | Manage Production Targets | Admin Produksi |
| UC05 | Input Daily Production | Admin Produksi |
| UC06 | View Target Achievement Insights | Both |
| UC07 | View Defect Rate Insights | Both |
| UC08 | View Performance Summary | Both (shown on dashboard) |
| UC09 | Generate Production Reports (PDF) | Both |

---

## Architecture (From DPPL PDF)

**6 Modules** (Block Diagram):

1. **Auth Module** — login, logout, session, role-based access
2. **Product Module** — product and category CRUD
3. **Production Module** — targets, daily production input
4. **Analytics Module** — target achievement %, defect rate, performance insights
5. **Report Module** — PDF generation
6. **Local Database** — PostgreSQL 15

All modules interact through the local database as the central data store.

---

## Design Classes (From DPPL PDF)

### Analysis Classes (CD/CS):
| ID | Class | Attributes |
|----|-------|------------|
| C-01 | User | userId, username, password, role |
| C-02 | AuthService | login(), logout(), validateUser(), hashPassword() |
| C-03 | Session | sessionId, userId, loginTime, status |
| C-04 | Produk | kodeProduk, namaProduk, deskripsiProduk, satuan, gambar, statusAktif, kategori (FK → namaKategori) |
| C-05 | KategoriProduk | kategori_id, namaKategori |
| C-06 | ProdukService | tambahProduk(), ubahProduk(), hapusProduk(), validasiProduk() |
| C-07 | KategoriService | tambahKategori(), hapusKategori(), validasiKategori() |
| C-08 | TargetProduksi | targetId, produk, periode, tanggalMulai, tanggalSelesai, jumlahTarget |
| C-09 | TargetService | tetapkanTarget(), ubahTarget(), hapusTarget() |
| C-10 | ProduksiHarian | produksiId, tanggal, produk, jumlahAktual, jumlahDefect, penanggungJawab, kendalaProduksi |
| C-11 | ProduksiHarianService | catatProduksi(), validasiProduksi() |
| C-12 | PencapaianService | hitungPersentasePencapaian() |
| C-13 | DefectService | hitungTingkatDefect() |
| C-14 | PerformaService | getRingkasanPerforma() |
| C-15 | LaporanService | generatePDF() |

### Design Classes (DPPL) add MVC layers:
- Controllers: DashboardController, ProdukController, KategoriController, TargetController, ProduksiController, PencapaianController, DefectController, PerformaController, LaporanController
- Views: LoginPage, DashboardView, ProdukListView, ProdukFormView, KonfirmasiHapusView, KategoriViewer, TargetViewer, ProduksiViewer, PencapaianViewer, DefectViewer, PerformaViewer, LaporanViewer, etc.

---

## Projects Data Relationships

```
User (1) ──< (N) Session
KategoriProduk (1) ──< (N) Produk
Produk (1) ──< (N) TargetProduksi
Produk (1) ──< (N) ProduksiHarian
```

---

## Design Decisions (From DPPL)

- **Sequence diagrams** exist for every use case (images referenced but not embedded in text extraction)
- **Visibility**: All operations are `public`, most attributes are `private`
- **Session-based auth**: Session object created on login, invalidated on logout, checked for page access
- **Validation in services**: ProdukService validates (name not empty, category required, code unique), ProduksiHarianService validates (non-negative, defect <= output)
- **PDF generation**: LaporanService generates PDF by period
- **Local DB**: No internet dependency, PostgreSQL runs locally

---

## Config Loader (Only Implemented Code)

`backend/internal/config/config.go` reads environment variables with defaults:
- `DB_HOST` (default: `localhost`)
- `DB_PORT` (default: `5432`)
- `DB_USER` (default: `postgres`)
- `DB_PASSWORD` (default: `secret`)
- `DB_NAME` (default: `simonpro`)
- `JWT_SECRET` (default: `your-secret-key`)
- `SERVER_PORT` (default: `8080`)
- `SONAR_URL` (default: `http://localhost:9000`) — likely leftover/placeholder from earlier incorrect plans
- `SONAR_TOKEN` (default: empty)
- `ZAP_URL` (default: `http://localhost:8080`) — likely leftover/placeholder

Note: The `SONAR_*` and `ZAP_*` config fields may be remnants of the deleted incorrect planning documents and may not belong in this project.

---

## What Is Missing

1. `go.mod` initialization with dependencies
2. `main.go` application bootstrap
3. Database schema (tables for users, products, categories, targets, daily production, sessions)
4. All backend layers: models → repository → services → handlers → middleware
5. All frontend pages and views
6. PDF report generation
7. Authentication & session management
8. Data validation logic
9. Tests

---

## Important Files to Reference

| File | Role |
|------|------|
| `docs/K01_G08_Draft_CD.pdf` | Authoritative SKPL: functional reqs, use cases, class diagrams |
| `docs/K01_G08_Draft_DPPL2.pdf` | Authoritative DPPL: architecture, design classes, sequence diagrams |
| `docs/K01_G08_Final_RG.docx` | Final report template |
| `backend/internal/config/config.go` | Only existing Go code (may have stale fields) |
| `backend/go_cheatsheet.md` | Team's Go reference |
