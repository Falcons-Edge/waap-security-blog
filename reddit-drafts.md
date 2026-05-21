# Reddit Drafts — WAAP Security Blog
# Post these to r/netsec, r/cybersecurity, r/cloudsecurity

---

## Draft 1: "I spent a month tuning WAF rules across production environments — here's what actually matters"

**Suggested subreddit:** r/netsec
**Type:** Text post with link in comments

---

Title: I spent a month tuning WAF rules across production environments — here's what actually matters

Body:

After tuning Web Application Firewalls across several production deployments, I noticed the same patterns emerging. The managed rule sets are a good start, but there are five rules that do the heavy lifting:

1. SQL injection blocking with context-aware inspection (not just signature matching)
2. Rate limiting at three levels: per-IP, per-endpoint, and per-credential
3. HTTP method validation — you'd be surprised how many apps have PUT and DELETE exposed
4. Request size limits — most API endpoints don't need to accept 100MB payloads
5. Content-Type enforcement — JSON endpoints accepting form-encoded data is a red flag

The biggest lesson: deploy in LOG mode for 48-72 hours before switching to BLOCK. Every app has false positives you won't predict.

I wrote up the full breakdown with examples if anyone's interested: [link to waap-security.uk/posts/top-5-waf-rules/]

What WAF rules do you consider non-negotiable?

---

## Draft 2: "API security in 2026 — the same three bugs keep showing up"

**Suggested subreddit:** r/cybersecurity
**Type:** Text post

---

Title: API security in 2026 and the same three vulnerabilities keep topping the charts

Body:

After reviewing API security findings this year, I keep seeing the same three issues regardless of the tech stack:

1. Broken Object Level Authorization (BOLA) — change an ID in the URL, access someone else's data. It's the OWASP API #1 for a reason.
2. Excessive data exposure — the API returns the full database row when the frontend only needs three fields.
3. Mass assignment — unexpected fields in the request body modify properties they shouldn't.

The fix for all three is the same: schema validation. Define your API with OpenAPI, validate every request against it, and reject anything that doesn't match. Stripping unexpected fields alone prevents mass assignment entirely.

Full write-up with mitigation strategies: [link to waap-security.uk/posts/api-security-best-practices/]

What API vulnerabilities are you seeing most in your audits?

---

## Draft 3: "DDoS attacks are getting smarter — volumetric-only protection isn't enough anymore"

**Suggested subreddit:** r/cloudsecurity
**Type:** Text post

---

Title: DDoS attacks are hitting 2 Tbps AND targeting Layer 7 now — volumetric scrubbing alone won't save you

Body:

The days of "just absorb it at the edge" are fading. Attackers are layering volumetric floods with precisely crafted application-layer requests that bypass traditional DDoS mitigation.

Three things that actually stop multi-vector attacks:

- Edge network capacity (200+ Tbps scrubbing across global PoPs)
- Behavioral rate limiting that adapts in real-time, not static thresholds
- Challenge-based mitigation (JS challenges, CAPTCHA, proof-of-work) for suspicious traffic

If you're only monitoring bandwidth thresholds, you're missing the L7 attacks that bring down application logic without saturating your pipe.

I mapped out the full mitigation flow here: [link to waap-security.uk/posts/ddos-protection-waap/]

How are you handling application-layer DDoS?
