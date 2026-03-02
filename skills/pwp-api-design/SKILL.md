---
name: pwp-api-design
description: "API design standards — consistent, predictable, documented endpoints. Use this skill whenever the user is building, reviewing, or planning an API. Also use when they mention REST endpoints, API routes, request/response shapes, status codes, pagination, versioning, or say things like 'design the API', 'what should this endpoint look like', 'review my API', or 'add an endpoint for X'. Covers REST conventions, status codes, response envelopes, pagination, filtering, versioning, and validation."
---

# API Design Skill

This skill defines standards for designing and reviewing APIs. Consistency and predictability are non-negotiable.

## API Design Principles

1. **Consistency over cleverness.** Every endpoint follows the same patterns.
2. **Predictability over flexibility.** Predictable APIs are easier to consume, test, and debug.
3. **Explicit over implicit.** Errors should be clear. Status codes should be correct.

## REST API Conventions

### URL Structure
```
GET    /api/v1/{resources}          → List
GET    /api/v1/{resources}/{id}     → Get one
POST   /api/v1/{resources}          → Create
PUT    /api/v1/{resources}/{id}     → Replace
PATCH  /api/v1/{resources}/{id}     → Partial update
DELETE /api/v1/{resources}/{id}     → Delete
```

Rules: plural nouns, no verbs in URLs, max 2 levels of nesting, query params for filtering/sorting/pagination.

### HTTP Status Codes

| Code | Meaning | When to Use |
|------|---------|------------|
| **200** | OK | Successful GET, PUT, PATCH |
| **201** | Created | Successful POST |
| **204** | No Content | Successful DELETE |
| **400** | Bad Request | Invalid input |
| **401** | Unauthorized | Missing/invalid auth |
| **403** | Forbidden | Insufficient permissions |
| **404** | Not Found | Resource doesn't exist |
| **409** | Conflict | Duplicate/state conflict |
| **422** | Unprocessable | Semantic errors |
| **429** | Too Many Requests | Rate limited |
| **500** | Server Error | Unhandled exception |

### Response Envelope

```json
// Success
{ "data": { ... }, "meta": { "page": 1, "per_page": 20, "total": 142 } }

// Error
{ "error": { "code": "VALIDATION_ERROR", "message": "Email is required", "details": [...] } }
```

### Pagination
Offset-based: `?page=2&per_page=20`
Cursor-based (large datasets): `?cursor=abc123&limit=20`

### Filtering & Sorting
`?category=electronics&min_price=100&sort=-created_at`

## Versioning
- Version in URL path: `/api/v1/...`
- Old versions supported 6+ months after deprecation

## Input Validation
- Validate at the boundary
- Return specific errors with field-level details
- Reject or ignore unknown fields

## Review Checklist
- [ ] URLs use plural nouns, no verbs
- [ ] HTTP status codes are correct
- [ ] Response shape is consistent
- [ ] Error responses include code, message, details
- [ ] Input validation at boundary
- [ ] Pagination on list endpoints
- [ ] Auth on protected endpoints
- [ ] Rate limiting configured
- [ ] API documented (OpenAPI or equivalent)

## Anti-Patterns

| Anti-Pattern | Do This Instead |
|-------------|----------------|
| Verbs in URLs | Use HTTP methods |
| 200 for everything | Correct status codes |
| Leaking internal errors | Safe, structured responses |
| No pagination | Always paginate lists |
| Breaking changes without versioning | Version and deprecate gracefully |
