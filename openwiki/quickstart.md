# Demo Target Service – Quickstart

## What This Is

A minimal HTTP API service for managing project notes. This repository is a smoke-test target for OpenWiki documentation automation.

**Core functionality:**
- Health check endpoint
- In-memory notes storage
- Create and list notes via REST API

**Architecture:** Single Node.js HTTP server with no external dependencies.

## Quick Start

```bash
# Run the service
node src/server.js

# In another terminal, verify it works
curl http://localhost:3000/healthz
curl http://localhost:3000/notes
curl -X POST http://localhost:3000/notes \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","body":"Hello from OpenWiki"}'
```

The server listens on port 3000 unless `PORT` is set.

## Key Files

- **[../src/server.js](../src/server.js)** — Single-file implementation containing routing, handlers, and in-memory data
- **[../README.md](../README.md)** — User-facing documentation with API examples
- **[../AGENTS.md](../AGENTS.md)** — Agent instructions for this repository

## API Surface

The service exposes three endpoints (all implemented in `../src/server.js`):

| Method | Path | Purpose |
|--------|------|---------|
| GET | `/healthz` | Returns `{"ok": true, "service": "demo-target"}` |
| GET | `/notes` | Returns `{"notes": [...]}` from in-memory array |
| POST | `/notes` | Accepts `{"title": "...", "body": "..."}`, assigns ID and timestamp, adds to memory |

## Domain Model

Notes have this shape (see `../src/server.js` lines 3-10):

```javascript
{
  id: "note-1",           // Generated as "note-N"
  title: "Welcome",       // String, defaults to "Untitled"
  body: "...",           // String, defaults to ""
  createdAt: "2026-07-02T00:00:00.000Z"  // ISO timestamp
}
```

Storage is in-memory only. Restart clears all notes except the initial seed note.

## Change Guidance

**Adding a new endpoint:**
1. Add route condition in `handleRequest` function (`../src/server.js` lines 47-79)
2. Use `sendJson` helper for responses
3. Update this wiki and README if the endpoint is user-facing
4. Test manually with curl commands

**Changing the domain model:**
1. Update note structure in the seed data (lines 3-10)
2. Update POST handler logic (lines 60-74)
3. Update this wiki's domain model section
4. Verify with POST and GET requests

**Adding persistence:**
- Notes array is currently in-memory (line 3)
- To persist, inject a storage layer before the `handleRequest` function
- Watch for async operations when reading/writing
- Update operational notes in this wiki

## No Tests, No CI

This repository intentionally has no tests or CI workflows. It exists to validate OpenWiki documentation quality on a minimal codebase.

## See Also

- [API Reference](api.md) — Detailed endpoint behavior and examples
