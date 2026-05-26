---
title: "Rate Limiting Best Practices for Modern APIs"
date: 2026-03-16T08:00:00Z
draft: false
featured_image: "/images/waap-architecture.svg"
tags: ["rate limiting", "API", "throttling", "best practices", "DDoS"]
---

Rate limiting is one of the oldest web security controls, yet it remains one of the most frequently misconfigured. In 2026, with API abuse becoming more sophisticated and distributed, getting rate limiting right is more important than ever. Here's what best practice looks like for modern API rate limiting.

## The Problem with Simple Rate Limiting

A single threshold — "100 requests per minute per IP" — is no longer sufficient. Attackers have adapted:

- **IP rotation.** Attackers route requests through botnets with thousands of IPs, making per-IP limits meaningless
- **Distributed attacks.** AI-orchestrated attacks spread across hundreds of endpoints, staying below per-endpoint thresholds
- **Asymmetric cost attacks.** A single expensive API call (search, report generation) can have the same server impact as thousands of cheap calls

## Layered Rate Limiting Architecture

### Layer 1: Global Rate Limits

Set an absolute maximum for your entire API surface. This is your circuit breaker — if your infrastructure can't handle more than 50,000 requests per second, configure the WAAP to reject anything beyond that at the edge, before it reaches your servers.

### Layer 2: Per-Client Rate Limits

Identify clients by API key, authentication token, or session cookie — not by IP address. Per-client limits are more meaningful because they follow the actual consumer, not the network address. Set limits based on the client's service tier: free tier gets 100 req/min, premium gets 1,000 req/min.

### Layer 3: Per-Endpoint Rate Limits

Different endpoints need different limits. A health check endpoint might handle 1,000 req/min. A login endpoint should never exceed 10 req/min per client. A POST to `/api/checkout` should have stricter limits than a GET to `/api/products`.

### Layer 4: Per-User Rate Limits

For authenticated endpoints, track rate limits at the user level, not just the API key level. A compromised API key used by an attacker should be limited even if the key belongs to a premium account.

### Layer 5: Cost-Based Rate Limiting

Assign a computational cost to each endpoint. A simple data lookup costs 1 unit. A full-text search costs 10 units. A report generation costs 50 units. Set a budget per client per time window, and reject requests that would exceed the budget regardless of request count.

## Rate Limiting Response Headers

Good rate limiting implementations inform clients of their limits through standard headers:

- `X-RateLimit-Limit` — maximum requests per window
- `X-RateLimit-Remaining` — remaining requests in current window
- `X-RateLimit-Reset` — when the window resets (Unix timestamp)
- `Retry-After` — seconds to wait before retrying (on 429 responses)

## The Bottom Line

Rate limiting in 2026 requires a layered approach. No single threshold can protect against modern distributed attacks. Combine global, per-client, per-endpoint, per-user, and cost-based limits into a unified rate limiting strategy, and ensure your WAAP supports all five layers.

For advanced traffic segmentation strategies that complement rate limiting, visit [microsegmentation.uk](https://microsegmentation.uk). And for AI-driven traffic pattern analysis, check out [aisecurities.uk](https://aisecurities.uk).

---

*Want to go deeper? Check out these resources on Amazon:*

- [High Performance Browser Networking](https://www.amazon.com/dp/1449344763?tag=falconsedge-20)
- [API Security in Action](https://www.amazon.com/dp/1617296024?tag=falconsedge-20)

*As an Amazon Associate I earn from qualifying purchases.*
