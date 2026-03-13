# Starlight Pediatrics — Patient Manager

## Overview

A single-page web app for **Starlight Pediatrics**, Dr. Yogini Prajapati's Direct Primary Care (DPC) pediatric practice. Replaces the practice's Excel spreadsheet with a live, branded patient management tool.

- **Live URL:** https://starlight-pediatrics.netlify.app
- **GitHub Pages:** https://dsurana04.github.io/starlight-pediatrics-app/
- **Repo:** https://github.com/dsurana04/starlight-pediatrics-app
- **Built by:** Deepak Surana (CPO)
- **Doctor:** Dr. Yogini Prajapati (`drp@starlightpediatrics.com`)

---

## Features

### Patient Management
- Add, edit, delete patients via modal form
- Fields: first/last name, DOB, status, parent/guardian, phone, email, notes, fee override
- Active vs. Prospect status
- 20 seed patients pre-loaded (16 active, 4 prospects) from original Excel spreadsheet
- Patient table with sorting, filtering (All / Active / Prospects)

### Age-Based Pricing Tiers
Automatic fee calculation based on patient age (months since DOB):

| Plan | Age Range | Monthly Fee |
|------|-----------|-------------|
| Orion | 0–2 months | $295 |
| Lyra | 3–12 months | $225 |
| Sirius | 1–4 years | $175 |
| Pegasus | 5–18 years | $150 |
| Polaris | 19–22 years | $100 |

- Auto-detects when a patient crosses a tier boundary
- Generates billing change alerts with exact dates
- Fee override option for custom pricing

### Wellness Check Scheduling
Based on AAP guidelines, calculates wellness check dates from DOB:
- 2, 4, 6, 9, 12, 15, 18, 24, and 30 months
- Schedule modal with date picker and notes (for Dr. Prajapati)
- Green "completed" badges for scheduled items
- Tracks scheduled vs. unscheduled checks

### Prospect Pipeline
Kanban-style board with 4 stages:
1. **Inquiry** — new leads/contacts
2. **Meet & Greet** — initial conversation
3. **Scheduled Visit** — visit booked
4. **Ready to Enroll** — final step before active membership

- Move prospects between stages with arrow buttons
- "Enroll" button converts prospect to active member
- Prospect-specific fields: pipeline stage, due date, referral source
- KPIs: total prospects, ready to enroll, due within 60 days, visits scheduled

### Revenue Dashboard
- MRR (Monthly Recurring Revenue)
- ARR (Annualized Revenue)
- Average revenue per patient
- Revenue by tier breakdown
- Active patient count

### Action Items
- Auto-generated alerts for:
  - Upcoming wellness checks
  - Billing tier changes (EMR updates needed)
  - Prospect follow-ups
- Urgency levels: urgent, upcoming, info
- Schedule buttons with date tracking

### Monthly Email Reminders
- Generates a full monthly checklist email with:
  - Wellness checks to schedule
  - Billing changes (EMR updates)
  - Revenue snapshot
  - Prospect follow-ups
  - Already-scheduled items
- Configurable: toggle which sections to include
- **Auto-sends on the 24th of each month** (one week before the 1st)
- Manual send button available anytime
- "Open in Email App" and "Copy to Clipboard" fallbacks

### Pricing Guide
- Full plan details with descriptions (star-themed names: Orion, Lyra, Sirius, Pegasus, Polaris)
- Wellness check schedule table
- Plan transition rules
- Family savings info (25% sibling discount, $500/mo family cap, 5% annual prepay)

### Data & Backup
- Export all data as JSON file
- Import/restore from backup file
- Stats display (active patients, prospects, scheduled items)
- "How Your Data Works" explainer for non-technical users

---

## Integrations

### EmailJS (Email Sending)
- **Service:** EmailJS (https://emailjs.com)
- **Plan:** Free tier — 200 emails/month
- **Purpose:** Sends monthly checklist emails directly to Dr. P's inbox
- **How it works:** Client-side JavaScript calls EmailJS API, which sends via connected Gmail account

**Configuration (in `index.html`):**
```javascript
const EMAILJS_PUBLIC_KEY = '3-Bh0T88xgy2EPTRh';
const EMAILJS_SERVICE_ID = 'service_ni5vodt';
const EMAILJS_TEMPLATE_ID = 'template_ldemwak';
```

**EmailJS Template Setup:**
- **Subject:** `{{subject}}`
- **To Email:** `{{to_email}}`
- **Content (HTML mode):** `{{{message_html}}}` (triple curly braces for HTML rendering)
- **Reply To:** `drp@starlightpediatrics.com`
- **Service Type:** Gmail (NOT "Gmail API" — that causes 412 scope errors)

**EmailJS Account:** Registered under Deepak Surana's email at https://dashboard.emailjs.com

### Netlify (Hosting)
- **Site:** starlight-pediatrics
- **URL:** https://starlight-pediatrics.netlify.app
- **Site ID:** `3bc2dfac-9a6e-400c-ad1e-b49327d8ad94`
- **Deploy method:** Manual CLI deploy (not auto-deploy from git)
- **Deploy command:**
  ```bash
  mkdir -p /tmp/starlight-deploy
  cp index.html doctor.jpg logo-mark.png logo-full.png /tmp/starlight-deploy/
  npx netlify-cli deploy --prod --dir=/tmp/starlight-deploy --site=3bc2dfac-9a6e-400c-ad1e-b49327d8ad94
  ```

### GitHub (Source Control)
- **Repo:** https://github.com/dsurana04/starlight-pediatrics-app
- **Branch:** main
- **GitHub Pages:** enabled (legacy deployment from main branch)

---

## Technical Architecture

### Stack
- **Frontend:** Single HTML file (`index.html`) with inline CSS and vanilla JavaScript
- **Font:** Nunito (Google Fonts) — web substitute for brand font Lorin
- **No framework, no build step, no dependencies** (except EmailJS CDN)
- **Assets:** `doctor.jpg` (Dr. Yogini photo), `logo-mark.png` (heart+star icon), `logo-full.png` (full logo)

### Data Storage
- **Primary:** Browser `localStorage`
  - `sp_patients` — patient array (JSON)
  - `sp_schedules` — schedule data (JSON)
  - `sp_settings` — email/reminder settings (JSON)
  - `sp_version` — data model version (currently `'5'`)
  - `sp_last_backup` — timestamp of last manual backup
  - `sp_auto_backup` — redundant copy of all data (auto-saved on every change)
  - `sp_auto_download_week` — tracks weekly auto-download (e.g. `'2026-W11'`)
  - `sp_auto_email_sent_2026-04` — tracks auto-sent monthly emails
  - `sp_last_email_sent` — timestamp of last manual email send

### Data Protection (4 Layers)
1. **Persistent Storage API** — `navigator.storage.persist()` prevents browser from evicting data
2. **Redundant auto-backup** — second localStorage key updated on every save, auto-recovers if primary data lost
3. **Weekly auto-download** — backup JSON file auto-downloads on first visit each week
4. **Reminder banner** — yellow banner appears if no backup in 7+ days, one-click download

### Auto-Send Monthly Email
- Triggers when app is opened on the **24th or later** of any month
- Sends next month's checklist (e.g., opens March 24 → sends April checklist)
- Tracked per month so it only sends once per cycle
- Subject tagged with "(Auto-Reminder)"
- Requires Dr. P to open the app at least once between the 24th and 1st

### Versioning
Data model changes require bumping the version number in two places:
```javascript
if (saved && version === '5') {  // check
localStorage.setItem('sp_version', '5');  // set
```
This forces a re-seed of patient data from `SEED_PATIENTS`. Any user-added data will be lost on version bump unless backed up first.

---

## Database Research — HIPAA Compliance

### Current State
The app uses browser `localStorage` — no server-side database. This works for single-user, single-device use but has limitations:
- Data lives only in one browser on one device
- Clearing browser data = data loss (mitigated by auto-backup downloads)
- No multi-device sync
- No HIPAA compliance infrastructure

### Why HIPAA Matters
The app stores Protected Health Information (PHI):
- Patient first/last names
- Dates of birth
- Phone numbers
- Email addresses
- Medical notes

Under HIPAA, any system storing PHI must have a signed Business Associate Agreement (BAA) with the hosting/database provider.

### Supabase (Researched March 2026)
- **Free tier:** Available, but **NOT HIPAA compliant**. No BAA available.
- **Pro plan ($25/mo):** NOT HIPAA compliant. No BAA.
- **Team plan ($599/mo):** Eligible for HIPAA. Requires separate HIPAA add-on.
- **HIPAA add-on:** **$350/month** on top of Team plan.
- **Total for HIPAA-compliant Supabase: ~$950/month**
- **Docs:** https://supabase.com/docs/guides/security/hipaa-compliance
- **Verdict:** Too expensive for a small DPC practice right now.

### Other HIPAA-Compliant Database Options (To Research)
| Provider | HIPAA BAA | Approx. Cost | Notes |
|----------|-----------|--------------|-------|
| AWS RDS/DynamoDB | Yes (on Business Support) | ~$50-200/mo | Requires AWS Business Support ($100/mo min) for BAA |
| Google Cloud Firestore | Yes | ~$30-100/mo | BAA available on all paid plans |
| Azure Cosmos DB | Yes | ~$25-100/mo | BAA included with Azure |
| MongoDB Atlas | Yes (Dedicated tier) | ~$57/mo min | BAA on dedicated clusters only, not shared |
| CareCloud / DrChrono | Yes | Varies | Purpose-built for medical practices |

### Recommended Path Forward
1. **Now:** localStorage + auto-backup downloads (free, works for single user)
2. **Phase 2 (when selling to other doctors):** Add Supabase free tier for multi-device sync, accept the HIPAA gap for MVP, add data encryption at rest
3. **Phase 3 (production/revenue):** Google Cloud Firestore or AWS with BAA, proper HIPAA compliance, user authentication

---

## Brand Guide

### Colors (from official brand guide)
| Name | Hex | CSS Variable | Usage |
|------|-----|-------------|-------|
| Navy/Teal | `#245C7B` | `--sp-navy` | Primary, headers, sidebar |
| Gold/Tan | `#BB9973` | `--sp-gold` | Accents, badges, CTAs |
| Silver | `#BFBBBD` | `--sp-silver` | Secondary elements |
| Warm Beige | `#E5DBD6` | `--sp-border` | Borders, dividers |
| Light Warm | `#EAE3DF` | `--sp-warm` | Hover states, subtle backgrounds |
| Cream | `#F4EEEA` | `--sp-bg` | Page background |

### Additional UI Colors
| Name | Hex | CSS Variable | Usage |
|------|-----|-------------|-------|
| Mint | `#4D8E7F` | `--sp-mint` | Success, wellness, pipeline |
| Rose | `#B0616F` | `--sp-rose` | Warnings, alerts |
| Sky | `#3A7FA3` | `--sp-sky` | Links, secondary actions |

### Typography
- **Brand font:** Lorin (Extra Bold for headers, Light for body)
- **Web substitute:** Nunito (Google Fonts) — weights 300, 400, 600, 700, 800, 900
- **Header style:** "Starlight" in light weight, "PEDIATRICS" in uppercase, spaced, gold

### Logo Assets
- `logo-mark.png` — Heart + shooting star icon only (from Final Logos folder, variant 04)
- `logo-full.png` — Full logo with text (variant 01)
- `doctor.jpg` — Dr. Yogini Prajapati headshot (displayed in header)
- Source files in CPO repo: `/Final Logos/` folder (includes .ai, .eps, .pdf, .png, .jpg variants)

---

## File Structure
```
starlight-app/
├── index.html          # The entire app (HTML + CSS + JS, ~3000 lines)
├── doctor.jpg          # Dr. Yogini Prajapati headshot
├── logo-mark.png       # Heart + star logo mark
├── logo-full.png       # Full logo with text
├── .gitignore          # Ignores .DS_Store, node_modules, .env, .netlify
└── DOCS.md             # This file
```

---

## Deployment Checklist

When making changes:
1. Edit `index.html`
2. Test locally (just open in browser)
3. Deploy to Netlify:
   ```bash
   cp index.html doctor.jpg logo-mark.png logo-full.png /tmp/starlight-deploy/
   npx netlify-cli deploy --prod --dir=/tmp/starlight-deploy --site=3bc2dfac-9a6e-400c-ad1e-b49327d8ad94
   ```
4. Commit and push:
   ```bash
   git add -A && git commit -m "description" && git push origin main
   ```

---

## Known Limitations

1. **Single-device data** — localStorage doesn't sync across devices. Use backup/restore to transfer.
2. **No authentication** — anyone with the URL can access the app. Fine for now (single user), needs auth before multi-user.
3. **Auto-send requires app visit** — monthly email only sends if Dr. P opens the app between the 24th and 1st. A server-side cron would fix this but requires a backend.
4. **Version bump resets data** — changing the data model version (`sp_version`) re-seeds from `SEED_PATIENTS`, overwriting user data. Always back up before version bumps.
5. **EmailJS free tier limit** — 200 emails/month. More than enough for monthly reminders + occasional manual sends.

---

## Changelog

| Date | Change |
|------|--------|
| 2026-03-13 | Initial build: patient management, pricing tiers, wellness checks, billing alerts, revenue dashboard, email reminders |
| 2026-03-13 | Prospect pipeline: Kanban board with 4 stages, move/enroll functionality |
| 2026-03-13 | Brand refresh: exact brand guide colors, real logo, Dr. Yogini photo in header |
| 2026-03-13 | Replaced all "Atlas MD" references with "EMR" |
| 2026-03-13 | Fixed email reminders toggle layout, removed practice name field |
| 2026-03-13 | Data & Backup page: export/import, auto-backup, persistent storage, recovery |
| 2026-03-13 | EmailJS integration: real email sending (free, 200/mo) |
| 2026-03-13 | Auto-send monthly email on the 24th (one week before 1st) |
