# Demo Target Service — Quickstart

This repository is a small smoke-test target for OpenWiki documentation automation.

## What It Does

The Demo Target Service is a minimal HTTP API for managing project notes in memory:

- `GET /healthz` — service health check
- `GET /notes` — list all notes
- `POST /notes` — create a new note with auto-generated id and timestamp

The implementation is intentionally small so documentation quality is easy to inspect.

Source: [../README.md](../README.md)

## Repository Structure

```
openwiki-demo-target/
├── AGENTS.md          # Agent instructions
├── README.md          # Primary documentation
└── src/
    └── server.js      # HTTP server, routing, handlers, in-memory note storage
```

The entire application logic lives in [../src/server.js](../src/server.js).

## Running the Service

Start the server:

```bash
node src/server.js
```

The server listens on port `3000` by default, or `PORT` environment variable if set.

Source: [../README.md](../README.md), [../src/server.js](../src/server.js#L81-L88)

## Verifying Behavior

Test the health endpoint:

```bash
curl http://localhost:3000/healthz
```

List notes:

```bash
curl http://localhost:3000/notes
```

Create a note:

```bash
curl -X POST http://localhost:3000/notes \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","body":"Hello from OpenWiki"}'
```

Source: [../README.md](../README.md#L23-L31)

## Domain Model

### Note

Notes are plain JavaScript objects stored in an in-memory array:

```javascript
{
  id: "note-1",              // Auto-generated: "note-N" where N = notes.length + 1
  title: "Welcome",          // String, defaults to "Untitled"
  body: "...",               // String, defaults to ""
  createdAt: "2026-07-02..." // ISO 8601 timestamp
}
```

Source: [../src/server.js](../src/server.js#L3-L10), [../src/server.js](../src/server.js#L63-L68)

## Architecture

This is a single-file Node.js HTTP server with no external dependencies or persistence layer.

- **Framework**: Node.js built-in `http` module
- **Storage**: In-memory array, resets on restart
- **Routing**: Manual URL path matching in `handleRequest`
- **Request parsing**: Custom `readJson` promise-based body parser
- **Response formatting**: Custom `sendJson` helper

Source: [../src/server.js](../src/server.js)

## Change Surfaces

When modifying this service:

- **Add or change routes**: Edit `handleRequest` in [../src/server.js](../src/server.js#L47-L79)
- **Modify note schema**: Update the note creation logic in [../src/server.js](../src/server.js#L63-L68)
- **Change persistence**: Replace the in-memory `notes` array with a database or file storage
- **Add validation**: Extend the `POST /notes` handler starting at [../src/server.js](../src/server.js#L60)

## Testing

There are no automated tests in this demo target. Manual verification uses the curl commands documented in [Verifying Behavior](#verifying-behavior).

Source: [../README.md](../README.md#L33-L37)

## Deployment

No deployment configuration exists. The service is designed for local smoke-testing of OpenWiki documentation generation.

## Agent Guidance

When working in this repository:

- Keep generated OpenWiki documentation under `openwiki/`
- Do not edit `src/server.js` unless explicitly asked
- Use `node src/server.js` and the README curl commands for manual verification
- Focus changes on documentation, not application logic

Source: [../AGENTS.md](../AGENTS.md)
