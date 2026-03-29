# NeuroNudge — AI-Assisted Mental Wellness Platform

A backend-heavy SaaS application for monitoring burnout, tracking emotional wellbeing, chatting with an AI companion, and scheduling doctor consultations.

---

## Tech Stack

| Layer      | Technology                                      |
|------------|-------------------------------------------------|
| Frontend   | HTML5, Tailwind CSS (CDN), Vanilla JS, Chart.js |
| Backend    | Java 17, Spring Boot 3.2, Spring Security       |
| Database   | MongoDB                                          |
| Auth       | JWT (JJWT 0.11.5), Google OAuth                 |
| AI         | Groq API (LLaMA 3.3 70B)                        |
| Email      | Gmail SMTP                                       |
| Build      | Maven                                            |

---

## Project Structure

```
neuronudge/
├── frontend/
│   ├── login.html                  # Login page
│   ├── signup.html                 # Registration page
│   ├── forgot-password.html        # Password reset
│   ├── navbar.js                   # Shared navigation component
│   ├── api.js                      # Shared API utility
│   ├── user-dashboard.html         # User main dashboard
│   ├── insights.html               # Wellness analytics & charts
│   ├── chat.html                   # AI chat companion (Groq)
│   ├── nudges.html                 # Daily wellness nudges
│   ├── peer.html                   # Peer support rooms
│   ├── assessment.html             # Mental health assessment
│   ├── appointments.html           # Book & manage appointments (user)
│   ├── my-reports.html             # Session reports from doctor
│   ├── profile.html                # User profile
│   ├── session-report.html         # Session report view
│   ├── doctor-dashboard.html       # Doctor overview dashboard
│   ├── doctor-appointments.html    # Doctor appointment management
│   ├── patients.html               # Doctor patient insights panel
│   └── admin-dashboard.html        # Admin panel
│
└── backend/
    ├── pom.xml
    └── src/main/java/com/neuronudge/
        ├── NeuroNudgeApplication.java
        ├── config/
        │   ├── AppConfig.java          # UserDetailsService bean
        │   ├── SecurityConfig.java     # Spring Security + JWT + CORS
        │   ├── WebConfig.java          # Web MVC config
        │   └── DataSeeder.java         # Initial data seeding
        ├── controller/
        │   ├── AuthController.java         # Login, signup, OTP
        │   ├── CheckInController.java      # Daily wellness check-ins
        │   ├── BehaviorController.java     # Passive behavior tracking
        │   ├── ChatController.java         # AI chat via Groq
        │   ├── AppointmentController.java  # Full appointment workflow
        │   ├── DoctorController.java       # Doctor-specific endpoints
        │   ├── DoctorsController.java      # Doctor listing
        │   ├── RecommendationController.java # Doctor recommendations
        │   ├── SessionReportController.java  # Post-session reports
        │   ├── NudgeController.java        # Wellness nudges
        │   ├── PeerController.java         # Peer support rooms
        │   ├── AssessmentController.java   # Mental health assessments
        │   ├── NotificationController.java # In-app notifications
        │   ├── ProfileController.java      # User profile management
        │   ├── UserController.java         # User management
        │   └── AdminController.java        # Admin operations
        ├── model/
        │   ├── User.java
        │   ├── DailyLog.java
        │   ├── BehaviorLog.java
        │   ├── Appointment.java
        │   ├── ChatMessage.java
        │   ├── Recommendation.java
        │   ├── SessionReport.java
        │   ├── Nudge.java
        │   ├── NudgeCompletion.java
        │   ├── PeerRoom.java
        │   ├── PeerMessage.java
        │   ├── PeerJoinRequest.java
        │   ├── Assessment.java
        │   └── Notification.java
        ├── repository/
        │   ├── UserRepository.java
        │   ├── DailyLogRepository.java
        │   ├── BehaviorLogRepository.java
        │   ├── AppointmentRepository.java
        │   ├── ChatMessageRepository.java
        │   ├── RecommendationRepository.java
        │   ├── SessionReportRepository.java
        │   ├── NudgeRepository.java
        │   ├── NudgeCompletionRepository.java
        │   ├── PeerRoomRepository.java
        │   ├── PeerMessageRepository.java
        │   ├── PeerJoinRequestRepository.java
        │   ├── AssessmentRepository.java
        │   └── NotificationRepository.java
        ├── service/
        │   ├── GeminiService.java      # Groq AI integration
        │   ├── EmailService.java       # Gmail SMTP emails
        │   ├── ReminderService.java    # Scheduled reminders
        │   └── NotificationService.java # Notification handling
        └── security/
            ├── JwtUtil.java
            └── JwtAuthFilter.java
```

---

## Prerequisites

- Java 17+
- Maven 3.8+
- MongoDB Atlas account (free tier works)
- Groq API key — [console.groq.com](https://console.groq.com)
- Gmail account with App Password enabled
- A modern browser

---

## Setup & Run

### 1. Clone the repository

```bash
git clone https://github.com/mehulprajapati2610/neuronudge.git
cd neuronudge
```

---

### 2. Configure environment variables

Copy the example env file:

```bash
cp .env.example backend/.env
```

Fill in your actual values in `backend/.env`:

```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/neuronudge
JWT_SECRET=your_jwt_secret_here
GROQ_API_KEY=your_groq_api_key_here
GMAIL_USERNAME=your_gmail@gmail.com
GMAIL_PASSWORD=your_16_char_app_password
GOOGLE_CLIENT_ID=your_google_oauth_client_id
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

**Option A — Copy into Spring Boot static folder (recommended):**
```bash
cp -r frontend/* backend/src/main/resources/static/
mvn spring-boot:run
# Visit http://localhost:8080/login.html
```

**Option B — VS Code Live Server or any static file server:**
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

| Method | Endpoint           | Auth | Description        |
|--------|--------------------|------|--------------------|
| POST   | /api/auth/signup   | None | Register new user  |
| POST   | /api/auth/login    | None | Login, returns JWT |

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

| Method | Endpoint             | Auth   | Description               |
|--------|----------------------|--------|---------------------------|
| POST   | /api/checkin         | Bearer | Submit daily wellness log |
| GET    | /api/checkin/history | Bearer | All logs for current user |
| GET    | /api/checkin/week    | Bearer | Last 7 days of logs       |

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

### AI Chat (Groq)

| Method | Endpoint          | Auth   | Description                |
|--------|-------------------|--------|----------------------------|
| POST   | /api/chat         | Bearer | Send message, get AI reply |
| GET    | /api/chat/history | Bearer | Retrieve chat history      |

**POST body:**
```json
{ "message": "I've been feeling very stressed lately", "sessionId": "optional-uuid" }
```

---

### Appointments

| Method | Endpoint                        | Auth   | Role   | Description             |
|--------|---------------------------------|--------|--------|-------------------------|
| POST   | /api/appointments/request       | Bearer | USER   | Request appointment     |
| GET    | /api/appointments/my            | Bearer | USER   | View my appointments    |
| POST   | /api/appointments/cancel        | Bearer | Both   | Cancel with reason      |
| POST   | /api/appointments/accept        | Bearer | DOCTOR | Accept appointment      |
| POST   | /api/appointments/reject        | Bearer | DOCTOR | Reject with reason      |
| GET    | /api/appointments/doctor        | Bearer | DOCTOR | All doctor appointments |
| GET    | /api/appointments/doctor/pending| Bearer | DOCTOR | Pending requests only   |

---

### Session Reports

| Method | Endpoint                  | Auth   | Role   | Description               |
|--------|---------------------------|--------|--------|---------------------------|
| POST   | /api/session-reports       | Bearer | DOCTOR | Submit post-session report |
| GET    | /api/session-reports/my    | Bearer | USER   | View my session reports   |

---

### Doctor Endpoints

| Method | Endpoint                           | Auth   | Role   | Description            |
|--------|------------------------------------|--------|--------|------------------------|
| GET    | /api/doctor/patients               | Bearer | DOCTOR | All patients + burnout |
| GET    | /api/doctor/patients/{id}/insights | Bearer | DOCTOR | Patient wellness data  |
| GET    | /api/doctor/dashboard              | Bearer | DOCTOR | Doctor dashboard stats |
| POST   | /api/recommendations               | Bearer | DOCTOR | Send recommendation    |
| GET    | /api/recommendations/my            | Bearer | USER   | User's recommendations |

---

## MongoDB Collections

| Collection         | Description                          |
|--------------------|--------------------------------------|
| `users`            | User accounts with roles             |
| `daily_logs`       | Daily mood/stress/sleep check-ins    |
| `behavior_logs`    | Browser interaction patterns         |
| `appointments`     | Consultation scheduling              |
| `chat_messages`    | AI chat session history              |
| `recommendations`  | Doctor recommendations to patients   |
| `session_reports`  | Post-consultation clinical reports   |
| `nudges`           | Wellness nudge definitions           |
| `nudge_completions`| User nudge completion tracking       |
| `peer_rooms`       | Peer support group rooms             |
| `peer_messages`    | Peer room messages                   |
| `assessments`      | Mental health assessment results     |
| `notifications`    | In-app notification records          |

---

## Features

### User Side
-  Dashboard with burnout risk, mood, sleep, stress indicators
-  Interactive daily check-in with sliders
-  Mood/stress/sleep/burnout charts (Chart.js)
-  AI chat companion powered by Groq (LLaMA 3.3 70B)
-  Daily wellness nudges with streak tracking
-  Peer support rooms
-  Mental health assessments (PHQ-9, GAD-7)
-  Browse doctors and book appointments
-  Real-time appointment status tracking
-  Post-session clinical reports from doctor
-  Passive browser behavior tracking (typing speed, mouse activity, idle time)
-  Email reminders for appointments and nudges
-  In-app notifications
-  Google OAuth login

### Doctor Side
-  Overview dashboard (today's appointments, pending requests, high-burnout patients)
-  Accept / reject appointment requests with reasons
-  Complete appointment management panel with filters
-  Patient list with risk levels and burnout scores
-  Per-patient wellness charts (mood + stress history)
-  Submit detailed post-session reports (PHQ-9, GAD-7, ICD-10, clinical notes)
-  Send personalized recommendations to patients

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

## Security Notes

- JWT tokens expire after 24 hours
- Passwords are hashed with BCrypt (strength 10)
- Role-based access: `ROLE_USER` and `ROLE_DOCTOR`
- All sensitive config loaded via environment variables — no hardcoded credentials
- CORS is open in dev mode — restrict in production

---

## License

MIT — Free to use and modify.