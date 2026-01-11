# Event Booking System

An asynchronous backend API for managing event registrations and ticket bookings.

## ⚙️ Tech Stack
- **Backend:** Node.js, Express.js
- **Database:** PostgreSQL
- **ORM:** Prisma
- **Task Queue:** BullMQ with Redis
- **Authentication:** JWT (JSON Web Tokens)

---

## 🚀 Key Features & Background Tasks

### 1. Role-Based Access Control (RBAC)
- **Organizers:** Can create, update, and manage events.
- **Customers:** Can browse events and book tickets.
- **Design Decision:** Middleware validates the JWT "role" claim to ensure strict boundary separation between user types.

### 2. Async Booking Confirmations (Task 1)
- When a customer books a ticket, the API offloads the confirmation logic to a background worker.
- **Design Decision:** This ensures the user receives a "Success" response immediately without waiting for the email server simulation to complete.

### 3. Event Update Notifications (Task 2)
- When an Organizer updates event details, all registered attendees are added to a notification queue.
- **Design Decision:** Using a queue prevents the API from timing out when an event has thousands of attendees to notify.

---

## 🛠️ Design Decisions (For the Assignment)

- **Concurrency Control:** To prevent overbooking, I implemented **database transactions** using Prisma. This ensures that the system checks ticket availability and decrements the count in a single atomic operation, preventing `available_tickets` from ever dropping below zero.
- **Decoupling:** Background tasks are handled by a separate Worker process. This follows a microservices-ready architecture where the API and the Worker can scale independently.
- **Error Handling:** The system uses global error-handling middleware to ensure consistent JSON responses for both 4xx and 5xx errors, improving the developer experience for frontend integration.

---

## 📸 Demo
[Link to your Loom Video here]

---

## 🛠 Setup Instructions
1. **Clone the repo:** `git clone <your-repo-url>`
2. **Install dependencies:** `npm install`
3. **Database Migration:** `npx prisma migrate dev`
4. **Start the API:** `npm run dev`
5. **Start the Worker:** `node src/workers/notificationWorker.js`
