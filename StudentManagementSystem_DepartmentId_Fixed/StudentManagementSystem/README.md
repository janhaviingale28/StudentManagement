# Student Management System

Spring Boot 3.4.5, Java 17, Spring Web, Spring Data JPA, PostgreSQL, and JUnit 5/Mockito.

## Included features
- Student CRUD REST APIs at `/api/students`
- Student and StudentProfile: one-to-one mapping
- Department and Students: one-to-many / many-to-one mapping
- Department REST APIs at `/api/departments`
- Student service unit tests and relationship/service unit tests

## PostgreSQL setup
1. Create a PostgreSQL database named `student_db`.
2. Open `src/main/resources/application.properties`.
3. Replace `CHANGE_ME` with your local PostgreSQL password.
4. Start PostgreSQL, then run `StudentManagementSystemApplication`.

## Run tests
In Eclipse: right-click each test class under `src/test/java` > Run As > JUnit Test.
Or from the project root using Maven: `mvn test`.

The Mockito-based unit tests do not require a running PostgreSQL server. They verify service behaviour and in-memory object associations. To verify that mappings persist and load correctly in PostgreSQL, also run database integration tests against a configured test database.

## Postman: create a Department first, then a Student

1. Start the app after setting the real PostgreSQL password in `application.properties`.
2. Create a Department: `POST http://localhost:8080/api/departments` with Body > raw > JSON:

```json
{
  "departmentName": "Information Technology"
}
```

3. Read `GET http://localhost:8080/api/departments` and note the actual generated `id`.
4. Create a Student: `POST http://localhost:8080/api/students` with Body > raw > JSON. Replace `1` with the Department ID returned in step 3:

```json
{
  "name": "Janhavi Ingale",
  "email": "janhavi.profile@example.com",
  "course": "Java",
  "profile": {
    "address": "Ahmedabad",
    "phone": "9876543210"
  },
  "department": {
    "id": 1
  }
}
```

The service loads the Department by ID before saving. A missing Department ID or unknown ID is rejected instead of silently saving a Student with `department_id = NULL`. The Department property is accepted in the request but omitted from Student JSON responses to avoid recursive JSON.

## Verify in pgAdmin

Run in Query Tool (adjust table/column names only if you changed the entities):

```sql
SELECT id, department_name FROM departments;
SELECT id, full_name, email, course, department_id FROM students;
```

Older rows that already have `department_id IS NULL` are not automatically repaired by this code. First identify the correct Department ID, then update only the intended Student rows. Example template:

```sql
UPDATE students SET department_id = 1 WHERE id = 1;
```

Use the real IDs from your database; do not run the example blindly.

## Example: create a Department with students
POST `http://localhost:8080/api/departments`

```json
{
  "departmentName": "Information Technology",
  "students": [
    {
      "name": "Rahul",
      "email": "rahul.it@example.com",
      "course": "Java"
    },
    {
      "name": "Priya",
      "email": "priya.it@example.com",
      "course": "Spring Boot"
    }
  ]
}
```

`Department` uses `mappedBy="department"`; `Student` owns the relationship through `department_id`. The `addStudent` helper sets both sides correctly.
