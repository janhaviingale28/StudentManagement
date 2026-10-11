# StudentManagementSystem — Spring Boot + PostgreSQL + JPA

A beginner-friendly CRUD project with an `Student` entity, Spring Data JPA repository, service layer, REST controller, and Mockito/JUnit 5 unit tests for the service layer.

## Requirements
- JDK 17 or later
- Maven (or use the Maven integration in your IDE)
- PostgreSQL and pgAdmin (optional, but helpful)
- Eclipse IDE with Spring Tools, or IntelliJ IDEA

## Step 1: Create the PostgreSQL database
1. Open pgAdmin and connect to your PostgreSQL server.
2. Right-click **Databases** → **Create** → **Database**.
3. Set database name to `student_db`, choose your PostgreSQL user as owner, and click **Save**.
4. Confirm the server is running and note the PostgreSQL username/password and port (default is `5432`).

## Step 2: Configure database credentials
Open `src/main/resources/application.properties` and change:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/student_db
spring.datasource.username=postgres
spring.datasource.password=CHANGE_ME
```

Replace `CHANGE_ME` with your actual PostgreSQL password. Do not share or commit real credentials.

## Step 3: Import into Eclipse
1. Extract the ZIP file.
2. Open Eclipse → **File → Import**.
3. Select **Maven → Existing Maven Projects** → **Next**.
4. Browse to the extracted `StudentManagementSystem` folder.
5. Select `pom.xml` → **Finish**.
6. If dependencies do not load, right-click project → **Maven → Update Project**.
7. Ensure Eclipse uses JDK 17: **Window → Preferences → Java → Installed JREs**.

## Step 4: Run the application
1. Make sure PostgreSQL is running and the `student_db` database exists.
2. Open `StudentManagementSystemApplication.java`.
3. Right-click → **Run As → Java Application** (or **Spring Boot App** if Spring Tools is installed).
4. Hibernate will create/update the `students` table because `spring.jpa.hibernate.ddl-auto=update` is configured.

## Step 5: Run JUnit service tests
The service tests use Mockito, so they do not need a live PostgreSQL connection.

In Eclipse: right-click `StudentServiceTest.java` → **Run As → JUnit Test**.

With Maven installed, run in a terminal from the project folder:

```bash
mvn test
```

## REST API endpoints
Base URL: `http://localhost:8080/api/students`

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/students` | Create a student |
| GET | `/api/students` | Get all students |
| GET | `/api/students/1` | Get one student by ID |
| PUT | `/api/students/1` | Update student 1 |
| DELETE | `/api/students/1` | Delete student 1 |

### Example POST JSON
```json
{
  "name": "Janhavi Ingale",
  "email": "janhavi@example.com",
  "course": "Information Technology"
}
```

Use Postman to send the request as `POST` to `http://localhost:8080/api/students` with body → raw → JSON.

## How the layers work
- **Entity (`Student`)**: `@Entity`, `@Table`, `@Id` and other JPA annotations map the Java class to the PostgreSQL `students` table.
- **Repository (`StudentRepository`)**: `JpaRepository` provides CRUD database methods.
- **Service (`StudentService`)**: contains application logic and handles missing student IDs.
- **Controller (`StudentController`)**: exposes HTTP endpoints so you can test CRUD operations with Postman.
- **JUnit tests**: verify service behavior using a mocked repository; the tests do not require a database.

If startup reports a database connection error, first verify PostgreSQL is running, the port is correct, `student_db` exists, and the password in `application.properties` is correct.
