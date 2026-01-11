# Event Booking System

A scalable backend API for managing event registrations and ticket bookings, featuring role-based access control and asynchronous background processing.

## 📺 Demo Video
**[REPLACE THIS: Link to your Loom Video here]**
*Note: The demo includes a walkthrough of the code, a live API demonstration using Postman, and real-time logs of the background tasks.*

---

## 🛠 Tech Stack
- **Runtime:** Node.js (Express.js)
- **Database:** PostgreSQL
- **ORM:** Prisma
- **Task Queue:** BullMQ (powered by Redis)
- **Security:** JWT (JSON Web Tokens) & Bcrypt (Password Hashing)
- **Tooling:** Postman (API Testing), Nodemon (Development)

---

## 🚀 Key Features
- **Dual User Roles:**
    - **Organizers:** Can create and update event details.
    - **Customers:** Can browse events and book tickets.
- **Role-Based Access Control (RBAC):** Custom middleware ensures that customers cannot modify events and organizers cannot book tickets.
- **Asynchronous Task Processing:** Decoupled architecture using Redis to handle heavy notification logic without slowing down the API response.

---

## 🏗 Design Decisions & Architecture

### 1. Asynchronous Background Jobs
I implemented **BullMQ with Redis** to handle two critical background tasks:
- **Booking Confirmation:** When a user books a ticket, the API returns a success message immediately. The "email" is handled by a background worker to ensure low latency for the user.
- **Event Update Notifications:** When an organizer updates an event, the system identifies all attendees and queues up individual notifications. Using a queue prevents the API from timing out if an event has a large number of bookings.

### 2. Concurrency & Data Integrity
To prevent **overbooking**, I utilized database-level logic. Before a booking is saved, the system checks if `availableTickets > 0`. 
*Decision:* I chose a relational database (PostgreSQL) to handle ticket counts to ensure strict ACID compliance during high-traffic booking periods.

### 3. Middleware-Based Authorization
Security is handled via a central `authorize` middleware. This decodes the JWT and verifies the `role` property.
*Decision:* This approach keeps the controller logic clean and ensures that security rules are enforced consistently across all endpoints.

---

## ⚙️ Setup & Installation

### Prerequisites
- Node.js (v16+)
- PostgreSQL
- Redis Server (Running on port 6379)

### Installation
1. **Clone the repository:**
   ```bash
   git clone <your-repo-url>
   cd event-booking-system
