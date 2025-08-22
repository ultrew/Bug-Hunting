# 🔐 IDOR (Insecure Direct Object Reference) – Complete Bug Bounty Guide

IDOR is one of the most common and impactful vulnerabilities in bug bounty programs. This guide covers **definition, impacts, workflow, dorks, and best practices** for hunting IDORs.  

---

## 📖 What is IDOR?
**Insecure Direct Object Reference (IDOR)** occurs when an application exposes internal object references (IDs, keys, file names, etc.) without proper access control. Attackers can manipulate these references to access or modify unauthorized data.

Example:
```
https://example.com/order?id=12345
```
If changing `12345` to `12346` exposes another user’s order → that’s an IDOR.

---

## 🚨 Impacts of IDOR Vulnerabilities

### 📂 Information Disclosure
- Accessing other users’ personal data (PII)
- Viewing invoices, orders, receipts
- Accessing HR/student/employee records
- Downloading confidential docs (PDFs, reports, contracts)
- Leaking emails, phone numbers, addresses

### ✍️ Data Modification
- Editing another user’s profile
- Changing account details (email, phone)
- Modifying orders, payroll, or financial data
- Manipulating support tickets or case notes

### ❌ Unauthorized Actions
- Resetting another user’s password
- Cancelling or altering another user’s order
- Deleting files or records
- Escalating privileges via `role` or `account_id` changes
- Approving or denying workflow items

### 💸 Financial Impact
- Unauthorized refunds or transactions
- Accessing payment details
- Stealing loyalty points or vouchers
- Downloading invoices to commit fraud
- Manipulating shopping cart or discounts

### 🔓 Authentication & Access Control Bypass
- Logging in as another user via predictable IDs
- Accessing admin-only endpoints
- Switching accounts without valid credentials
- Taking over accounts in multi-tenant apps

### 🏴‍☠️ Severe Exploits
- Mass data scraping via sequential IDs
- Business logic abuse (e.g., free services by tampering IDs)
- Insider abuse of predictable IDs
- Regulatory violations (GDPR, HIPAA, PCI DSS)

---

## 🛠️ IDOR Testing Workflow (Bug Bounty Checklist)

### 🔍 Recon & Identification
- [ ] Look for numeric or sequential IDs in URLs
- [ ] Check common parameters (`user_id`, `order_id`, `file_id`, etc.)
- [ ] Explore APIs (`/api/v1/`, `/rest/`, `/graphql`)
- [ ] Identify file download/upload endpoints
- [ ] Map user roles (guest, user, admin)

### 🧪 Testing with Multiple Accounts
- [ ] Create **two accounts** (Account A & Account B)
- [ ] Perform an action with Account A
- [ ] Replay request as Account B, replace `id` with Account A’s value
- [ ] If successful → possible IDOR

### 🛠️ Parameter Manipulation
- [ ] Change query parameters (`?id=123 → ?id=124`)
- [ ] Modify POST/PUT body data (`"user_id": 123 → 124`)
- [ ] Alter hidden form fields
- [ ] Test JSON/XML request bodies
- [ ] Try path manipulation (`/profile/123 → /profile/124`)

### 🔄 Access Control Verification
- [ ] Test read, write, delete permissions
- [ ] Check if normal users can hit admin endpoints
- [ ] Attempt role escalation (`role=admin`, `is_staff=true`)

### ⚡ Advanced Testing
- [ ] Use Burp Repeater for request replay
- [ ] Use Burp Intruder / ffuf for fuzzing sequential IDs
- [ ] Automate enumeration (`1001, 1002, 1003...`)
- [ ] Test UUIDs/hashes (sometimes predictable)
- [ ] Inspect GraphQL queries for object references

### 📌 Impact Demonstration
- [ ] Show how Account B accesses/modifies Account A’s data
- [ ] Provide before/after screenshots
- [ ] Highlight sensitive data (PII, orders, files)
- [ ] Assess business impact (data leak, account takeover, fraud)

### 📝 Reporting Best Practices
- [ ] Include vulnerable endpoint
- [ ] Provide step-by-step reproduction
- [ ] Attach sample requests/responses
- [ ] Use separate test accounts
- [ ] Suggest fix: **Implement server-side access control checks**

---

## 🔍 Google Dorks for Finding IDOR Endpoints

### 🎯 General Object IDs
```
inurl:"id=" site:target.com
inurl:"uid=" site:target.com
inurl:"userid=" site:target.com
inurl:"user_id=" site:target.com
inurl:"account_id=" site:target.com
inurl:"profile_id=" site:target.com
inurl:"customer_id=" site:target.com
```

### 📦 Orders / Payments / Transactions
```
inurl:"order_id=" site:target.com
inurl:"invoice=" site:target.com
inurl:"transaction_id=" site:target.com
inurl:"payment_id=" site:target.com
inurl:"receipt_id=" site:target.com
```

### 📂 Files / Documents
```
inurl:"doc=" site:target.com
inurl:"document_id=" site:target.com
inurl:"file_id=" site:target.com
inurl:"download?id=" site:target.com
inurl:"attachment_id=" site:target.com
inurl:"report_id=" site:target.com
```

### 🎟️ Tickets / Support Systems
```
inurl:"ticket_id=" site:target.com
inurl:"case_id=" site:target.com
inurl:"support_id=" site:target.com
inurl:"request_id=" site:target.com
```

### 🏛️ Educational / Internal Systems
```
inurl:"student_id=" site:target.com
inurl:"rollno=" site:target.com
inurl:"regno=" site:target.com
inurl:"emp_id=" site:target.com
inurl:"staff_id=" site:target.com
```

### ⚙️ API Endpoints
```
inurl:"/api/" site:target.com
inurl:"/api/v1/" site:target.com
inurl:"/rest/" site:target.com
inurl:"/graphql" site:target.com
```

### 🧭 Misc Parameters
```
inurl:"ref_id=" site:target.com
inurl:"cid=" site:target.com
inurl:"pid=" site:target.com
inurl:"gid=" site:target.com
inurl:"project_id=" site:target.com
inurl:"message_id=" site:target.com
```

---

# ✅ Summary
- **Google Dorks** help discover potential IDOR-prone endpoints.  
- **Workflow** ensures structured testing with multiple accounts & parameter manipulation.  
- **Impacts** range from minor leaks to account takeover and financial fraud.  
- Always test **responsibly within bug bounty scope** and demonstrate **real-world impact** in reports.
