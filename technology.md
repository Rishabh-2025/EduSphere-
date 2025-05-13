# 🎓 EduSphere — Unified Educational ERP System (MERN + IoT)

EduSphere is a modular, full-stack Educational ERP platform designed to unify academic, administrative, and attendance operations across **web, mobile, and desktop** platforms. It also includes **IoT integration for biometric/RFID attendance**.

---

## 🧠 Key Technologies

| Layer         | Stack                                                    |
|---------------|----------------------------------------------------------|
| **Frontend**  | React (Web), React Native (Mobile), Electron (Desktop)   |
| **Backend**   | Node.js (Express), MongoDB Atlas, Socket.IO/MQTT         |
| **IoT Layer** | ESP32/NodeMCU with RFID/Biometric + WiFi                 |
| **Auth**      | JWT, Role-Based Access Control (RBAC)                    |
| **DevOps**    | Docker, GitHub Actions, Netlify/Vercel, Expo EAS         |

---

## 🌐 Ecosystem Architecture

   ┌───────────────┐
   │ Electron App  │ ◄──── Desktop Admin/Faculty Panel
   └───────────────┘
          ▲
          │ Shared React UI
          ▼
┌───────────────────────────┐
│     React Web App         │ ◄──── Admin/Faculty/Student/Parent Portal
└───────────────────────────┘
          ▲
          │ Shared REST API
          ▼
┌───────────────────────────┐
│    React Native App       │ ◄──── Students & Parents Mobile App
└───────────────────────────┘
          ▲
          │
          ▼
┌───────────────────────────┐
│   Express + MongoDB       │ ◄──── Central API & Data Layer
└───────────────────────────┘
          ▲
          │ MQTT/WebSocket
          ▼
┌───────────────────────────┐
│     IoT Device (ESP32)    │ ◄──── Biometric/RFID Attendance Device
└───────────────────────────┘



---

## 🧾 IoT Attendance System Flow

### 💡 Devices Used
- `ESP32` or `NodeMCU` Wi-Fi-enabled microcontroller
- `RC522` RFID Scanner or Fingerprint Sensor (R305)
- Uses MQTT or HTTPS (via `WiFiClientSecure`) for data sync

### 🔄 Attendance Flow
1. Student taps RFID/fingerprint.
2. Device sends `student_id + timestamp` to backend via MQTT/HTTPS.
3. Express server logs attendance into MongoDB.
4. WebSocket broadcasts update to Web & Electron dashboards.
5. Parents receive push notification via Firebase (React Native App).

---

## ⚙️ Development Phases

### 🔹 Phase 1: API & Core Backend (Express + MongoDB)
- Setup `auth`, `students`, `attendance`, `notifications` modules.
- Add `/attendance/iot-sync` endpoint.
- Integrate `Socket.IO` for real-time dashboard.
- Setup `MQTT` broker for IoT devices.

### 🔹 Phase 2: Web + Electron Desktop UI (React)
- Build Web UI using React.
- Convert to Electron Desktop app using `electron-builder`.
- Integrate WebSocket dashboard updates.
- Enable printing, offline capabilities in Electron.

### 🔹 Phase 3: Mobile App (React Native)
- Use `Expo` for development and builds.
- Integrate:
  - Attendance logs
  - Push notifications (FCM via Expo)
  - Parent/student views

### 🔹 Phase 4: IoT Firmware
- Develop C++ firmware using `Arduino IDE` or `PlatformIO`.
- Send attendance data securely via:
```json
{
  "device_id": "iot-room1",
  "student_id": "S10234",
  "timestamp": "2025-05-12T08:10:30Z"
}
```
Use PubSubClient for MQTT or HTTPClient for HTTPS

| App             | Platform            | Deployment Method                    |
| --------------- | ------------------- | ------------------------------------ |
| Electron App    | Windows/Linux/macOS | `electron-builder`, GitHub Releases  |
| React Web       | Browser             | Vercel / Netlify / Render            |
| React Native    | Android/iOS         | Expo EAS, Google Play Store          |
| Express Backend | Cloud/VPS/Docker    | Render / Railway / AWS EC2           |
| IoT Devices     | Wi-Fi enabled       | OTA Flash / USB Upload (Arduino IDE) |


#### 🔐 Security Considerations
Use HTTPS/TLS for all device communications.

Authenticate IoT devices using pre-shared tokens.

Use WebSocket with JWT for real-time updates.

Validate device-student association to prevent spoofing.

Use CORS & Helmet in Express backend.

#### 📦 Module Overview
| Module               | Description                                   |
| -------------------- | --------------------------------------------- |
| Student Management   | Admission, ID, Guardians, Profiles            |
| Faculty/HR           | Onboarding, Attendance, Payroll               |
| Academics            | Programs, Batches, Timetables                 |
| Attendance           | IoT-based, Manual, Analytics                  |
| Exams & Grading      | Mark Entry, Results, Transcripts              |
| Fees & Finance       | Fee Structure, Invoices, Razorpay Integration |
| Notifications        | Email, SMS, In-app, FCM                       |
| Library (Optional)   | Books, Lending, Fine, Catalog                 |
| Hostel (Optional)    | Rooms, Visitors, Complaints                   |
| Transport (Optional) | Routes, Vehicles, Driver Logs                 |
| Placements (Planned) | Drives, Applications, Status, Records         |

#### 📁 Repository Structure
/server           → Express.js + MongoDB Backend
/web              → React Web Dashboard
/desktop          → Electron App Shell (wraps web)
/mobile           → React Native App (Expo)
/iot-firmware     → ESP32 Source Code (C++)
.env.example      → Sample environment variables
README.md         → Project documentation

### 📌 Future Enhancements
PWA support for mobile web users

Blockchain-based certificate storage

AI-based timetable generator

Face recognition attendance system

OpenAPI Docs via Swagger
