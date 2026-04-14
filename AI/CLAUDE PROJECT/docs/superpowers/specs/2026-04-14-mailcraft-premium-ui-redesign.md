# MailcraftAI Premium UI Redesign

**Date:** 2026-04-14  
**Status:** Approved — ready for implementation  
**Goal:** Upgrade the MailcraftAI app UI to premium YC-backed startup quality. Linear.app meets Resend.com. Dense, purposeful, dark.

---

## 1. Design System

### Colors (CSS custom properties on `:root`)

| Variable | Value | Use |
|---|---|---|
| `--bg` | `#0a0a0a` | Page background |
| `--surface` | `#111111` | Cards |
| `--surface-elevated` | `#1a1a1a` | Hover states |
| `--border` | `#222222` | Default borders |
| `--border-hover` | `#333333` | Hover borders |
| `--primary` | `#00DC82` | Green accent |
| `--primary-hover` | `#00c974` | Green hover |
| `--primary-muted` | `rgba(0,220,130,0.08)` | Green tint bg |
| `--text-primary` | `#ffffff` | Headings, labels |
| `--text-secondary` | `#888888` | Body text |
| `--text-tertiary` | `#555555` | Muted hints |
| `--error` | `#ff4444` | Error states |
| `--warning` | `#ffaa00` | Warning states |
| `--success` | `#00DC82` | Success states |

### Typography

- **Font**: Inter from Google Fonts (add to root layout)
- **Sizes**: 11px labels · 13px body · 15px subheadings · 22px headings
- **Weights**: 400 / 500 / 600 only
- **Letter-spacing**: `0.04em` on all-caps labels

### Spacing & Shape

- Sidebar: 220px wide
- Content padding: 32px
- Card padding: 20px 24px
- Card gap: 12px
- Border radius: 8px cards · 6px inputs · 20px badges

---

## 2. Architecture Changes

### New Routes (all under `src/app/(app)/`)

| Route | File | Notes |
|---|---|---|
| `/analytics` | `analytics/page.tsx` | New page, mock data |
| `/sequences` | `sequences/page.tsx` | New page, mock data |
| `/prospects` | `prospects/page.tsx` | New page, mock data |

### Modified Files

| File | Change |
|---|---|
| `src/app/globals.css` | Add CSS variables, Inter font import, base resets |
| `src/components/layout/app-shell.tsx` | Full rebuild: new sidebar, topbar, user block, usage bar |
| `src/app/(app)/dashboard/page.tsx` | Full rebuild: 4 stat cards, campaigns table, activity feed, perf chart, quick actions |
| `src/app/(app)/campaigns/page.tsx` | Full rebuild: filter tabs, card layout, status badges |
| `src/app/(app)/quick-send/page.tsx` | Premium polish: section headers, enhanced output panel |
| `src/app/(app)/settings/page.tsx` | Organized into left-nav sections |

### New Components

| Component | Purpose |
|---|---|
| `src/components/ui/command-palette.tsx` | ⌘K command palette |
| `src/components/ui/toast.tsx` | Toast notification system |
| `src/components/layout/topbar.tsx` | Per-page header with title + actions |

### New Dependency

- **recharts** — for performance chart on dashboard and analytics page

---

## 3. Sidebar

220px sidebar, `#0a0a0a` bg, `1px solid #222` right border.

**Top section:**
- Logo: mail icon + "MailcraftAI" text + "v1.0" badge
- User block: avatar circle with initials, name, plan badge ("Starter" / "Pro")

**Nav items** (with icons):
1. Dashboard — `LayoutDashboard`
2. Campaigns — `Mail` + active count badge
3. Quick Send — `Zap`
4. Analytics — `BarChart2` (new)
5. Sequences — `GitBranch` (new)
6. Prospects — `Users` (new)
7. Settings — `Settings`

**Nav item states:**
- Inactive: `#888` text, no bg
- Hover: `#fff` text, `#1a1a1a` bg, 0.1s transition
- Active: `#fff` text, `rgba(0,220,130,0.08)` bg, `2px solid #00DC82` left border

**Bottom section:**
- Usage progress bar: "847 / 2,000 emails this month"
- Upgrade link (shown when < 20% remaining)
- Docs link
- `⌘K` hint

---

## 4. Topbar

Fixed header above content area per page:
- **Left**: Page title (h1 style) + optional breadcrumb
- **Right**: Search bar (opens ⌘K), notification bell with dot, "New Campaign" primary button

---

## 5. Dashboard Page

### Section 1 — 4 Stat Cards (grid-cols-4)

| Card | Value | Trend |
|---|---|---|
| Emails Sent | 1,247 | +12% green |
| Open Rate | 34.2% | +4.1% green |
| Reply Rate | 8.7% | -1.2% red |
| Active Campaigns | 3 | — |

Card: `#111` bg, `0.5px solid #222` border, 8px radius, 20/24 padding. Hover: border `#333`, scale 1.01.

### Section 2 — 60/40 Split

**Left (Recent Campaigns table):**
- Columns: Name, Status badge, Prospects, Sent, Open%, Reply%, Created, Actions
- Status badges: Live (green), Paused (yellow), Draft (gray), Complete (blue)
- Row height 44px, hover `#1a1a1a`, `0.5px solid #1a1a1a` dividers
- Header: 11px uppercase, `#555`, letter-spacing 0.06em

**Right (Activity Feed):**
- Title: "Recent Activity"
- 5 items: icon circle + description + company + timestamp
- Item height 52px, hover `#1a1a1a`

### Section 3 — Performance Chart

Full-width card. Recharts `LineChart`. Toggle: Sent | Opens | Replies. Green line, `#1a1a1a` grid, curved, dots on hover only.

### Section 4 — 3 Quick Action Cards

Start a Campaign · Quick Send · Import Prospects. Ghost button per card, green icon circle, hover border `#333`.

---

## 6. Campaigns Page

**Top bar**: Title + "X active, Y total" subtitle + filter dropdown + "New Campaign" button.

**Filter tabs**: All | Live | Paused | Draft | Complete. Active tab: green underline.

**Campaign cards** (not table):
- Top row: name + status badge + 3-dot menu
- Stats row: X Prospects | X Sent | X% Opens | X% Replies
- Progress bar: "Stage 2 of 3" with dots
- Bottom row: created date + "View details" + action button (Pause/Resume/Launch)
- Hover: border `#333`

**New Campaign**: multi-step wizard (Step 1: Basics → Step 2: Audience → Step 3: Sequence → Step 4: Review). Progress bar at top.

---

## 7. Quick Send Page

Keep existing 2-column layout. Enhancements:

**Left panel:**
- Input height: 42px
- Focus ring: `1px solid #00DC82`
- Label: 11px uppercase `#888`
- "Recent prospects" dropdown (last 5)

**Right panel:**
- Hook card: green left border, larger hook text
- Subject: `SUBJECT LINE` caps label, 16px text, copy icon on hover
- Body: monospace, prospect name highlighted green
- Action bar: Copy (outline) + Regenerate (outline) + Send (primary, large)
- "Sending from {email}" note below

---

## 8. Analytics Page (`/analytics`)

- Date range picker: 7d / 30d / 90d / Custom
- Row 1: 6 stat cards (Total Sent, Delivered, Opens, Clicks, Replies, Unsubscribes)
- Row 2: Bar chart (daily volume) + Line chart (open rate trend) side-by-side
- Row 3: Campaign breakdown table, sortable, Export CSV button
- Row 4: Top 5 subject lines by open rate
- **Data**: mock/placeholder for now

---

## 9. Sequences Page (`/sequences`)

Visual vertical flow builder:
- Day indicator circle on left
- Stage cards: name + subject preview + Edit/Preview buttons
- Dotted connector lines between stages + wait time pills
- "Add Stage" button at bottom
- **Data**: mock/placeholder for now

---

## 10. Prospects Page (`/prospects`)

- Search + filter tabs + Import CSV + Add manually
- Table: Name+Company, Title, Email, Campaign, Stage badge, Last contacted, Status, Actions
- Row click opens 380px slide-in right panel: prospect details + email timeline
- **Data**: mock/placeholder for now

---

## 11. Settings Page

Left-nav tabs: General · Email Sending · Email Defaults · Billing · Integrations · API · Danger Zone

Each section organized as labeled form groups with consistent input styling.

---

## 12. Global Components

### Command Palette (`⌘K`)
- Full-screen overlay, centered modal, 560px wide
- Search input at top
- Results: campaigns, quick actions, nav links
- Arrow keys to navigate, Enter to execute
- Styled like Linear's command palette

### Toast Notifications
- Bottom-right, slide in from right (200ms)
- Success: green left border · Error: red · Info: blue
- Auto-dismiss after 4 seconds

### Empty States
- Every page: icon + title + subtitle + CTA button
- Skeleton screens (pulse animation) for loading, not spinners

### Confirmation Dialogs
- Centered modal for destructive actions
- Red confirm button, Cancel on left

---

## 13. Mobile

- Sidebar → bottom tab bar
- Cards stack vertically
- Tables → card lists

---

## 14. Animations

| Element | Duration |
|---|---|
| Page transitions | 150ms fade |
| Card hover | 150ms |
| Sidebar active | 100ms |
| Modal open | 200ms scale from 0.95 |
| Toast slide in | 200ms from right |

No bouncing, no spinning.

---

## 15. Build Order

1. Design system (CSS variables, Inter font, globals.css)
2. Sidebar + Topbar (app-shell.tsx)
3. Dashboard rebuild
4. Campaigns page upgrade
5. Quick Send upgrade
6. Analytics page (new)
7. Sequences page (new)
8. Prospects page (new)
9. Settings upgrade
10. Global components (command palette, toasts, empty states, skeletons)
11. Mobile responsive pass
12. Final polish + recharts install

---

## 16. Key Constraints

- Keep all existing API routes and functionality intact
- No breaking changes to Supabase queries
- Dark mode only — no light mode toggle
- Use existing tech stack + add recharts
- Brand name: **MailcraftAI** (not MailZoo)
