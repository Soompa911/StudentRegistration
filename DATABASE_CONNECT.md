
# 🔌 Database Connection Documentation
![Last Updated](https://img.shields.io/badge/Last%20Updated-2025--03--01%2023:55:24%20UTC-blue)
![Author](https://img.shields.io/badge/Author-jhnplsrno-green)
![Database](https://img.shields.io/badge/Database-PostgreSQL-blue)

## ConnectDB.java

### Overview
This class provides a simple database connection tester for the PostgreSQL database used in the Student Registration System. It verifies the connection settings and prints the connection status.

### Dependencies
```java
import java.sql.Connection;
import java.sql.DriverManager;
```

### Class Structure
```java
public class ConnectDB
```

### Database Configuration
```properties
JDBC_DRIVER = org.postgresql.Driver
DB_URL      = jdbc:postgresql://localhost:5432/postgres
DB_USER     = postgres
DB_PASSWORD = johnpaul
```

### Full Source Code
```java
package database;

import java.sql.Connection;
import java.sql.DriverManager;

public class ConnectDB {
    public static void main(String[] args) {
        
        Connection connection = null;
        
        try {
            // Load PostgreSQL JDBC Driver
            Class.forName("org.postgresql.Driver");
            
            // Establish database connection
            connection = DriverManager.getConnection(
                "jdbc:postgresql://localhost:5432/postgres",
                "postgres",
                "johnpaul"
            );
            
            // Check connection status
            if(connection != null) {
                System.out.println("Connection OK");
            } else {
                System.out.println("Connection Failed");
            }
            
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

### Method Documentation

#### `main(String[] args)`
The main method that tests the database connection.

**Process Flow:**
1. Initialize connection object as null
2. Load PostgreSQL JDBC driver
3. Attempt database connection
4. Print connection status
5. Handle any potential exceptions

### Usage Example
```bash
# Compile the class
javac ConnectDB.java

# Run the connection test
java ConnectDB
```

### Expected Output
```bash
# Successful connection
Connection OK

# Failed connection
Connection Failed
```

### Important Notes
1. Connection parameters are hardcoded
2. No connection closure implementation
3. Basic exception handling
4. No connection pooling
5. Single-purpose testing class

### Security Considerations
- Database credentials are exposed in code
- No SSL/TLS configuration
- No connection timeout settings
- No password encryption

### Improvement Suggestions

#### 1. Configuration Management
```java
// Move database credentials to properties file
public class DatabaseConfig {
    private static Properties props = new Properties();
    
    static {
        try (InputStream input = new FileInputStream("config.properties")) {
            props.load(input);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 2. Connection Closure
```java
// Add proper connection closure
finally {
    try {
        if (connection != null && !connection.isClosed()) {
            connection.close();
            System.out.println("Connection Closed");
        }
    } catch (SQLException e) {
        e.printStackTrace();
    }
}
```

#### 3. Enhanced Error Handling
```java
// Implement specific exception handling
try {
    // ... connection code ...
} catch (ClassNotFoundException e) {
    System.err.println("PostgreSQL JDBC Driver not found.");
    e.printStackTrace();
} catch (SQLException e) {
    System.err.println("Database connection failed!");
    e.printStackTrace();
}
```

#### 4. Connection Pooling
```java
// Example using HikariCP
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

    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }
}
```

### Future Enhancements
1. Implement connection pooling
2. Add configuration file support
3. Enhance error handling
4. Add connection timeout settings
5. Implement SSL/TLS security
6. Add connection status logging
7. Include connection metrics
8. Add retry mechanism

---

<div align="center">

**Documentation by: [jhnplsrno](https://github.com/jhnplsrno)**

</div>
