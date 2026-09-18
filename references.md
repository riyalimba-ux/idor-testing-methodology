# 📚 References & Further Reading

This page contains resources used for learning more about IDOR, broken access control, API security, and web application security.

---

## OWASP

### OWASP Top 10 — Broken Access Control

Broken Access Control is one of the major categories of web application security risks and includes authorization failures that can lead to unauthorized access.

* OWASP Top 10
* OWASP Broken Access Control
* OWASP Web Security Testing Guide

---

## OWASP API Security

API authorization issues can occur when applications fail to properly verify access to individual objects.

Recommended topics:

* Broken Object Level Authorization (BOLA)
* Broken Function Level Authorization
* API authorization testing
* API access control

---

## PortSwigger Web Security Academy

PortSwigger Web Security Academy provides interactive web security labs covering access control and related vulnerabilities.

Recommended topics:

* Access control vulnerabilities
* Authentication
* Authorization
* Privilege escalation
* API security

---

## Burp Suite

Burp Suite can be used during authorized security testing to intercept, inspect, and modify HTTP requests.

Useful features include:

* Proxy
* Repeater
* HTTP history
* Comparer

---

## Recommended Learning Approach

A good way to learn IDOR and access-control vulnerabilities is to practice in intentionally vulnerable environments.

Suggested progression:

```text
HTTP Fundamentals
        ↓
Authentication
        ↓
Authorization
        ↓
Access Control
        ↓
IDOR / BOLA
        ↓
Privilege Escalation
        ↓
API Security
        ↓
Advanced Web Security
```

---

## ⚠️ Responsible Security Testing

Only test applications, APIs, systems, and accounts where you have explicit authorization.

For hands-on practice, use dedicated security labs, CTFs, and intentionally vulnerable applications.

---

## 📌 Repository

This repository is an educational resource focused on understanding IDOR and access-control vulnerabilities from a defensive and authorized security-testing perspective.
