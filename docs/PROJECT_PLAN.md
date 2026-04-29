# SimonPro Project Plan

## Overview

**Project**: SimonPro - Software Security Testing & Analysis System
**Course**: IF2050 DRPL (Software Engineering)
**Team**: K01_G08
**Duration**: 1 Month

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React.js + Vite |
| Backend | Go + Go Fiber |
| Database | PostgreSQL |
| Security Tools | SonarQube, OWASP ZAP (Docker) |
| PDF Generation | go-pdf or wkhtmltopdf |

## Database Schema

### Tables

```sql
-- users: id, name, email, password_hash, role, created_at, updated_at
-- projects: id, name, description, owner_id, status, created_at, updated_at
-- scan_results: id, project_id, scan_type, status, started_at, completed_at
-- vulnerabilities: id, scan_result_id, cwe_id, severity, title, description, file_path, line_number
```

### Relationships

```
users (1) ─── (N) projects
projects (1) ─── (N) scan_results
scan_results (1) ─── (N) vulnerabilities
```

---

## Task Assignments

### Frontend 1 (Auth + Dashboard)
- Login/Register pages
- User profile management
- Dashboard with statistics and charts
- Navigation layout

### Frontend 2 (Scanning + Reports)
- Project management UI (list, create, edit)
- SAST upload and results display
- Dependency scan upload and CVE display
- Report generation and download

### Database (ERD + Migrations)
- Design full ERD
- Write init.sql with all tables, indexes, constraints
- Write seed.sql with sample data
- Write migration scripts

### Backend 1 (Auth + Core API)
- JWT authentication (login/register)
- User CRUD endpoints
- Project CRUD endpoints
- Middleware (auth, logging, CORS)

### Backend 2 (Scanning + Reports)
- SonarQube API integration (SAST)
- OWASP ZAP API integration
- SBOM/dependency scanner
- PDF report generation
- Webhook handlers for scan completion

---

## 4-Week Schedule

### Week 1: Foundation

**Database**
- [ ] Design ERD
- [ ] Write init.sql with all tables
- [ ] Write seed.sql with sample data
- [ ] Test migrations on PostgreSQL

**Backend 1**
- [ ] Initialize Go module with dependencies
- [ ] Setup config loader (env vars)
- [ ] Connect to PostgreSQL
- [ ] Implement JWT auth (login/register)
- [ ] User CRUD endpoints
- [ ] Project CRUD endpoints

**Frontend 1**
- [ ] Setup React + Vite project
- [ ] Configure routing
- [ ] Login/Register pages
- [ ] Dashboard with charts

**Frontend 2**
- [ ] Setup project structure
- [ ] Create reusable components
- [ ] Project list/create UI

---

### Week 2: Security Features

**Backend 1**
- [ ] Add SonarQube client service
- [ ] SAST scan trigger endpoint
- [ ] Parse and store scan results
- [ ] CWE vulnerability classification

**Backend 2**
- [ ] Add dependency scanner service
- [ ] SBOM upload endpoint
- [ ] CVE lookup integration
- [ ] Store vulnerability results

**Frontend 1**
- [ ] SAST upload component
- [ ] Scan results display with CWE
- [ ] Vulnerability details modal

**Frontend 2**
- [ ] Dependency scan upload
- [ ] CVE results table view
- [ ] Link vulnerabilities to projects

---

### Week 3: Reports + Integration

**Backend 1**
- [ ] PDF generation service
- [ ] Report download endpoint
- [ ] Chart generation for reports

**Backend 2**
- [ ] OWASP ZAP integration
- [ ] ZAP scan trigger endpoint
- [ ] Parse ZAP results to vulnerabilities

**Frontend 1**
- [ ] Report generation UI
- [ ] PDF download functionality
- [ ] Dashboard charts (scan history)

**Frontend 2**
- [ ] ZAP scan configuration UI
- [ ] ZAP results display
- [ ] Combined vulnerability view

---

### Week 4: Polish + Deploy

**All Team**
- [ ] End-to-end integration testing
- [ ] Bug fixes and edge cases
- [ ] Error handling improvements
- [ ] Write README documentation
- [ ] Prepare final presentation

**Backend 1 + 2**
- [ ] API documentation (Swagger)
- [ ] Docker compose for all services
- [ ] Production-ready configuration

**Database**
- [ ] Optimize queries with indexes
- [ ] Backup/restore scripts

---

## Project Structure

```
simonpro/
├── backend/
│   ├── cmd/api/main.go
│   ├── internal/
│   │   ├── config/
│   │   ├── database/
│   │   ├── handlers/
│   │   ├── middleware/
│   │   ├── models/
│   │   ├── repository/
│   │   └── services/
│   ├── pkg/
│   │   └── scanner/
│   ├── go.mod
│   └── Dockerfile
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   └── App.jsx
│   ├── package.json
│   └── vite.config.js
├── database/
│   ├── init.sql
│   ├── seed.sql
│   └── migrations/
├── docs/
│   ├── README.md
│   ├── K01_G08_Draft_CD.pdf
│   └── K01_G08_Draft_DPPL2.pdf
└── docker-compose.yml
```

---

## API Endpoints

### Auth
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login, returns JWT

### Users
- `GET /api/users` - List all users
- `GET /api/users/:id` - Get user by ID
- `PUT /api/users/:id` - Update user
- `DELETE /api/users/:id` - Delete user

### Projects
- `GET /api/projects` - List projects
- `POST /api/projects` - Create project
- `GET /api/projects/:id` - Get project
- `PUT /api/projects/:id` - Update project
- `DELETE /api/projects/:id` - Delete project

### Scans
- `POST /api/scans/sast` - Trigger SAST scan
- `POST /api/scans/dependency` - Upload SBOM for dependency scan
- `GET /api/scans/:id/results` - Get scan results
- `GET /api/scans/:id/vulnerabilities` - Get vulnerabilities

### Reports
- `GET /api/reports/:scanId` - Generate PDF report
- `GET /api/reports/:scanId/download` - Download PDF

---

## Docker Services

```yaml
# docker-compose.yml
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: simonpro
    ports:
      - "5432:5432"

  sonar:
    image: sonarqube:latest
    ports:
      - "9000:9000"
    memory: 4GB

  zap:
    image: owasp/zap2docker-stable
    ports:
      - "8080:8080"
```

---

## Communication Plan

| Day | Time | Activity |
|-----|------|----------|
| Monday | 10:00 | Sprint planning |
| Wednesday | 10:00 | Progress check |
| Friday | 10:00 | Code review |
| Daily | 20:00 | Async update on chat |

---

## Scanner Integration Notes

### SonarQube (SAST)
- Analyzes source code for vulnerabilities
- Classifies by CWE (Common Weakness Enumeration)
- API endpoint: `POST /api/ce/task` to trigger scan
- Results via: `GET /api/issues/search`

### OWASP ZAP (DAST)
- Web app vulnerability scanner
- API endpoint: `http://localhost:8080/JSON/ascan/action/scan`
- Results via: `GET /JSON/report`

### Dependency Scanner
- Analyzes SBOM (Software Bill of Materials)
- Checks against CVE database
- API: `https://services.nvd.nist.gov/rest/json/cves/2.0`

### Mock Mode
For development without real tools, create mock responses that return realistic but fake vulnerability data.

---

## Database Design

### ERD Summary

```
┌─────────────┐     ┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│   users     │     │  projects   │     │ scan_results │     │ vulnerabilities │
├─────────────┤     ├─────────────┤     ├──────────────┤     ├─────────────────┤
│ id (PK)     │────<│ owner_id    │     │ id (PK)      │────<│ scan_result_id  │
│ name        │     │ id (PK)     │────<│ project_id   │     │ id (PK)         │
│ email       │     │ name        │     │ scan_type    │     │ cwe_id          │
│ password    │     │ description │     │ status       │     │ severity        │
│ role        │     │ status      │     │ started_at   │     │ title           │
│ created_at  │     │ created_at  │     │ completed_at │     │ description     │
│ updated_at  │     │ updated_at  │     └──────────────┘     │ file_path       │
└─────────────┘     └─────────────┘                           │ line_number     │
                                                                  └─────────────────┘
```

### Indexes
- users.email (unique)
- projects.owner_id
- scan_results.project_id
- vulnerabilities.scan_result_id
- vulnerabilities.cwe_id