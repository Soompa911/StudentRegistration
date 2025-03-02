# 🗄️ SQL Table Documentation
![Last Updated](https://img.shields.io/badge/Last%20Updated-2025--03--02%2000:10:16%20UTC-blue)
![Author](https://img.shields.io/badge/Author-jhnplsrno-green)
![Database](https://img.shields.io/badge/Database-PostgreSQL-blue)

## CreateTable.java

### Overview
This class is responsible for creating the `students` table in PostgreSQL database. It establishes the core schema for storing student registration information with proper constraints and data types.

### Dependencies
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
```

### Database Schema
```sql
CREATE TABLE IF NOT EXISTS students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age INTEGER NOT NULL,
    sex VARCHAR(10) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    academic_level VARCHAR(50) NOT NULL,
    academic_program VARCHAR(50) NOT NULL
);
```

### Table Structure
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | SERIAL | PRIMARY KEY | Auto-incrementing identifier |
| name | VARCHAR(100) | NOT NULL | Student's full name |
| age | INTEGER | NOT NULL | Student's age |
| sex | VARCHAR(10) | NOT NULL | Student's gender |
| email | VARCHAR(100) | UNIQUE, NOT NULL | Student's email address |
| academic_level | VARCHAR(50) | NOT NULL | Academic level (e.g., Grade 11, Undergraduate) |
| academic_program | VARCHAR(50) | NOT NULL | Specific program (e.g., STEM, BSCpE) |

### Full Source Code
```java
package database;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class CreateTable {
    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:5432/postgres";
        String user = "postgres";
        String password = "johnpaul";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             Statement stmt = conn.createStatement()) {

            // SQL statement to create the students table (from RegistrationForm)
            String createStudentsTable = "CREATE TABLE IF NOT EXISTS students (" +
                                         "id SERIAL PRIMARY KEY, " +
                                         "name VARCHAR(100) NOT NULL, " +
                                         "age INTEGER NOT NULL, " +
                                         "sex VARCHAR(10) NOT NULL, " +
                                         "email VARCHAR(100) UNIQUE NOT NULL, " +
                                         "academic_level VARCHAR(50) NOT NULL, " +
                                         "academic_program VARCHAR(50) NOT NULL" +
                                         ");";
            stmt.executeUpdate(createStudentsTable);
            System.out.println("Students table created successfully!");

        } catch (SQLException e) {
            System.err.println("Error creating table: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Usage
```bash
# Compile the class
javac CreateTable.java

# Run the table creation script
java CreateTable
```

### Expected Output
```bash
Students table created successfully!
```

### Configuration
```properties
DB_URL      = jdbc:postgresql://localhost:5432/postgres
DB_USER     = postgres
DB_PASSWORD = johnpaul
```

### Key Features
1. Uses `IF NOT EXISTS` to prevent duplicate table creation
2. Implements auto-incrementing primary key (`SERIAL`)
3. Uses try-with-resources for automatic resource management
4. Enforces data integrity with `NOT NULL` constraints
5. Ensures email uniqueness with `UNIQUE` constraint
6. Provides appropriate field sizes for different data types

### Security Considerations
- Database credentials are hardcoded
- No input validation for field lengths
- No password encryption for sensitive data
- No connection pooling implemented
- No SSL/TLS configuration

### Improvement Suggestions

#### 1. Add Data Validation
```sql
-- Add check constraints
ALTER TABLE students
ADD CONSTRAINT valid_age CHECK (age >= 15 AND age <= 100),
ADD CONSTRAINT valid_email CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$'),
ADD CONSTRAINT valid_sex CHECK (sex IN ('Male', 'Female'));
```

#### 2. Add Indexes
```sql
-- Add indexes for frequently queried columns
CREATE INDEX idx_student_email ON students(email);
CREATE INDEX idx_academic_level ON students(academic_level);
CREATE INDEX idx_academic_program ON students(academic_program);
```

#### 3. Add Audit Columns
```sql
-- Add timestamp columns for tracking
ALTER TABLE students
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
```

### Future Enhancements
1. Implement connection pooling
2. Move database credentials to configuration file
3. Add data validation constraints
4. Create indexes for better performance
5. Add audit trail columns
6. Implement backup procedures
7. Add foreign key relationships
8. Create maintenance scripts

---

<div align="center">

**Last Updated: 2025-03-02 00:10:16 UTC**  
**Documentation by: [jhnplsrno](https://github.com/jhnplsrno)**

</div>
