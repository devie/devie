# Changelog — Portfolio Projects

## 2026-02-28 — Security Audit

Full security review conducted across all 4 portfolio projects.

### StockAlpha (`stock.zuhdi.id`)
| Severity | Count | Key Issues |
|----------|-------|------------|
| CRITICAL | 2 | No authentication on any endpoint; No CSRF protection (no SECRET_KEY) |
| HIGH | 5 | SSRF in news URL fetch; XSS via innerHTML (50+ locations); Header injection in export filename; No rate limiting; Binds 0.0.0.0 |
| MEDIUM | 6 | Exception messages leaked; No ticker validation; Unbounded indicator window; No period validation in Alpha; Unbounded batch sizes; SQLite default permissions |
| LOW | 3 | CDN without SRI; Missing security headers; AI status endpoint reveals infra |

### DataForge API (`api.zuhdi.id`)
| Severity | Count | Key Issues |
|----------|-------|------------|
| CRITICAL | 1 | Hardcoded predictable demo API key (df_demo_key_123456) |
| HIGH | 2 | Hardcoded DATABASE_URL; Rate limiting only on root `/` endpoint |
| MEDIUM | 6 | No CORS; No security headers; Timing-vulnerable API key lookup; No RBAC; LIKE pattern injection; API key printed to stdout |
| LOW | 8 | No validation on ProductUpdate; No description max_length; Health endpoint unprotected; Docs publicly exposed; No HTTPS enforcement; generate_api_key() unused; Float for money |

### GroqMind (`chat.zuhdi.id`)
| Severity | Count | Key Issues |
|----------|-------|------------|
| CRITICAL | 2 | XSS via unsanitized LLM output (innerHTML + marked.parse); No authentication |
| HIGH | 4 | No rate limiting (Groq billing abuse); Unbounded in-memory sessions (OOM); Raw exceptions leaked; No prompt injection defense |
| MEDIUM | 6 | No CORS; Binds 0.0.0.0; reload=True hardcoded; XSS in error message; Truncated session IDs (48-bit); CDN without SRI |
| LOW | 4 | Missing security headers; Tech stack in error msg; No input length validation; No SSE timeout |

### NexaFlow CRM (`crm.zuhdi.id`)
| Severity | Count | Key Issues |
|----------|-------|------------|
| CRITICAL | 2 | Hardcoded JWT secret with weak default; No CORS configuration |
| HIGH | 5 | JWT in localStorage; CDN without SRI; 24h token no revocation; No rate limiting on auth; Binds 0.0.0.0 with reload |
| MEDIUM | 6 | EmailStr imported unused; No enum validation on status; No pagination; Predictable DB path; No CSRF; Unchecked error responses in frontend |
| LOW | 5 | Password min 6 (should be 8); bcrypt 72-byte truncation; Email enumeration; No security headers; API docs exposed |

### Common Issues (All Projects)
- No rate limiting
- Binds 0.0.0.0 with reload=True (dev config)
- No security headers (CSP, X-Frame-Options, HSTS)
- CDN scripts without SRI hashes
- Exception messages leaked to clients

---

## 2026-02-28 — Initial Build

### Infrastructure
- Cloudflare tunnel config updated: crm.zuhdi.id:6001, chat.zuhdi.id:6002, api.zuhdi.id:6003
- CNAME DNS records added for all subdomains
- Start/stop scripts: `~/start-portfolio.sh`, `~/stop-portfolio.sh`

### StockAlpha (`stock.zuhdi.id`)
- Root `/` now redirects to `/alpha/` (StockAlpha dashboard)
- README updated with full feature documentation

### DataForge API (`api.zuhdi.id`) — NEW
- FastAPI + SQLAlchemy + SQLite
- Products CRUD with categories, search, pagination
- Currency converter (IDR/USD/SGD/EUR)
- Analytics endpoints (summary, top products)
- API key authentication (X-API-Key header)
- Rate limiting via slowapi
- Auto-generated Swagger + ReDoc docs
- GitHub: devie/dataforge-api

### GroqMind (`chat.zuhdi.id`) — NEW
- FastAPI + Groq SDK + SSE streaming
- Multi-turn conversation memory
- 4 personas (General, Code Helper, Business Advisor, Bahasa Indonesia)
- Model switcher (Llama 3.1 8B / Llama 3.3 70B)
- Markdown rendering + code block copy
- Token usage display
- GitHub: devie/groqmind

### NexaFlow CRM (`crm.zuhdi.id`) — NEW
- FastAPI + SQLAlchemy + SQLite + JWT auth
- User registration/login with bcrypt
- Contacts CRUD (name, email, phone, company, tags)
- Projects CRUD (linked to contacts, status, value)
- Invoices CRUD (linked to projects, paid/unpaid)
- Dashboard with summary cards
- Responsive UI with Tailwind CSS
- GitHub: devie/nexaflow-crm

### GitHub Profile
- Updated featured projects table with all 4 projects + live demo links
- Added Groq badge to tech stack
