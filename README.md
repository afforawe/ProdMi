[ProdMi_README.md](https://github.com/user-attachments/files/26547474/ProdMi_README.md)
# ProdMi

ProdMi is a Spring Boot web application for user management with role-based access control. The project combines a server-rendered UI built with Thymeleaf and a REST API for administrative operations. It demonstrates authentication, authorization, CRUD workflows, persistence with JPA/Hibernate, and a layered backend architecture.

## Overview

The application provides two access levels:

- **USER** — can view their own profile
- **ADMIN** — can view all users and perform create, update, and delete operations

The UI is rendered with **Thymeleaf**, while user management actions are handled through **REST endpoints** consumed by JavaScript on the client side.

## Features

- Authentication with **Spring Security**
- Role-based authorization for `USER` and `ADMIN`
- User CRUD operations for administrators
- Personal profile view for authenticated users
- Server-side rendering with Thymeleaf
- REST API for admin and user actions
- Persistence with Spring Data JPA and MySQL
- Automatic initialization of demo roles and users at startup
- Password hashing with `PasswordEncoder`

## Tech Stack

- **Java 11**
- **Spring Boot 2.6.2**
- **Spring MVC**
- **Spring Security**
- **Spring Data JPA / Hibernate**
- **Thymeleaf**
- **MySQL**
- **Bootstrap 4**
- **JavaScript (Fetch API)**
- **Lombok**
- **Maven**

## Architecture

The project follows a classic layered structure:

- **controller** — MVC controllers and REST endpoints
- **service** — business logic
- **repository** — data access layer
- **entity** — JPA entities
- **dto / mapper** — data transfer objects and mapping between layers
- **configs / security** — security configuration, authentication flow, password encoding, startup initialization

### Main modules

- `controller.admin` — admin page and admin REST API
- `controller.user` — user page and profile API
- `security` — custom `UserDetailsService` and security user model
- `configs` — security setup and login success handling
- `configs.init` — bootstrap data for roles and demo users

## Security Model

Access rules are configured as follows:

- `/admin/**` and `/api/admin/**` — accessible only to users with role `ADMIN`
- `/user/**` and `/api/user/**` — accessible to users with role `USER` or `ADMIN`
- all other routes require authentication

The application uses form-based login provided by Spring Security.

## Domain Model

### User

Represents an application user.

Fields:
- `id`
- `email`
- `name`
- `lastName`
- `password`
- `roles`

### Role

Represents a user role.

Fields:
- `id`
- `name`

Relationship:
- `User` ↔ `Role` — `ManyToMany`

## API Endpoints

### Admin API

Base path: `/api/admin/users`

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/admin/users` | Get all users |
| GET | `/api/admin/users/{id}` | Get user by id |
| POST | `/api/admin/users` | Create user |
| PUT | `/api/admin/users` | Update user |
| DELETE | `/api/admin/users/{id}` | Delete user |

### User API

Base path: `/api/user`

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/user/getMyProfile` | Get profile of the authenticated user |

## UI Pages

- `/admin` — admin panel with users table and user creation form
- `/user` — user profile page

The main UI is composed using Thymeleaf templates and fragments. Administrative actions such as create, edit, and delete are performed asynchronously via JavaScript and Fetch API.

## Demo Data

At startup, the application creates the following roles if they do not exist:

- `ROLE_USER`
- `ROLE_ADMIN`

It also seeds several demo users, including an administrator:

| Email | Password | Roles |
|------|----------|-------|
| `ivan@mail.com` | `111111` | `ROLE_USER`, `ROLE_ADMIN` |
| `sergey@mail.com` | `222222` | `ROLE_USER` |
| `artem@mail.com` | `333333` | `ROLE_USER` |
| `vadim@mail.com` | `444444` | `ROLE_USER` |
| `artyr@mail.com` | `555555` | `ROLE_USER` |

> Passwords are encoded before being stored in the database.

## Database Configuration

Current local configuration is defined in `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydbtest
spring.datasource.username=root
spring.datasource.password=root

spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

### Notes

- Make sure the `mydbtest` database exists before starting the application.
- `create-drop` recreates tables on startup and drops them on shutdown, which is convenient for local development but not recommended for production.
- Update datasource properties according to your local environment.

## Getting Started

### Prerequisites

- Java 11+
- Maven 3.8+
- MySQL 8+

### 1. Create database

```sql
CREATE DATABASE mydbtest;
```

### 2. Configure datasource

Open `src/main/resources/application.properties` and update the database connection if needed.

### 3. Run the application

Using Maven wrapper:

```bash
./mvnw spring-boot:run
```

Or with Maven installed locally:

```bash
mvn spring-boot:run
```

### 4. Open in browser

By default, the application runs on:

```text
http://localhost:8080
```

After startup, use one of the demo accounts to sign in.

## Build

To build the project:

```bash
./mvnw clean package
```

The generated artifact will be located in:

```text
target/
```

## Project Structure

```text
src/main/java/ru/kata/spring/boot_security/demo
├── configs
│   ├── init
│   └── security configuration
├── controller
│   ├── admin
│   └── user
├── dto
├── entity
├── mapper
├── repository
├── security
└── service

src/main/resources
├── static
│   ├── css
│   └── js
└── templates
    └── fragments
```

## Possible Improvements

- Add validation and global exception handling
- Add pagination and filtering for the users table
- Replace `create-drop` with migrations
- Add automated tests
- Add Docker support for local startup
- Introduce audit logging for admin actions

## License

This project is provided for educational and demonstration purposes.
