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
| 3.1 | `screens/Activity1/` | Database enumeration and data extraction |
| 3.2 | `screens/Activity2/` | Authentication bypass and web-based SQL injection |
| 3.3 | `screens/Activity4/` | Unsafe backend statements and privilege escalation |
| 3.4 | `screens/*.png` | Additional execution results and database state changes |
| 4 | `README.md` | Repository overview and usage instructions |

---

## Database Structure Overview
Understanding the target is the first step in identifying a potential injection point. The document outlines the following structure for the `credential` table:
- Database Name: `Users` 
- Target Table: `credential` 
- Key Fields: The table includes `ID`, `Name`, `EID`, `Salary`, `birth`, `SSN`, `PhoneNumber`, `Address`, `Email`, `NickName`, and `Password`.
- Field Types: Numerical data like `ID` and `Salary` use `int`, while textual data like `Name` and `PhoneNumber` use `varchar`.

---

## How SQL Injection Works
An SQL injection attack occurs when an attacker "injects" malicious SQL code into an input field, which is then executed by the backend database. Using the commands from the lab as examples:
- **Standard Query**: A legitimate search for a user might look like:
```sql
SELECT * FROM credential WHERE Name='Samy';.
```
- **The Vulnerability**: If the application does not sanitize input, an attacker could input 
```sql
' OR '1'='1
``` 
into a name field.
- **The Result**: The executed command becomes: 
```sql
SELECT * FROM credential WHERE Name='' OR '1'='1'; 
```
Because `'1'='1'` is always true, the database returns every record in the table, bypassing authentication or privacy controls.

---

## Data Exposure and Security
The document highlights what an attacker stands to gain and how developers attempt to mitigate these risks:
- **Sensitive Information**: Successful injection can expose SSN (Social Security Numbers), Salary details, and Address information.
+1
- **Password Protection**: To prevent simple credential theft, passwords in this environment are stored as digests calculated by a hash algorithm (specifically SHA-1).
+1
- **Example Hash**: A password for the user "Alice" appears as `fdbe918bdae83000aa54747fc95fe0470fff4976`. Even if an attacker uses SQL injection to download the table, they still need to "crack" these hashes to get the actual passwords.

---

## Mitigation Strategies
The lab demonstrates the importance of proper database management to prevent unauthorized access:
- **Input Validation**: Ensuring that only expected data types (like int for salary) are accepted.
- **Authentication**: Logging into the MySQL server requires specific root credentials.
- **Hashing**: Never storing passwords in plain text.

---

## SQL Injection in SELECT Statements

In the laboratory exercise, the following SQL command is used to retrieve specific user data:

```sql
SELECT * FROM credential WHERE Name='Samy';
```

### The Vulnerability
If a web application accepts user input (e.g., a name field) and directly concatenates it into the SQL query without validation or sanitization, an attacker can alter the query’s logic.

#### Malicious Input
```sql
' OR 1=1 --
```
#### Resulting Query
```sql
SELECT * FROM credential WHERE Name='' OR 1=1 --';
```

#### Impact
Because `1=1` is always true, the condition evaluates to true for every row in the table.
As a result, the database returns all 6 rows of the credential table instead of a single user, exposing sensitive data such as:
- SSN
- Salary
- Address and contact information
- Password hashes for all users

---

## SQL Injection in UPDATE Statements
The lab demonstrates inserting new users and updating passwords. SQL Injection in an `UPDATE` statement can be even more damaging than in a `SELECT`.

### Standard Query
```sql
UPDATE credential SET Password='[hash]' WHERE ID=7;
```

### The Vulnerability
If the `ID` value is taken directly from user input, an attacker can manipulate the update condition.

#### Malicious Input for ID
```sql
7 OR 1=1
```
#### Resulting Query
```sql
UPDATE credential SET Password='hacker_hash' WHERE ID=7 OR 1=1;
```
#### Impact
Since `1=1` is always true, every row in the table is updated.
This means:
- All users’ passwords are replaced with the attacker’s chosen hash
- Legitimate users are locked out
- The attacker gains control over all accounts

---

## Countermeasure: Prepared Statements
The most effective defense against the SQL Injection vulnerabilities demonstrated in this lab is the use of Prepared Statements (also known as Parameterized Queries).

### How It Works
Instead of dynamically building SQL strings with user input, the application sends a query template to the database using placeholders.

#### Preparation
```sql
SELECT * FROM credential WHERE Name = ?;
```
The database parses and compiles the SQL structure without any user input.

#### Binding
User input is sent separately and bound to the placeholder as a literal value.

#### Execution
The database treats the input strictly as data, not executable SQL.
If an attacker provides:
```sql
' OR 1=1
```
the database searches for a user whose name is literally `'` OR `1=1`, rather than executing the injected logic.

### Benefits of Prepared Statements

#### Separation of Code and Data
- User input is never interpreted as part of the SQL command.

#### Type Safety
- Ensures fields like ID or Salary are handled as integers, not strings containing hidden SQL logic.

##### Performance
- The database can reuse the compiled query plan for multiple executions, improving efficiency.

---

# Installation & Setup Guide  

This guide describes how to set up the required environment and reproduce the **SQL Injection laboratory exercises** using **MySQL** in a **controlled academic setting**.  
The project is part of the **Information Technology Security** course at the **University of West Attica (UNIWA)**.

> **Warning**  
> This project demonstrates real security vulnerabilities.  
> It must be executed **only in an isolated laboratory environment** (local machine or virtual machine).  
> **Never apply these techniques to production systems.**

---

## Prerequisites

### 1. Operating System

Recommended environments:

- **Linux (preferred)**
  - Ubuntu 16.04 / 18.04 / 20.04
  - SEED Ubuntu VM (fully compatible)

---

### 2. Required Software

#### MySQL Server

The laboratory uses **MySQL** as the backend database.

Install MySQL:
```bash
sudo apt update
sudo apt install -y mysql-server
```
Verify installation:
```bash
mysql --version
```
Start MySQL service:
```bash
sudo systemctl start mysql
sudo systemctl enable mysql
```

#### MySQL Client (CLI)
Installed automatically with MySQL Server.

Verify:
```bash
mysql -u root -p
```

#### (Optional) Web Stack (for Web-based SQL Injection)
If you want to reproduce web-form SQL injection scenarios:

Install LAMP stack:
```
bash
sudo apt install -y apache2 php php-mysql
```
Verify Apache:
```bash
http://localhost
```

---

## Installation
### 1. Clone the Repository
```bash
git clone https://github.com/Information-Technology-Security/SQL-Injection.git
cd SQL-Injection
```

---

## Database Setup
### 1. Log in to MySQL as Root
```bash
sudo mysql -u root
```

### 2. Create the Database
```sql
CREATE DATABASE Users;
USE Users;
```

### 3. Create the Vulnerable Table
```sql
CREATE TABLE credential (
    ID INT PRIMARY KEY,
    Name VARCHAR(50),
    EID VARCHAR(20),
    Salary INT,
    birth DATE,
    SSN VARCHAR(20),
    PhoneNumber VARCHAR(20),
    Address VARCHAR(100),
    Email VARCHAR(50),
    NickName VARCHAR(50),
    Password VARCHAR(100)
);
```

### 4. Insert Sample Data
```sql
INSERT INTO credential VALUES
(1, 'Samy', 'E001', 50000, '1990-01-01', '123-45-6789', '2101234567', 'Athens', 'samy@example.com', 'samy', 'hash1'),
(2, 'Alice', 'E002', 52000, '1992-03-10', '987-65-4321', '2107654321', 'Piraeus', 'alice@example.com', 'alice', 'fdbe918bdae83000aa54747fc95fe0470fff4976');
```

---

## Running the Laboratory Exercises
### 1. Basic Query Execution
```sql
SELECT * FROM credential WHERE Name='Samy';
```

### 2. SQL Injection Example (SELECT)
Malicious Input:
```sql
' OR 1=1 --
```
Resulting Query:
```sql
SELECT * FROM credential WHERE Name='' OR 1=1 --';
```
> Returns all records, demonstrating data leakage.

### 3. SQL Injection in UPDATE Statement
```sql
UPDATE credential SET Password='hacked_hash' WHERE ID=7 OR 1=1;
```
> Updates all users, demonstrating privilege escalation.

---

## Countermeasure Demonstration
Prepared Statements (Conceptual Example)
```sql
SELECT * FROM credential WHERE Name = ?;
```
- SQL logic is compiled separately
- User input is treated strictly as data
- Injection payloads are neutralized

---

## Troubleshooting
| Issue | Cause | Solution |
|-------|-------|---------|
| Cannot connect to MySQL | Service not running | `sudo systemctl start mysql` |
| Access denied for root | Auth plugin issue | Use `sudo mysql` |
| Queries fail | Wrong database | `USE Users;` |
| Injection not working | Input sanitized | Verify unsafe query logic |

---

## Open the Documentation
1. Navigate to the `docs/` directory
2. Open the report corresponding to your preferred language:
    - English: `SQL-Injection.pdf`
    - Greek: `Έγχυση-SQL.pdf`