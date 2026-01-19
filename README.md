<p align="center">
  <img src="https://www.especial.gr/wp-content/uploads/2019/03/panepisthmio-dut-attikhs.png" alt="UNIWA" width="150"/>
</p>

<p align="center">
  <strong>UNIVERSITY OF WEST ATTICA</strong><br>
  SCHOOL OF ENGINEERING<br>
  DEPARTMENT OF COMPUTER ENGINEERING AND INFORMATICS
</p>

<hr/>

<p align="center">
  <strong>Information Technology Security</strong>
</p>

<h1 align="center" style="letter-spacing: 1px;">
  SQL Injection
</h1>

<p align="center">
  <strong>Vasileios Evangelos Athanasiou</strong><br>
  Student ID: 19390005
</p>

<p align="center">
  <a href="https://github.com/Ath21" target="_blank">GitHub</a> ·
  <a href="https://www.linkedin.com/in/vasilis-athanasiou-7036b53a4/" target="_blank">LinkedIn</a>
</p>

<hr/>

<p align="center">
  <strong>Supervision</strong>
</p>

<p align="center">
  Supervisor: Ioanna Kantzavelou, Associate Professor<br>
</p>

<p align="center">
  <a href="https://ice.uniwa.gr/en/emd_person/ioanna-kantzavelou/" target="_blank">UNIWA Profile</a> ·
  <a href="https://www.linkedin.com/in/ioanna-kantzavelou-74685934/" target="_blank">LinkedIn</a>
</p>


<p align="center">
  Co-supervisor: Angelos Georgoulas, Assistant Professor<br>
</p>

<p align="center">
  <a href=https://scholar.google.com/citations?user=Djium2IAAAAJ&hl=en" target="_blank">UNIWA Profile</a> ·
  <a href="https://www.linkedin.com/in/aggelos-georgoulas/?originalSubdomain=uk" target="_blank">LinkedIn</a>
</p>

</hr>


<p align="center">
  Athens, May 2023
</p>


---

# Project Overview

This laboratory project focuses on **Information Technology Security**, with emphasis on **SQL Injection vulnerabilities** and **database management** within a **MySQL** environment. The lab was conducted as part of the **8th semester** curriculum for **Computer Engineering and Information Technology** at the **University of West Attica (UNIWA)**.

The main objective is to understand how databases are structured and accessed, how SQL queries operate, and how improper handling of user input can lead to serious security vulnerabilities such as SQL Injection.

---

## Table of Contents

| Section | Path / File | Description |
|--------:|-------------|-------------|
| 1 | `assign/` | Official laboratory exercise specifications |
| 1.1 | `assign/Exercise 3 (SQL Injection)_2023.pdf` | Assignment description (English) |
| 1.2 | `assign/Άσκηση 3 (SQL Injection)_2023.pdf` | Assignment description (Greek) |
| 2 | `docs/` | Technical reports and theoretical background |
| 2.1 | `docs/SQL-Injection.pdf` | Laboratory report and analysis (English) |
| 2.2 | `docs/Έγχυση-SQL.pdf` | Laboratory report and analysis (Greek) |
| 3 | `screens/` | Experimental results and attack demonstrations |
| 3.1 | `screens/Drast1/` | Database enumeration and data extraction |
| 3.2 | `screens/Drast2/` | Authentication bypass and web-based SQL injection |
| 3.3 | `screens/Drast4/` | Unsafe backend statements and privilege escalation |
| 3.4 | `screens/*.png` | Additional execution results and database state changes |
| 4 | `README.md` | Repository overview and usage instructions |

---

## Prerequisites
- **Operating System**: Ubuntu (SEED VM recommended)
- **Database Management System**: MySQL 5.7.19
- **Terminal Emulator**: Terminator

---

## Installation
### 1. Clone the Repository
```bash
git clone https://github.com/Information-Technology-Security/SQL-Injection.git
cd SQL-Injection/src
```

---

## Database Setup and Exploration
The project utilizes a database named **`Users`** with a primary table called **`credential`**.

### Initial Access
To access the MySQL console server, use:
```bash
mysql -u root -pseedubuntu
```

---

## Exploration Commands
Common SQL commands used to explore the database:
- List Databases
```sql
SHOW DATABASES;
```
- Select Database
```sql
USE Users;
```
- List Tables
```sql
SHOW TABLES;
```
- View Table Structure
```sql
DESCRIBE credential;
```

---

## Table Structure: `credential`
The `credential` table includes the following key fields:
- `ID`: Primary key, unsigned integer, auto-incremented
- `Name`: VARCHAR(30)
- `Salary`: INT
- `Password`: VARCHAR(300) (stored as hash digests)

Additional fields:
- EID
- birth
- SSN
- PhoneNumber
- Address
- Email
- NickName

---

## Activities Performed
### 1. Data Retrieval
Retrieve all employee records or specific entries:
```sql
SELECT * FROM credential;
SELECT * FROM credential WHERE Name='Samy';
```

### 2. Adding New Users
New users can be added using the `INSERT INTO` command. Passwords are initialized or updated later using secure hash functions:
```sql
INSERT INTO credential (Name, EID, Salary, birth, SSN, PhoneNumber, Address, Email, NickName)
VALUES ('Vasilis', '19390005', 150, '3/19', '12345678', '6977777777', 'Evangelistrias34', 'billath@gmail.com', 'KillBill');
```

### 3. Password Security
Passwords are not stored in plain text. Instead, they are saved as hash digests, specifically using the SHA-1 algorithm, to enhance database security and protect user credentials.

---

## Learning Outcomes
- Understand basic MySQL database management and schema exploration
- Perform SQL data retrieval and insertion operations
- Recognize the importance of password hashing
- Gain foundational awareness of SQL Injection risks and database security practices

---

## Conclusion
This lab provides practical experience with database security concepts and highlights the importance of secure SQL query handling. It serves as an introduction to identifying and mitigating SQL Injection vulnerabilities in real-world applications.

---

## Open the Documentation
1. Navigate to the `docs/` directory
2. Open the report corresponding to your preferred language:
    - English: `SQL-Injection.pdf`
    - Greek: `Έγχυση-SQL.pdf`