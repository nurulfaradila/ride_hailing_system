# 🚀 NexRide: Distributed Ride-Hailing Platform

![Java](https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.2.2-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![React](https://img.shields.io/badge/React-18.2.0-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Vite](https://img.shields.io/badge/Vite-5.1.0-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-7-005571?style=for-the-badge&logo=elasticsearch&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-3-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-24-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-3.9-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)


NexRide is a high-performance, scalable ride-hailing system built on a modern microservices architecture. It simulates the end-to-end lifecycle of a ride-matching platform, from real-time driver location tracking to automated dispatching and trip management.

This project demonstrates expertise in **distributed systems**, **event-driven architecture**, and **full-stack development**, showcasing how complex industrial applications handle high-frequency data and real-time state synchronization.

---

## 🌟 Key Features

*   **Real-time Geospatial Matching**: Leverages **Elasticsearch** for high-speed proximity searches, ensuring riders are matched with the closest available drivers.
*   **Dynamic Location Tracking**: Handles high-frequency driver updates using **Redis** for sub-millisecond latency.
*   **Event-Driven Communication**: Decoupled microservices communicate through **RabbitMQ**, ensuring system resilience and eventual consistency.
*   **Comprehensive Trip Lifecycle**: Robust state management for trips (REQUESTED → MATCHED → IN_PROGRESS → COMPLETED).
*   **Interactive Full-Stack Dashboard**: React-based control center with real-time map integration via **Leaflet.js**.

---

## 🏗️ System Architecture

The system is composed of specialized microservices, each owning a specific domain and scaling independently.

```mermaid
graph TD
    subgraph "Frontend Layer"
        UI[React Dashboard]
    end

    subgraph "Service Layer"
        DS[Dispatch Service]
        RS[Rider Service]
        DRS[Driver Service]
        TS[Trip Service]
    end

    subgraph "Infrastructure Layer"
        RMQ[RabbitMQ]
        ES[(Elasticsearch)]
        RD[(Redis)]
        PG[(PostgreSQL)]
    end

    UI <--> RS & DRS & TS
    RS --> PG
    DRS --> PG
    DRS --> RD
    DRS --> ES
    TS --> PG
    
    DS -- "Events" --> RMQ
    RMQ -- "Notify" --> TS & DRS & RS
```

---

## 🛠️ Technology Stack

| Category | Technologies |
| :--- | :--- |
| **Backend** | Java 17+, Spring Boot 3, Spring Data (JPA, Redis, Elasticsearch) |
| **Frontend** | React, Vite, Leaflet.js, Axios, CSS Modules |
| **Database** | PostgreSQL 15 (Relational), Redis 7 (Cache/Location), Elasticsearch 7 (Geo-Search) |
| **Messaging** | RabbitMQ 3 (Event Bus) |
| **DevOps** | Docker, Docker Compose, Maven |

---

## 📂 Project Structure

```text
.
├── backend/
│   ├── common/             # Shared DTOs and utilities
│   ├── driver-service/     # Driver management & location ingestion
│   ├── rider-service/      # Rider profiles & authentication
│   ├── trip-service/       # Trip state & history management
│   └── dispatch-service/   # Matching engine & orchestration
├── frontend/               # React application (Vite-powered)
├── infra/                  # Docker Compose infrastructure
└── README.md
```

---

## 🚀 Installation Guide

### Prerequisites
*   **Docker & Docker Compose**
*   **Java 17+** & **Maven**
*   **Node.js 18+**

### Step 1: Spin up Infrastructure
```bash
cd infra
docker-compose up -d
```

### Step 2: Build & Start Backend Services
In the `backend` directory, run each service (ideally in separate terminals or as background processes):
```bash
mvn spring-boot:run -pl driver-service
mvn spring-boot:run -pl trip-service
mvn spring-boot:run -pl dispatch-service
mvn spring-boot:run -pl rider-service
```

### Step 3: Launch Frontend
```bash
cd frontend
npm install
npm run dev
```
The application will be available at `http://localhost:3000`.

---

## 📖 API Documentation Example

### Trip Request
`POST /api/trips`
Initiates a new ride request and triggers the dispatch matching engine.

**Request Payload:**
```json
{
  "riderId": "rider-123",
  "pickupLat": 1.2878,
  "pickupLng": 103.8666,
  "dropoffLat": 1.3000,
  "dropoffLng": 103.9000
}
```

**Workflow:**
1.  `Trip Service` persists the request.
2.  `Dispatch Service` queries `Elasticsearch` for nearby online drivers.
3.  `RabbitMQ` broadcasts the match to relevant services and the frontend.

---

## 🔄 System Workflow

```mermaid
sequenceDiagram
    participant R as Rider UI
    participant TS as Trip Service
    participant DS as Dispatch Service
    participant ES as Elasticsearch
    participant RMQ as RabbitMQ
    participant D as Driver UI

    R->>TS: POST /api/trips (Request)
    TS->>DS: Trigger Match Engine
    DS->>ES: Geo-Spatial Query (Radius Search)
    ES-->>DS: Return Available Drivers
    DS->>RMQ: Publish MATCH_FOUND Event
    RMQ-->>D: Notify Driver
    RMQ-->>TS: Update Trip Status
    TS-->>R: Dispatch Confirmed
```

---

## 📸 Screenshots

![NexRide Display](assets/ride-hailing.png)
*Figure: NexRide Rider and Driver Dashboards featuring real-time map integration and trip controls.*

---

## 🛡️ License
Distribute under the MIT License. See `LICENSE` for more information.
