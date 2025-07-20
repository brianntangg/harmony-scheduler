# 🎼 Volunteer Scheduling App for Harmonies for the Elderly (HFTE)

A full-stack web application built for *Harmonies for the Elderly* (HFTE), a Vanderbilt club that organizes musical performances at elderly homes. Volunteers submit their availability, instrument type, and ranked date preferences. The app automatically generates balanced schedules with complete musical groups for each event. This project aims to help a friend save time by eliminating the manual process of sorting through dozens of ranked preferences.

## 🧰 Tech Stack

| Layer     | Tech                          |
|-----------|-------------------------------|
| Frontend  | React + TypeScript + Vite     |
| Backend   | Java + Spring Boot (REST API) |
| Database  | H2 (in-memory dev DB)         |
| Build     | Gradle                        |
| API       | JSON (RESTful endpoints)      |

## 🧩 Core Features

- Submit volunteer profile with:
  - Name
  - Instrument (e.g., Piano, Violin)
  - Available dates
  - Ranked preferences for each available date
- Submit event dates with required instrument counts
- Generate a fair schedule that:
  - Assigns multiple volunteers per date
  - Fulfills instrument type requirements (e.g., 1 piano, 2 violins per event)
  - Respects each volunteer’s availability and preferences
  - Distributes assignments fairly over time

## 🗂️ Project Structure
```
volunteer-scheduler/
├── backend/
│ └── src/main/java/com/example/scheduler/
│ ├── controller/
│ ├── service/
│ ├── model/
│ └── repository/
├── frontend/
│ └── src/
│ ├── pages/
│ └── components/
└── README.md
```

## 🚀 Getting Started

### 1. Clone the Repo

```bash
git clone https://github.com/your-username/volunteer-scheduler.git
cd volunteer-scheduler
```

### 2. Start the Backend

```bash
cd backend
./gradlew bootRun
# Runs at http://localhost:8080
```

### 3. Start the Frontend
```bash
cd frontend
npm install
npm run dev
# Runs at http://localhost:5173
```

## 🔄 API Endpoints (PoC)

### ➕ Add Volunteer
**POST** `/api/volunteers`
```json
{
  "name": "Alice",
  "instrument": "Violin",
  "availableDates": ["2025-08-01", "2025-08-03"],
  "preferences": ["2025-08-03", "2025-08-01"]
}
```

### 🗓️ Add Event Dates and Instrument Requirements
**POST** `/api/events`
```json
{
  "events": [
    {
      "date": "2025-08-01",
      "requiredInstruments": {
        "Piano": 1,
        "Violin": 2,
        "Flute": 1
      }
    },
    {
      "date": "2025-08-03",
      "requiredInstruments": {
        "Piano": 1,
        "Guitar": 1
      }
    }
  ]
}
```
### 📅 Generate Schedule
**GET** `/api/schedule`
```
[
  {
    "date": "2025-08-01",
    "assignments": {
      "Piano": ["Bob"],
      "Violin": ["Alice", "Tom"],
      "Flute": ["Sara"]
    }
  },
  {
    "date": "2025-08-03",
    "assignments": {
      "Piano": ["Eli"],
      "Guitar": ["Nina"]
    }
  }
]
```

## 📐 Scheduling Logic

For each event date:

1. Load required instruments (e.g., `Piano: 1`, `Violin: 2`, `Flute: 1`)
2. For each instrument:
   - Filter volunteers who:
     - Are **available** on that date
     - Play the **required instrument**
   - Score each volunteer by their **preference rank**:
     - 1st choice = 3 points  
     - 2nd choice = 2 points  
     - 3rd choice = 1 point
   - Assign the top-scoring volunteer(s) to that instrument slot
3. Ensure:
   - No volunteer is assigned to multiple dates unless capacity allows
   - All instrument requirements are fulfilled
   - Assignments are **fairly distributed** over the schedule

## ✅ Testing

### 🧪 Backend Tests (Spring Boot)

Backend tests are written using **JUnit 5** and **Spring Boot Test**.

To run all backend tests:

```bash
cd backend
./gradlew test
```
Test coverage includes:
- Schedule generation logic
- Volunteer assignment per instrument
- REST API request/response validation

### 🧪 Frontend Tests (React + TypeScript)
Frontend tests use Vitest and React Testing Library.

To run all frontend tests:
```bash
cd frontend
npm run test
```
Test coverage includes:
- Form input validation
- API request handling
- Schedule rendering logic

### 🔧 Planned Test Cases
- Backend schedule fairness assertions
- Conflict resolution (overlapping assignments)
- Instrument fulfillment edge cases
- Frontend test coverage reporting
