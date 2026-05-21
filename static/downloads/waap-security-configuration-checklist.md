# WAAP Security Configuration Checklist

A practical step-by-step checklist for configuring Web Application and API Protection. Use this when deploying or auditing your WAAP platform.

## 1. WAF Configuration
- [ ] Enable managed rule set (OWASP ModSecurity CRS or Cloudflare Managed)
- [ ] Set SQLi rules to BLOCK (not LOG) in production
- [ ] After 48-72 hours of monitoring in LOG mode, manually switch XSS rules to BLOCK
- [ ] Configure HTTP method validation (block unused methods: PUT, DELETE, TRACE, OPTIONS)
- [ ] Set maximum request body size limit
- [ ] Set maximum URL length limit (recommended: 8KB)
- [ ] Configure Content-Type validation
- [ ] Enable positive security model (allow known-good traffic, deny everything else by default)
- [ ] Create allowlist exceptions for false positives
- [ ] Deploy rules in LOG mode for 48-72 hours before switching to BLOCK

## 2. Rate Limiting
- [ ] Configure per-IP rate limits
- [ ] Configure per-endpoint rate limits (stricter for login endpoints)
- [ ] Configure per-credential rate limits (track failed attempts per username)
- [ ] Set geolocation blocking for non-business regions (tune carefully to avoid blocking legitimate traffic)
- [ ] Enable session-based rate limiting
- [ ] Set burst limits for API endpoints

## 3. API Security
- [ ] Define and enforce OpenAPI/Swagger schema validation
- [ ] Implement authentication on all API endpoints
- [ ] Verify object-level authorization (prevent BOLA/IDOR)
- [ ] Strip unexpected fields from request bodies (prevent mass assignment) alongside input validation
- [ ] Log all API requests with client IP, endpoint, method, status, and response size
- [ ] Set rate limits per API key
- [ ] Implement request signing (HMAC or JWT) for sensitive endpoints
- [ ] Create API key rotation schedule

## 4. Bot Management
- [ ] Enable bot scoring for all traffic
- [ ] Configure challenge thresholds (CAPTCHA/JS challenge at score 15-49, block at 0-14)
- [ ] Block known TOR exit nodes
- [ ] Block or challenge public cloud IP ranges (unless expected)
- [ ] Block known VPN and proxy IPs
- [ ] Block traffic from countries with no business presence
- [ ] Enable JavaScript challenge for suspicious visitors
- [ ] Configure behavioral analysis for session-level detection
- [ ] Set up browser fingerprinting

## 5. DDoS Protection
- [ ] Enable network-layer DDoS mitigation (always-on)
- [ ] Configure application-layer DDoS rules
- [ ] Set per-IP connection limits
- [ ] Enable challenge-based mitigation (JS challenge, CAPTCHA, proof-of-work)
- [ ] Configure traffic profiling baselines
- [ ] Create emergency mitigation playbook
- [ ] Set up alerting for traffic anomalies
- [ ] Verify edge network scrubbing capacity

## 6. Monitoring & Maintenance
- [ ] Review blocked events weekly for false positives
- [ ] Update allowlist exceptions as application changes
- [ ] Rotate API keys quarterly
- [ ] Review rate limit thresholds monthly
- [ ] Test WAF rules after application updates
- [ ] Audit bot detection logs monthly
- [ ] Review DDoS event reports
- [ ] Update emergency playbook quarterly

## 7. Encryption & Transport Security
- [ ] Enforce HTTPS across all endpoints (HTTP to HTTPS redirect)
- [ ] Configure TLS 1.2 minimum (disable TLS 1.0/1.1)
- [ ] Enable HSTS headers
- [ ] Encrypt sensitive data at rest
- [ ] Encrypt sensitive data in transit (beyond TLS)

## 8. Compliance (PCI DSS / HIPAA / SOC2)
- [ ] Enable audit logging for all WAF events
- [ ] Export logs to SIEM
- [ ] Verify logging includes required fields (timestamp, source IP, action, rule ID)
- [ ] Configure log retention per compliance requirements
- [ ] Document all rule changes
- [ ] Create change management process for rule updates

> **Pro Tip:** Deploy changes in monitoring/log mode first. Review for false positives for 48-72 hours before enabling block actions. This prevents accidental disruption to legitimate traffic.
