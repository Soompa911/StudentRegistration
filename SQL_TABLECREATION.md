# 🗄️ SQL Table Documentation
![Last Updated](https://img.shields.io/badge/Last%20Updated-2025--03--01%2023:57:51%20UTC-blue)
![Author](https://img.shields.io/badge/Author-Soompa911-green)
![Database](https://img.shields.io/badge/Database-PostgreSQL-blue)

## sqlTable.java

### Overview
This class handles the creation of the `studentdetails` table in PostgreSQL database. It sets up the necessary schema for storing student locker information with automated primary key generation.

### Dependencies
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
```

### Database Schema
```sql
CREATE TABLE IF NOT EXISTS studentdetails (
    id SERIAL PRIMARY KEY,
    year INT NOT NULL,
    studentId VARCHAR(50) NOT NULL,
    passcode VARCHAR(10) NOT NULL,
    lockerId VARCHAR(50) NOT NULL,
    usage_start TIMESTAMP DEFAULT NULL,
    usage_end TIMESTAMP DEFAULT NULL
);
```

### Table Structure
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | SERIAL | PRIMARY KEY | Auto-incrementing identifier |
| year | INT | NOT NULL | Student's academic year |
| studentId | VARCHAR(50) | NOT NULL | Student identification number |
| passcode | VARCHAR(10) | NOT NULL | Locker access code |
| lockerId | VARCHAR(50) | NOT NULL | Unique locker identifier |
| usage_start | TIMESTAMP | DEFAULT NULL | Start time of locker usage |
| usage_end | TIMESTAMP | DEFAULT NULL | End time of locker usage |

### Full Source Code
```java
package database;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class sqlTable {
    public static void main(String[] args) {
        // Database connection details
        String url = "jdbc:postgresql://localhost:5432/postgres";
        String user = "postgres";
        String password = "johnpaul";

        // SQL to create the table
        String createTableSQL = "CREATE TABLE IF NOT EXISTS studentdetails (" +
                                "id SERIAL PRIMARY KEY, " +
                                "year INT NOT NULL, " +
                                "studentId VARCHAR(50) NOT NULL, " +
                                "passcode VARCHAR(10) NOT NULL, " +
                                "lockerId VARCHAR(50) NOT NULL, " +
                                "usage_start TIMESTAMP DEFAULT NULL, " +
                                "usage_end TIMESTAMP DEFAULT NULL" +
                                ");";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             Statement stmt = conn.createStatement()) {

            // Check if the database connection is valid
            if (conn != null) {
                System.out.println("Connected to the database!");

                // Execute the SQL statement to create the table
                stmt.executeUpdate(createTableSQL);
                System.out.println("Table 'studentdetails' created successfully (if it didn't exist already)!");
            }

        } catch (SQLException e) {
            // Handle SQL errors
            System.out.println("Error occurred while connecting to the database or creating the table.");
            e.printStackTrace();
        }
    }
}
```

### Usage
```bash
# Compile the class
javac sqlTable.java

# Run the table creation script
java sqlTable
```

### Expected Output
```bash
Connected to the database!
Table 'studentdetails' created successfully (if it didn't exist already)!
```

### Configuration
```properties
DB_URL      = jdbc:postgresql://localhost:5432/postgres
DB_USER     = postgres
DB_PASSWORD = johnpaul
```

### Important Features
1. Uses `IF NOT EXISTS` to prevent duplicate table creation
2. Implements auto-incrementing primary key (`SERIAL`)
3. Uses try-with-resources for automatic resource management
4. Includes timestamp fields for usage tracking
5. Provides appropriate field sizes for different data types

### Security Considerations
- Database credentials are hardcoded
- No input validation for student data
- No index creation for frequently queried fields
- No foreign key constraints
- No password encryption for passcode

### Improvement Suggestions

#### 1. Add Indexes
```sql
-- Add indexes for frequently queried columns
CREATE INDEX idx_student_id ON studentdetails(studentId);
CREATE INDEX idx_locker_id ON studentdetails(lockerId);
CREATE INDEX idx_usage_dates ON studentdetails(usage_start, usage_end);
```

#### 2. Add Data Validation Constraints
```sql
-- Add check constraints
ALTER TABLE studentdetails
ADD CONSTRAINT valid_year CHECK (year >= 1 AND year <= 12),
ADD CONSTRAINT valid_passcode CHECK (length(passcode) >= 4),
ADD CONSTRAINT valid_usage CHECK (usage_end >= usage_start);
```

#### 3. Implement Connection Pool
```java
public class ConnectionPool {
    private static HikariConfig config = new HikariConfig();
    private static HikariDataSource ds;

    static {
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/postgres");
        config.setUsername("postgres");
        config.setPassword("johnpaul");
        config.setMaximumPoolSize(10);
        ds = new HikariDataSource(config);
    }
}
```

### Future Enhancements
1. Add data validation constraints
2. Create indexes for better performance
3. Implement foreign key relationships
4. Add audit trail columns (created_at, updated_at)
5. Implement connection pooling
6. Add backup and restore procedures
7. Create maintenance procedures
8. Implement data archival strategy

---

<div align="center">

**Last Updated: 2025-03-01 23:57:51 UTC**  
**Documentation by: [Soompa911](https://github.com/Soompa911)**

</div>
