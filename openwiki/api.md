# API Reference

Complete endpoint documentation for the Demo Target Service.

## GET /healthz

Health check endpoint for monitoring and load balancers.

**Request:**
```bash
curl http://localhost:3000/healthz
```

**Response (200 OK):**
```json
{
  "ok": true,
  "service": "demo-target"
}
```

**Implementation:** `../src/server.js` lines 50-53

## GET /notes

Retrieve all notes from in-memory storage.

**Request:**
```bash
curl http://localhost:3000/notes
```

**Response (200 OK):**
```json
{
  "notes": [
    {
      "id": "note-1",
      "title": "Welcome",
      "body": "This in-memory note helps OpenWiki discover the domain model.",
      "createdAt": "2026-07-02T00:00:00.000Z"
    }
  ]
}
```

**Implementation:** `../src/server.js` lines 55-58

## POST /notes

Create a new note with generated ID and timestamp.

**Request:**
```bash
curl -X POST http://localhost:3000/notes \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","body":"Hello from OpenWiki"}'
```

**Request Body:**
```json
{
  "title": "My Note Title",
  "body": "Note content here"
}
```

Both fields are optional:
- `title` defaults to `"Untitled"`
- `body` defaults to `""`

**Response (201 Created):**
```json
{
  "note": {
    "id": "note-2",
    "title": "My Note Title",
    "body": "Note content here",
    "createdAt": "2026-07-03T13:00:00.000Z"
  }
}
```

**Response (400 Bad Request)** when body is not valid JSON:
```json
{
  "error": "Invalid JSON body"
}
```

**Implementation:** `../src/server.js` lines 60-76

**ID Generation:** Sequential counter based on array length (`note-${notes.length + 1}`)

## 404 Not Found

All other routes return:

```json
{
  "error": "Not found"
}
```

**Implementation:** `../src/server.js` line 78
