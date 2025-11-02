![htb](../assets/htb.png)
# MySQL Enumeration Writeup – HTB Academy

## Overview

This exercise focuses on enumerating a MySQL service running on the target machine `10.129.3.64`. The objective was to identify the service version, exploit weak credentials, and retrieve sensitive information from the database.


## Step 1: Initial Nmap Scan

**Command:**

```bash
nmap -sC -sV -p3306 10.129.3.64
```

**Purpose:**
To detect the MySQL service, its version, and potential accessible information.

**Result:**

```
PORT     STATE SERVICE VERSION
3306/tcp open  mysql   MySQL (Too many connections)
```

**Observation:**
The port **3306** is open, confirming the presence of a MySQL database service.


## Step 2: Detailed Nmap Service Enumeration

**Command:**

```bash
nmap -sC -sV -p3306 10.129.3.64
```

**Purpose:**
To gather detailed version information and MySQL capabilities.

**Result:**

```
PORT     STATE SERVICE VERSION
3306/tcp open  mysql   MySQL 8.0.27-0ubuntu0.20.04.1 (Ubuntu)
| mysql-info:
|   Protocol: 10
|   Version: 8.0.27-0ubuntu0.20.04.1
|   Capabilities: ODBCClient, SupportsTransactions, Speaks41ProtocolNew, ...
|   Auth Plugin Name: caching_sha2_password
```

**Finding:**
The MySQL version in use is **MySQL 8.0.27**.



## Step 3: Logging into MySQL with Known Credentials

**Command:**

```bash
mysql -u robin -probin -h 10.129.3.64
```

**Purpose:**
To authenticate using the weak credentials found during reconnaissance.

**Result:**

```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 550
Server version: 8.0.27-0ubuntu0.20.04.1 (Ubuntu)
```

Connection was successful, granting full access to the database.



## Step 4: Enumerating Databases

**Command:**

```sql
SHOW DATABASES;
```

**Result:**

```
+--------------------+
| Database           |
+--------------------+
| customers          |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

**Finding:**
A database named **`customers`** is available for exploration.



## Step 5: Exploring Tables in the Customers Database

**Commands:**

```sql
USE customers;
SHOW TABLES;
```

**Result:**

```
+------------------+
| Tables_in_customers |
+------------------+
| myTable          |
+------------------+
```


## Step 6: Examining Table Structure

**Command:**

```sql
SHOW COLUMNS FROM myTable;
```

**Result:**

```
+-----------+---------------------+------+-----+---------+----------------+
| Field     | Type                | Null | Key | Default | Extra          |
+-----------+---------------------+------+-----+---------+----------------+
| id        | mediumint unsigned  | NO   | PRI | NULL    | auto_increment |
| name      | varchar(255)        | YES  |     | NULL    |                |
| email     | varchar(255)        | YES  |     | NULL    |                |
| country   | varchar(100)        | YES  |     | NULL    |                |
| postalZip | varchar(20)         | YES  |     | NULL    |                |
| city      | varchar(255)        | YES  |     | NULL    |                |
| address   | varchar(255)        | YES  |     | NULL    |                |
| pan       | varchar(255)        | YES  |     | NULL    |                |
| cvv       | varchar(255)        | YES  |     | NULL    |                |
+-----------+---------------------+------+-----+---------+----------------+
```


## Step 7: Extracting Email Data

**Command:**

```sql
SELECT email FROM myTable;
```

**Result:**

```
+----------------------------+
| email                      |
+----------------------------+
| diam.eu@icloud.htb         |
| tellus.id@google.htb       |
| lobortis@outlook.htb       |
| donec@protonmail.htb       |
| suspendisse.aliquet@yahoo.htb |
| neque@aol.htb              |
| eget@google.htb            |
| non@icloud.htb             |
| metus@hotmail.htb          |
+----------------------------+
```


## Step 8: Extracting Specific User Information

**Command:**

```sql
SELECT name, id, email FROM myTable;
```

**Purpose:**
To identify specific user details and verify database content.

**Result:**
Includes multiple entries such as:

```
| Otto Lang | 88 | ultrices@google.htb |
```

**Finding:**
Email address for **Otto Lang** is **`ultrices@google.htb`**.



## Step 9: Final Answers Summary

| Question                       | Answer                                            |
| ------------------------------ | ------------------------------------------------- |
| **MySQL Version**              | MySQL 8.0.27                                      |
| **Customer 'Otto Lang' Email** | [ultrices@google.htb](mailto:ultrices@google.htb) |




## Conclusion

This exercise demonstrates MySQL enumeration and exploitation through weak credentials. By using simple reconnaissance and SQL queries, sensitive customer data was retrieved.

**Key Takeaways:**

* Always disable remote MySQL access when unnecessary.
* Enforce strong credentials and limit user privileges.
* Regularly audit and sanitize database contents.

---

##### A supporter is worth a thousand followers! [Buy Me a Coffee](https://www.buymeacoffee.com/dx73r). If you like this blog, follow me on [GitHub](https://github.com/dx7er) and [LinkedIn](https://www.linkedin.com/in/naqvio7/). 
