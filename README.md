# NeuroNudge — AI-Assisted Mental Wellness Platform

A full-stack SaaS application for monitoring burnout, tracking emotional wellbeing, chatting with an AI companion, and scheduling doctor consultations.

---

## Tech Stack

| Layer      | Technology                              |
|------------|-----------------------------------------|
| Frontend   | HTML5, Tailwind CSS (CDN), Vanilla JS, Chart.js |
| Backend    | Java 17, Spring Boot 3.2, Spring Security |
| Database   | MongoDB                                  |
| Auth       | JWT (JJWT 0.11.5)                       |
| Build      | Maven                                   |

---

## Project Structure

```
neuronudge/
├── frontend/
│   ├── login.html              # Login page
│   ├── signup.html             # Registration page
│   ├── navbar.js               # Shared navigation component
│   ├── user-dashboard.html     # User main dashboard
│   ├── insights.html           # Wellness analytics & charts
│   ├── chat.html               # AI chat companion
│   ├── appointments.html       # Book & manage appointments (user)
│   ├── doctor-dashboard.html   # Doctor overview dashboard
│   ├── doctor-appointments.html # Doctor appointment management
│   └── patients.html           # Doctor patient insights panel
│
└── backend/
    ├── pom.xml
    └── src/main/java/com/neuronudge/
        ├── NeuroNudgeApplication.java
        ├── config/
        │   ├── AppConfig.java          # UserDetailsService bean
        │   └── SecurityConfig.java     # Spring Security + JWT + CORS
        ├── controller/
        │   ├── AuthController.java     # POST /api/auth/login, /signup
        │   ├── CheckInController.java  # POST /api/checkin, GET /week
        │   ├── BehaviorController.java # POST /api/behavior
        │   ├── ChatController.java     # POST /api/chat, GET /history
        │   ├── AppointmentController.java # Full appointment workflow
        │   ├── DoctorController.java   # Doctor-specific endpoints
        │   └── RecommendationController.java # Recommendations
        ├── model/
        │   ├── User.java
        │   ├── DailyLog.java
        │   ├── BehaviorLog.java
        │   ├── Appointment.java
        │   ├── ChatMessage.java
        │   └── Recommendation.java
        ├── repository/
        │   ├── UserRepository.java
        │   ├── DailyLogRepository.java
        │   ├── BehaviorLogRepository.java
        │   ├── AppointmentRepository.java
        │   ├── ChatMessageRepository.java
        │   └── RecommendationRepository.java
        └── security/
            ├── JwtUtil.java
            └── JwtAuthFilter.java
```

---

## Prerequisites

- Java 17+
- Maven 3.8+
- MongoDB (local or Atlas)
- A modern browser

---

## Setup & Run

### 1. Start MongoDB

**Local:**
```bash
mongod --dbpath /data/db
```

**Or use MongoDB Atlas** — update the URI in `application.properties`:
```properties
spring.data.mongodb.uri=mongodb+srv://<user>:<password>@cluster.mongodb.net/neuronudge
```

---

### 2. Configure the Backend

Edit `backend/src/main/resources/application.properties`:

```properties
server.port=8080
spring.data.mongodb.uri=mongodb://localhost:27017/neuronudge
spring.data.mongodb.database=neuronudge

# Change this secret in production!
app.jwt.secret=NeuroNudge_Super_Secret_JWT_Key_2024_Change_In_Production_Minimum_256_Bits
app.jwt.expiration=86400000
```

---

### 3. Build & Run the Backend

```bash
cd backend
mvn clean install
mvn spring-boot:run
```

The API will be available at `http://localhost:8080`

---

### 4. Serve the Frontend

**Option A — Copy into Spring Boot static folder:**
```bash
cp -r frontend/* backend/src/main/resources/static/
# Then rebuild and run the backend
mvn spring-boot:run
# Visit http://localhost:8080/login.html
```

**Option B — Use VS Code Live Server or any static file server:**
```bash
cd frontend
npx serve .
# Visit http://localhost:3000/login.html
```

**Option C — Python simple server:**
```bash
cd frontend
python3 -m http.server 3000
# Visit http://localhost:3000/login.html
```

---

## API Reference

### Authentication

| Method | Endpoint           | Auth | Description           |
|--------|--------------------|------|-----------------------|
| POST   | /api/auth/signup   | None | Register new user     |
| POST   | /api/auth/login    | None | Login, returns JWT    |

**Login request:**
```json
{ "email": "user@example.com", "password": "yourpassword" }
```

**Login response:**
```json
{ "token": "eyJ...", "role": "USER", "name": "Alex Johnson", "userId": "..." }
```

---

### Daily Check-In

| Method | Endpoint            | Auth     | Description              |
|--------|---------------------|----------|--------------------------|
| POST   | /api/checkin        | Bearer   | Submit daily wellness log |
| GET    | /api/checkin/history| Bearer   | All logs for current user |
| GET    | /api/checkin/week   | Bearer   | Last 7 days of logs       |

**POST body:**
```json
{
  "mood": 7,
  "stress": 5,
  "sleepHours": 7.5,
  "focusLevel": 6,
  "workHours": 8
}
```

---

### Behavior Tracking

| Method | Endpoint        | Auth   | Description               |
|--------|-----------------|--------|---------------------------|
| POST   | /api/behavior   | Bearer | Log browser behavior data  |

**POST body:**
```json
{ "typingSpeed": 240.5, "mouseActivity": 85, "idleTime": 12 }
```

---

### AI Chat

| Method | Endpoint          | Auth   | Description             |
|--------|-------------------|--------|-------------------------|
| POST   | /api/chat         | Bearer | Send message, get AI reply |
| GET    | /api/chat/history | Bearer | Retrieve chat history    |

**POST body:**
```json
{ "message": "I've been feeling very stressed lately", "sessionId": "optional-uuid" }
```

---

### Appointments

| Method | Endpoint                       | Auth   | Role   | Description               |
|--------|--------------------------------|--------|--------|---------------------------|
| POST   | /api/appointments/request      | Bearer | USER   | Request appointment       |
| GET    | /api/appointments/my           | Bearer | USER   | View my appointments      |
| POST   | /api/appointments/cancel       | Bearer | Both   | Cancel with reason        |
| POST   | /api/appointments/accept       | Bearer | DOCTOR | Accept appointment        |
| POST   | /api/appointments/reject       | Bearer | DOCTOR | Reject with reason        |
| GET    | /api/appointments/doctor       | Bearer | DOCTOR | All doctor appointments   |
| GET    | /api/appointments/doctor/pending| Bearer | DOCTOR | Pending requests only    |

---

### Doctor Endpoints

| Method | Endpoint                              | Auth   | Role   | Description             |
|--------|---------------------------------------|--------|--------|-------------------------|
| GET    | /api/doctor/patients                  | Bearer | DOCTOR | All patients + burnout  |
| GET    | /api/doctor/patients/{id}/insights    | Bearer | DOCTOR | Patient wellness data   |
| GET    | /api/doctor/dashboard                 | Bearer | DOCTOR | Doctor dashboard stats  |
| POST   | /api/recommendations                  | Bearer | DOCTOR | Send recommendation     |
| GET    | /api/recommendations/my               | Bearer | USER   | User's recommendations  |

---

## Demo Mode

The frontend works **without a running backend** for demonstration.

On the login page, click either:
- **Demo User** — opens the user dashboard with sample data
- **Demo Doctor** — opens the doctor dashboard

All check-in submissions, chat messages, and appointment actions fall back gracefully when the API is unavailable.

---

## MongoDB Collections

| Collection      | Description                         |
|-----------------|-------------------------------------|
| `users`         | User accounts with roles            |
| `daily_logs`    | Daily mood/stress/sleep check-ins   |
| `behavior_logs` | Browser interaction patterns        |
| `appointments`  | Consultation scheduling             |
| `chat_messages` | AI chat session history             |
| `recommendations` | Doctor recommendations to patients |

---

## Security Notes

- JWT tokens expire after 24 hours (`app.jwt.expiration=86400000` ms)
- Passwords are hashed with BCrypt (strength 10)
- Role-based access: `ROLE_USER` and `ROLE_DOCTOR`
- CORS is open in dev mode — restrict `app.cors.allowed-origins` in production
- **Change `app.jwt.secret`** before deploying to production

---

## Features

### User Side
- ✅ Dashboard with burnout risk, mood, sleep, stress indicators
- ✅ Interactive daily check-in with sliders
- ✅ Mood/stress/sleep charts (Chart.js)
- ✅ Mood calendar heatmap
- ✅ AI chat companion with keyword-aware empathetic responses
- ✅ Browse doctors and book appointments
- ✅ Real-time appointment status tracking
- ✅ Cancel appointments with reason
- ✅ Passive browser behavior tracking (typing speed, mouse activity, idle time)

### Doctor Side
- ✅ Overview dashboard (today's appointments, pending requests, high-burnout patients)
- ✅ Accept / reject appointment requests with reasons
- ✅ Complete appointment management panel with filters
- ✅ Patient list with risk levels and burnout scores
- ✅ Per-patient wellness charts (mood + stress history)
- ✅ Send personalized recommendations to patients

---

## Burnout Score Algorithm

```
Burnout% = (stress/10 × 35) + ((10-mood)/10 × 25) + (min(workHours/12,1) × 25) + (max(0,8-sleepHours)/8 × 15)
```

- Stress contributes 35% of the score
- Low mood contributes 25%
- Overwork contributes 25%
- Sleep deficit contributes 15%

---

## License

MIT — Free to use and modify.
