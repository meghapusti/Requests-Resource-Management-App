<div align="center">

<img src="https://img.shields.io/badge/SUTD-50.003-005EB8?style=for-the-badge&logoColor=white" />
<img src="https://img.shields.io/badge/Dell_Technologies-Collaboration-007DB8?style=for-the-badge&logo=dell&logoColor=white" />
<img src="https://img.shields.io/badge/Status-Complete-28a745?style=for-the-badge" />

# 🛠️ Presales Workshop Manager
### Dell Technologies × SUTD — Resource & Workshop Management Web Application

*A full-stack enterprise web application built in collaboration with Dell Technologies Singapore, designed to streamline the end-to-end lifecycle of presales workshops — from client requests to trainer assignment and leave management.*

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
- [API Documentation](#-api-documentation)
- [Testing](#-testing)
- [Project Structure](#-project-structure)
- [Team](#-team)
- [Acknowledgements](#-acknowledgements)

---

## 🔭 Overview

The **Presales Workshop Manager** is a multi-role enterprise web application developed as part of the **50.003 Elements of Software Construction** module at SUTD, in direct collaboration with **Dell Technologies Singapore**.

The platform addresses a real operational challenge: coordinating and managing the lifecycle of presales technical workshops — tracking requests from enterprise clients, assigning qualified trainers, managing availability through a leave request system, and giving administrators full visibility through a live dashboard.

The system supports **three distinct user roles** — Admin, Trainer, and Client — each with their own dedicated interface, permissions, and workflows.

---

## ✨ Features

### 👤 Admin
- Full dashboard with analytics and an interactive calendar view of all workshops
- Create, review, approve, or reject workshop requests submitted by clients
- Manage trainer accounts and assign trainers to workshops
- View and action trainer leave requests
- Real-time visibility over workshop status and resource availability

### 🧑‍🏫 Trainer
- Personalised dashboard showing assigned upcoming workshops
- Submit and manage leave requests
- View full workshop details and scheduling information

### 🏢 Client
- Submit new workshop requests with detailed configuration (location, type, dates, participant count, deal size)
- Track the status of submitted requests (pending / approved / rejected)
- View full workshop history

---

## 🏗️ Architecture

The application follows a classic **client-server architecture** using the **MERN stack**, containerised with Docker for consistent local deployment.

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                         │
│              React.js SPA  (localhost:3001)                 │
│    Admin UI  │  Trainer UI  │  Client UI  │  Auth / Login   │
└──────────────────────────┬──────────────────────────────────┘
                           │ HTTP / REST (Axios)
┌──────────────────────────▼──────────────────────────────────┐
│                      API LAYER (Express.js)                 │
│                     Node.js  (localhost:3000)               │
│  /admin  │  /trainer  │  /client  │  /workshop  │  /leave   │
│              JWT Auth Middleware  │  Role-Based Access       │
└──────────────────────────┬──────────────────────────────────┘
                           │ Mongoose ODM
┌──────────────────────────▼──────────────────────────────────┐
│                     DATA LAYER (Docker)                     │
│   MongoDB  :27017  │  Mongo Express  :8081  │  Swagger :8080 │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧰 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React.js 18, React Router v6, Ant Design 5, Chart.js, React Big Calendar |
| **Backend** | Node.js, Express.js 4 |
| **Database** | MongoDB (via Mongoose ODM) |
| **Authentication** | JSON Web Tokens (JWT), bcrypt password hashing |
| **Containerisation** | Docker, Docker Compose |
| **API Documentation** | Swagger UI / OpenAPI 3.0 |
| **Testing** | Jest, Supertest, fast-check (property-based testing), React Testing Library |
| **HTTP Client** | Axios |
| **Date Handling** | Moment.js |

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/en) (v18+)
- [npm](https://www.npmjs.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Git](https://git-scm.com/)

### Installation & Setup

**1. Clone the repository**
```bash
git clone https://github.com/vancenceho/presales-workshop-manager.git
cd presales-workshop-manager
```

**2. Start the backend services (MongoDB + Swagger + Mongo Express)**
```bash
cd backend-service
docker-compose up
```

Verify Docker containers are running in Docker Desktop, then:
```bash
npm install
npm start
```

| URL | Service |
|---|---|
| `localhost:3000` | Express.js API Server |
| `localhost:8080` | Swagger API Documentation |
| `localhost:8081` | Mongo Express (Database UI) |

**3. Start the frontend**

Open a new terminal:
```bash
cd frontend-service/react-res-management-app
npm install
npm start
```

The React application will be available at `localhost:3001`.

### Environment Variables

Create a `.env` file in `frontend-service/react-res-management-app/` if you need to override the default API URL:

```env
REACT_APP_API_URL=http://localhost:3000
PORT=3001
```

---

## 📖 API Documentation

Full interactive API documentation is available via **Swagger UI** at `localhost:8080` when the Docker services are running.

The API follows **RESTful** conventions and is documented against the **OpenAPI 3.0** specification. All protected routes require a valid **JWT Bearer token**, obtained from the login endpoints.

### Key Endpoints

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `POST` | `/admin/login` | Admin login → returns JWT | ❌ |
| `POST` | `/trainer/login` | Trainer login → returns JWT | ❌ |
| `POST` | `/client/login` | Client login → returns JWT | ❌ |
| `GET` | `/workshop` | Get all workshop requests | ✅ |
| `POST` | `/workshop/create` | Create a new workshop request | ✅ Client |
| `PATCH` | `/workshop/:id` | Update workshop status | ✅ Admin |
| `DELETE` | `/workshop/:id` | Delete a workshop | ✅ Admin |
| `GET` | `/leaveRequest` | Get all leave requests | ✅ Admin |
| `POST` | `/leaveRequest/create` | Submit a leave request | ✅ Trainer |
| `GET` | `/admin/trainers` | Get all trainers | ✅ Admin |
| `POST` | `/admin/trainer/create` | Create a trainer account | ✅ Admin |

---

## 🧪 Testing

The project implements a **multi-layered testing strategy**:

```bash
# Run all backend tests
cd backend-service
npm test
```

### Test Coverage

| Layer | Type | Tools |
|---|---|---|
| **Unit Tests** | Model validation, individual controller functions | Jest |
| **Integration Tests** | API route + controller + DB interactions | Supertest, Jest |
| **System / E2E Tests** | Full user workflow scenarios (admin, trainer, client) | Supertest |
| **Property-Based Tests** | Boundary conditions and invariants | fast-check |
| **Frontend Tests** | Component rendering and interaction | React Testing Library |

Test files are organised under:
- `backend-service/unittests/` — model-level unit tests
- `backend-service/systemintegrationtests/` — API integration + system tests
- `backend-service/pathpropertytests/` — property-based tests
- `frontend-service/.../components/**/*.test.js` — React component tests

---

## 📁 Project Structure

```
presales-workshop-manager/
├── backend-service/
│   ├── bin/www                    # Server entry point
│   ├── controller/                # Business logic
│   │   ├── adminController.js
│   │   ├── clientController.js
│   │   ├── trainerController.js
│   │   ├── workshopManagement.js
│   │   └── leaveRequestController.js
│   ├── middleware/
│   │   └── auth.js                # JWT authentication & role authorisation
│   ├── models/                    # Mongoose schemas
│   │   ├── admin.js
│   │   ├── client.js
│   │   ├── trainer.js
│   │   ├── workshopRequest.js
│   │   ├── leaveRequest.js
│   │   └── db.js
│   ├── routes/                    # Express route definitions
│   ├── doc/swagger.yaml           # OpenAPI 3.0 spec
│   ├── unittests/
│   ├── systemintegrationtests/
│   ├── pathpropertytests/
│   ├── docker-compose.yml
│   └── app.js
│
└── frontend-service/
    └── react-res-management-app/
        └── src/
            ├── components/
            │   ├── Admin/         # Admin dashboard, calendar, workshop & leave mgmt
            │   ├── Trainer/       # Trainer home, workshop view
            │   └── Client/        # Client home, new request, workshop history
            ├── App.js             # Root component + routing
            └── index.js
```

---

## 👥 Team

| Name | Student ID | Role |
|---|---|---|
| Vancence Ho | 1007239 | Full-Stack Development |
| Megha Pusti | 1007128 | Front-End Development |
| Koo Rou Zhen | 1007038 | Back-End Development |
| Swasti Arya | 1007235 | Front-End Development |
| Hetavi Shah | 1007034 | Front-End Development |
| Shrinidhi Arul Prakasam | 1007007 | Front-End Development |

---

## 🙏 Acknowledgements

This project was developed as part of the [50.003 — Elements of Software Construction](https://istd.sutd.edu.sg/undergraduate/courses/50003-elements-of-software-construction/) module during **Summer 2024** under the **Information Systems Technology & Design (ISTD)** faculty at **SUTD**, in collaboration with **Dell Technologies Singapore**.

> Copyright © 2024 *Dell Technologies Singapore* — All project contents  
> Copyright © 2024 *TEAM 4* | ISTD | SUTD — Documentation and coursework

---

<div align="center">
  <sub>Built with ☕ and late nights by Team 4 @ SUTD</sub>
</div>
