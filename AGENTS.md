# AI Developer Guide — neostronghold / NeoParadise

This repository is the **monolith** for **neostronghold** — a premium open-source smart home platform by **NeoParadise** (Cape Town, South Africa).

---

## Overview

neostronghold is an AI-powered smart home platform built on a fork of Home Assistant. The monolith references everything as submodules:

| What | Where | Repo | Tech |
|---|---|---|---|
| 🌐 **Brand website / pitch deck** | `web/` | neostronghold/web | Next.js 16, Tailwind v4, Three.js, Framer Motion |
| 📚 **Strategy & business docs** | `docs/` | — (in monolith) | Markdown |
| 🚧 **Main app (Prototype 1)** | `app/` | neostronghold/app | React 19, Vite, shadcn/ui, Tailwind v4 |
| 🐍 **Home Assistant fork (backend)** | `core/` | neostronghold/core | Python 3.14, asyncio, SQLAlchemy |
| 🖥️ **neostrongholdOS (HA OS fork)** | `os/` | neostronghold/os | Buildroot (embedded Linux) |
| 📖 **Reference implementations** | `inspiration/` | Various | HA frontend (Lit), openclaw, opencode |

**Do NOT write substantial code in the submodules directly from this repo.** Changes should be committed in their own repos first, then the submodule pointer is updated here.

---

## Quick Start

```bash
# Clone everything (slow — huge repos)
git clone --recurse-submodules git@github.com:neostronghold/monolith.git
cd monolith

# Or clone just the monolith first, then init submodules as needed
git clone git@github.com:neostronghold/monolith.git
cd monolith
git submodule init
git submodule update --depth 1 web app os
```

### Website

```bash
cd web
pnpm install
pnpm dev   # → http://localhost:3000
pnpm build
pnpm start
```

### App (Prototype 1)

```bash
cd app
pnpm install
pnpm dev   # → http://localhost:5173
```

---

## Repository Structure

```
monolith/
├── web/                          # 🌐 Brand website (submodule → neostronghold/web)
├── app/                          # 🚧 Main smart home app (submodule → neostronghold/app)
├── core/                         # 🐍 HA Core fork (submodule → neostronghold/core)
├── os/                           # 🖥️ neostrongholdOS (submodule → neostronghold/os)
├── docs/                         # 📚 Strategy & business documents (in monolith)
│   ├── BUSINESS_PLAN.md
│   ├── PATH_TO_1B.md
│   ├── HARDWARE_STRATEGY.md
│   ├── AI_PRICING_MODEL.md
│   ├── SOLAR_PARTNERSHIP.md
│   ├── ELON_X_STRATEGY.md
│   ├── YC_APPLICATION.md
│   ├── ARCHITECT_CHANNEL.md
│   ├── DIASPORA_GTM.md
│   ├── MARK2_WEBSITE_SPEC.md
│   ├── INSPIRATION.md
│   ├── app-planning-prompt.md
│   └── promotions/
├── inspiration/                  # 📖 Reference implementations (submodules)
│   ├── frontend/                 # HA frontend (Lit, research only)
│   ├── openclaw/                 # Multi-agent orchestration
│   └── opencode/                 # AI agent interface & tool-use
└── scripts/
    └── convert_logo.py
```

---

## Design System

| Token | Value | CSS Variable |
|---|---|---|
| **Theme** | Cosmic dark, glassmorphism | — |
| **Primary** | HSL 196 94% 48% (electric cyan) | `--primary` |
| **Secondary** | HSL 263 70% 50% (nebula purple) | `--secondary` |
| **Background** | HSL 228 30% 6% (deep space) | `--background` |
| **Glass** | `blur(20px) saturate(180%)` | `.glass` utility |
| **Fonts** | Space Grotesk (headings), Inter (body), JetBrains Mono (code) | `--font-heading`, `--font-sans`, `--font-mono` |
| **Radius** | 1rem (16px) | `--radius` |

All theme tokens are defined in `web/src/app/globals.css` and should be replicated in `app/src/lib/theme.ts`.

---

## Key Scripts

```bash
# Web
cd web
pnpm dev          # Dev server :3000
pnpm build        # Production build

# App (when scaffolded)
cd app
pnpm dev          # Dev server :5173
pnpm build        # Production build
```

---

## Submodule Usage

All submodules and their repos:

| Path | Repo | Purpose | Maintained At |
|---|---|---|---|
| `core/` | neostronghold/core | HA Core fork | core/ |
| `os/` | neostronghold/os | neostrongholdOS | os/ |
| `web/` | neostronghold/web | Brand website | web/ |
| `app/` | neostronghold/app | Main app | app/ |
| `inspiration/frontend/` | neoparadise/citadelevolve-frontend | HA frontend reference | n/a (research) |
| `inspiration/openclaw/` | openclaw/openclaw | Multi-agent reference | n/a (research) |
| `inspiration/opencode/` | opencode-ai/opencode | Agent interface reference | n/a (research) |

```bash
# Pull latest from all submodules
git submodule update --remote --merge

# Update a specific submodule
git submodule update --remote --merge web
git submodule update --remote --merge core

# Commit submodule pointer change
git add web core
git commit -m "chore: update web and core submodules"
git push
```

---

## Environment Variables

| Variable | Required For | Where to Set |
|---|---|---|
| `NEXT_PUBLIC_WEB3FORMS_KEY` | Contact form email sending | `.env.local` in `web/` |
| `VITE_SUPABASE_URL` | Supabase backend | `.env` in `app/` |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | Supabase backend | `.env` in `app/` |
| `VITE_MODE` | `local` or `cloud` | `.env` in `app/` |

---

## Deployment

| App | Platform | URL | Trigger |
|---|---|---|---|
| **Brand website** | Vercel | neostronghold.com | Push to `neostronghold/web` main |
| **Main app** | Vercel (future) | app.neostronghold.com | Push to `neostronghold/app` main |

---

## Prototype 1 Status

**Phase: Active development** — See `docs/app-planning-prompt.md` for the full plan.

- [ ] HA Core WebSocket connection (auth, subscribe, reconnect)
- [ ] Entity state display (8+ core cards)
- [ ] Sections + Masonry views
- [ ] Dashboard editor (visual + YAML)
- [ ] Card registry with lazy loading
- [ ] Theme system (cosmic dark)
- [ ] Sidebar navigation (Discord-inspired)
- [ ] Agent with a SOUL (Orion)
- [ ] Agent chat interface (streaming, tool calls)
- [ ] Agent tool: HA service calls
- [ ] Permission dialog for tool approval
- [ ] Session management
- [ ] Supabase auth + sync
- [ ] Docker Compose for local dev
- [ ] neostrongholdOS branding changes
