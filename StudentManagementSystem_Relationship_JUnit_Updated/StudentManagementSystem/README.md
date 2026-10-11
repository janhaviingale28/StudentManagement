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

## Example: create a Student with a profile
POST `http://localhost:8080/api/students`

```json
{
  "name": "Janhavi Ingale",
  "email": "janhavi.profile@example.com",
  "course": "Java",
  "profile": {
    "address": "Ahmedabad",
    "phone": "9876543210"
  }
}
```

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
