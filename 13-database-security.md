# 🔒 Database Security

Database security is a critical aspect of system design, ensuring that sensitive data is protected from unauthorized access, breaches, and misuse. This chapter explores key concepts in database security, including authentication, authorization, encryption, and best practices for securing REST APIs.

---

## 📚 Table of Contents

1. [Introduction to Database Security](#introduction-to-database-security)
2. [Authentication and Authorization](#authentication-and-authorization)
3. [Database Encryption](#database-encryption)
4. [Securing REST APIs](#securing-rest-apis)
5. [MongoDB Wire Protocol and Wireshark Analysis](#mongodb-wire-protocol-and-wireshark-analysis)
6. [Best Practices for Database Security](#best-practices-for-database-security)
7. [Key Takeaways](#key-takeaways)
8. [Quiz Time! 🧠](#quiz-time-)

---

## 🏁 Introduction to Database Security

Database security involves implementing measures to protect data integrity, confidentiality, and availability. It encompasses strategies like user authentication, role-based access control, encryption, and auditing.

> **Analogy:** Think of database security as locking your house (authentication), giving keys only to trusted individuals (authorization), and installing alarms to detect intrusions (auditing).

---

## 🧑‍💼 Authentication and Authorization

### 🔑 Authentication
- Verifies the identity of users or applications attempting to access the database.
- **Example:** Using username and password or multi-factor authentication (MFA).

### 🛡️ Authorization
- Grants permissions to authenticated users based on their roles.
- **Example:** A `READ_ONLY` user can query data but cannot modify it.

**Code Example:**
```sql
-- Create a user with limited permissions
CREATE USER read_only_user WITH PASSWORD 'secure_password';
GRANT SELECT ON employees TO read_only_user;
```

> **Critical Insight:** Always follow the principle of least privilege—grant only the permissions necessary for a user's role.

---

## 🔐 Database Encryption

Encryption ensures that data is unreadable to unauthorized users, both at rest and in transit.

### Types of Encryption:
1. **At Rest:**
   - Encrypts data stored on disk.
   - **Example:** Transparent Data Encryption (TDE) in SQL Server.
2. **In Transit:**
   - Encrypts data transmitted between the client and server.
   - **Example:** Using SSL/TLS for database connections.

**Code Example:**
```bash
# Enable SSL for PostgreSQL
ssl = on
ssl_cert_file = '/path/to/server.crt'
ssl_key_file = '/path/to/server.key'
```

> **Tip:** Regularly rotate encryption keys and certificates to minimize risks.

---

## 🛡️ Securing REST APIs

REST APIs often interact with databases, making them a potential attack vector. Securing these APIs is crucial for protecting sensitive data.

### Best Practices:
1. **Use HTTPS:**
   - Ensure all API requests are encrypted using HTTPS.
2. **Validate Input:**
   - Prevent SQL injection by sanitizing user inputs.
3. **Rate Limiting:**
   - Limit the number of requests a user can make within a time window.
4. **Token-Based Authentication:**
   - Use tokens like JWT (JSON Web Tokens) for secure authentication.

**Example:**
```python
from flask import Flask, request, jsonify
from functools import wraps
import jwt

app = Flask(__name__)
SECRET_KEY = "your_secret_key"

def token_required(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        token = request.headers.get('Authorization')
        if not token:
            return jsonify({"message": "Token is missing!"}), 403
        try:
            jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
        except:
            return jsonify({"message": "Token is invalid!"}), 403
        return f(*args, **kwargs)
    return decorated

@app.route('/secure-data', methods=['GET'])
@token_required
def secure_data():
    return jsonify({"data": "This is secure data"})

if __name__ == '__main__':
    app.run(ssl_context='adhoc')
```

> **Analogy:** Securing an API is like setting up a guarded gate—only authorized vehicles (requests) with valid passes (tokens) are allowed entry.

---

## 🕵️ MongoDB Wire Protocol and Wireshark Analysis

The **MongoDB Wire Protocol** defines how clients communicate with MongoDB servers. Analyzing this communication using tools like Wireshark helps identify vulnerabilities.

### Example:
1. Capture packets using Wireshark.
2. Inspect the protocol to ensure sensitive data (e.g., queries, credentials) is encrypted.

**Key Observations:**
- Unencrypted connections expose queries and responses.
- Encrypted connections show only encrypted payloads.

> **Important:** Always use TLS/SSL for MongoDB connections to prevent eavesdropping.

---

## 🏆 Best Practices for Database Security

1. **Regular Audits:**
   - Monitor database activity to detect anomalies.
2. **Patch Management:**
   - Keep database software up-to-date to address vulnerabilities.
3. **Backup and Recovery:**
   - Maintain encrypted backups and test recovery procedures regularly.
4. **Network Segmentation:**
   - Isolate databases from public networks to reduce exposure.

---

## 🧠 Key Takeaways

- **Authentication** verifies user identity, while **authorization** grants appropriate permissions.
- **Encryption** protects data at rest and in transit.
- Securing **REST APIs** involves HTTPS, input validation, rate limiting, and token-based authentication.
- Analyzing protocols like MongoDB's Wire Protocol helps identify security gaps.
- Follow best practices like regular audits, patch management, and network segmentation.

---

## Quiz Time! 🧠

1. **What is the difference between authentication and authorization?**
2. **Why is it important to encrypt data both at rest and in transit?**
3. **List two best practices for securing REST APIs.**

---

[⬅️ Previous: Cursors](12-database-cursors.md) | [⬅️ Back to Course Index](README.md) | [➡️ Next: Homomorphic Encryption](14-homomorphic-encryption.md)