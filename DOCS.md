# Starlight Pediatrics — Patient Manager

## Overview

A single-page web app for **Starlight Pediatrics**, Dr. Yogini Prajapati's Direct Primary Care (DPC) pediatric practice. Replaces the practice's Excel spreadsheet with a live, branded patient management and communication tool.

- **Live URL:** https://starlight-pediatrics.netlify.app
- **GitHub Pages:** https://dsurana04.github.io/starlight-pediatrics-app/
- **Repo:** https://github.com/dsurana04/starlight-pediatrics-app
- **Built by:** Deepak Surana (CPO)
- **Doctor:** Dr. Yogini Prajapati (`drp@starlightpediatrics.com`)

---

## Features

### Patient Management
- Add, edit, delete patients via modal form
- Fields: first/last name, DOB, status, parent/guardian, phone, email, notes, fee override, family group, referred by
- Active vs. Prospect status
- 21 seed patients pre-loaded (17 active including Riya Surana test patient, 4 prospects)
- Patient table with sorting on ALL columns, filtering (All / Active / Prospects)
- **Prospect-specific table view:** shows Name, Phone, Email, Due Date/DOB, Stage, editable Follow-Up Date (instead of wellness/plan change columns)
- Payment status dots (green = paid, red = unpaid) next to patient names
- Google Review star badge next to patients who have left a review
- Auto-merges new seed patients into existing localStorage data on load

### Patient Detail View
- Full patient profile with KPIs (monthly fee, annual value, DOB, next plan change)
- Contact info card
- EMR action alerts for upcoming billing changes
- Wellness check timeline (past/current/future with scheduling)
- Family members card (when linked to a family group)
- Payment status toggle (paid/unpaid this month)
- Google Review status + "Send Review Request" button
- Visit log with type-colored badges and follow-up flags
- Notes with timestamped entries
- Email history showing all messages sent to this patient

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
- Family savings: 25% sibling discount, $500/mo family cap, 5% annual prepay

### Family Linking
- Assign patients to family groups via dropdown in patient modal
- "Create New Family Group" option generates a unique family ID
- Family members card in patient detail shows all siblings
- Family discount information displayed when family group exists

### Visit Log
- Log visits with type selection: Well-Child, Sick, Home Visit, Phone/Text Consult, Other
- Fields: date, visit type, chief complaint, follow-up needed checkbox, notes
- Color-coded visit type badges in patient detail timeline
- Visit history displayed in reverse chronological order

### Newborn Home Visit Checklist
- Structured checklist for Orion-tier (newborn) patients
- 5 categories with 20 assessment items:
  - **Feeding:** latch, formula, frequency, weight gain
  - **Vitals & Growth:** weight, temperature, heart rate, head circumference
  - **Newborn Exam:** jaundice, cord care, skin, fontanelle
  - **Safety & Sleep:** sleep position, crib safety, car seat, smoke detectors
  - **Parent Wellness:** postpartum mood, support system, questions, follow-up
- Each item rated: Normal / Concern / N/A
- Saves as a visit record with full checklist data
- "Home Visit" button only appears for Orion-tier patients

### Payment Tracking
- Per-patient, per-month payment status (paid/overdue)
- Toggle payment status from patient detail, billing page, or dashboard
- Color-coded status dots in patient table
- Clickable payment toggle in billing table
- Overdue payment alerts on dashboard with "Mark Paid" buttons
- Payment rate tracked in practice report

### Referral Tracking
- "Referred By" field on all patients (not just prospects)
- Referral leaderboard on dashboard showing top referral sources
- Referral data included in practice report

### Wellness Check Scheduling
Based on AAP guidelines, calculates wellness check dates from DOB:
- 2, 4, 6, 9, 12, 15, 18, 24, and 30 months
- Schedule modal with date picker and notes (for Dr. Prajapati)
- Green "completed" badges for scheduled items
- Tracks scheduled vs. unscheduled checks
- Month-by-month view for next 12 months

### Prospect Pipeline
Kanban-style board with 4 stages:
1. **Inquiry** — new leads/contacts
2. **Meet & Greet** — initial conversation
3. **Scheduled Visit** — visit booked
4. **Ready to Enroll** — final step before active membership

- Move prospects between stages with arrow buttons
- "Enroll" button converts prospect to active member
- Prospect-specific fields: pipeline stage, due date
- KPIs: total prospects, ready to enroll, due within 60 days, visits scheduled
- **Automated Nurture Sequence** — see Prospect Nurture section below

### Wix Website Lead Capture
- Automatic lead capture via URL parameters (`?lead=1&firstName=...&lastName=...&phone=...&email=...&notes=...`)
- Creates a new prospect in the pipeline automatically
- Embeddable HTML form snippet displayed in Data & Backup page
- Dr. P can paste the form into a Wix "Embed HTML" element on her website
- Leads tagged with "Website" referral source
- Duplicate detection prevents re-adding existing patients

### Revenue Dashboard
- MRR (Monthly Recurring Revenue) and ARR (Annualized Revenue)
- Average revenue per patient and lifetime value (3-year)
- Revenue by tier breakdown with patient counts
- 6-month forecast chart
- 12-month projection table with tier change annotations
- Historical revenue data from 2025 (charges-by-category.csv)
- Year filter dropdown (2025 / 2026 / All) for historical view
- Bar chart + detailed table with growth summary

### Growth Simulator
- "What If?" sliders for each pricing tier (0–20 patients each)
- Live projection of: total patients, projected MRR, projected ARR, growth %
- Helps Dr. P visualize practice growth scenarios

### Action Items
- Auto-generated alerts for:
  - Upcoming wellness checks
  - Billing tier changes (EMR updates needed)
  - Prospect follow-ups
- Urgency levels: urgent, upcoming
- Schedule buttons with date tracking
- Filter by type (all, billing, wellness, follow-up)

---

## Messages & Email Hub

### Patient Journey Timeline (default tab)
Visual birth-to-childhood communication map with 16 milestone stages:

| Stage | Age | Emails Triggered |
|-------|-----|-----------------|
| Welcome | Day 1 | Welcome New Family, What to Expect |
| First Week | Day 3–5 | Newborn First Week Check-In |
| First Visit | 2 Weeks | Schedule First Wellness Visit |
| Wellness | 2 Months | Wellness Check Reminder, 1-Week Reminder |
| Tier Change | 3 Months | Billing Change (Orion→Lyra), Google Review Request |
| Wellness | 4, 6, 9 Months | Wellness Check Reminders |
| Birthday + Tier | 1 Year | Birthday, Billing Change (Lyra→Sirius), Wellness |
| Wellness | 15, 18, 24, 30 Months | Wellness Check Reminders |
| Annual | 3+ Years | Birthday, Seasonal Check-In, Review Request |
| Tier Change | 4 Years | Billing Change (Sirius→Pegasus) |
| Ongoing | As needed | Missed Appointment, Payment Reminder |

- Color-coded timeline nodes (onboarding, wellness, billing, birthday, review)
- Each stage shows plan tier when it changes
- Preview of every email template at each stage
- One-click **Edit** button to customize any template
- Legend and "How this works" guide

### Email Templates (17 built-in)
Organized by category with merge tags that auto-fill per patient:

**Onboarding (4):**
- Welcome New Family
- Newborn — First Week Check-In
- Newborn — 2 Week Follow-Up
- New Patient — What to Expect

**Wellness (2):**
- Upcoming Wellness Check
- Wellness Check — 1 Week Reminder

**Billing (2):**
- Billing Change Notice
- Payment Reminder

**General (3):**
- Missed Appointment Follow-Up
- Happy Birthday
- Seasonal Check-In

**Review (1):**
- Google Review Request (uses first names only — `{{parent_first}}`, `{{patient_first}}`)

**Nurture (5):**
- Nurture: Welcome Inquiry (Day 0)
- Nurture: Meet Dr. P (Day 2)
- Nurture: Why DPC Works (Day 5)
- Nurture: What Families Say (Day 10)
- Nurture: We're Here When You're Ready (Day 21)

**Available merge tags:** `{{patient_name}}`, `{{patient_first}}`, `{{parent_name}}`, `{{parent_first}}`, `{{age}}`, `{{next_wellness}}`, `{{monthly_fee}}`, `{{tier_name}}`, `{{google_review_link}}`

New templates auto-merge into existing localStorage on load (no data loss).

### Smart Reminders
Auto-generates all messages Dr. P needs to send right now based on actual patient data:
- Wellness checks due in next 30 days
- Billing changes in next 60 days
- Newborn check-ins (Orion tier, < 1 month old)
- Google review requests (active > 3 months, no review)
- Each reminder has: preview, Copy button, Send Email button (if patient has email)

### Email Sending
- **Preview modal** — always see the full rendered email before sending (works even without an email on file)
- Professional HTML email template with:
  - Logo + navy gradient header
  - "Starlight Pediatrics — Direct Primary Care for Children"
  - Signature: "Yogini Prajapati MD, Founder, Starlight Pediatrics, drp@starlightpediatrics.com"
  - Social links: Instagram, Facebook, Website
  - **CAN-SPAM compliant:** business name, Austin TX, unsubscribe mailto link
- **From name:** "Yogini Prajapati MD" (no comma — EmailJS treats commas as separators)
- Email log tracks every sent message per patient
- Auto-append Google Review nudge to outbound emails for non-reviewers
- All emails logged in patient's Email History and global Email Log tab

### Google Review Tracking
- `googleReview` field on each patient (date reviewed or false)
- Gold star badge in patient table for reviewed families
- Review toggle + "Send Review Request" in patient detail
- **Reviews tab** in Messages shows:
  - Review request template preview with Edit button
  - Who hasn't reviewed with last-sent date tracking
  - **30-day follow-up cadence:** green "Sent Xd ago" badge if < 30 days, yellow "Follow up" badge when 30+ days
  - Total send count per patient
  - Google Review link settings (moved inside Reviews tab)
- **Default Google Review link:** `https://g.page/r/CTEazz5Muc6nEBM/review`
- Auto-nudge: review request footer appended to all outbound emails for non-reviewers

### Prospect Nurture Sequences
Automated 5-step drip campaign for converting prospects into active patients:

| Step | Day | Template | Applicable Stages |
|------|-----|----------|-------------------|
| 1 | 0 | Welcome Inquiry — what makes Starlight special | All |
| 2 | 2 | Meet Dr. P — personal intro, invite to Meet & Greet | Inquiry, Meet & Greet |
| 3 | 5 | Why DPC Works — value prop, direct access, no copays | Inquiry, Meet & Greet, Scheduled Visit |
| 4 | 10 | What Families Say — social proof, Google reviews link | Inquiry, Meet & Greet, Scheduled Visit |
| 5 | 21 | Gentle Follow-Up — we're here when you're ready | All |

- **Auto-initializes** when a prospect is created (manual or via Wix lead capture)
- **Stage-aware:** steps auto-skip when prospect advances past applicable stages
- **Nurture tab** in Messages: per-prospect step-by-step view with Preview & Send buttons
- **Pause/Resume** toggle per prospect
- Marks step as sent after successful email delivery
- Badge count shows total due nurture steps
- Data stored per patient in `nurture: { startDate, stepsSent[], paused }`

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

---

## Other Pages

### Billing
- Current MRR and ARR
- Plan changes in 30/60/90 days
- Full billing table with payment status column (clickable toggle)
- Next plan change and EMR action for each patient

### Pricing Guide
- Full plan details with descriptions (star-themed names)
- Wellness check schedule table
- Plan transition rules
- Family savings info

### Practice Report
- Printable/PDF-able practice summary
- Patient census by tier with bar chart
- Key metrics: MRR, ARR, avg revenue, wellness compliance, payment rate, visit count
- Pipeline summary by stage
- Referral sources leaderboard
- Visits by type breakdown
- Print-optimized CSS (hides sidebar/header)

### Data & Backup
- Export all data as JSON file (patients, schedules, payments, templates)
- Import/restore from backup file
- Stats display (active patients, prospects, scheduled items)
- "How Your Data Works" explainer for non-technical users
- Wix Lead Capture embed code with copy button

---

## Integrations

### EmailJS (Email Sending)
- **Service:** EmailJS (https://emailjs.com)
- **Plan:** Free tier — 200 emails/month
- **Purpose:** Sends monthly checklist emails to Dr. P + individual emails to parents
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

### Wix Website (Lead Capture)
- Embeddable HTML form for Dr. P's Wix website
- Form submits via GET params to the app URL
- App auto-creates prospect on load when `?lead=1` is detected
- Embed code available in Data & Backup page

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
- **Frontend:** Single HTML file (`index.html`) with inline CSS and vanilla JavaScript (~5,200 lines)
- **Font:** Nunito (Google Fonts) — web substitute for brand font Lorin
- **No framework, no build step, no dependencies** (except EmailJS CDN)
- **Assets:** `doctor.jpg` (Dr. Yogini photo), `logo-mark.png` (heart+star icon), `logo-full.png` (full logo)

### Data Model (version 7)
Patient object:
```javascript
{
  id, firstName, lastName, dob, status, // 'active' | 'prospect'
  parent, phone, email, notes, feeOverride, notesList[],
  pipelineStage, dueDate,              // prospect-only
  familyId,                             // links siblings
  referredBy,                           // referral source
  visits[],                             // visit log entries
  googleReview,                         // false or ISO date string
  emailLog[],                           // sent email records
  nurture,                              // prospect-only: { startDate, stepsSent[], paused }
}
```

Visit object:
```javascript
{
  id, date, type,    // 'well-child' | 'sick' | 'home-visit' | 'phone' | 'other'
  complaint, followUp, notes,
  checklistData,     // only for home visits — structured checklist results
}
```

### Data Storage
- **Primary:** Browser `localStorage`
  - `sp_patients` — patient array (JSON)
  - `sp_schedules` — schedule data (JSON)
  - `sp_payments` — payment status per patient per month (JSON)
  - `sp_message_templates` — email templates (JSON)
  - `sp_settings` — email/reminder settings (JSON)
  - `sp_version` — data model version (currently `'7'`)
  - `sp_google_review_link` — configurable Google Review URL
  - `sp_last_backup` — timestamp of last manual backup
  - `sp_auto_backup` — redundant copy of all data (auto-saved on every change)
  - `sp_auto_download_week` — tracks weekly auto-download (e.g. `'2026-W11'`)
  - `sp_auto_email_sent_2026-04` — tracks auto-sent monthly emails
  - `sp_last_email_sent` — timestamp of last manual email send

### Sidebar Navigation (14 pages)
| Page | data-page | Description |
|------|-----------|-------------|
| Dashboard | `dashboard` | KPIs, actions, wellness, overdue payments, referral leaderboard |
| Patients | `patients` | Patient table with search, sort, filter |
| Pipeline | `pipeline` | Kanban board for prospects |
| Billing | `billing` | Billing table with payment toggles |
| Wellness | `wellness` | 12-month wellness check calendar |
| Actions | `actions` | All action items with scheduling |
| Revenue | `revenue` | Revenue dashboard + growth simulator |
| Reminders | `reminders` | Monthly email generator + auto-send |
| Messages | `messages` | Patient journey, nurture sequences, templates, reviews, email log |
| Pricing | `pricing` | Plan details and family savings |
| Report | `report` | Printable practice report |
| Backup | `backup` | Export/import, Wix embed code |

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
Data model version is currently `'7'`. Migration chain: v5 → v6 → v7.
- `migrateToV6()` — adds familyId, referredBy, visits[], googleReview, emailLog[]
- `migrateToV7()` — adds nurture field (auto-initializes for existing prospects with startDate 7 days ago)
- Migrations run automatically on load. New seed patients are auto-merged by ID.

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
- Visit records

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
├── index.html          # The entire app (HTML + CSS + JS, ~4,800 lines)
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
4. **Version bump resets data** — changing the data model version re-seeds from `SEED_PATIENTS`, overwriting user data. Always back up before version bumps. Migration functions handle graceful upgrades (v5→v6).
5. **EmailJS free tier limit** — 200 emails/month. Sufficient for monthly reminders + parent emails for a small practice. Monitor if sending volume increases.
6. **Google Review link** — pre-configured to `https://g.page/r/CTEazz5Muc6nEBM/review`. Can be changed in Messages > Reviews tab.
7. **Email preview works without email** — patients without an email address can preview what would be sent, but Send is disabled. Copy is available as fallback.
8. **EmailJS from name** — must not contain commas (EmailJS treats commas as list separators). Currently set to "Yogini Prajapati MD" in both app code and EmailJS dashboard template.

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
| 2026-03-13 | Added 2025 historical revenue data with year filter on revenue page |
| 2026-03-13 | 10 new features: payment tracking, visit log, family linking, referral tracking, newborn home visit checklist, message templates, wellness reminders, growth simulator, practice report, Wix lead capture |
| 2026-03-13 | Data model v5→v6 migration: familyId, referredBy, visits[], googleReview, emailLog[] |
| 2026-03-13 | Email Hub: 12 lifecycle templates, send-to-parent via EmailJS, email log tracking |
| 2026-03-13 | Google Review tracking: per-patient status, review progress banner, auto-nudge in emails |
| 2026-03-13 | Patient Journey Timeline: visual birth-to-childhood communication map with 16 stages, linked templates, one-click edit |
| 2026-03-13 | Pill-nav redesign for Messages tab — icons, badges, clearer clickable UI |
| 2026-03-13 | All patient table columns now sortable (added Next Wellness, Plan Change) with sort direction arrows |
| 2026-03-13 | Prospect-specific table view: phone, email, stage, editable follow-up date columns |
| 2026-03-13 | Professional HTML email template: logo header, CAN-SPAM footer, social links, signature block |
| 2026-03-13 | Email preview modal — see exact email before sending, works even without email on file |
| 2026-03-13 | From name changed to "Yogini Prajapati MD" across all email sends |
| 2026-03-13 | Google Review template rewritten with Dr. P's authentic voice, first names only |
| 2026-03-13 | Added `{{patient_first}}` and `{{parent_first}}` merge tags |
| 2026-03-13 | Review tab: last-sent tracking per patient with 30-day follow-up cadence badges |
| 2026-03-13 | Google Review link set to real URL: `https://g.page/r/CTEazz5Muc6nEBM/review` |
| 2026-03-13 | Added Riya Surana as test patient (dual email: yogini + dsurana) |
| 2026-03-13 | Auto-merge new seed patients into existing localStorage on load |
| 2026-03-13 | Data model v6→v7: nurture field on patient objects |
| 2026-03-13 | Automated Prospect Nurture Sequences: 5-step drip campaign (Day 0, 2, 5, 10, 21) |
| 2026-03-13 | Nurture tab in Messages: per-prospect step tracker, Preview & Send, Pause/Resume |
| 2026-03-13 | Stage-aware nurture: steps auto-skip when prospect advances past applicable stages |
| 2026-03-13 | New nurture templates auto-merge into existing localStorage |
