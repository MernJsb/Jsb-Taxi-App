# Hotel Cab Booking Mobile App – Project Specification (Rough Estimation - Changes to be proposed)

## Overview

A mobile application for hotel clients and cab drivers (operated by hotel-affiliated vendors), allowing:
- Hotel clients to book cabs to/from airports and landmarks.
- Drivers to accept and manage client ride requests.

## Technologies
- React Native with Expo (Frontend)
- REST API (or GraphQL) backend with real-time capabilities (WebSocket or Push Notifications)
- Authentication (JWT or session-based)
- Firebase or third-party service for push notifications
- WebRTC or VOIP for calls (optional, or third-party integration like Twilio)

---

## User Roles

### 1. Client (Hotel Guest)
- Login using hotel-provided credentials (mobile mandatory)
- Book cab based on location and car preference
- Can schedule a ride for later
- View booking status
- Chat/Call assigned driver
- Provide OTP for ride start
- View past ride history

### 2. Driver (Car Vendor)
- Login using hotel-issued credentials
- Receive ride requests
- Accept or reject requests
- Navigate to pickup location
- Enter OTP to begin ride
- Mark ride as completed
- View ride history

---

## Authentication Flow

- Login (Mobile No. + optional Email)
- Role determined via backend
- Token-based session maintained securely

→ Frontend-Backend Communication:
- POST /auth/login
- GET /user/profile

---

## Client Workflow

1. **Login Screen**
   - Input: Mobile number, optional email
   - Submit credentials → receive token and role

2. **Booking Screen**
   - Input current location (auto via GPS)
   - Input destination
   - Select car category
   - Choose car model
   - Book Now

→ Frontend-Backend Communication:
- GET /cars/categories
- GET /cars?category={selected}
- POST /rides/book (send user, location, car type)

3. **Ride Status**
   - Shows driver details if request accepted
   - Real-time updates (WebSocket/push notification)

4. **Chat/Call**
   - Secure messaging and/or calling interface

→ Frontend-Backend Communication:
- WebSocket: /notifications or /rides/{id}/status
- POST /messages
- (Optional) integrate Twilio for call or VOIP

5. **OTP Verification**
   - Client provides OTP to start ride

→ Frontend-Backend Communication:
- POST /rides/{id}/start (with OTP)

6. **History Screen**
   - View previous bookings

→ GET /rides/history

---

## Driver Workflow

1. **Login Screen**
   - Same as client

2. **Home/Dashboard**
   - Tabs: Incoming Requests, Ongoing Ride, History

3. **Incoming Ride Request**
   - Shows client location, car type, category
   - Accept / Reject within time

→ Real-time Communication:
- WebSocket or push notification to receive request
- POST /rides/{id}/accept or /rides/{id}/reject

4. **Ongoing Ride**
   - Navigate to pickup
   - Enter OTP to start ride
   - End ride on arrival

→ Frontend-Backend Communication:
- POST /rides/{id}/start (with OTP)
- POST /rides/{id}/complete

5. **Ride History**
   - See completed rides

→ GET /rides/history

---

## Real-Time Flow

- Client sends booking request
- Server broadcasts to eligible drivers via:
  - WebSocket / Firebase Cloud Messaging
- First driver to accept is assigned
- Others are notified ride was taken

---

## Backend Integration Points (Summary)

| Functionality          | Endpoint                        | Method |
|------------------------|----------------------------------|--------|
| Login                  | /auth/login                     | POST   |
| Fetch Profile          | /user/profile                   | GET    |
| List Car Categories    | /cars/categories                | GET    |
| List Cars              | /cars?category=SUV              | GET    |
| Book Ride              | /rides/book                     | POST   |
| Ride Status (Real-time)| WebSocket /rides/{id}/status    | WS     |
| Accept Ride            | /rides/{id}/accept              | POST   |
| Start Ride             | /rides/{id}/start               | POST   |
| Complete Ride          | /rides/{id}/complete            | POST   |
| Ride History           | /rides/history                  | GET    |
| Chat Messages          | /messages                       | POST   |

---

## Notification System

- Push Notification (Expo Push or Firebase FCM)
- Sent to:
  - Drivers when ride is requested
  - Clients when driver accepts ride

---

## Optional Features

- Estimated Arrival Time
- Rating/Feedback system
- Google Maps SDK for live navigation and directions
- Driver status: Online/Offline toggle

---

## Folder Structure (Frontend) - (To be Decided)

```shell
src/
├── api/              # Axios instances and API service methods
├── auth/             # Auth context and hooks
├── screens/          # Screens: Login, Booking, RideStatus, etc.
├── components/       # Reusable UI components
├── contexts/         # App-wide contexts like UserContext
├── hooks/            # Custom React hooks
├── navigation/       # React Navigation config
├── utils/            # Helper functions (formatting, validation, etc.)
└── assets/           # Images, fonts, etc.
