# FrameForge — AI Video SaaS Platform

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white) ![Playwright](https://img.shields.io/badge/Playwright-2EAD33?logo=playwright&logoColor=white) ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white) ![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/CI%2FCD-2088FF?logo=githubactions&logoColor=white)

> A multi-user SaaS that generates AI short-form videos/images and auto-posts them to Instagram, TikTok, and YouTube — orchestrated through a Telegram bot.

> **Note:** Public showcase of a private project. The source is closed; this repo documents the architecture and engineering.

---

## What it does

FrameForge generates short-form vertical video and image content via Google Gemini / Veo, then auto-posts it to social platforms through cloud-phone RPA automation — all driven from a Telegram-bot interface with per-user projects, billing, and scheduling.

## Stack

| Layer | Technology |
|---|---|
| Backend | **Python · FastAPI + Uvicorn** · `python-telegram-bot` |
| Browser automation | **Playwright** (Chromium) + VNC/noVNC auth bootstrap |
| Database | **PostgreSQL** |
| Job queue | Postgres-backed (`SELECT … FOR UPDATE SKIP LOCKED`, lease-based crash recovery) |
| AI | Google **Gemini · Veo 3.1** · NanaBanana image model |
| Admin | React · Vite · TypeScript panel (FastAPI backend) |
| Infra | Docker Compose (**17 services**) · nginx · WAL-G Postgres backups · GitHub Actions (7 workflows) |

## Engineering highlights

- **~84k lines of Python** across ~7 code services + operational containers, with **~118 pytest test files**.
- **Fault-tolerant job queue** — Postgres-backed with `FOR UPDATE SKIP LOCKED`, lease-based crash recovery, and horizontally scalable worker replicas, isolating posting execution from the user-facing bot.
- **Browser automation** — Playwright/Chromium with persistent per-account profiles, VNC-based auth bootstrap, and an account-pool state machine (active / busy / resting / reauth).
- **API-key rotation** — round-robin across multiple Gemini keys to avoid rate limits.
- **Production ops** — admin panel secured with bcrypt, HMAC tokens, **TOTP 2FA**, and slowapi rate limiting; WAL-G offsite Postgres backups with restore drills; infra/disk monitoring with Telegram alerts.
- **CI/CD** — a 7-workflow GitHub Actions pipeline (fast checks, Docker build+smoke, E2E, nightly load) gating auto-deploy.

---

*Built by [Bohdan Olkhovskyi](https://github.com/b0d9a) — Backend Engineer (Python · Go).*
