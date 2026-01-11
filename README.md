# Event Booking System

## 📌 Project Overview
A robust Event Booking API supporting Event Organizers and Customers. Built with Node.js, Express, PostgreSQL, and Redis.

## 🛠 Tech Stack
- **Framework:** Express.js
- **Database:** PostgreSQL (Prisma ORM)
- **Queue Management:** BullMQ (Redis)
- **Auth:** JWT (JSON Web Tokens)

## 🏗 Design Decisions (Important)
- **Asynchronous Processing:** I chose **BullMQ** to handle notifications. This prevents the main API thread from blocking while waiting for external simulations (like emails), ensuring a high-performance experience.
- **Role-Based Access Control (RBAC):** Users are assigned roles (ORGANIZER/CUSTOMER). I implemented custom middleware to restrict sensitive actions (like creating events) to Organizers only.
- **Concurrency Strategy:** Used database transactions during ticket booking to ensure that if two people book the last ticket simultaneously, only one succeeds and the `availableTickets` count remains accurate.

## 🚦 How to Run
1. **Clone the repo:** `git clone [your-repo-link]`
2. **Install dependencies:** `npm install`
3. **Setup environment:** Create a `.env` file based on the `.env.example`.
4. **Database Migration:** `npx prisma migrate dev`
5. **Start Redis:** Ensure Redis is running locally.
6. **Start Application:** `npm start`

## 📺 Demo Video
[Link to your Loom Video here]
