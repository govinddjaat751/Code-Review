# Secure Coding Review for Flask Application

This repository contains a simple Flask application reviewed and updated to follow secure coding practices. Below, you'll find details about the original vulnerabilities and the solutions implemented to enhance security.

---

## **Identified Security Vulnerabilities**

1. **SQL Injection**  
   - The application used string interpolation in SQL queries, making it vulnerable to SQL injection attacks.

2. **Cross-Site Scripting (XSS)**  
   - User inputs were rendered in templates without sanitization, allowing potential XSS exploits.

3. **Sensitive Data Exposure**  
   - Passwords were stored in plaintext, posing a significant security risk in case of database compromise.

4. **Debug Mode Enabled**  
   - The Flask app ran in debug mode, which could leak sensitive information in a production environment.

---

## **Secure Coding Practices Applied**

### **1. Prevent SQL Injection**
Replaced unsafe SQL queries with parameterized queries to ensure proper handling of user input:

```python
cursor.execute("SELECT password FROM users WHERE username=?", (username,))
```

### **2. Prevent Cross-Site Scripting (XSS)**
Escaped user input using `markupsafe.escape` before rendering it in templates:

```python
from markupsafe import escape
...
return render_template_string(template, name=escape(name))
```

### **3. Hash Passwords**
Implemented `generate_password_hash` and `check_password_hash` from Werkzeug for secure password storage and verification:

```python
from werkzeug.security import generate_password_hash, check_password_hash
...
hashed_password = generate_password_hash(password)
...
if stored_password and check_password_hash(stored_password[0], password):
    return "Login successful!"
```

### **4. Disable Debug Mode**
Set `debug=False` in the production environment to prevent sensitive information leakage:

```python
if __name__ == '__main__':
    app.run(debug=False)
```

---

## **Usage Instructions**

1. Clone the repository:
   ```bash
   git clone <repository-url>
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Set up the SQLite database:
   - Create a database `example.db`.
   - Add a `users` table with columns for `username` and `password`.

4. Run the Flask application:
   ```bash
   python app.py
   ```

5. Access the application in your browser at `http://127.0.0.1:5000`.

---

## **Key Learnings**

- Always use parameterized queries to prevent SQL injection.
- Sanitize user input to mitigate XSS risks.
- Hash passwords to ensure secure storage.
- Disable debug mode in production to avoid leaking sensitive server details.

---

## **Contributing**

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Submit a pull request with a detailed description of your changes.

---

## **License**

This project is licensed under the MIT License. See the LICENSE file for details.

---
