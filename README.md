# RepairTrack



RepairTrack is a multi-tenant repair shop management system for electronics service businesses.
It combines a responsive frontend (desktop + mobile), an Express/SQLite backend API, and an admin portal for platform-level controls.

## What this project includes

- **Repair operations**: intake, queue management, status tracking, delivery records
- **Customer workflow**: public tracking links (`/track/...`) and WhatsApp-ready status messaging
- **Inventory management**: spare parts stock, low-stock alerts, part-to-repair linking
- **Team management**: technicians, assignment flow, and commission reports
- **Billing layer**: FREE/PRO usage limits, verification requests, cancellation requests
- **Admin portal** (`admin.html`): verify payments, manage subscriptions, platform settings
- **Notifications**: CallMeBot-based WhatsApp + Telegram alerts (admin + customer + daily summary)
- **PWA support**: `manifest.json` + `service-worker.js` caching
- **Desktop packaging**: Electron app support (`main.js`, `npm run start`)

## Repository structure

- `/index.html` - main frontend UI (responsive app shell and screens)
- `/script.js` - primary app controller logic (auth, repairs, inventory, billing, reports)
- `/js/store.js` - data manager + API abstraction and client-side state
- `/js/views/` - mobile/desktop view generators
- `/style.css` and `/css/` - UI styles
- `/server/server.js` - Express API, auth/session handling, SQLite setup/migrations, cron jobs
- `/server/notificationService.js` - WhatsApp/Telegram notification flows
- `/admin.html` - SaaS/admin dashboard for approvals and overrides
- `/_redirects` - Netlify routing/proxy rules to backend
- `/main.js` - Electron main process (desktop runtime)

## Architecture at a glance

1. **Frontend** calls API endpoints under `/api/*`.
2. **Netlify redirects** proxy `/api/*`, `/uploads/*`, and `/track/*` to the backend host.
3. **Backend** persists data in SQLite (`server/laptop_repair.db`) and exposes workshop-scoped APIs.
4. **Notifications + cron** run server-side for status pushes, subscription expiry checks, and daily summaries.
5. **Admin panel** uses protected admin endpoints for verifications, cancellations, workshop overrides, and global settings.

## Local setup

### 1) Backend

```bash
cd /home/runner/work/repairtrack/repairtrack/server
npm install
npm start
```

Server runs on `http://localhost:3000` by default.

### 2) Frontend (web)

Use any static server from repo root, then open the app in browser:

```bash
cd /home/runner/work/repairtrack/repairtrack
# example:
python -m http.server 8080
```

Then open `http://localhost:8080`.

### 3) Desktop app (Electron)

```bash
cd /home/runner/work/repairtrack/repairtrack
npm install
npm start
```

## Deployment notes

- Backend deployment helper script: `/server/user-data.sh`
- Netlify proxy rules: `/_redirects`
- Uploads are served from `/server/uploads`

## Tech stack

- **Frontend**: HTML, CSS, vanilla JavaScript
- **Desktop**: Electron
- **Backend**: Node.js, Express, SQLite (`sqlite3` + `sqlite`)
- **File uploads**: Multer
- **Notifications**: CallMeBot HTTP APIs

## Current scripts

### Root (`package.json`)
- `npm start` - run Electron app
- `npm run build` - build desktop package via electron-builder

### Server (`server/package.json`)
- `npm start` - run backend API server

## Notes

- No automated test suite is currently defined in `package.json` scripts.
- Keep secrets/tokens out of source and configure them through admin/system settings.
