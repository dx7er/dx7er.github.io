![htb](../assets/htb.png)

# MSSQL Enumeration Writeup - HTB Academy

**Target:** 10.129.230.249 (Hostname: `ILF-SQL-01`)
**Scope:** HTB Academy Lab (Authorized environment)
**Author:** dx73r

---

# Executive Summary

During this authorized HTB Academy exercise, enumeration was performed against a Microsoft SQL Server 2019 instance hosted on the target machine. The goal was to identify the SQL hostname, authenticate using the provided credentials, enumerate available databases, and identify the non-default database.

**Key Findings:**

* SQL Server identified: **Microsoft SQL Server 2019 RTM**.
* Hostname discovered: **ILF-SQL-01**.
* Successful authentication using provided credentials: **backdoor : Password1**.
* One non-default database discovered: **Employees**.

---

# Tools Used

* **nmap** – service discovery & SQL detection
* **impacket-mssqlclient** – interactive MSSQL authentication & enumeration

---

# Methodology

## **1. Service Discovery**

The first step was to identify open ports and SQL services using Nmap:

```bash
nmap --script ms-sql-info 10.129.230.249
```

### **Result:**

* **1433/tcp** → Microsoft SQL Server 2019 RTM
* **Instance Name:** MSSQLSERVER
* **Hostname:** `ILF-SQL-01`

The hostname answered **Question 1**.

---

## **2. Connecting to SQL Server**

Valid credentials were provided for direct MSSQL login:

```bash
python3 /usr/local/bin/mssqlclient.py backdoor@10.129.230.249 -windows-auth
```

Successful login output:

```
[!] Press help for extra shell commands
SQL (ILF-SQL-01ackdoor dbo@master)>
```

---

## **3. Basic SQL Enumeration**

### Check SQL version:

```sql
select @@version;
```

### Check current authenticated user:

```sql
select system_user;
```

Output confirmed the login:

```
ILF-SQL-01ackdoor
```

---

## **4. Enumerating Databases**

To list all databases:

```sql
select name from sys.databases;
```

### **Result:**

```
master
tempdb
model
msdb
Employees
```

The first four entries are default system databases.
The only **non-default database** is:

### 🎯 **Employees**

This answered **Question 2**.

---

# Confirmed Answers

| Question             | Answer         |
| -------------------- | -------------- |
| MSSQL Hostname       | **ILF-SQL-01** |
| Non-default Database | **Employees**  |

---

# Impact & Risk

If similar misconfigurations appeared in a production system, weak or default credentials could allow unauthorized database access. This could expose sensitive data and allow attackers to enumerate or modify internal databases.

---

# Remediation Recommendations

* Enforce strong authentication for MSSQL services.
* Restrict MSSQL exposure to internal network segments.
* Implement network firewalls to limit port 1433 access.
* Regularly audit service accounts and database permissions.


##### A supporter is worth a thousand followers! [Buy Me a Coffee](https://www.buymeacoffee.com/dx73r). If you like this blog, follow me on [GitHub](https://github.com/dx7er) and [LinkedIn](https://www.linkedin.com/in/naqvio7/). 
