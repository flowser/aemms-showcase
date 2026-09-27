<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/theme/hero-light.svg"/>
  <img width="100%" src="assets/theme/hero-dark.svg" alt="AEMMS. Academic and exam platform, campus-owned."/>
</picture>

<p align="center">
  A campus-owned platform for colleges: staff desktop, secure student exam client, offline-first field app, on-premises AI, and signed licensing.<br/>
  Designed and built end to end by <a href="https://github.com/flowser"><b>Eng. Felix Nyachio</b></a>, Savvytex Marines Ltd.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-In_production-34d399?style=flat-square&labelColor=16162a" alt="In production"/>
  <img src="https://img.shields.io/badge/Django_REST-092E20?style=flat-square&logo=django&logoColor=white" alt="Django"/>
  <img src="https://img.shields.io/badge/Vue_3-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white" alt="Vue 3"/>
  <img src="https://img.shields.io/badge/Electron-47848F?style=flat-square&logo=electron&logoColor=white" alt="Electron"/>
  <img src="https://img.shields.io/badge/Capacitor-119EFF?style=flat-square&logo=capacitor&logoColor=white" alt="Capacitor"/>
  <img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI"/>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker"/>
</p>

> **This is a showcase repository.** The source code is private and in production use. It describes the product, architecture and engineering. A live walkthrough is available on request — [get in touch](#contact).

<p align="center"><img width="100%" src="assets/theme/divider.svg" alt=""/></p>

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

<p align="center"><img width="100%" src="assets/theme/divider.svg" alt=""/></p>

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

<p align="center"><img width="100%" src="assets/theme/divider.svg" alt=""/></p>

## Architecture

<p align="center">
  <img src="assets/three-layer-architecture.png" width="85%" alt="Three-layer architecture"/>
</p>

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','primaryColor':'#16162a','primaryTextColor':'#f0f0ff','primaryBorderColor':'#4f46e5','lineColor':'#0ea5e9','clusterBkg':'#0d0d1a','clusterBorder':'#4f46e5','titleColor':'#0ea5e9','edgeLabelBackground':'#16162a'}}}%%
flowchart TB
  subgraph vendor["☁️ Vendor control plane"]
    V["🔑 Licences · subscriptions · releases<br/>Ed25519-signed"]:::cloud
  end
  subgraph campus["🏫 Institution server · campus LAN"]
    API{{"⚙️ Django REST API<br/>JWT · Celery"}}:::core
    DB[("🗄️ PostgreSQL<br/>Redis")]:::data
    RT["⚡ Soketi<br/>real-time events"]:::data
    AI["🧠 AI service<br/>FastAPI · InsightFace · Ollama"]:::ai
  end
  SM["🖥️ SystemMate<br/>Electron · staff"]:::staff
  EM["🔒 ExamMate<br/>Electron · trainees"]:::trainee
  PF["📱 Field app<br/>Capacitor · offline-first"]:::field
  SM --> API
  EM --> API
  PF -.->|sync when online| API
  API <--> DB
  API --> RT
  API <-->|on-prem inference| AI
  V ==>|signed licence| API
  classDef ai fill:#9333ea,stroke:#d8b4fe,stroke-width:2px,color:#ffffff
  classDef cloud fill:#db2777,stroke:#f9a8d4,stroke-width:2px,color:#ffffff
  classDef core fill:#ea580c,stroke:#fdba74,stroke-width:3px,color:#ffffff
  classDef data fill:#1e1b4b,stroke:#818cf8,stroke-width:2px,color:#e0e7ff
  classDef field fill:#059669,stroke:#6ee7b7,stroke-width:2px,color:#ffffff
  classDef staff fill:#4f46e5,stroke:#a5b4fc,stroke-width:2px,color:#ffffff
  classDef trainee fill:#0284c7,stroke:#7dd3fc,stroke-width:2px,color:#ffffff
  style vendor fill:#1a0b16,stroke:#db2777,stroke-width:2px,color:#f9a8d4
  style campus fill:#0d0d1a,stroke:#4f46e5,stroke-width:2px,color:#7dd3fc
  linkStyle default stroke:#0ea5e9,stroke-width:2px
```

### One exam, end to end

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','actorBkg':'#4f46e5','actorBorder':'#a5b4fc','actorTextColor':'#ffffff','actorLineColor':'#6b6b8a','signalColor':'#0ea5e9','signalTextColor':'#0ea5e9','labelBoxBkgColor':'#f97316','labelBoxBorderColor':'#fdba74','labelTextColor':'#ffffff','loopTextColor':'#f97316','noteBkgColor':'#16162a','noteTextColor':'#f0f0ff','noteBorderColor':'#f97316','activationBkgColor':'#0ea5e9','activationBorderColor':'#7dd3fc','sequenceNumberColor':'#ffffff'}}}%%
sequenceDiagram
  autonumber
  participant S as 🖥️ SystemMate
  participant A as ⚙️ Institution API
  participant E as 🔒 ExamMate
  participant I as 🧠 AI service
  rect rgba(79, 70, 229, 0.18)
    Note over S,A: Prepare
    S->>A: Author paper, set exam type and window
  end
  rect rgba(14, 165, 233, 0.18)
    Note over A,E: Sit
    E->>A: Trainee signs in on the campus LAN
    A->>I: Face match, if the exam type requires it
    I-->>A: Verified (or proctor override)
    E->>E: Display guard blocks extra monitors
    E->>A: Submit answers
  end
  rect rgba(249, 115, 22, 0.18)
    Note over S,E: Mark and release
    S->>A: Mark, with AI assist on long answers
    S->>A: Release results
    A-->>E: Results and transcript available
  end
```

## Engineering highlights

- **Commercially SaaS, technically campus-owned.** Each institution runs its own server. The vendor issues **Ed25519-signed licence files** that the campus verifies offline, so exams never depend on an internet connection.
- **One API, many clients.** Two Electron desktops, a Capacitor mobile app and the AI service share one Django REST API, layered as controllers → repositories → models.
- **Real-time on a closed LAN.** Live dashboards use a self-hosted Pusher-compatible server (Soketi) instead of a cloud service.
- **AI that stays in the building.** Face verification runs on ONNX Runtime with InsightFace; the learning assistant uses local Ollama models with retrieval over course material. No student data goes to a third-party AI provider.
- **Offline-first field work.** The mobile app captures visits and assessments without connectivity and syncs later.
- **Two regulator editions** (KNEC for teacher-training colleges, CDACC for TVET colleges) built on one shared architecture and shipped as separate release lines, so each regulator's rules can change without breaking the other.
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

<!-- telemetry:aemms -->
**AEMMS by the numbers**

<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/theme/aemms-stats-light.svg"/>
  <img src="assets/theme/aemms-stats-dark.svg" width="100%" alt="AEMMS: API routes, data models, Vue components, automated tests"/>
</picture>

<p align="center"><img src="https://img.shields.io/badge/56-service_modules-0ea5e9?style=flat-square&labelColor=16162a" alt="56 service modules"/> <img src="https://img.shields.io/badge/61-desktop_IPC_handlers-9333ea?style=flat-square&labelColor=16162a" alt="61 desktop IPC handlers"/> <img src="https://img.shields.io/badge/13-CI_workflows-34d399?style=flat-square&labelColor=16162a" alt="13 CI workflows"/></p>

<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/theme/aemms-donuts-light.svg"/>
  <img src="assets/theme/aemms-donuts-dark.svg" width="100%" alt="AEMMS commits by app (1,461) and code by language"/>
</picture>

<sub>Measured from git on 2026-09-27 by <code>telemetry.py</code>: tracked files only, non-blank lines, CDACC forks counted once; dependencies, builds, migrations and third-party themes excluded.</sub>
<!-- /telemetry:aemms -->

- Deployed in production on a college campus — see the [campus network case study](https://github.com/flowser/flowser/blob/main/case-studies/chesta-campus-network.md)

<p align="center"><img width="100%" src="assets/theme/divider.svg" alt=""/></p>

## Contact

Want a walkthrough, or a platform like this built for your organisation?

<p>
  <a href="mailto:eng.felixnyachio@gmail.com?subject=AEMMS%20inquiry"><img src="https://img.shields.io/badge/Email-eng.felixnyachio%40gmail.com-ef4444?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"/></a>
  <a href="https://wa.me/254748650011?text=Hi%20Felix%2C%20I%20saw%20AEMMS%20on%20GitHub."><img src="https://img.shields.io/badge/WhatsApp-%2B254_748_650_011-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" alt="WhatsApp"/></a>
  <a href="https://github.com/flowser"><img src="https://img.shields.io/badge/Profile-flowser-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub profile"/></a>
</p>

<sub>© 2026 Savvytex Marines Ltd. All rights reserved. Screenshots and diagrams may not be reused without permission.</sub>
