**SQL Injection (SQLi) - Notes**

- **Definition**: A web security vulnerability where an attacker manipulates SQL queries made by an application to its database.
    
- **Consequences**:
    
    - **Unauthorized data access**: Attackers can retrieve data not intended for them (e.g., other users’ data).
        
    - **Data modification/deletion**: Attackers may alter or remove data, causing lasting changes to the application.
        
    - **Escalation risks**:
        
        - **Server compromise**: Attack can extend to the underlying server or infrastructure.
            
        - **Denial-of-service (DoS)**: Attackers might disrupt services through SQLi.
            
- **Impact**: Can affect application content, behavior, and back-end systems.
    

---

### **Detecting SQL Injection Vulnerabilities – Notes**

#### **Manual Detection Techniques**

- **Single quote test (`'`)**: Submit `'` and observe for errors or anomalies in the response.
    
- **SQL syntax manipulation**: Use syntax that:
    
    - Evaluates to the original value.
        
    - Evaluates to a different value.
        
    - Observe for systematic response differences.
        
- **Boolean conditions**: Submit payloads like:
    
    - `OR 1=1` (always true)
        
    - `OR 1=2` (always false)
        
    - Compare responses for inconsistencies.
        
- **Time delay payloads**: Inject commands that cause a delay (e.g., `SLEEP(5)`), and check for slower response times.
    
- **Out-of-band (OAST) payloads**: Use payloads that trigger external network interactions; monitor for those interactions.
    

#### **Automated Detection**

- **Burp Scanner**: Can detect most SQL injection vulnerabilities quickly and reliably.
    

---

### **SQL Injection Locations in Queries**

While **most SQLi attacks target the `WHERE` clause in `SELECT` statements**, they can also appear in other query parts:

- **`UPDATE` statements**:
    
    - In the `SET` values.
        
    - In the `WHERE` clause.
        
- **`INSERT` statements**:
    
    - In the values being inserted.
        
- **`SELECT` statements**:
    
    - In table or column names.
        
    - In the `ORDER BY` clause.
        

---

### **Retrieving Hidden Data – Notes**

#### **Scenario Overview**

- A shopping app shows products by category via a URL like:
    
    arduino
    
    CopyEdit
    
    `https://insecure-website.com/products?category=Gifts`
    
- This leads to a SQL query like:
    
    sql
    
    CopyEdit
    
    `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`
    
- Purpose:
    
    - Fetch all details from the `products` table.
        
    - Only for items in the `Gifts` category.
        
    - Only those where `released = 1` (hiding unreleased products).
        

#### **Exploiting the Query – Basic SQL Injection**

- **No SQLi protections in place.**
    
- An attacker can inject:
    
    arduino
    
    CopyEdit
    
    `https://insecure-website.com/products?category=Gifts'--`
    
- Which leads to:
    
    sql
    
    CopyEdit
    
    `SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1`
    
- **Effect**: The `--` comments out the rest of the query (`AND released = 1`), showing _all_ products, even unreleased ones.
    

#### **Broadening the Attack – Accessing All Products**

- Injecting:
    
    arduino
    
    CopyEdit
    
    `https://insecure-website.com/products?category=Gifts'+OR+1=1--`
    
- Results in:
    
    sql
    
    CopyEdit
    
    `SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1`
    
- **Effect**:
    
    - `OR 1=1` always evaluates to true.
        
    - Returns **all products**, from any category and release state.
        

#### **Important Warning**

- **Caution with `OR 1=1`**:
    
    - While it may appear safe, if reused in a **`DELETE` or `UPDATE`** statement, it could modify or remove _all records_.
        
    - SQL injection can have unintended and destructive side effects.
        

---

### **Subverting Application Logic – Notes**

#### **Login Mechanism (Normal Behavior)**

- Application checks login credentials via SQL query:
    
    sql
    
    CopyEdit
    
    `SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`
    
- If a match is found, the user is authenticated.
    
- If not, access is denied.
    

#### **SQL Injection Exploit**

- **Goal**: Bypass password verification and log in as any user.
    
- **Attack Input**:
    
    - Username: `administrator'--`
        
    - Password: _(left blank)_
        
- **Resulting Query**:
    
    sql
    
    CopyEdit
    
    `SELECT * FROM users WHERE username = 'administrator'--' AND password = ''`
    
- **Effect**:
    
    - The `--` sequence starts a SQL comment.
        
    - The rest of the query (`AND password = ''`) is ignored.
        
    - Only `username = 'administrator'` is evaluated.
        
- **Outcome**: The attacker is logged in as the **administrator** without knowing the password.
    

#### **Key Insight**

- SQL comments (`--`) can be used to **strip off security checks** in queries.
    
- This type of injection **subverts the application’s logic**, giving unauthorized access.
    

---

### **SQL Injection UNION Attacks – Notes**

#### **What is a UNION Attack?**

- A **SQL injection UNION attack** exploits vulnerabilities where:
    
    - An application is vulnerable to SQLi.
        
    - Query results are reflected in the application's response.
        
- **Goal**: Retrieve data from **other tables** in the database using the `UNION` SQL keyword.
    

#### **How UNION Works**

- `UNION` allows combining results from multiple `SELECT` queries into a single result set.
    
- Example:
    
    sql
    
    CopyEdit
    
    `SELECT a, b FROM table1 UNION SELECT c, d FROM table2`
    
- **Effect**: Returns a result set with two columns:  
    `a & b` from `table1`, and `c & d` from `table2`.
    

---

### **Requirements for a UNION Query to Work**

1. **Same number of columns**:
    
    - Each `SELECT` must return **the same number of columns**.
        
2. **Compatible data types**:
    
    - Corresponding columns in each `SELECT` must have **compatible data types** (e.g., both strings or both numbers).
        

---

### **Steps to Perform a UNION SQLi Attack**

1. **Determine the number of columns** in the original query.
    
2. **Identify which columns** in the original query:
    
    - Are **reflected in the response**.
        
    - Can **accept the data types** you plan to inject.
        
3. **Construct a `UNION SELECT`** query that:
    
    - Matches the number of columns.
        
    - Uses compatible data types.
        
    - Extracts data from target tables (e.g., `users`, `credentials`).
        

---

# 🎯 Finding Columns with a Useful Data Type (SQL Injection)

A **SQL injection UNION attack** can be used to extract results from an injected query.

## 🧠 Key Goal
Find **columns that accept string data**, so you can retrieve useful information like usernames, emails, etc.

---

## 📌 Why This Matters

- Most valuable data is stored as **strings** (e.g., `'admin'`, `'password'`, `'user@example.com'`).
- The injected data must be placed into a column that can **accept string data types** (like `VARCHAR`).

---

## 🧪 Testing Column Data Types

After finding the **number of columns** returned by the original query, test each one to see if it accepts strings.

### 🔁 Example with 4 Columns:

Submit a series of payloads like:

    ' UNION SELECT 'a', NULL, NULL, NULL--
    ' UNION SELECT NULL, 'a', NULL, NULL--
    ' UNION SELECT NULL, NULL, 'a', NULL--
    ' UNION SELECT NULL, NULL, NULL, 'a'--

- Place `'a'` in one column at a time.
- Use `NULL` in the others to avoid type conflicts.

---

## 🚨 Error to Watch For

If the column cannot store strings, the database may return an error like:

> Conversion failed when converting the varchar value 'a' to data type int.

This means the column expects a **non-string type** like `INT`.

---

## ✅ Success Indicator

If:
- No error occurs
- The response includes the string `'a'`

Then the column is **compatible with strings** and can be used to extract data.

---

## 📝 Summary

| Test                          | What to Look For           |
|-------------------------------|-----------------------------|
| Inject `'a'` into each column | Response without errors     |
| Response shows `'a'`          | ✅ Column accepts strings    |
| Error like type conversion    | ❌ Column doesn't support strings |

# 🛠️ Using a SQL Injection UNION Attack to Retrieve Interesting Data

Once you have:

- Determined the **number of columns** returned by the original query
- Identified **which columns accept string data**

You’re ready to start retrieving real data from the database.

---

## 📋 Scenario Setup

Suppose the following is true:

- The original query returns **2 columns**, both of which support string data
- The **injection point** is inside a quoted string in the `WHERE` clause
- The database contains a table called `users` with the columns:
  - `username`
  - `password`

---

## 🧪 Injection Payload Example

To extract data from the `users` table, you would submit:

    ' UNION SELECT username, password FROM users--

This UNION SELECT query replaces the original query’s result with:

- Data from the `username` and `password` columns
- From the `users` table

---

## 📌 Important Considerations

To run this query, you must know:

- The **name of the table** (`users`)
- The **names of the columns** (`username`, `password`)

Without this knowledge, you'd have to **guess** — which is inefficient.

---

## 🧭 Finding Table and Column Names

All modern relational databases allow you to **enumerate the schema** (database structure) to discover:

- What **tables** exist
- What **columns** are in each table

You can use this information to refine your injection and target valuable data.

---

## ✅ Summary

| Step | Action |
|------|--------|
| 1. Confirm column count & types | Make sure string data can be inserted |
| 2. Identify target table/columns | E.g., `users`, `username`, `password` |
| 3. Construct UNION SELECT | Pull data from real table |
| 4. Refine based on schema info | If unknown, enumerate the structure |
# Examining the Database in SQL Injection Attacks

To exploit SQL injection vulnerabilities, it's often necessary to gather information about the **database** itself. This includes:

- The **type and version** of the database software.
- The **tables and columns** that the database contains.

---

## Why This Matters

Knowing these details allows attackers to:

- Tailor payloads to the specific SQL dialect (e.g., Oracle, MySQL, SQL Server).
- Identify sensitive tables (like `users`, `accounts`, `payments`) to target for data extraction.
- Understand how to structure queries that retrieve useful information.

---

## Techniques

While this section doesn’t include specific code, typical methods include:

- Using `UNION SELECT` to infer table structure.
- Querying system tables (like `information_schema.tables` in MySQL or `all_tables` in Oracle).
- Identifying database version using functions like `@@version` (SQL Server) or `SELECT version()` (PostgreSQL).

> Always test payloads carefully and be aware of database-specific syntax and behaviors.

---

## Summary

To effectively exploit SQL injection:

- Detect the **DBMS type/version**.
- Enumerate **tables and columns**.
- Craft tailored injection payloads accordingly.

# Querying the Database Type and Version

You can potentially identify both the **database type and version** by injecting provider-specific queries to see which ones work.

---

## Common Queries to Determine Database Version

| Database Type      | Query                         |
|--------------------|-------------------------------|
| Microsoft, MySQL   | `SELECT @@version`            |
| Oracle             | `SELECT * FROM v$version`     |
| PostgreSQL         | `SELECT version()`            |

---

## Example: UNION-Based Injection

A possible injection payload:

```sql
' UNION SELECT @@version--
If successful, this might return version information. For example:

pgsql
Copy
Edit
Microsoft SQL Server 2016 (SP2) (KB4052908) - 13.0.5026.0 (X64)
Mar 18 2018 09:11:49
Copyright (c) Microsoft Corporation
Standard Edition (64-bit) on Windows Server 2016 Standard 10.0 <X64> (Build 14393: ) (Hypervisor)
This confirms that the database is Microsoft SQL Server, and reveals detailed version and system information.
```

# Listing the Contents of the Database

Most database types (except Oracle) have a set of views called the **information schema**. This schema provides metadata about the database.

---

## Listing Tables

You can query `information_schema.tables` to list all the tables in the database:

```sql
SELECT * FROM information_schema.tables
Sample Output
TABLE_CATALOG	TABLE_SCHEMA	TABLE_NAME	TABLE_TYPE
MyDatabase	dbo	Products	BASE TABLE
MyDatabase	dbo	Users	BASE TABLE
MyDatabase	dbo	Feedback	BASE TABLE

This output indicates that the database contains three tables: Products, Users, and Feedback.

Listing Columns in a Specific Table
To list columns of a specific table (e.g., Users), query information_schema.columns with a filter on the table name:

sql
Copy
Edit
SELECT * FROM information_schema.columns WHERE table_name = 'Users'
Sample Output
TABLE_CATALOG	TABLE_SCHEMA	TABLE_NAME	COLUMN_NAME	DATA_TYPE
MyDatabase	dbo	Users	UserId	int
MyDatabase	dbo	Users	Username	varchar
MyDatabase	dbo	Users	Password	varchar

```

# What is Blind SQL Injection?

**Blind SQL injection** occurs when an application is vulnerable to SQL injection, but its HTTP responses:

- Do **not** contain the results of the injected SQL query.
- Do **not** reveal details of any database errors.

---

## Key Points

- Common techniques like **UNION attacks** are often ineffective against blind SQL injection because they rely on seeing query results in the application's response.
- Despite this limitation, blind SQL injection can still be exploited to access unauthorized data.
- Exploiting blind SQL injection requires **different techniques** that do not depend on direct feedback from the server.

---
# Exploiting Blind SQL Injection by Triggering Conditional Responses

Consider an application that uses tracking cookies for analytics. Requests include a cookie header like:

Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4

yaml
Copy
Edit

---

## Vulnerable Query Example

The application runs a SQL query to check if the `TrackingId` is known:

```sql
SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'
This query is vulnerable to SQL injection.

However, query results are not returned to the user.

Application Behavior
If the TrackingId is recognized (query returns data), the user receives a "Welcome back" message.

If not recognized (no data returned), the response differs.

Exploiting the Vulnerability
Although direct query results are not shown, the difference in application responses can be used to infer data.

By injecting conditional statements into the TrackingId, you can trigger different responses.

This technique allows information retrieval through blind SQL injection by observing the application's behavior.

```

