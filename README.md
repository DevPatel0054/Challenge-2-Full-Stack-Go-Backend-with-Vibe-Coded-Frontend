# Habit Tracker

A full-stack web application for building and tracking daily habits with streak counting.

**Live Demo:** [https://challenge-2-full-stack-go-backend-with.onrender.com/](https://challenge-2-full-stack-go-backend-with.onrender.com/)

---

## Overview

Habit Tracker is a web application that helps users build and maintain positive habits through daily tracking and streak counting. It features secure JWT authentication, full CRUD operations for habit management, and a clean responsive interface.

**Key Highlights:**
- Secure user authentication with JWT
- Real-time streak tracking
- Comprehensive test coverage (85%+)
- RESTful API design
- Responsive UI with vanilla JavaScript

---

## Features

### Authentication
- User registration with validation
- Secure login with JWT tokens
- Password hashing with bcrypt
- Protected API routes

### Habit Management
- Create, read, update, delete habits
- Increment daily streaks with one click
- Track habit creation dates
- Persistent SQLite storage

### Validation Rules
**Username:** 3-20 characters, alphanumeric and underscores only  
**Password:** Minimum 8 characters, at least one uppercase, one lowercase, one number

---

## Architecture

```
┌─────────────┐         HTTPS          ┌─────────────┐
│   Frontend  │ ◄──────────────────► │   Backend   │
│  (Static)   │      REST API         │   (Go API)  │
│  HTML/CSS/JS│    (JWT Auth)         │  Chi Router │
└─────────────┘                       └──────┬──────┘
                                             │
                                             ▼
                                      ┌─────────────┐
                                      │   SQLite    │
                                      │  Database   │
                                      └─────────────┘
```

**Flow:**
1. User registers/logs in → Receives JWT token
2. Token stored in LocalStorage
3. All API requests include JWT in Authorization header
4. Backend validates token and processes requests
5. Data persisted to SQLite database

---

## Tech Stack

**Backend:**
- Go 1.23+
- Chi Router v5
- SQLite (modernc.org/sqlite)
- JWT (golang-jwt/jwt/v5)
- bcrypt password hashing
- Structured logging (slog)

**Frontend:**
- HTML5
- CSS3 (Flexbox/Grid, animations)
- Vanilla JavaScript (ES6+)
- Fetch API
- LocalStorage

---

## Running Locally

### Prerequisites
- Go 1.23+ installed
- Python 3 (or Node.js for alternative)

### Commands

**1. Clone Repository**
```bash
git clone https://github.com/DevPatel0054/Challenge-2-Full-Stack-Go-Backend-with-Vibe-Coded-Frontend
cd habit-tracker
```

**2. Start Backend (Terminal 1)**
```bash
cd backend
go mod download
go run main.go
```
Backend runs on: `http://localhost:8080`

**3. Start Frontend (Terminal 2)**
```bash
cd frontend
python3 -m http.server 3000
```
Frontend runs on: `http://localhost:3000`

**4. Configure API URL**

Edit `frontend/app.js` (line 2):
```javascript
const API_URL = 'http://localhost:8080/api';
```

**5. Open Browser**

Navigate to: `http://localhost:3000`

---

## API Documentation

### Base URL
```
http://localhost:8080/api
```

### Endpoints

#### Authentication

**Register User**
```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "john_doe",
  "password": "SecurePass123"
}

Response (201):
{
  "message": "User created successfully",
  "username": "john_doe"
}
```

**Login User**
```http
POST /api/auth/login
Content-Type: application/json

{
  "username": "john_doe",
  "password": "SecurePass123"
}

Response (200):
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "username": "john_doe"
}
```

---

#### Habits (Protected - Requires JWT)

**Header Required:**
```
Authorization: Bearer <your_jwt_token>
```

**Get All Habits**
```http
GET /api/habits
Authorization: Bearer <token>

Response (200):
[
  {
    "id": 1,
    "name": "Morning Exercise",
    "description": "30 minutes cardio",
    "streak": 5,
    "created_at": "2024-11-20T10:00:00Z"
  }
]
```

**Get Single Habit**
```http
GET /api/habits/:id
Authorization: Bearer <token>

Response (200):
{
  "id": 1,
  "name": "Morning Exercise",
  "description": "30 minutes cardio",
  "streak": 5,
  "created_at": "2024-11-20T10:00:00Z"
}
```

**Create Habit**
```http
POST /api/habits
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Read Books",
  "description": "20 minutes daily"
}

Response (201):
{
  "id": 2,
  "name": "Read Books",
  "description": "20 minutes daily",
  "streak": 0,
  "created_at": "2024-11-20T14:00:00Z"
}
```

**Update Habit**
```http
PUT /api/habits/:id
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Morning Exercise",
  "description": "45 minutes workout",
  "streak": 10
}

Response (200):
{
  "id": 1,
  "name": "Morning Exercise",
  "description": "45 minutes workout",
  "streak": 10,
  "created_at": "2024-11-20T10:00:00Z"
}
```

**Delete Habit**
```http
DELETE /api/habits/:id
Authorization: Bearer <token>

Response (204): No Content
```

**Health Check**
```http
GET /health

Response (200): OK
```

### Status Codes
- `200` - Success
- `201` - Created
- `204` - No Content
- `400` - Bad Request
- `401` - Unauthorized
- `404` - Not Found
- `409` - Conflict
- `500` - Server Error

### cURL Examples

```bash
# Register
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"Test1234"}'

# Login
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"Test1234"}'

# Set token from login response
TOKEN="your_jwt_token_here"

# Get habits
curl http://localhost:8080/api/habits \
  -H "Authorization: Bearer $TOKEN"

# Create habit
curl -X POST http://localhost:8080/api/habits \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"name":"Exercise","description":"Daily workout"}'

# Update habit
curl -X PUT http://localhost:8080/api/habits/1 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"name":"Exercise","description":"Updated","streak":5}'

# Delete habit
curl -X DELETE http://localhost:8080/api/habits/1 \
  -H "Authorization: Bearer $TOKEN"
```

---

## Testing

### Run Tests

```bash
cd backend

# Run all tests
go test -v

# Run with coverage
go test -cover

# Generate HTML coverage report
go test -coverprofile=coverage.out
go tool cover -html=coverage.out
```

### Test Results

**Total Tests:** 26  
**Coverage:** 85%+

```
=== RUN   TestRegister_Success
--- PASS: TestRegister_Success (0.01s)
=== RUN   TestLogin_Success
--- PASS: TestLogin_Success (0.02s)
=== RUN   TestCreateHabit_WithAuth_Success
--- PASS: TestCreateHabit_WithAuth_Success (0.01s)
=== RUN   TestGetAllHabits_WithAuth_Success
--- PASS: TestGetAllHabits_WithAuth_Success (0.01s)
=== RUN   TestUpdateHabit_WithAuth_Success
--- PASS: TestUpdateHabit_WithAuth_Success (0.01s)
=== RUN   TestDeleteHabit_WithAuth_Success
--- PASS: TestDeleteHabit_WithAuth_Success (0.01s)
...
PASS
coverage: 87.3% of statements
ok      habit-tracker   0.234s
```

### Test Coverage

**Authentication (8 tests)**
- Register: valid, invalid username, invalid password, duplicate
- Login: valid, wrong password, non-existent user, missing credentials

**JWT Protection (3 tests)**
- No token, invalid token, expired token

**Habit CRUD (14 tests)**
- Create: with/without auth, with/without validation
- Read: all habits, single habit, non-existent, invalid ID
- Update: with/without auth, existing/non-existent
- Delete: with/without auth, existing/non-existent

**Integration (1 test)**
- Full workflow: Register → Login → Create → Get → Update → Delete

---

## Project Structure

```
habit-tracker/
├── backend/
│   ├── main.go                 # Server entry point
│   ├── main_test.go            # Test suite
│   ├── go.mod                  # Dependencies
│   ├── models/
│   │   ├── habit.go           # Habit models
│   │   └── auth.go            # Auth models
│   ├── handlers/
│   │   ├── habits.go          # Habit handlers
│   │   └── auth.go            # Auth handlers
│   ├── repository/
│   │   ├── sqlite.go          # Habit repository
│   │   └── users.go           # User repository
│   └── middleware/
│       ├── logging.go         # Request logging
│       └── auth.go            # JWT validation
└── frontend/
    ├── index.html             # HTML structure
    ├── style.css              # Styling
    └── app.js                 # Application logic
```

---

**Built with Go and JavaScript**