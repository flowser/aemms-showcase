<h1 align="center">AEMMS — Academic &amp; Exam Management Platform</h1>

<p align="center">
  A campus-owned platform for colleges: staff desktop, secure student exam client, offline-first field app, on-premises AI, and signed licensing.<br/>
  Designed and built end to end by <a href="https://github.com/flowser"><b>Eng. Felix Nyachio</b></a>, Savvytex Marines Ltd.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-In_production-22c55e?style=flat-square" alt="In production"/>
  <img src="https://img.shields.io/badge/Django_REST-092E20?style=flat-square&logo=django&logoColor=white" alt="Django"/>
  <img src="https://img.shields.io/badge/Vue_3-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white" alt="Vue 3"/>
  <img src="https://img.shields.io/badge/Electron-47848F?style=flat-square&logo=electron&logoColor=white" alt="Electron"/>
  <img src="https://img.shields.io/badge/Capacitor-119EFF?style=flat-square&logo=capacitor&logoColor=white" alt="Capacitor"/>
  <img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI"/>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker"/>
</p>

> **This is a showcase repository.** The source code is private and in production use. It describes the product, architecture and engineering. A live walkthrough is available on request — [get in touch](#contact).

---

## Screenshots

<p align="center">
  <img src="assets/systemmate-dashboard.png" width="100%" alt="SystemMate staff dashboard"/>
  <br/><sub><b>SystemMate</b> — exams administration command center (staff desktop)</sub>
</p>

<table>
  <tr>
    <td width="50%"><img src="assets/systemmate-master-exams.png" alt="SystemMate master exams"/><br/><sub><b>SystemMate</b> — exam programme with windows, timetable and results hub</sub></td>
    <td width="50%"><img src="assets/exammate-dashboard.png" alt="ExamMate student dashboard"/><br/><sub><b>ExamMate</b> — student learning dashboard and exam access</sub></td>
  </tr>
</table>

<sub>Screenshots use demo accounts and seeded data.</sub>

---

## The products

| Product | Users | What it does |
|---|---|---|
| **SystemMate** | Staff | Desktop app for lessons and LMS content, assessment authoring with maths notation, marking, grading and mark weighting, results release, timetables, and field-supervision boards |
| **ExamMate** | Students | Locked-down desktop client for sitting exams, plus lessons, timetable, results, and an AI study assistant |
| **Field app** | Supervisors | Offline-first mobile app for school visits and industrial-attachment assessment: rubrics, photos, sync when back online |
| **Institution server** | All clients | Multi-tenant API with authentication, background jobs, device workers and real-time events |
| **AI service** | Staff, students | Face verification, retrieval-augmented learning assistant, task agents — all on campus hardware |
| **Vendor control plane** | Savvytex | Signs licences, manages subscriptions and releases |

<p align="center">
  <img src="assets/ecosystem-map.png" width="85%" alt="AEMMS ecosystem map"/>
</p>

---

## Architecture

<p align="center">
  <img src="assets/three-layer-architecture.png" width="85%" alt="Three-layer architecture"/>
</p>

```mermaid
flowchart TB
  subgraph vendor["Vendor control plane (cloud)"]
    V["Licences · subscriptions · releases"]
  end
  subgraph campus["Institution server (campus LAN)"]
    API[("Django REST API<br/>PostgreSQL · Redis · Celery")]
    RT["Soketi<br/>real-time events"]
    AI["AI service<br/>FastAPI · InsightFace · Ollama"]
  end
  SM["SystemMate<br/>Electron"] --> API
  EM["ExamMate<br/>Electron"] --> API
  PF["Field app<br/>Capacitor"] --> API
  API --> RT
  API --> AI
  V -->|Ed25519-signed licence| API
```

## Engineering highlights

- **Commercially SaaS, technically campus-owned.** Each institution runs its own server. The vendor issues **Ed25519-signed licence files** that the campus verifies offline, so exams never depend on an internet connection.
- **One API, many clients.** Two Electron desktops, a Capacitor mobile app and the AI service share one Django REST API, layered as controllers → repositories → models.
- **Real-time on a closed LAN.** Live dashboards use a self-hosted Pusher-compatible server (Soketi) instead of a cloud service.
- **AI that stays in the building.** Face verification runs on ONNX Runtime with InsightFace; the learning assistant uses local Ollama models with retrieval over course material. No student data goes to a third-party AI provider.
- **Offline-first field work.** The mobile app captures visits and assessments without connectivity and syncs later.
- **Two regulatory product lines** served from the same architecture, separated by configuration and branding rather than forked logic.
- **Delivery:** Docker Compose for development, Proxmox containers in production, GitHub Actions CI, auto-updating desktop installers for Windows, macOS and Linux.

## Stack

| Layer | Technologies |
|---|---|
| Backend | Python, Django REST Framework, SimpleJWT, PostgreSQL, Redis, Celery, Gunicorn, Nginx |
| Real-time | Soketi (Pusher protocol), Laravel Echo client |
| Desktop | Electron Forge, Vue 3, TypeScript, Pinia, TanStack Query, CKEditor 5, FullCalendar, Tailwind, AdminLTE |
| Mobile | Capacitor, Vue 3, Android |
| AI | FastAPI, InsightFace, ONNX Runtime, OpenCV, Ollama |
| Infrastructure | Docker, Proxmox VE, GitHub Actions |

## In numbers

- **1,400+ commits** across the platform repositories since February 2026
- **6 products** sharing one backend architecture
- Deployed in production on a college campus — see the [campus network case study](https://github.com/flowser/flowser/blob/main/case-studies/chesta-campus-network.md)

---

## Contact

Want a walkthrough, or a platform like this built for your organisation?

<p>
  <a href="mailto:eng.felixnyachio@gmail.com?subject=AEMMS%20inquiry"><img src="https://img.shields.io/badge/Email-eng.felixnyachio%40gmail.com-ef4444?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"/></a>
  <a href="https://wa.me/254748650011?text=Hi%20Felix%2C%20I%20saw%20AEMMS%20on%20GitHub."><img src="https://img.shields.io/badge/WhatsApp-%2B254_748_650_011-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" alt="WhatsApp"/></a>
  <a href="https://github.com/flowser"><img src="https://img.shields.io/badge/Profile-flowser-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub profile"/></a>
</p>

<sub>© 2026 Savvytex Marines Ltd. All rights reserved. Screenshots and diagrams may not be reused without permission.</sub>
