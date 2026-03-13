# Kesuma JMB Hub

A lightweight condominium management system built for **Residensi Kesuma, Beranang** — handling parcel tracking and visitor management for 475 units across 19 floors.

🌐 **Live:** [kesuma-jmb.netlify.app](https://kesuma-jmb.netlify.app)

---

## Systems

### 📦 Parcel System
- Dispatcher logs incoming parcels with photo, unit, and courier
- Unit owners verify and mark parcels as collected
- Admin monitors all units and overdue parcels

### 🪪 Visitor System
- Visitors register and receive a QR code + reference code (e.g. `KES4X7MNP`)
- Unit owners approve or reject visitor requests
- Security checks visitors in and out — only approved visitors can enter

---

## Pages

| Page | URL | PIN |
|------|-----|-----|
| Dispatcher | `/dispatcher` | 1125 |
| Owner | `/owner` | — |
| Security / Guard | `/guard` | 1125 |
| Admin Dashboard | `/admin` | 1125 |
| Register Visit | `/visitor-register` | — |
| Check Visit Status | `/visitor-check` | — |
| Scale Plan | `/plan` | — |
| How It Works | `/how` | — |
| About | `/about` | — |
| Contact | `/contact` | — |

---

## Tech Stack

| Layer | Tech |
|-------|------|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Backend | [Supabase](https://supabase.com) (PostgreSQL + Storage + RLS) |
| QR Code | [QRCode.js](https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js) |
| Hosting | [Netlify](https://netlify.com) (free tier, auto-deploy) |
| Version Control | GitHub |
| AI Copilot | [Claude](https://claude.ai) (Anthropic) |

> Zero server. Zero monthly cost. Pure static files talking directly to Supabase.

---

## Database Schema (Supabase)

```sql
-- Unit owners
create table owners (
  unit text primary key,
  email text not null,
  phone text not null
);

-- Parcels
create table parcels (
  id bigint generated always as identity primary key,
  unit text references owners(unit),
  courier text not null,
  image_url text,
  status text default 'Pending',
  created_at timestamptz default now(),
  collected_at timestamptz
);

-- Visitors
create table visitors (
  id bigint generated always as identity primary key,
  ref_code text unique not null,
  name text not null,
  ic_no text not null,
  phone text not null,
  unit text references owners(unit),
  purpose text not null,
  vehicle_type text,
  vehicle_plate text,
  status text default 'Pending',
  created_at timestamptz default now(),
  approved_at timestamptz,
  checked_in_at timestamptz,
  checked_out_at timestamptz
);
```

---

## Features

- 🌐 4 languages — Bahasa Malaysia, English, 中文, தமிழ்
- 🎨 4 themes — Obsidian, Light, Breeze, Candy
- 📱 Fully responsive — mobile first
- 🔒 Row Level Security (RLS) on all tables
- 🖼️ Parcel photos stored in Supabase Storage
- 📋 QR code generation with copy & download
- 🚗 Vehicle plate tracking for visitors

---

## Deployment

Auto-deploys via Netlify on every push to `main`.

```
GitHub push → Netlify build → Live in ~30 seconds
```

Clean URLs configured in `netlify.toml` — no `.html` extensions needed.

---

## Project Story

Built by a QA engineer who vibe coded this to solve a real problem — parcels piling up in the lobby and no way to track visitors at Residensi Kesuma. Built with Claude AI as coding copilot in a few weeks. Not a corporate project. Neighbour-to-neighbour. 🏠

---

*475 units · 19 floors · 2 systems · 0 servers*
