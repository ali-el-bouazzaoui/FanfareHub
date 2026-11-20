# FanfareHub

Web application to organize a brass band (*fanfare*): members, instrument sections, groups and events with registrations.

Academic project (web programming course, Polytech Lyon) · May 2026 · team of two: [Ali El Bouazzaoui](https://github.com/ali-el-bouazzaoui) and Nawal ([@Nawal-EL-KHAL](https://github.com/Nawal-EL-KHAL)). The user interface is in French.

## Features

- Sign up, log in and edit a member profile
- List of events and registration of members to each event
- Instrument sections (*pupitres*) and groups
- Administration page to manage the data
- Access control: a servlet filter redirects unauthenticated users to the login page, JSP pages are only reachable through the controllers, and the `/Admin` routes require an admin role

## Architecture

MVC with the DAO pattern.

| Layer | Content |
|---|---|
| View | JSP pages: `connexion.jsp`, `inscription.jsp`, `evenements.jsp`, `evenementParticipation.jsp`, `groupesPupitres.jsp`, `administration.jsp` |
| Controller | Servlets (`Auth`, `Profil`, `Evenement`, `Choix`, `Admin`) and an authentication filter, in `WEB-INF/classes/metier` |
| Model | Plain Java objects: `Fanfaron` (member), `Pupitre`, `Groupe`, `Evenement`, `Participer` |
| Data access | One DAO per entity in `WEB-INF/classes/dao`, using JDBC |
| Database | PostgreSQL, schema in `docs/FanfareHub_script.sql` (model in `docs/FanfareHub_MLD.md` and `docs/FanfareHub_MCD.png`) |

An architecture walkthrough is available in `docs/architecture_tutoriel.pdf`.

## Tech stack

Java · Servlets & JSP (Jakarta Servlet API, Tomcat 10 or later) · JDBC · PostgreSQL (with the `pgcrypto` extension)

## Getting started

1. Create a PostgreSQL database named `fanfarehub` and run `docs/FanfareHub_script.sql` (it enables `pgcrypto`).
2. Provide the database password through an environment variable. The user is `postgres` and the URL is `localhost:5432/fanfarehub`; both are set in `WEB-INF/classes/dao/DbConnectionManager.java`.
   ```bash
   export DB_PASSWORD="your-password"      # Windows: set DB_PASSWORD=your-password
   ```
3. Add the PostgreSQL JDBC driver to `WEB-INF/lib/` (jar files are not versioned).
4. Place the project folder in Tomcat's `webapps/` directory, then compile and restart Tomcat with `rebuild_restart.bat` (Windows), or compile the sources in `WEB-INF/classes` against Tomcat's servlet API yourself.
5. Open `http://localhost:8080/FanfareHub/`.

## Notes

- Passwords are hashed in SQL with SHA-256 (`pgcrypto`), without a salt. This is fine for a course project but not production-grade.
- The database connection class is based on a template provided by the course.
