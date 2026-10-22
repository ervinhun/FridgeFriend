# FridgeFriend

> **Status: planning phase.** Nothing here is built yet — this repository currently holds the plan and the empty project skeleton. Everything described below is the *intended* scope of the project and may still change.

FridgeFriend is a cross-platform, multi-user fridge/freezer inventory app. It lets people track what is in their fridges and freezers, share access with their household (or office), and get notified before food expires — so less of it ends up in the bin.

## Planned features

- **Cross-platform client** — one Flutter codebase for Android and iOS.
- **Accounts & security** — dedicated backend API with authentication and authorization.
- **Multi-user sharing** — each user can belong to several independent fridges/freezers, with granular rights (add-only, use-only, both), time-limited access, multiple owners, ownership transfer and an owner-only activity log.
- **Core inventory** — items with per-container and per-product-type default expiry, categories, product images, search and filtering.
- **Barcode scanning** — via the phone camera or an ESP32-based scanner mounted at the fridge, with product data looked up. Scans become *pending* entries that the user confirms (amount, expiry).
- **Offline support** — the app stays usable without a connection; queued operations sync on reconnect (per-item last-write-wins, delete wins over edit).
- **Expiry notifications** — configurable timing and recipients.
- **Localization** — ships with at least two languages, structured so more can be added later.
- **Feature flags** — non-core features (e.g. scanning, monetization) can be switched off per region without a redeploy.

## Planned tech stack

| Area | Technology |
|---|---|
| Mobile client | Flutter (Android + iOS) |
| Backend API | C# / .NET 10, Docker |
| Database | PostgreSQL (local in dev, online db in staging/prod), Flyway migrations |
| Hosting | free hosting service for now, might be VPS later |
| Feature flags | FeatureHub (self-hosted) |
| Hardware | ESP32 barcode scanner |
| Monitoring | Firebase Crashlytics & Analytics (mobile), Sentry (backend) |
| CI/CD | GitHub Actions |

## Repository layout

This repo is the parent project. `App/` and `Backend/` are git submodules, each pointing at its own repository so the client and the API can be developed, versioned and reviewed independently.

| Folder | Contents | Repository |
|---|---|---|
| [`App/`](App/) | Flutter client application | [ervinhun/App](https://github.com/ervinhun/App) |
| [`Backend/`](Backend/) | Backend API | [ervinhun/Backend](https://github.com/ervinhun/Backend) |
| [`.github/`](.github/) | CI/CD workflows and code owners | — |

### Cloning

```sh
git clone --recurse-submodules git@github.com:ervinhun/FridgeFriend.git
```

Already cloned without `--recurse-submodules`? Run:

```sh
git submodule update --init --recursive
```

### Updating a submodule pointer

After pushing changes in `App` or `Backend`, bump the parent repo's pointer so it tracks the new commit:

```sh
cd App   # or Backend
git checkout main && git pull
cd ..
git add App   # or Backend
git commit -m "Bump App submodule"
```

## Roadmap — sprints

The project is planned as five two-week sprints, from **26 Oct 2026 to 3 Jan 2027**, following the Scrum plan. Each story has two checkboxes: **Planned** (ticked once the story is in the Scrum backlog) and **Done** (ticked once the story is finished).

### Sprint 1 — "Decide, scaffold, order"

_26 Oct – 6 Nov 2026_

**Goal:** Problem definition submitted for approval, requirements gathered from real users, data model and API contract locked, hardware ordered, app and backend skeletons running with CI, backend auth endpoints working.

- **US-01 — Approved problem definition & project identity**
  - [ ] Planned
  - [ ] Done
- **US-02 — Locked design: requirements, data model, API contract, wireframes**
  - [ ] Planned
  - [ ] Done
- **US-03 — Hardware procured**
  - [ ] Planned
  - [ ] Done
- **US-04 — Development environment, skeletons & CI**
  - [ ] Planned
  - [ ] Done
- **US-10 (backend part) — Register & log in**
  - [ ] Planned
  - [ ] Done

### Sprint 2 — "Log in, create a fridge, put things in it"

_9 Nov – 20 Nov 2026_

**Goal:** A user can register, log in, create a fridge with containers, invite someone by code, add/edit/remove items with correct default expiry, and owners can manage member rights. The ESP32 posts its first barcode to the API. Every merge to `main` deploys to the dev environment.

- **US-10 (client part) — Register & log in**
  - [ ] Planned
  - [ ] Done
- **US-20 — Fridges, containers & joining**
  - [ ] Planned
  - [ ] Done
- **US-30 — Item CRUD with default expiry**
  - [ ] Planned
  - [ ] Done
- **US-40 — Member & rights management**
  - [ ] Planned
  - [ ] Done
- **US-51 (v0) — ESP32 posts a barcode**
  - [ ] Planned
  - [ ] Done
- **US-90/91/92/94 — CI/CD & engineering practice (part 2)**
  - [ ] Planned
  - [ ] Done
- **US-101/103 — Report & contribution logs (part 1)**
  - [ ] Planned
  - [ ] Done

### Sprint 3 — "Share it, scan it, read it offline"

_23 Nov – 4 Dec 2026_

**Goal:** Full sharing model, barcode scanning via phone and ESP32 with the pending-confirmation flow, search/filter, owner expiry overrides and an offline read cache. Ends with the integration checkpoint, the first production deployment and the mid-term progress report.

- **US-41 — Time-limited access**
  - [ ] Planned
  - [ ] Done
- **US-42 — Multiple owners, ownership transfer, leave**
  - [ ] Planned
  - [ ] Done
- **US-43 — Owner-only activity log**
  - [ ] Planned
  - [ ] Done
- **US-50 — Phone camera scan + Open Food Facts lookup**
  - [ ] Planned
  - [ ] Done
- **US-52 — Pair an ESP32 device to a fridge**
  - [ ] Planned
  - [ ] Done
- **US-53 — Pending scans confirmation flow**
  - [ ] Planned
  - [ ] Done
- **US-51 (v1) — Firmware robustness**
  - [ ] Planned
  - [ ] Done
- **US-34 — Search & filter**
  - [ ] Planned
  - [ ] Done
- **US-72 — Owner expiry overrides**
  - [ ] Planned
  - [ ] Done
- **US-60 — Offline read cache**
  - [ ] Planned
  - [ ] Done
- **US-93 (part 1) — Validation & error model**
  - [ ] Planned
  - [ ] Done
- **US-90/91 — CD to production & mobile delivery**
  - [ ] Planned
  - [ ] Done
- **US-100/101/103 — Integration checkpoint & mid-term report**
  - [ ] Planned
  - [ ] Done

### Sprint 4 — "Sync, notify, flag, harden"

_7 Dec – 18 Dec 2026_

**Goal:** Offline write sync with conflict rules, expiry push notifications, product images, two languages, FeatureHub flags with dormant monetization, security pass, usability test and rollback drill. **Feature-complete, code freeze on Fri 18 Dec.**

- **US-61 — Offline writes & sync**
  - [ ] Planned
  - [ ] Done
- **US-70 — Expiry push notifications**
  - [ ] Planned
  - [ ] Done
- **US-71 — Notification settings**
  - [ ] Planned
  - [ ] Done
- **US-35 — Product images**
  - [ ] Planned
  - [ ] Done
- **US-73 — Two languages**
  - [ ] Planned
  - [ ] Done
- **US-80 — Feature flags via FeatureHub**
  - [ ] Planned
  - [ ] Done
- **US-81 — Dormant monetization**
  - [ ] Planned
  - [ ] Done
- **US-74 — Account settings**
  - [ ] Planned
  - [ ] Done
- **US-93 (part 2) — Security & hardening**
  - [ ] Planned
  - [ ] Done
- **US-90 (part 4) — Release engineering & rollback**
  - [ ] Planned
  - [ ] Done
- **US-94 — Test, usability & quality gate**
  - [ ] Planned
  - [ ] Done
- **US-101/102/103 — Report (part 3)**
  - [ ] Planned
  - [ ] Done
- **Stretch (only if everything above is done) — iOS delivery to TestFlight, smart shopping list (US-110)**
  - [ ] Planned
  - [ ] Done

### Sprint 5 — "Fix, polish, write"

_21 Dec 2026 – 3 Jan 2027_

**Goal:** No new features. Fix bugs from the regression and usability test, polish, finish the exam report, record the demo and prepare the individual exam presentations.

- **US-120 — Bug fixes & polish**
  - [ ] Planned
  - [ ] Done
- **US-101 (final) — Exam report**
  - [ ] Planned
  - [ ] Done
- **US-102 — Demo & handover**
  - [ ] Planned
  - [ ] Done
