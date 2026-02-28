# Security Fix Plan — All Portfolio Projects

Status: **In Progress**
Started: 2026-02-28

## Execution Order
Serial: Stock → API → Chat → CRM
Priority: CRITICAL → HIGH → MEDIUM (per round across all projects)

---

## Round 1: CRITICAL Fixes

### StockAlpha (`stock.zuhdi.id`)
- [ ] Add Flask SECRET_KEY + session-based CSRF protection
- [ ] Add basic API key authentication middleware for state-mutating endpoints

### DataForge API (`api.zuhdi.id`)
- [ ] Replace hardcoded demo API key with `generate_api_key()` + mask in logs

### GroqMind (`chat.zuhdi.id`)
- [ ] Add DOMPurify to sanitize LLM output before innerHTML
- [ ] Add basic bearer token auth (env-based shared secret)

### NexaFlow CRM (`crm.zuhdi.id`)
- [ ] Remove hardcoded JWT secret default — require SECRET_KEY env var or auto-generate
- [ ] Add CORS middleware with explicit origin allowlist

---

## Round 2: HIGH Fixes

### StockAlpha
- [ ] Validate/restrict URLs in news service (SSRF fix)
- [ ] Add HTML escaping utility for innerHTML in app.js (XSS fix)
- [ ] Sanitize filename in export Content-Disposition header
- [ ] Add Flask-Limiter rate limiting on key endpoints
- [ ] Bind to 127.0.0.1 by default

### DataForge API
- [ ] Load DATABASE_URL from env var
- [ ] Apply rate limiting to all router endpoints (not just `/`)

### GroqMind
- [ ] Add slowapi rate limiting on /api/chat
- [ ] Add session TTL + max session cap (prevent OOM)
- [ ] Sanitize exception messages (don't leak to client)
- [ ] Add basic prompt injection guardrails

### NexaFlow CRM
- [ ] Add SRI hash to Tailwind CDN script tag
- [ ] Reduce token expiry to 1h, add refresh endpoint
- [ ] Add slowapi rate limiting on auth endpoints
- [ ] Bind to 127.0.0.1 by default

---

## Round 3: MEDIUM Fixes

### StockAlpha
- [ ] Sanitize exception messages returned to clients
- [ ] Add ticker symbol regex validation
- [ ] Cap indicator window parameter (max 500)
- [ ] Add period validation in Alpha dashboard route
- [ ] Cap batch sizes (max 20 tickers)
- [ ] Set restrictive SQLite file permissions

### DataForge API
- [ ] Add CORS middleware
- [ ] Add security headers middleware
- [ ] Escape LIKE wildcards in product search
- [ ] Add max_length to description field
- [ ] Add validation constraints to ProductUpdate schema
- [ ] Bind to 127.0.0.1, reload via env var

### GroqMind
- [ ] Add CORS middleware
- [ ] Bind to 127.0.0.1, reload via env var
- [ ] Fix XSS in error message rendering (use textContent)
- [ ] Use full UUID for session IDs
- [ ] Add SRI hashes + pin CDN versions
- [ ] Add input length validation on message field

### NexaFlow CRM
- [ ] Use EmailStr for email validation
- [ ] Add enum validation on status fields (ProjectStatus, InvoiceStatus)
- [ ] Add pagination to list endpoints
- [ ] Load DATABASE_URL from env var
- [ ] Add security headers middleware
- [ ] Disable /docs in production
- [ ] Handle error responses properly in frontend JS
