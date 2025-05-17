# Backend Requirement Specifications – Airbnb Clone

This document outlines the technical and functional requirements for three core backend features of the Airbnb Clone project: **User Authentication**, **Property Management**, and **Booking System**.

---

## 1. User Authentication

### Functional Requirements
- Allow users to register as a guest or host.
- Allow login via email/password and third-party OAuth (Google, Facebook).
- Use JWT (JSON Web Tokens) for session management.
- Users can update profile info and password.

### API Endpoints

#### POST /api/v1/auth/register
- **Input:**
  ```json
  {
    "name": "Aisha Khalifan",
    "email": "aisha@example.com",
    "password": "StrongP@ss123",
    "role": "host"
  }
Validation Rules:

Email must be valid and unique

Password min 8 chars, includes numbers/symbols

Role must be "guest" or "host"

Output:

```json
{
  "token": "JWT_TOKEN",
  "user": {
    "id": "uuid",
    "name": "Aisha Khalifan",
    "role": "host"
  }
}
```
```POST /api/v1/auth/login```
Input:

```json
{
  "email": "aisha@example.com",
  "password": "StrongP@ss123"
}
```
Output: Same as register

2. Property Management
### Functional Requirements
- Hosts can create, update, delete property listings.

- Properties must include title, description, price, availability, location, and amenities.

- Images should be stored via file upload (locally or to a cloud service).

### API Endpoints
POST /api/v1/properties
Input:

```json
{
  "title": "Luxury Villa",
  "description": "A seaside villa with private pool.",
  "price": 250,
  "location": "Mombasa",
  "amenities": ["Wi-Fi", "Pool", "Air Conditioning"],
  "availability": {
    "start_date": "2025-06-01",
    "end_date": "2025-08-31"
  }
}
```
Validation Rules:

Price must be > 0

Start date must be before end date

All required fields must be present

Output:

```json
{
  "id": "uuid",
  "host_id": "user_uuid",
  "message": "Property successfully listed"
}
```
```GET /api/v1/properties?location=Mombasa&guests=2```
Output: Array of property listings with pagination

3. Booking System
### Functional Requirements
Guests can book available properties by selecting dates.

Prevent double booking.

Bookings have statuses: pending, confirmed, cancelled, completed.

Guests and hosts can cancel under defined policies.

### API Endpoints
```POST /api/v1/bookings``
Input:

```json
{
  "property_id": "uuid",
  "check_in": "2025-07-10",
  "check_out": "2025-07-15",
  "guests": 2
}
```
Validation Rules:

Check availability before booking

check_in < check_out

- Property must exist

Output:

```json
{
  "booking_id": "uuid",
  "status": "pending",
  "message": "Booking request submitted"
}
```
PATCH /api/v1/bookings/:id/cancel
Cancels a booking and updates availability

### Performance Criteria
- All endpoints should respond in <500ms under average load.

- API should be RESTful, follow naming conventions, and return appropriate status codes.

- Use Redis or similar caching for high-demand endpoints like property search.

### Security Notes
- All endpoints must be protected with JWT middleware.

- Role-based access:

  - Guests: Can view/book properties

  - Hosts: Can manage their own listings

  - Admins: Can manage all data

