# Restaurant Web App

A fullstack web application for restaurants — allowing **guests** to sign up, browse the menu,  while **admins** manage users, menu items, and more.

---

## Tech Stack

### Backend
- **Node.js** + **Express**
- **PostgreSQL** (via `pg`)
- **JWT** for authentication
- **bcrypt** for password hashing

### Frontend
- **Vue 3**
- **Pinia** (state management)
- **Axios** (API requests)

### Security
- JWT-based authentication
- Role-based access (admin, user)
- Secure password storage using bcrypt

---

## How It Works

### Sign-Up & Login

- Guests use `POST /register` and `POST /login`
- On successful login, they receive a JWT token with their role (`user` or `admin`)
- Token is signed with `JWT_SECRET` and expires in 1 hour

### Protecting Admin Routes

Inside `authMiddleware.js`:

- `authenticateToken`: Verifies the presence and validity of `Authorization: Bearer <token>`
- `authorizeAdmin`: Ensures the user has role `admin`

For example, the `POST /register-admin` route is protected by both middleware.
All CRUD operations on menu items (POST, GET, PUT, DELETE to /menu) are also protected to ensure only admins can manage the restaurant’s offerings.

---

## Frontend Logic

- `frontend/api/index.js`: Axios instance includes the token from sessionStorage automatically
- Request interceptor: adds `Authorization` header
- Response interceptor: logs out on 401/403 and redirects to `/login`

---

## Database Overview

- Users stored with:
  - `id`
  - `email`
  - `hashed password`
  - `role` (default: `user`)
- Secure queries with parameterized SQL to prevent SQL injection
