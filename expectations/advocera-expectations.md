# Advocera — Expectations Document

> Source: `the_penny_lane_project/Advocera/advocera_report.md`
> Last reviewed: 2026-03-08

---

## 1. Language and Runtime Constraints

### 1.1 Python stdlib only
Advocera uses **Python standard library only** — no external packages (no `pip` dependencies). Any new dependency must be explicitly approved before being added. If a third-party package is found in `requirements.txt` or imported without prior approval, file a `critical` issue.

### 1.2 No framework
There is no web framework (no Flask, FastAPI, Django). The HTTP server is implemented using `http.server` from the stdlib. Do not introduce a framework without explicit approval.

---

## 2. Database Constraints

### 2.1 SQLite via raw `sqlite3`
The database layer uses Python's built-in `sqlite3` module. No ORM (SQLAlchemy, Tortoise, Peewee, etc.) is permitted. If an ORM import is found, file `critical`.

### 2.2 Parameterized queries
All SQL statements must use parameterized queries (the `?` placeholder syntax). Raw string interpolation into SQL is prohibited. If found, file `critical`.

### 2.3 DB_PATH must be configurable
The database path (`DB_PATH`) must be read from an environment variable or configuration — **not** hardcoded to a fixed path. If hardcoded, file `warning`.

### 2.4 Migration naming convention
Migration files must follow the sequential naming pattern `000N_<description>.sql` (e.g., `0001_initial.sql`, `0002_add_users.sql`). Files that deviate from this pattern must be flagged `warning`.

---

## 3. API Constraints

### 3.1 All endpoints under `/v1`
Every HTTP endpoint must be prefixed with `/v1`. Any endpoint found outside this prefix must be flagged `warning`.

### 3.2 Validation failure response
On input validation failure, endpoints must return HTTP `422` with a JSON body containing a `field_errors` key. Any deviation from this contract (wrong status code or missing `field_errors`) must be filed `critical`.

### 3.3 Conflict response
When a conflict occurs (e.g., duplicate resource), endpoints must return HTTP `409`. File `warning` for any conflict handled with a different status code.

### 3.4 Not-found response
When a requested resource does not exist, endpoints must return HTTP `404`. File `warning` for not-found cases handled with a different status code.

### 3.5 OpenAPI contract sync
The OpenAPI contract in `api-contracts.yaml` must stay in sync with the routes and schemas in `server.py`. If new endpoints or changed signatures are found in `server.py` without a corresponding update to `api-contracts.yaml`, file `warning`.

---

## 4. Security and Authentication

### 4.1 No authentication (prototype)
Advocera is an unauthenticated prototype. No authentication is implemented by design. **If any authentication mechanism is added** (API keys, JWT, session tokens, etc.) it must be flagged for review with label `warning` — auth additions require explicit approval before merging.

### 4.2 No hardcoded secrets or production URLs
No secrets (API keys, passwords, tokens) or production URLs may be hardcoded in any source file. Use environment variables. File `critical` for any violation.

---

## 5. Audit Logging

### 5.1 `_audit_log()` on every mutation
The `_audit_log()` function must be called on **every** mutation operation (create, update, delete). Any mutation path that does not invoke `_audit_log()` must be filed `critical`.

---

## 6. Tests

### 6.1 `unittest`-based integration tests must remain passing
Tests live in `tests/` and use Python's `unittest` framework. All existing tests must remain passing. If a code change causes test failures, file `critical`.

### 6.2 Test coverage for new endpoints
Any new API endpoint added to `server.py` must have a corresponding integration test in `tests/`. Missing test coverage for new endpoints should be filed `warning`.

---

## 7. Out-of-Scope Constraints

- Do not deploy to a public server without owner approval
- Do not add a frontend, authentication layer, or persistent user accounts without explicit scope change approval
