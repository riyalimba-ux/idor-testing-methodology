# 🔐 IDOR Testing Methodology

**IDOR (Insecure Direct Object Reference)** is an access-control vulnerability that occurs when an application exposes a reference to an internal object—such as a user ID, invoice, document, order, or account—and fails to properly verify whether the requesting user is authorized to access that object.

IDOR is commonly associated with **Broken Access Control** and can lead to unauthorized access to sensitive information or actions.

> ⚠️ Test only applications and systems where you have explicit permission.

---

## 🧠 What Does IDOR Look Like?

Imagine an application makes this request:

```http
GET /api/profile/1001
```

The application returns information belonging to user `1001`.

During an authorized security assessment, you test whether changing the object reference to another authorized test account changes the response:

```http
GET /api/profile/1002
```

If the application returns another user's information without verifying authorization, this may indicate an access-control flaw.

The important question isn't:

> "Can I change the ID?"

It's:

> **"Does the server verify that I am authorized to access the object represented by that ID?"**

---

# 🔎 IDOR Testing Methodology

## 1️⃣ Map the Application

Start by understanding how the application handles objects.

Look for:

* User IDs
* Account IDs
* Order IDs
* Invoice IDs
* Document IDs
* File IDs
* Message IDs
* Project IDs
* API resource identifiers

Example:

```text
/profile/123
/order/456
/document/789
/api/users/123
/api/invoices/456
```

---

## 2️⃣ Capture Requests

Use an HTTP proxy such as **Burp Suite** during an authorized assessment.

Look for parameters such as:

```http
GET /api/orders/123
```

or:

```http
GET /download?file_id=123
```

or:

```http
POST /api/users/update
Content-Type: application/json

{
    "user_id": 123
}
```

Record which parameters appear to reference server-side objects.

---

## 3️⃣ Identify the Authorization Boundary

Create two test accounts when the application permits it:

```text
Account A → Object A
Account B → Object B
```

For example:

```text
User A → Order 1001
User B → Order 1002
```

The goal is to determine whether User A can access User B's object.

---

## 4️⃣ Change the Object Reference

With permission, modify the identifier:

```http
GET /api/orders/1001
```

to:

```http
GET /api/orders/1002
```

Then compare the response.

Check:

* HTTP status code
* Response body
* Returned object
* Error messages
* Redirect behavior
* Response length
* Sensitive information

---

## 5️⃣ Test Different Request Types

Don't restrict testing to GET requests.

Check how authorization behaves across:

```text
GET
POST
PUT
PATCH
DELETE
```

For example:

```http
PATCH /api/profile/1002
```

may require stronger authorization than simply viewing a resource.

---

# 🔄 Test More Than Numeric IDs

IDOR isn't limited to simple numbers.

Applications may use:

### Numeric IDs

```text
1001
1002
1003
```

### UUIDs

```text
550e8400-e29b-41d4-a716-446655440000
```

### Filenames

```text
invoice_1001.pdf
```

### Slugs

```text
/api/projects/project-alpha
```

### Encoded identifiers

```text
/base64_encoded_value
```

The underlying issue is still the same:

**The server must enforce authorization regardless of how the object is referenced.**

---

# 🧪 Important IDOR Test Cases

| Test                   | What to Check                                       |
| ---------------------- | --------------------------------------------------- |
| Object ID modification | Can another user's object be accessed?              |
| Horizontal access      | Can User A access User B's resources?               |
| Vertical access        | Can a lower-privileged user access admin resources? |
| Read access            | Can unauthorized data be viewed?                    |
| Write access           | Can unauthorized data be modified?                  |
| Delete access          | Can unauthorized objects be deleted?                |
| API endpoints          | Does authorization work consistently across APIs?   |
| File access            | Can another user's files be retrieved?              |

---

# 👥 Horizontal vs Vertical Access

### Horizontal Privilege Escalation

One normal user accesses another normal user's resource.

```text
User A
   ↓
User B's Resource
```

### Vertical Privilege Escalation

A lower-privileged user accesses functionality intended for a higher-privileged role.

```text
Normal User
     ↓
Admin Resource
```

Both situations should be tested during an authorized assessment.

---

# 🚨 What Makes an IDOR Vulnerability Serious?

Impact depends on what the exposed object represents.

Potential consequences can include:

* Unauthorized information disclosure
* Exposure of personal data
* Unauthorized modification
* Unauthorized deletion
* Access to private documents
* Account-related data exposure
* Business data exposure

The severity should be assessed based on the actual impact and application context—not simply because an identifier is changeable.

---

# 🛡️ How Developers Can Prevent IDOR

The primary defense is **server-side authorization**.

Never rely solely on:

```text
Hidden fields
Frontend validation
Obfuscated IDs
JavaScript checks
Client-side restrictions
```

Instead, verify authorization on every sensitive request.

Conceptually:

```python
if resource.owner_id != current_user.id:
    return unauthorized
```

For more complex applications, use a centralized authorization model such as:

```text
Authentication
      ↓
Authorization
      ↓
Resource Access
```

---

# 📝 IDOR Reporting Structure

If you discover an access-control issue during an authorized assessment, a useful report can contain:

### Title

```text
Broken Object Level Authorization Allows Unauthorized Access to Order Records
```

### Summary

Explain the affected functionality and authorization failure.

### Steps to Reproduce

1. Log in as the first authorized test account.
2. Access the relevant resource.
3. Capture the request.
4. Modify the object reference to another authorized test object's identifier.
5. Observe the server response.

### Expected Result

```text
The server should deny access.
```

### Actual Result

```text
The server returns the requested resource.
```

### Impact

Describe exactly what information or functionality becomes accessible.

### Remediation

Recommend enforcing server-side object-level authorization for every request.

---

# 💡 Key Takeaways

```text
IDOR ≠ Just Changing an ID

IDOR =
Object Reference
      +
Missing/Incorrect Authorization
      +
Unauthorized Access
```

When testing for IDOR, don't focus only on changing numbers.

Focus on the **authorization logic behind the object**.

Ask:

> **"Should this user actually be allowed to access this resource?"**

That's the heart of IDOR testing.

---

## 🔗 Related Topics

* OWASP Top 10 — Broken Access Control
* API Security
* Authentication vs Authorization
* Privilege Escalation
* Web Application Security
* Bug Bounty Methodology
* Burp Suite

---

### ⚠️ Ethical Testing Notice

All techniques discussed here should be used only against applications, APIs, labs, or systems where you have explicit authorization to perform security testing.

#Cybersecurity #WebSecurity #IDOR #BugBounty #AppSec #Pentesting #EthicalHacking #APISecurity #BurpSuite #OWASP #BugBountyTips
