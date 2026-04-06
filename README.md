#  Finance Dashboard Backend

> Scalable finance dashboard backend featuring role-based access control, financial record processing, and aggregated analytics APIs built with Spring Boot, JPA, and Maven.

[![Java](https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=java)](https://www.java.com/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.0.5-brightgreen?style=flat-square&logo=springboot)](https://spring.io/projects/spring-boot)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Neon-blue?style=flat-square&logo=postgresql)](https://neon.tech/)
[![Maven](https://img.shields.io/badge/Maven-Build-red?style=flat-square&logo=apachemaven)](https://maven.apache.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)

---

##  Overview

The **Finance Dashboard Backend** is a RESTful API service designed to power a personal or organizational finance management application. It provides endpoints to manage financial records, users, budgets, and generate aggregated analytics — all backed by a cloud-hosted PostgreSQL database (Neon).

The system supports role-based access control (RBAC), enabling different levels of data visibility and modification rights depending on the authenticated user's role (e.g., `ADMIN`, `USER`).

---


##  API Documenation

Full API documentation with request examples and expected responses is available here:

[View Here](https://documenter.getpostman.com/view/53754325/2sBXiqE8Tm)


---
##  Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 21 |
| Framework | Spring Boot 4.0.5 |
| ORM | Spring Data JPA / Hibernate 7 |
| Database | PostgreSQL (hosted on [Neon](https://neon.tech)) |
| Build Tool | Maven |
| API Testing | Postman |
| Security | Spring Security (RBAC) |
| Connection Pool | HikariCP |

---

##  Project Structure

```
Finance-Dashboard-Backend/
├── .gitattributes
├── .gitignore
├── HELP.md
└── dashboard/
    ├── .mvn/
    └── src/
        ├── main/
        │   ├── java/
        │   │   └── com/finance/dashboard/
        │   │       ├── controller/
        │   │       │   ├── AuthController.java         # Register & login endpoints
        │   │       │   ├── DashboardController.java    # Dashboard summary endpoints
        │   │       │   ├── RecordController.java       # Financial record CRUD
        │   │       │   └── UserController.java         # User management endpoints
        │   │       ├── exception/
        │   │       │   ├── CustomException.java        # Custom exception class
        │   │       │   └── GlobalExceptionHandler.java # @ControllerAdvice handler
        │   │       ├── model/
        │   │       │   ├── FinancialRecord.java        # Financial record entity
        │   │       │   ├── Role.java                   # ADMIN, ANALYST, VIEWER enum
        │   │       │   └── User.java                   # User entity
        │   │       ├── repository/
        │   │       │   ├── RecordRepository.java       # JPA repo for records
        │   │       │   └── UserRepository.java         # JPA repo for users
        │   │       ├── security/
        │   │       │   └── SecurityConfig.java         # Spring Security & RBAC config
        │   │       ├── service/
        │   │       │   ├── DashboardService.java       # Dashboard business logic
        │   │       │   ├── RecordService.java          # Record business logic
        │   │       │   └── UserService.java            # User business logic
        │   │       └── DashboardApplication.java       # Main entry point
        │   └── resources/
        │       └── application.properties              # App configuration
        └── test/                                       # Unit & Integration tests
```

---

## ️ Database

This project uses **PostgreSQL** hosted on [Neon](https://neon.tech) — a serverless, cloud-native Postgres platform.

### Why Neon?
- Serverless & auto-scaling
- Free tier available
- Branching support (production / dev environments)
- Built-in SQL Editor on the dashboard

### Configuration (`application.properties`)

```properties
spring.application.name=dashboard

spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
spring.datasource.driver-class-name=org.postgresql.Driver

spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

> ⚠️ Credentials are stored as **environment variables** — never hardcoded. Set `DB_URL`, `DB_USERNAME`, and `DB_PASSWORD` in your IntelliJ Run Configuration or system environment.

---

##  Getting Started

### Prerequisites
- Java 21+
- Maven 3.8+
- A [Neon](https://neon.tech) account with a project created

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Geetha-R-27/Finance-Dashboard-Backend.git
   cd Finance-Dashboard-Backend/dashboard
   ```

2. **Set environment variables** (IntelliJ → Run → Edit Configurations → Environment Variables)
   ```
   DB_URL=jdbc:postgresql://<your-neon-host>/neondb?sslmode=require&channel_binding=require
   DB_USERNAME=neondb_owner
   DB_PASSWORD=your_neon_password
   ```

3. **Run the application**
   ```bash
   mvn spring-boot:run
   ```

4. **Access the API**
   ```
   http://localhost:8080
   ```

---

##  API Endpoints

> Full Postman documentation: [View Here](https://documenter.getpostman.com/view/53754325/2sBXiqE8Tm)

###  Auth Endpoints

| Method | Endpoint | Description | Role |
|---|---|---|---|
| `POST` | `/api/register` | Register as Admin | ADMIN |
| `POST` | `/api/register` | Register as Analyst | ANALYST |
| `POST` | `/api/register` | Register as Viewer | VIEWER |

###  User Endpoints

| Method | Endpoint | Description | Role |
|---|---|---|---|
| `GET` | `/api/users` | Get all users | ADMIN |
| `GET` | `/api/users/{id}` | Get user by ID | ADMIN |
| `GET` | `/api/users` | Get users | ANALYST |

###  Records Endpoints

| Method | Endpoint | Description | Role |
|---|---|---|---|
| `POST` | `/api/records` | Create a new financial record | ADMIN |
| `GET` | `/api/records` | Get all financial records | ANALYST |
| `GET` | `/api/records?type=expense` | Filter records by type | ANALYST |
| `DELETE` | `/api/records/{id}` | Delete a record (access controlled) | ADMIN only |

**Sample Request Body (POST /api/records):**
```json
{
  "amount": 3157.00,
  "type": "expense",
  "category": "Rent",
  "date": "2022-05-23",
  "notes": "Monthly rent"
}
```

**Sample Response:**
```json
{
  "id": 1,
  "amount": 3157.0,
  "category": "Rent",
  "date": "2022-05-23",
  "notes": "Monthly rent",
  "type": "expense"
}
```

###  Dashboard Endpoints

| Method | Endpoint | Description | Role |
|---|---|---|---|
| `GET` | `/api/dashboard/summary` | Get aggregated financial summary | ANALYST |
| `GET` | `/api/dashboard/summary` | Get dashboard summary | VIEWER (read-only) |

---

##  Role-Based Access Control (RBAC)

| Role | Permissions |
|---|---|
| `ADMIN` | Full access — create, read, update, delete all records |
| `USER` | Read own records, create transactions and budgets |

---

##  Assumptions

- Each transaction belongs to a single user.
- A user can have multiple budgets across different categories.
- Amounts are stored in a single currency (no multi-currency support currently).
- Authentication is handled via Spring Security — JWT integration is assumed for production use.
- `ddl-auto=update` is used for development; production should use `validate` or Flyway migrations.
- All timestamps are stored in UTC.

---

## 🔮 Future Improvements

- [ ] **JWT Authentication** — Replace basic auth with JWT token-based authentication
- [ ] **Multi-currency Support** — Add currency conversion using an exchange rate API
- [ ] **Export to CSV/PDF** — Allow users to export their financial reports
- [ ] **Email Notifications** — Monthly summary emails and budget alerts
- [ ] **Recurring Transactions** — Support for scheduled/recurring entries
- [ ] **Database Migrations** — Replace `ddl-auto=update` with Flyway for production-safe migrations
- [ ] **Pagination & Filtering** — Add query params for paginated and filtered responses
- [ ] **Docker Support** — Containerize the application with Docker & Docker Compose
- [ ] **CI/CD Pipeline** — Set up GitHub Actions for automated testing and deployment
- [ ] **Rate Limiting** — Prevent API abuse with request throttling
- [ ] **Swagger/OpenAPI Docs** — Auto-generate interactive API documentation

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 👨‍💻 Author

**Geetha R**
- GitHub: (https://github.com/Geetha-R-27/)

---

> ⭐ If you found this useful, consider starring the repo!