
# 🌐 API Design – Backend (FastAPI)

The CheckIN API is built with FastAPI following RESTful principles. It handles authentication, users, events, check-ins, venues, friendships, messages, notifications, and discovery.

---

## 🔐 Authentication

### `POST /register`
- Register user without password (`id`, `email`, `name`)

### `POST /login`
- Issue JWT using `email`

---

## 👤 Users

### `GET /users/me`
- Returns the authenticated user

### `PUT /users/me`
- Update profile

### `GET /users?search=...`
- Search by name or email

---

## 🗺️ Venues

### `GET /venues`
- List venues (filters: `search`, `sort_by`, `sort_order`)

### `GET /venues/{venue_id}`
- Venue details

### `POST /venues`
- Create venue

### `PATCH /venues/{venue_id}` / `DELETE /venues/{venue_id}`
- Update or delete a venue

---

## 📍 Check-ins

### `POST /checkins`
- Create check-in (`user_id`, `venue_id`, `rating?`, `review?`, ...)

### `GET /checkins/{checkin_id}` / `PATCH /checkins/{checkin_id}` / `DELETE /checkins/{checkin_id}`
- Get, update, or delete a check-in (owner only)

### `GET /users/{user_id}/checkins` / `GET /venues/{venue_id}/checkins`
- List check-ins by user or venue

---

## 📅 Events

### `POST /events` / `GET /events`
- Create and list events (filters: `group_id`, `venue_id`)

### `GET /events/{event_id}` / `PATCH /events/{event_id}`
- Get and update event (creator only)

### RSVP
- `POST /events/{event_id}/attendees`
- `GET /events/{event_id}/attendees`
- `PATCH /events/{event_id}/attendees/{user_id}`
- `DELETE /events/{event_id}/attendees/{user_id}`

---

## 🧠 Discovery

### `GET /venues/search`
- Venue search by category, tags, rating, price range

### `GET /events/search`
- Event search by group/venue/date

---

## 🤝 Friendships

### `POST /friendships/requests`
- Send friend request

### `GET /friendships/requests/incoming` / `GET /friendships/requests/outgoing`
- List incoming/outgoing requests

### `POST /friendships/{friendship_id}/accept` / `POST /friendships/{friendship_id}/reject`
- Accept or reject

### `GET /users/{user_id}/friends`
- List friends

---

## 🏷️ Interests, Badges, Messages

- Interests: CRUD `(/interests)` and user/group links
- Badges: CRUD `(/badges)` and user links
- Messages: threads, conversations, mark-as-read

---

## ⚙️ Technical Notes

- Protected routes require `Authorization: Bearer <token>`
- Interactive docs at `/docs` (Swagger) and `/redoc`
- Structure: `main.py` (routes), `models.py` (ORM), `schemas.py` (Pydantic), `crud.py` (services)

