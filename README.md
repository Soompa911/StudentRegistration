# 🎓 Student Registration System
![Last Updated](https://img.shields.io/badge/Last%20Updated-2025--03--01%2023:49:28%20UTC-blue)
![Author](https://img.shields.io/badge/Author-Soompa911-green)
![Status](https://img.shields.io/badge/Status-Active-success)
![Language](https://img.shields.io/badge/Language-Java-orange)
![Database](https://img.shields.io/badge/Database-PostgreSQL-blue)

## 📚 Table of Contents
- [Overview](#-overview)
- [Features](#-features)
- [System Architecture](#-system-architecture)
- [Installation](#-installation)
- [Usage Guide](#-usage-guide)
- [Security Features](#-security-features)
- [Future Enhancements](#-future-enhancements)
- [Contributing](#-contributing)

## 🌟 Overview
A comprehensive Java-based Student Registration System featuring a user-friendly Swing GUI interface and PostgreSQL database integration. This system streamlines the student registration process across multiple academic levels with real-time program selection and administrative oversight.

## 🚀 Features

### Core Functionality
- **Student Registration Form**
  - Personal Information Capture
  - Academic Level Selection
  - Dynamic Program Assignment
  - Consent Management

### Academic Programs

<details>
<summary>Senior High School (Grades 11-12)</summary>

- 💻 ICT (Information and Communications Technology)
- 🔬 STEM (Science, Technology, Engineering, and Mathematics)
- 📊 ABM (Accountancy, Business, and Management)
- 📚 HUMSS (Humanities and Social Sciences)
- 🎓 GAS (General Academic Strand)
</details>

<details>
<summary>Undergraduate Studies</summary>

- 🖥️ BSCpE (Computer Engineering)
- 🏗️ BSCE (Civil Engineering)
- 📡 BSECE (Electronics and Communications)
- ⚙️ BSME (Mechanical Engineering)
- ⚡ BSEE (Electrical Engineering)
- 🌐 BSSA (System Administration)
</details>

<details>
<summary>Graduate Studies</summary>

- 🎓 Master's Programs in:
  - Computer Engineering
  - Civil Engineering
  - Electronics Engineering
  - Mechanical Engineering
  - Electrical Engineering
  - System Administration
</details>

## 🏗 System Architecture
```
StudentRegistration/
├── src/
│   └── database/
│       ├── ConnectDB.java
│       └── RegistrationForm.java
├── lib/
│   └── postgresql-jdbc.jar
├── docs/
│   └── SOURCE_CODE.md
└── README.md
```

## 🔧 Installation

### Prerequisites
- Java Development Kit (JDK) 8 or higher
- PostgreSQL 9.6 or higher
- PostgreSQL JDBC Driver
- Minimum 2GB RAM
- 100MB free disk space

### Database Setup
1. Install PostgreSQL
2. Create database and table
3. Configure connection settings

### Application Setup
1. Clone the repository
   ```bash
   git clone https://github.com/Soompa911/StudentRegistration.git
   ```
2. Configure database connection in `ConnectDB.java`
3. Compile the source code
4. Run the application

## 📖 Usage Guide

### Student Registration Process
1. Launch the application
2. Fill in personal details:
   - Name
   - Age
   - Sex
   - Email
3. Select academic level
4. Choose appropriate program
5. Accept the consent checkbox
6. Click "NEXT" to submit

### Administrative Access
1. Click "ADMIN LOG-IN"
2. Enter default password: `password`
3. View and manage student records
4. Use "BACK" to return to main form

## 🔐 Security Features
- Password-protected admin access
- Input validation
- Data integrity checks
- Consent verification

## 🚀 Future Enhancements
- [ ] Implement password hashing
- [ ] Add email verification
- [ ] Create student dashboard
- [ ] Enable bulk registration
- [ ] Add reporting features
- [ ] Implement connection pooling
- [ ] Add user session management
- [ ] Create backup and restore functionality

## 🤝 Contributing
1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

Please read [SOURCE_CODE.md](docs/SOURCE_CODE.md) for detailed code documentation.

---

<div align="center">

**Made with ❤️ by [Soompa911](https://github.com/Soompa911)**  
Last Updated: 2025-03-01 23:49:28 UTC

</div>
