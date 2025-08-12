
# 🗃️ Entity-Relationship Model (ER) – Backend

The CheckIN data model reflects social interactions and place discovery. Below are the main entities, attributes, and relationships.

---

## 🔑 Main Entities (as implemented)

### 🧑 User
- `id`: UUID (PK)
- `email`: string (unique)
- `name`: string
- `avatar_url?`, `bio?`, `location?`, `birth_date?`, `phone?`
- Privacy: `is_connectable`, `profile_visibility`, `auto_checkin_visibility`, `allow_messages_from`, `review_delay`, `notifications_enabled`

**Relationships:**
- 1:N with `Checkin`, `Event`, `Message`, `Notification`
- N:N via `user_interests` (→ `Interest`) and `user_badges` (→ `Badge`)

---

### 📍 Venue
- `id`: UUID (PK), `name` (unique), `description?`, `category`, `address?`, `latitude?`, `longitude?`, `phone?`, `website?`, `hours?`, `price_range?`, `rating`, `total_reviews`, `image_url?`, `tags[]?`, `is_active`, `created_at`, `updated_at`

**Relationships:**
- 1:N with `Checkin`, `Event`

---

### 📅 Event
- `id`: UUID (PK)
- `title`, `description?`, `venue_id?` → Venue, `group_id?` → Group, `created_by` → User
- `start_time`, `end_time?`, `max_attendees?`, `is_public`, `created_at`, `updated_at`

**Relationships:**
- N:1 with `User`, `Venue`, `Group`

---

### 📍 Checkin
- `id`: UUID (PK)
- `user_id` → User, `venue_id` → Venue
- Fields: `rating?` (1–5), `review?`, `amount_spent?`, `photos[]?`, `is_anonymous`, `created_at`, `updated_at`

---

### 📨 Message
- `id`: UUID (PK)
- `sender_id` → User, `receiver_id` → User, `content`, `is_read`, `created_at`

---

### 🔔 Notification
- `id`: UUID (PK)
- `user_id` → User, `type`, `title`, `message`, `data?`, `is_read`, `created_at`

---

### 🤝 Friendship
- `id`: UUID (PK)
- `user_id` → User, `friend_id` → User, `status` (`pending` | `accepted` | `blocked`), `created_at`, `updated_at`

### 👥 Group
- `id`: UUID (PK), `name` (único), `description?`, `avatar_url?`, `radius_km`, `is_public`, `max_members?`, `created_by` → User, `created_at`, `updated_at`

### 👥 GroupMember (join)
- `id`: UUID (PK), `group_id` → Group, `user_id` → User, `role`, `joined_at`

### 🎯 Interest
- `id`: UUID (PK), `name` (único), `category?`, `icon?`, `created_at`

### 🏅 Badge
- `id`: UUID (PK), `name` (único), `description?`, `icon?`, `category?`, `criteria?`, `created_at`

### 💸 Promotion
- `id`: UUID (PK), `venue_id` → Venue, `title`, `description?`, `discount_percentage?`, `discount_amount?`, `min_purchase?`, `start_date`, `end_date`, `is_active`, `created_at`, `updated_at`

---

## 🧠 Notes

- All tables use UUID as primary key
- Relationships cover: check-ins, events, friendships, messages, notifications, groups, interests, badges, promotions
- Types and key constraints are defined in `app/models.py` (e.g., rating range via `CheckConstraint`)

---

## 📎 Visual Diagram

The full visual diagram can be added at:  
`/04-architecture/backend/er-diagram.png`  
or edited in dbdiagram.io / draw.io

