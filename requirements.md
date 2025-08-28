# Backend Requirement Specifications

This document outlines the technical and functional requirements for the core backend features of the ALX Airbnb Project.

---

## 1. User Authentication

### Description
Enables users to register, log in, and manage their accounts securely.

### Functional Requirements
- Users can create an account with email, password, and personal details.
- Users can log in using valid credentials.
- Passwords must be stored securely (hashed & salted).
- JWT-based authentication will be used for session management.

### API Endpoints
- **POST /api/auth/register**
  - **Input:** `{ "name": "John Doe", "email": "john@example.com", "password": "********" }`
  - **Output:** `{ "message": "Registration successful", "userId": "12345" }`
  - **Validation Rules:**
    - Email must be unique and valid.
    - Password must be at least 8 characters with a mix of letters/numbers.
- **POST /api/auth/login**
  - **Input:** `{ "email": "john@example.com", "password": "********" }`
  - **Output:** `{ "token": "jwt_token", "userId": "12345" }`
  - **Validation Rules:**
    - Credentials must match an existing user.
- **GET /api/auth/profile**
  - **Input:** Header Authorization: Bearer token
  - **Output:** `{ "id": "12345", "name": "John Doe", "email": "john@example.com" }`

### Performance Criteria
- Authentication response time < 300ms.
- Must handle at least 100 concurrent login requests.

---

## 2. Property Management

### Description
Allows property owners to create, update, view, and delete property listings.

### Functional Requirements
- Property owners can add new properties with details (title, description, location, price, images).
- Owners can update or delete their properties.
- Users can view available properties.

### API Endpoints
- **POST /api/properties**
  - **Input:** `{ "title": "Modern Apartment", "description": "2 bedroom flat", "location": "Lagos", "price": 100, "images": ["url1", "url2"] }`
  - **Output:** `{ "message": "Property created successfully", "propertyId": "98765" }`
  - **Validation Rules:**
    - Title is required.
    - Price must be a positive number.
    - Images must be valid URLs.
- **GET /api/properties**
  - **Output:** `[ { "id": "98765", "title": "Modern Apartment", "location": "Lagos", "price": 100 } ]`
- **PUT /api/properties/:id**
  - **Input:** `{ "price": 120 }`
  - **Output:** `{ "message": "Property updated successfully" }`
- **DELETE /api/properties/:id**
  - **Output:** `{ "message": "Property deleted successfully" }`

### Performance Criteria
- Must return property listings within < 500ms.
- System should support up to 10,000 property records.

---

## 3. Booking System

### Description
Manages property bookings, ensuring availability and secure reservations.

### Functional Requirements
- Users can book available properties.
- System prevents double-booking for the same property and dates.
- Users can cancel bookings.

### API Endpoints
- **POST /api/bookings**
  - **Input:** `{ "userId": "12345", "propertyId": "98765", "startDate": "2025-09-01", "endDate": "2025-09-05" }`
  - **Output:** `{ "message": "Booking confirmed", "bookingId": "B123" }`
  - **Validation Rules:**
    - Start date must be before end date.
    - Property must be available.
- **GET /api/bookings/:userId**
  - **Output:** `[ { "bookingId": "B123", "propertyId": "98765", "startDate": "2025-09-01", "endDate": "2025-09-05" } ]`
- **DELETE /api/bookings/:id**
  - **Output:** `{ "message": "Booking canceled successfully" }`

### Performance Criteria
- Booking confirmation must complete within 500ms.
- Handle up to 500 concurrent booking requests.
- Ensure ACID compliance for booking transactions.

---

## General Notes
- All endpoints must follow REST principles.
- Input validation errors must return status `400 Bad Request`.
- Unauthorized access attempts must return `401 Unauthorized`.
- All dates must follow `YYYY-MM-DD` format.
- System must log all API requests and errors.
