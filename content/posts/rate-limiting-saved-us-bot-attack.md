---
title: "Rate limiting saved us from a bot that didn't care about our WAF"
date: 2026-06-15T10:00:00Z
draft: false
featured_image: "/images/waf-rate-limiting.svg"
description: "A sophisticated bot was bypassing our WAF rules by mimicking human behavior. Rate limiting at the application layer caught what the WAF missed."
tags: ["waf", "rate limiting", "bot mitigation", "api security", "ddos"]
canonical_url: "https://waap-security.uk/posts/rate-limiting-saved-us-bot-attack/"
---
Our WAF was configured perfectly. OWASP top ten covered. Custom rules for our API endpoints. Bot detection enabled. Then a bot spent three days scraping our entire product catalog and we didn't notice until the CDN bill arrived.

The bot wasn't doing anything a WAF would flag. It was sending one request every 2-3 seconds. Valid user agents. Realistic browser headers. Full cookie acceptance. It looked like a normal user browsing the site — just one that never stopped and never slept.

The WAF rules didn't fire because there was nothing malicious about any individual request. No SQL injection, no XSS, no path traversal. Just GET requests to legitimate product pages. The bot was patient. It varied its timing, rotated IPs across three different residential proxy pools, and never hit the same endpoint twice in a row.

What caught it was application-layer rate limiting. Not the kind that blocks based on IP or simple request count — the bot would have evaded that easily. We implemented a sliding window that tracked session-level behavior: how many unique products a single session viewed per minute, how long between page loads, and whether the navigation pattern matched human browsing.

Human patterns have pauses. People read, scroll, compare. Bots don't. Even a sophisticated bot that randomizes timing still follows a more regular cadence than a real person. Our rate limiter flagged sessions that viewed more than 50 unique products in 10 minutes. No human does that. It also flagged sessions with zero variance in page-load interval — bots are too consistent.

The fix wasn't just blocking those sessions. We also added a proof-of-work challenge (a simple JavaScript hash calculation) for any session hitting the rate limit threshold. Humans never notice it. Bots don't run JS the same way browsers do.

The takeaway: a WAF is essential, but it's not sufficient. Application-layer rate limiting catches the attacks that look legitimate one request at a time. Layer your defenses, and don't assume the WAF sees everything.

---

*As an Amazon Associate I earn from qualifying purchases.*
