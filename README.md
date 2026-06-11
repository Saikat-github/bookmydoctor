# 🩺 BookMyDoctor — Real-Time Doctor Appointment Platform

A three-sided MERN platform (patients, doctors, admin) with a **live serial-number queue system** powered by Socket.io. Patients can see in real time which token is currently being served — no more guessing when to arrive.

**Live:** https://bookmydoctor-userpanel.vercel.app/

---
## Code Architecture

- backend → https://github.com/Saikat-github/bookmydoctor-backend
- admin panel → https://github.com/Saikat-github/bookmydoctor-adminpanel
- user panel → https://github.com/Saikat-github/bookmydoctor-userpanel

---
## Features

### Patient Side
- **Book without registration** — just fill name, phone, reason
- **Date picker** — frontend auto-calculates available dates for the next 14 days based on each doctor's weekly schedule; no manual slot management on the doctor's side
- **reCAPTCHA** — booking protected against bots (Google reCAPTCHA v2)
- **Serial number** — each patient gets a unique serial number per doctor per date; generated using MongoDB atomic `$inc` to prevent race conditions
- **Live queue** — real-time Socket.io updates showing which serial is currently being checked by the doctor

### Doctor Panel
- **Account creation** — email OTP verification or Google login
- **Onboarding form** — personal, academic, professional, clinic info
- **Admin verification** — doctor goes live only after admin approval
- **Subscription** — free trial period → monthly/yearly plan via Razorpay (webhook verified)
- **Custom QR code** — unique QR linking directly to the doctor's booking page; doctors can print and display in their chamber
- **Patient stats** — total patients, gender filter, date filter, attendance tracking
- **Block patients** — flag and block repeat no-shows

### Admin Panel
- Manage all doctors and patients (update, delete, verify)
- View platform-wide booking trends, patient demographics, geographic demand
- Identify popular doctors by area for strategic decisions

---

## Tech Stack

| Layer | Tech |
|---|---|
| Frontend | React, TailwindCSS, React Hook Form, Context API |
| Backend | Node.js, Express.js, Passport.js |
| Database | MongoDB, Mongoose (aggregation, atomic ops, migration) |
| Real-time | Socket.io, WebSocket |
| Auth | Email OTP, Google OAuth, RBAC |
| Payments | Razorpay (webhook HMAC verification) |
| File Storage | Cloudinary, Multer |
| Email | Resend |
| Jobs | node-cron |
| Security | express-rate-limit, express-validator, Google reCAPTCHA |
| Logging | Winston |


---

## Key Implementation Highlights

- **Atomic serial number** — `findOneAndUpdate` with `$inc` ensures no two patients get the same serial even under concurrent bookings
- **Real-time queue** — Socket.io rooms scoped per `doctorId + date`; doctor marks "next patient" → all connected clients in that room receive the updated serial instantly
- **14-day availability** — doctor stores `availableDays: ['Monday', 'Wednesday']`; frontend maps this to actual dates for the next 14 days client-side
- **Subscription gating** — cron job checks doctor subscription expiry daily; expired doctors automatically paused from receiving new bookings

---

## Screenshots
<img width="1342" height="590" alt="bookmydoctor-pic-1" src="https://github.com/user-attachments/assets/723cc185-a1a7-4f7f-a351-3150ea3bdcb4" />
<img width="1341" height="594" alt="bookmydoctor-pic-3" src="https://github.com/user-attachments/assets/f05446f3-5e58-4057-8961-867fb8e32963" />
<img width="1346" height="595" alt="bookmydoctor-pic-2" src="https://github.com/user-attachments/assets/d895e8a7-5eb7-49b7-92d4-bd9f1ea6ca82" />

