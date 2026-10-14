# 🌿 EcoTrack — AI-Powered Smart Waste Management System

> **Cleaner Streets. Smarter Cities.**  
> A centralized, real-time civic waste management platform connecting citizens, collection teams, and municipal administrators with AI-assisted vision detection, IoT fill telemetry, and route optimization.

---

## 🌟 Key Highlights & Hackathon Features

1. **AI-Assisted Waste Vision Analysis**  
   - Photo upload with computer vision suggestions (category detection, urgency level, auto-generated title and descriptions, confidence score).  
   - Fully operational in Demo Mode with modular fallback for OpenAI Vision / Google Gemini.
2. **Smart Dustbin IoT Telemetry & Threshold Alarms**  
   - Real-time fill level monitoring (0–49% Normal, 50–79% Filling Up, 80–100% Critical).  
   - Interactive simulation controls for administrators to simulate live ultrasonic sensor fluctuations.  
   - Automatic warnings generated at 80% and emergency critical alerts at 95% capacity.
3. **Role-Based Workflows (RBAC)**  
   - **Citizen:** Submit waste reports with photo & GPS coordinates, track status timeline, earn Eco Points.  
   - **Collection Worker:** View assigned pickups, launch turn-by-turn navigation, update statuses, submit completion proof photos.  
   - **Administrator:** Command dashboard, worker dispatch, sensor controls, CSV reports export, municipal surveillance.
4. **Collection Route Optimization**  
   - Automated Nearest-Neighbor Traveling Salesperson heuristic solver.  
   - Orders pickup waypoints to minimize collection vehicle distance and emissions.
5. **Community Gamification**  
   - Points awarded on verified submissions (+10 pts) and completed cleanups (+50 pts).  
   - Badges: *Eco Rookie*, *Street Guardian*, *Recycling Champion*, and *Earth Hero*.  
   - City leaderboard with audited transaction histories.
6. **Environmental Impact & Analytics**  
   - Computed metrics: Average resolution turnaround time, category breakdown, fill distribution.  
   - Estimated remediated municipal waste (kg) and carbon emission avoidance (EPA standard 0.45 kg CO₂e/kg).

---

## 🏗️ Architecture & Tech Stack

```
NIAT/
├── client/                     # Frontend (React 18, Vite, TypeScript, Tailwind CSS)
│   ├── src/
│   │   ├── components/         # MapView, WasteCard, SmartBinCard, AiWasteClassifier, etc.
│   │   ├── context/            # AuthContext, NotificationContext
│   │   ├── pages/              # Landing, Dashboards, Smart Bins, Routes, Analytics, Ranks
│   │   └── types/              # TypeScript schema interfaces
│   └── dist/                   # Production build
└── server/                     # Backend (Node.js, Express, TypeScript, Prisma ORM)
    ├── prisma/
    │   ├── schema.prisma       # Relational models: User, WasteReport, SmartBin, Task, etc.
    │   └── dev.db              # SQLite Database
    └── src/
        ├── routes/             # auth, reports, bins, tasks, notifications, analytics, leaderboard
        ├── middleware/         # JWT verification & Role-based Access Control (RBAC)
        └── seed.ts             # Realistic 16 reports, 12 bins, users & badges
```

- **Frontend:** React 18, Vite, TypeScript, Tailwind CSS, Lucide React, Recharts, Leaflet, OpenStreetMap
- **Backend:** Node.js, Express, TypeScript, Prisma ORM, SQLite
- **Auth & Security:** JWT tokens, bcrypt password hashing, Helmet, Rate Limiting, Multer

---

## ⚡ Quick Start & Run Commands

### Prerequisites
- Node.js (v18 or higher recommended)
- npm

### 1. Backend Setup & Run

Open a terminal in `server/`:

```powershell
cd server

# 1. Install dependencies
npm install

# 2. Push database schema & generate Prisma client
npx prisma db push

# 3. Seed demo data (citizens, workers, admin, 12 bins, 16 reports)
npx ts-node src/seed.ts

# 4. Start the backend development server
npm run dev
```

The backend API will run on `http://localhost:5000`.

### 2. Frontend Setup & Run

Open a second terminal in `client/`:

```powershell
cd client

# 1. Install dependencies
npm install

# 2. Start the Vite development server
npm run dev
```

The frontend will run on `http://localhost:5173`.

---

## 🔑 Demo Login Credentials

For quick evaluation during hackathons, demo credentials are built in:

| Role | Email | Password | Permissions & Dashboard |
|---|---|---|---|
| **Citizen** | `citizen@ecotrack.org` | `citizen123` | Submit reports, photo uploads, view my reports, earn points |
| **Worker** | `worker@ecotrack.org` | `worker123` | View assigned tasks, start pickup, submit completion photos |
| **Admin** | `admin@ecotrack.org` | `admin123` | Full access: assign tasks, verify reports, simulate IoT sensors, CSV export |

*Tip: The Login page and Landing page also feature **1-click quick-login buttons** to seamlessly switch personas.*

---

## 📡 REST API Documentation

### Authentication
- `POST /api/auth/register` - Create new citizen or worker account
- `POST /api/auth/login` - Authenticate and receive JWT
- `GET /api/auth/me` - Get profile and eco points
- `POST /api/auth/logout` - Sign out

### Waste Reports
- `GET /api/reports` - List reports (role filtered, supports query filters & search)
- `POST /api/reports` - Create report (multipart/form-data with photo, GPS, category)
- `GET /api/reports/:id` - Single report details & audit history
- `PATCH /api/reports/:id/status` - Update status (SUBMITTED, VERIFIED, IN_PROGRESS, RESOLVED)
- `PATCH /api/reports/:id/assign` - Admin worker assignment

### Smart Bins (IoT)
- `GET /api/bins` - Retrieve all bins with fill levels
- `POST /api/bins` - Register new smart bin (Admin)
- `PATCH /api/bins/:id` - Update fill level or operational status
- `POST /api/bins/simulate` - Simulate dynamic telemetry fluctuations across grid

### Tasks
- `GET /api/tasks` - Worker assigned tasks
- `PATCH /api/tasks/:id` - Update status, notes, and upload completion proof

### Analytics & Gamification
- `GET /api/analytics/overview` - High level counts, resolution rate, environmental impact
- `GET /api/analytics/reports` - Category, status, and timeline aggregations
- `GET /api/analytics/export/csv` - Download CSV export of all reports
- `GET /api/leaderboard` - Community rankings and points
- `GET /api/leaderboard/my-rewards` - Citizen points transaction history and badges

---

## 🧪 Automated Testing

To run the automated verification suite covering health, authentication, smart bins, sensor simulation, analytics, and leaderboard:

```powershell
cd server
npx ts-node src/test-api.ts
```

---

## 📄 License & Hackathon Notice
Created as an open demonstration of Smart City civic tech and environmental management. All sensor telemetry and AI predictions run with realistic simulations and do not require external proprietary keys.
