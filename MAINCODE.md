# 📝 Source Code Documentation
![Last Updated](https://img.shields.io/badge/Last%20Updated-2025--03--01%2023:52:32%20UTC-blue)
![Author](https://img.shields.io/badge/Author-Soompa911-green)

## RegistrationForm.java

### Overview
This file contains the main registration form implementation for the Student Registration System. It provides a graphical user interface for student registration and administrative functions.

### Class Structure
```java
public class RegistrationForm extends JFrame implements ActionListener
```

### Dependencies
```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.table.DefaultTableModel;
import java.sql.*;
```

### Components
```java
// GUI Components
private JLabel nameLabel, ageLabel, sexLabel, emailLabel, academicLevelLabel, 
        academicProgramLabel, passwordLabel, messageLabel;
private JTextField nameField, ageField, emailField;
private JRadioButton maleRadio, femaleRadio;
private JComboBox<String> academicLevelCombo, academicProgramCombo;
private JPasswordField adminPassword;
private JCheckBox consentCheckbox;
private JButton nextButton, adminButton, passwordButton, backButton, closeButton;

// Data Components
private int age = -1;
private String sex = "";
private ButtonGroup sexGroup;
private JTable studentTable;
private Connection connection;
```

### Full Source Code
<details>
<summary>Click to expand full source code</summary>

```java

package database;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.table.DefaultTableModel;
import java.sql.*;

public class RegistrationForm extends JFrame implements ActionListener {
    // Define instance variables for all GUI components
    private JLabel nameLabel, ageLabel, sexLabel, emailLabel, academicLevelLabel, academicProgramLabel, passwordLabel, messageLabel;
    private JTextField nameField, ageField, emailField;
    private JRadioButton maleRadio, femaleRadio;
    private JComboBox<String> academicLevelCombo, academicProgramCombo;
    private JPasswordField adminPassword;
    private JCheckBox consentCheckbox;
    private JButton nextButton, adminButton, passwordButton, backButton, closeButton;
    private int age = -1;
    private String sex = "";
    private ButtonGroup sexGroup;
    private JTable studentTable;
    private Connection connection;

    public RegistrationForm() {
        // Set up the frame
        setTitle("Registration Form");
        setSize(900, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new GridLayout(8, 2));
        setLocationRelativeTo(null);
        studentTable = new JTable();
        studentTable.setModel(new DefaultTableModel(
                new Object[][] {},
                new String[] {"Name", "Age", "Sex", "Email Address", "Academic Level", "Academic Program"}));

        // Add GUI components
        addComponents();

        // Establish database connection
        connectToDatabase();

        // Load existing student data into the table
        populateTable();

        // Show the frame
        setVisible(true);
    }

    private void addComponents() {
        nameLabel = new JLabel("     Name:");
        add(nameLabel);
        nameField = new JTextField(10);
        add(nameField);

        ageLabel = new JLabel("     Age:");
        add(ageLabel);
        ageField = new JTextField(10);
        add(ageField);

        sexLabel = new JLabel("     Sex:");
        add(sexLabel);
        maleRadio = new JRadioButton("Male");
        femaleRadio = new JRadioButton("Female");
        sexGroup = new ButtonGroup();
        sexGroup.add(maleRadio);
        sexGroup.add(femaleRadio);
        JPanel sexPanel = new JPanel();
        sexPanel.add(maleRadio);
        sexPanel.add(femaleRadio);
        add(sexPanel);

        emailLabel = new JLabel("     Email:");
        add(emailLabel);
        emailField = new JTextField();
        add(emailField);

        academicLevelLabel = new JLabel("     Academic Level:");
        add(academicLevelLabel);
        String[] academicLevels = {null, "Grade 11", "Grade 12", "Under-Graduate Studies", "Graduate Studies"};
        academicLevelCombo = new JComboBox<>(academicLevels);
        academicLevelCombo.addActionListener(this);
        add(academicLevelCombo);

        academicProgramLabel = new JLabel("     Academic Program:");
        add(academicProgramLabel);
        String[] gradePrograms = {null, "ICT", "STEM", "ABM", "HUMSS", "GAS"};
        academicProgramCombo = new JComboBox<>(gradePrograms);
        add(academicProgramCombo);

        consentCheckbox = new JCheckBox("I consent to register in the school");
        add(consentCheckbox);

        nextButton = new JButton("NEXT");
        nextButton.addActionListener(this);
        add(nextButton);

        adminButton = new JButton("ADMIN LOG-IN");
        add(adminButton);
        adminButton.addActionListener(e -> showAdminLogin());

        closeButton = new JButton("EXIT");
        closeButton.addActionListener(e -> {
            closeConnection();
            System.exit(0);
        });
        add(closeButton);
    }

    private void showAdminLogin() {
        JFrame passwordFrame = new JFrame();
        passwordFrame.setTitle("ADMIN");
        passwordFrame.setSize(300, 200);
        passwordFrame.setDefaultCloseOperation(HIDE_ON_CLOSE);
        passwordFrame.setLocationRelativeTo(null);
        passwordLabel = new JLabel("Password:");
        messageLabel = new JLabel();
        adminPassword = new JPasswordField(20);
        passwordButton = new JButton("LOG-IN");

        passwordButton.addActionListener(e -> {
            String password = new String(adminPassword.getPassword());
            if (password.equals("password")) {
                showAdminFrame();
                passwordFrame.dispose();
            } else {
                messageLabel.setText("INVALID ADMIN PASSWORD");
            }
        });

        backButton = new JButton("BACK");
        backButton.addActionListener(e -> passwordFrame.dispose());

        JPanel passwordPane = new JPanel(new FlowLayout());
        passwordPane.add(passwordLabel);
        passwordPane.add(adminPassword);
        passwordPane.add(passwordButton);
        passwordPane.add(backButton);
        passwordPane.add(messageLabel);

        passwordFrame.add(passwordPane);
        passwordFrame.setVisible(true);
        adminButton.setEnabled(false);
    }

    private void showAdminFrame() {
        JFrame adminFrame = new JFrame();
        adminFrame.setTitle("Register Students");
        adminFrame.setSize(900, 400);
        adminFrame.setDefaultCloseOperation(HIDE_ON_CLOSE);
        adminFrame.setLocationRelativeTo(null);

        JButton adminBack = new JButton("BACK");
        adminBack.addActionListener(e -> {
            adminFrame.dispose();
            adminButton.setEnabled(true); // Re-enable the admin button when the admin frame is closed
        });

        JPanel adminPanel = new JPanel(new FlowLayout());
        adminPanel.add(adminBack);

        adminFrame.add(adminPanel, BorderLayout.SOUTH);
        adminFrame.add(new JScrollPane(studentTable));
        adminFrame.setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == academicLevelCombo) {
            handleAcademicLevelChange();
        } else if (e.getSource() == nextButton) {
            registerStudent();
        }
    }

    private void handleAcademicLevelChange() {
        String selectedLevel = (String) academicLevelCombo.getSelectedItem();
        String[] programs;
        switch (selectedLevel) {
            case "Grade 11":
            case "Grade 12":
                programs = new String[]{"ICT", "STEM", "ABM", "HUMSS", "GAS"};
                break;
            case "Under-Graduate Studies":
                programs = new String[]{"BSCpE", "BSCE", "BSECE", "BSME", "BSEE", "BSSA"};
                break;
            case "Graduate Studies":
                programs = new String[]{"Master’s in CpE", "Master’s in CE", "Master’s in ECE", "Master’s in ME", "Master’s in EE", "Master’s in SA"};
                break;
            default:
                programs = new String[]{null};
        }
        academicProgramCombo.setModel(new DefaultComboBoxModel<>(programs));
    }

    private void registerStudent() {
        String name = nameField.getText();
        String ageString = ageField.getText();
        try {
            age = Integer.parseInt(ageString);
        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this, "Please enter a valid age.", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        sex = maleRadio.isSelected() ? "Male" : femaleRadio.isSelected() ? "Female" : "";
        String email = emailField.getText();
        String academicLevel = (String) academicLevelCombo.getSelectedItem();
        String academicProgram = (String) academicProgramCombo.getSelectedItem();
        boolean consent = consentCheckbox.isSelected();

        if (name.isEmpty() || ageString.isEmpty() || email.isEmpty() || academicLevel == null || academicProgram == null || !consent) {
            JOptionPane.showMessageDialog(this, "Please fill out all required fields and consent to register in the school.", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        // Show confirmation dialog with student details
        int confirmation = JOptionPane.showConfirmDialog(this, 
            "Please confirm your details:\n" +
            "Name: " + name + "\n" +
            "Age: " + ageString + "\n" +
            "Sex: " + sex + "\n" +
            "Email: " + email + "\n" +
            "Academic Level: " + academicLevel + "\n" +
            "Academic Program: " + academicProgram,
            "Confirm Registration", JOptionPane.YES_NO_OPTION);

        if (confirmation == JOptionPane.YES_OPTION) {
            // Proceed to register the student in the database
            try {
                String sql = "INSERT INTO students (name, age, sex, email, academic_level, academic_program) VALUES (?, ?, ?, ?, ?, ?)";
                PreparedStatement pstmt = connection.prepareStatement(sql);
                pstmt.setString(1, name);
                pstmt.setInt(2, age);
                pstmt.setString(3, sex);
                pstmt.setString(4, email);
                pstmt.setString(5, academicLevel);
                pstmt.setString(6, academicProgram);
                pstmt.executeUpdate();

                populateTable();
                JOptionPane.showMessageDialog(this, "You have successfully registered in this School");

                clearFields();
            } catch (SQLException e) {
                e.printStackTrace();
                JOptionPane.showMessageDialog(this, "Error registering student: " + e.getMessage(), "Error", JOptionPane.ERROR_MESSAGE);
            }
        } else {
            // If the user clicks "No", just return without registering
            JOptionPane.showMessageDialog(this, "Registration canceled.");
        }
    }

    private void clearFields() {
        nameField.setText("");
        ageField.setText("");
        emailField.setText("");
        sexGroup.clearSelection();
        academicLevelCombo.setSelectedIndex(0);
        academicProgramCombo.setModel(new DefaultComboBoxModel<>(new String[]{null}));
        consentCheckbox.setSelected(false);
    }

    private void connectToDatabase() {
        try {
            // Replace with your database connection details
            String url = "jdbc:postgresql://public.johnpaul:5432/postgres";
            String username = "postgres";
            String password = "johnpaul";
            connection = DriverManager.getConnection(url, username, password);
        } catch (SQLException e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(this, "Error connecting to the database: " + e.getMessage(), "Error", JOptionPane.ERROR_MESSAGE);
        }
    }

    private void populateTable() {
        DefaultTableModel model = (DefaultTableModel) studentTable.getModel();
        model.setRowCount(0); // Clear the table before repopulating

        try {
            Statement stmt = connection.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM students");
            while (rs.next()) {
                String name = rs.getString("name");
                int age = rs.getInt("age");
                String sex = rs.getString("sex");
                String email = rs.getString("email");
                String academicLevel = rs.getString("academic_level");
                String academicProgram = rs.getString("academic_program");

                model.addRow(new Object[]{name, age, sex, email, academicLevel, academicProgram});
            }
        } catch (SQLException e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(this, "Error retrieving student data: " + e.getMessage(), "Error", JOptionPane.ERROR_MESSAGE);
        }
    }

    private void closeConnection() {
        try {
            if (connection != null && !connection.isClosed()) {
                connection.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(RegistrationForm::new);
    }
}


```
</details>

### Key Methods

#### 1. Constructor
```java
public RegistrationForm()
```
Initializes the registration form window and sets up all necessary components.

#### 2. GUI Setup
```java
private void addComponents()
```
Creates and arranges all GUI elements in the form.

#### 3. Admin Interface
```java
private void showAdminLogin()
private void showAdminFrame()
```
Handles administrative login and dashboard functionality.

#### 4. Event Handling
```java
public void actionPerformed(ActionEvent e)
private void handleAcademicLevelChange()
```
Manages user interactions and dynamic content updates.

#### 5. Student Registration
```java
private void registerStudent()
```
Processes and validates student registration data.

#### 6. Database Operations
```java
private void connectToDatabase()
private void populateTable()
private void closeConnection()
```
Manages database connections and operations.

### Database Configuration
```java
String url = "jdbc:postgresql://public.johnpaul:5432/postgres";
String username = "postgres";
String password = "johnpaul";
```

### Academic Programs
```java
// Senior High School
String[] programs = {"ICT", "STEM", "ABM", "HUMSS", "GAS"};

// Undergraduate Studies
String[] programs = {"BSCpE", "BSCE", "BSECE", "BSME", "BSEE", "BSSA"};

// Graduate Studies
String[] programs = {"Master's in CpE", "Master's in CE", "Master's in ECE", 
                    "Master's in ME", "Master's in EE", "Master's in SA"};
```

### Usage Example
```java
public static void main(String[] args) {
    SwingUtilities.invokeLater(RegistrationForm::new);
}
```

### Important Notes
1. The admin password is currently hardcoded as "password"
2. Database credentials are hardcoded in the source
3. The application uses a single-table database structure
4. Form validation is implemented but could be enhanced
5. The GUI uses GridLayout for component arrangement

### Security Considerations
- Admin password should be hashed
- Database credentials should be moved to configuration file
- Input validation could be strengthened
- Session management could be implemented

### Future Improvements
1. Implement connection pooling
2. Add logging functionality
3. Enhance input validation
4. Add email verification
5. Implement user session management
6. Create backup functionality

---

<div align="center">

**Last Updated: 2025-03-01 23:52:32 UTC**  
**Documentation by: [Soompa911](https://github.com/Soompa911)**

</div>
