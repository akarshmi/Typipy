# 📝 Blogify Backend API

A **RESTful Blog Application Backend** built using **Spring Boot 3.x** and **PostgreSQL (Supabase)**.  
It provides secure user authentication, authorization (JWT), and complete CRUD operations for posts and comments — designed for seamless integration with a **Next.js frontend**.

---

## 🚀 Tech Stack

| Layer | Technology |
|--------|-------------|
| Language | Java 17+ |
| Framework | Spring Boot 3.x |
| Security | Spring Security + JWT |
| Database | PostgreSQL (Supabase) |
| ORM | Spring Data JPA |
| Validation | Jakarta Validation API |
| Build Tool | Maven |
| Others | Lombok, BCrypt, io.jsonwebtoken |

---

## 📦 Project Structure

src/main/java/com/blog/
│
├── controller/ # REST API controllers
├── service/ # Business logic layer
├── repository/ # JPA repositories
├── entity/ # Database entities (User, Post, Comment)
├── dto/ # Data Transfer Objects
├── security/ # JWT & Spring Security setup
├── exception/ # Custom exceptions and global handler
└── BlogApplication.java



---

## 🧩 Entities & Relationships

### **User**
- `id: Long`
- `username: String (unique)`
- `email: String (unique)`
- `password: String (BCrypt encoded)`
- `role: Enum(USER, ADMIN)`
- `bio: String (optional)`
- `createdAt: Timestamp`

### **Post**
- `postId: Long`
- `title: String`
- `slug: String (unique)`
- `summary: TEXT`
- `content: LONGTEXT`
- `category: String`
- `tags: JSON/String[]`
- `views: int`
- `likes: int`
- `authorId: FK → User.id`
- `createdAt`, `updatedAt: Timestamp`

### **Comment**
- `id: Long`
- `content: TEXT`
- `postId: FK → Post.id`
- `authorId: FK → User.id`
- `createdAt: Timestamp`

---

## 🔐 Authentication & Authorization

- Implemented using **Spring Security** + **JWT**
- **Token Validity:**
  - Access Token → 1 day  
  - Refresh Token → 7 days (optional)
- Passwords are securely hashed using **BCrypt**
- **Public Endpoints:**
  - `/api/auth/**`
  - `GET /api/posts/**`
- **Private Endpoints (JWT required):**
  - `POST /api/posts`
  - `PUT /api/posts/{id}`
  - `DELETE /api/posts/{id}`
  - `POST /api/posts/{id}/comments`
- **Roles:**
  - `USER`: Can create/edit own posts & comments
  - `ADMIN`: Can manage all posts & users

---

## 🧠 API Endpoints

### 🧍 Auth
| Method | Endpoint | Description |
|--------|-----------|-------------|
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Login and return JWT |

### 📰 Posts
| Method | Endpoint | Description |
|--------|-----------|-------------|
| GET | `/api/posts` | List all posts (public) |
| GET | `/api/posts/{slug}` | Get post by slug (public) |
| POST | `/api/posts` | Create new post (auth required) |
| PUT | `/api/posts/{id}` | Update own post |
| DELETE | `/api/posts/{id}` | Delete post (author/admin) |

### 💬 Comments
| Method | Endpoint | Description |
|--------|-----------|-------------|
| GET | `/api/posts/{id}/comments` | List comments (public) |
| POST | `/api/posts/{id}/comments` | Add comment (auth required) |
| DELETE | `/api/comments/{id}` | Delete comment (author/admin) |

---

## ⚙️ Configuration

### `application.properties`
```properties
spring.datasource.url=jdbc:postgresql://<supabase-host>:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=<your-password>

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# JWT Secret Key
app.jwt.secret=${JWT_SECRET}

# Allowed origins
cors.allowed-origins=http://localhost:3000,https://your-frontend.vercel.app
```
🧾 JWT Claims
Claim	Description
sub	User ID
username	User's username
role	USER / ADMIN
🧰 Error Handling

Handled globally with @ControllerAdvice returning structured JSON:
```
{
  "status": 400,
  "message": "Invalid credentials"
}
```
🧑‍💻 Setup & Run Locally
1️⃣ Clone the repository
git clone https://github.com/yourusername/blogify-backend.git
cd blogify-backend

2️⃣ Configure environment

Create .env file or set system environment variable:
```
JWT_SECRET=your_super_secret_key
```
3️⃣ Update database credentials

Update your Supabase credentials inside application.properties.

4️⃣ Build and run
```
./mvnw clean install
./mvnw spring-boot:run
```
5️⃣ Access
```
API Base URL: http://localhost:8080/api

Frontend (Next.js): http://localhost:3000
```
🌐 Deployment
Component	Platform
Backend	Fly.io / Render
Database	Supabase (PostgreSQL)
Frontend	Vercel/Netlify

Make sure to set the following environment variables:
```
JWT_SECRET=<your-secret>
DATABASE_URL=<your-supabase-url>
DATABASE_USERNAME=postgres
DATABASE_PASSWORD=<your-password>
```
🧩 Example Request (Login)
POST /api/auth/login
Content-Type: application/json
```
{
  "username": "akarsh",
  "password": "password123"
}

```
✅ Response
```
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5..."
}
```
📘 License

This project is licensed under the MIT License.

💡 Author

Akarsh Mishra
Backend Developer • Passionate about building clean, scalable backend systems.
📧 Contact: [your-email@example.com
]
🔗 GitHub: github.com/yourusername
