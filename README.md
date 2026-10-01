# SiwBarber

**Online booking web app for a barber shop** ("IntroBarber"), built with **Spring Boot**, **Spring Security**, **Thymeleaf** and **PostgreSQL**.
Individual project for the *Sistemi Informativi su Web* (Web Information Systems) course, Bachelor's degree in Computer Engineering, Roma Tre University.

Customers browse the services, pick a barber and a time slot, and manage their bookings. Barbers have a private area where they see their upcoming appointments, add new services and update prices.

---

## Features

**Public**
- Landing page with opening hours, address and the list of services with prices
- Sign-up and login

**Customer**
- Book an appointment by choosing **service, barber and date/time**
- See upcoming bookings and **cancel** them

**Barber (admin area)**
- Dashboard with the **upcoming appointments** assigned to them
- Cancel an appointment
- **Add new services** (name, description, price) and **edit prices**

**Business rules and validation**
- A barber cannot be booked twice for the **same time slot** (custom `PrenotazioneValidator`)
- Bookings must be in the **future** (`@Future`), prices cannot be negative (`@Min(0)`)
- Server-side validation for users, credentials and services, with error messages from `messages.properties`

## Tech stack

| Layer | Technology |
|---|---|
| Backend | Java 17, Spring Boot 3.3, Spring MVC |
| Security | Spring Security: form login, role-based access (`DEFAULT` / `ADMIN`), BCrypt password hashing |
| Persistence | Spring Data JPA, Hibernate, PostgreSQL |
| Frontend | Thymeleaf (with Spring Security extras), Bootstrap 4, custom CSS |
| Build | Maven (wrapper included) |

## Architecture

Classic layered MVC structure:

```
src/main/java/it/uniroma3/siw/
├── model/           # JPA entities: User, Credentials, Servizio (service), Prenotazione (booking)
├── repository/      # Spring Data repositories
├── service/         # business logic
├── controller/      # MVC controllers + validator/ (custom validators)
└── authentication/  # Spring Security configuration
src/main/resources/
├── templates/       # Thymeleaf views (public, customer, admin/)
├── static/          # CSS and images
└── application.properties
```

**Data model**: a `Prenotazione` (booking) links a customer (`User`), a barber (`User` with the `ADMIN` role), a `Servizio` (service) and a date/time. `Credentials` stores username, hashed password and role, one-to-one with `User`.

## Getting started

Requirements: **Java 17+** and **PostgreSQL**.

1. **Create the database**
   ```sql
   CREATE DATABASE siwbarber;
   ```

2. **Set the database credentials** (defaults: user `postgres`, password `postgres`, database on `localhost:5432`)
   ```bash
   # Linux / macOS
   export DB_USERNAME=postgres
   export DB_PASSWORD=your_password
   # Windows (PowerShell)
   $env:DB_USERNAME="postgres"; $env:DB_PASSWORD="your_password"
   ```

3. **Run the app**
   ```bash
   ./mvnw spring-boot:run        # Windows: mvnw.cmd spring-boot:run
   ```
   Open http://localhost:8080. The tables are created automatically (`ddl-auto=update`).

4. **Create a barber account**: register a user from the site, then promote it to barber:
   ```sql
   UPDATE credentials SET role = 'ADMIN' WHERE username = 'your_username';
   ```
   Barbers appear in the booking form and get access to the admin area.

## Author

**Bogdan Andrei Tutuianu** – [GitHub](https://github.com/bogX2)
