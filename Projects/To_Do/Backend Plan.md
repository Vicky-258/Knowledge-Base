
# 🧱 To-Do App Backend Spec — *Rouge Coders Edition* 🦾

---

## 🔥 1. Project Overview

A backend API service for a **To-Do App**, built using **Django** and **Django REST Framework** (DRF).  
Authentication is powered by **JWT**.  
The backend is designed to be consumed by a decoupled **Next.js frontend**.

---

## 📦 2. Modules

| Module  | Purpose                                    |
|---------|--------------------------------------------|
| `users` | User registration, login, JWT management   |
| `todos` | CRUD operations on to-do items             |
| `core`  | Utilities, shared configs (optional)       |

---

## 🧠 3. Data Models

### ✅ `User`
> Inherits from Django’s `AbstractUser`

- `id` (Auto)
- `username` (String)
- `email` (Email)
- `password` (Hashed)
- Custom fields (optional, like `is_verified`)

### 📝 `ToDo`
- `id` (Auto)
- `user` → ForeignKey to `User`
- `title` (CharField)
- `description` (TextField, optional)
- `is_done` (BooleanField)
- `created_at` (DateTime)
- `updated_at` (DateTime)
- `due_date` (optional)
- `priority` or `tag`

---

## 🔐 4. Auth Strategy

- JWT (via `djangorestframework-simplejwt`)
- Refresh + Access tokens
- Blacklist support (optional)
- Registration & Login APIs

---

## 🌐 5. Endpoints Design

### 🔒 Auth Routes
| Method | Route              | Purpose            |
|--------|--------------------|--------------------|
| POST   | `/api/register/`   | Register new user  |
| POST   | `/api/token/`      | Login, get tokens  |
| POST   | `/api/token/refresh/` | Refresh access   |

### 📋 To-Do Routes (Auth Protected)
| Method | Route               | Action           |
|--------|---------------------|------------------|
| GET    | `/api/todos/`       | List user todos  |
| POST   | `/api/todos/`       | Create todo      |
| PUT    | `/api/todos/<id>/`  | Update todo      |
| DELETE | `/api/todos/<id>/`  | Delete todo      |

Optional filters like:
- `/api/todos/?is_done=true`
- `/api/todos/?due_before=2025-04-30`

---

## 🧪 6. Testing Plan

- ✅ Unit tests for each model/view/serializer
- ✅ Token generation/expiration tests
- ✅ CRUD flow tests for `todos`
- ✅ Permission tests (auth-only endpoints)

---

## 🗃️ 7. Project Structure (Proposed)

```bash
backend/
├── manage.py
├── todo_project/
│   ├── settings.py
│   ├── urls.py
│   └── __init__.py
├── users/
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   ├── urls.py
│   └── tests.py
├── todos/
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   ├── urls.py
│   └── tests.py
```

---

## 💡 Optional Ideas (If Time Permits)

- Labels/Tags for todos
- Calendar or timeline views
- Collaboration (shared todos)
- Reminder notifications
