---
title: "Top 5 WAF Rules Every Website Should Have"
date: 2026-05-16T08:00:00Z
draft: false
featured_image: "/images/waap-architecture.svg"
---

A Web Application Firewall is only as good as the rules it enforces. Default rule sets are a solid starting point, but they don't account for your application's specific threat profile. Here are five essential WAF rules that every website — regardless of technology stack or industry — should have in place.

## 1. SQL Injection Blocking

SQL injection remains one of the most dangerous and prevalent attack vectors. Despite being well-understood for over two decades, it consistently appears at the top of the OWASP Top 10.

A good WAF rule for SQL injection goes beyond signature-based detection. Modern rules should use context-aware inspection — understanding whether a parameter is expected to contain SQL syntax. For example, a search query parameter that contains "SELECT * FROM users" is obviously an attack. But a base64-encoded or URL-encoded payload requires decoding before inspection.

What to configure:
- Enable managed SQLi rule set (most WAAP platforms include one)
- Add custom rules for any parameter that gets passed directly to a database query
- Set action to BLOCK (not just LOG) for production environments
- Monitor false positives closely for the first week; search forms are common triggers

## 2. Cross-Site Scripting (XSS) Prevention

XSS attacks inject malicious scripts into web pages viewed by other users. They can steal session cookies, capture keystrokes, redirect users to phishing sites, or deface your application.

The challenge with XSS is balancing protection with functionality. Many legitimate applications use HTML in user-generated content — rich text editors, comments, forum posts. A heavy-handed WAF rule will break your product.

Best practice for XSS rules:
- Use a positive security model where possible: define exactly what HTML tags and attributes are allowed
- For user-generated content, combine WAF rules with server-side output encoding
- Enable reflected XSS detection (scripts in URL parameters) with a BLOCK action
- Set stored XSS detection to LOG initially, tune to BLOCK after reviewing alerts

## 3. Rate Limiting and Brute Force Protection

Rate limiting isn't a WAF rule in the traditional sense, but it's one of the most effective protections in any WAAP platform. Brute force attacks on login pages, API endpoints, and password reset forms are automated and relentless.

Effective rate limiting requires layered thresholds:
- **Per-IP.** Maximum requests per second from a single IP address. Start with a generous limit and tighten over time.
- **Per-endpoint.** Login endpoints should have stricter limits than static content. A user doesn't need to POST to /login twenty times in a minute.
- **Per-credential.** Track failed login attempts per username. After five failures, block further attempts on that account for 15 minutes.
- **Geolocation.** Block or challenge traffic from countries where you have no business presence.

## 4. HTTP Method Validation

Many web applications only use GET and POST. PUT, DELETE, PATCH, OPTIONS, and TRACE are often exposed by accident. Each unnecessary HTTP method is a potential attack vector.

A simple but powerful rule: allow only the HTTP methods your application actually uses and block everything else.

- GET for retrieving resources
- POST for creating or submitting data
- Block PUT, DELETE, PATCH unless your API explicitly uses them
- Block TRACE entirely (it's used for cross-site tracing attacks)
- Restrict OPTIONS to development environments

## 5. Request Size and Structure Limits

Attackers frequently use oversized payloads to trigger buffer overflows, exhaust server resources, or hide malicious content inside large request bodies.

Configure these limits at the WAF level:
- **Maximum request body size.** Set based on your largest legitimate request. A file upload endpoint might need 100MB. Most API endpoints should be 1MB or less.
- **Maximum URL length.** Browsers typically support URLs up to 8KB. Set your WAF to block anything over that.
- **Maximum header count.** Most legitimate requests have 10-20 headers. Block anything with more than 50.
- **Content-Type validation.** Reject requests where Content-Type doesn't match the content. JSON content with a form-encoded Content-Type is suspicious.

## Rule Tuning: The Critical Step You Can't Skip

Deploying these five rules and walking away is a recipe for false positives. Every application is different. A rule that blocks requests with "ALTER TABLE" in the body might break your legitimate administrative interface.

The recommended deployment sequence is:
1. Enable rules in LOG mode for 48-72 hours
2. Review blocked events and identify false positives
3. Create allowlist exceptions for legitimate traffic
4. Switch to BLOCK mode gradually, one rule at a time
5. Continue monitoring for the first two weeks


---

*Want to go deeper? Check out these resources on Amazon:*

- [OWASP Testing Guide](https://www.amazon.com/dp/B09VWTYB8J?tag=falconsedge-20)
- [Real-World Bug Hunting](https://www.amazon.com/dp/1593279076?tag=falconsedge-20)

*As an Amazon Associate I earn from qualifying purchases.*



> **Download the full WAAP Security Configuration Checklist** with 60 actionable items covering WAF, rate limiting, API security, bot management, and DDoS. [Get it on Gumroad for $7](https://falconer2072.gumroad.com/l/qczxj).


## The Bottom Line

These five rules form the foundation of any effective WAF deployment. They cover the most common attack vectors, balance security with usability, and provide a framework for ongoing tuning. Start with these, verify they don't break your application, and expand from there based on your specific threat landscape.
