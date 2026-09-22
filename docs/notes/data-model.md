# Data Model

## Workflow

1. Submit URL
2. Receive audit ID
3. Poll status
4. Fetch results, or cancel while
still queued

## Endpoints
| Method + path | Purpose | Success code | Error codes |
|---|---|---|---|
| `POST /audits` | Submit a URL for an audit | `202 Accepted` | `400 Bad Request`, `422 Unprocessable Content` |
| `GET /audits/{auditId}` | Get the status and results of a specific audit | `200 OK` | `404 Not Found` |
| `GET /audits` | List audits | `200 OK` | `400 Bad Request` |
| `DELETE /audits/{auditId}` | Delete a specific audit | `204 No Content` | `404 Not Found` |

## Audit
- Audit: The resource the API returns (make this up)
- every property
- its type
- whether it is always present
- example values: id , url , status , conformanceLevel , createdAt , completedAt

{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "url": "https://example.com/checkout",
  "status": "queued",
  "conformanceLevel": "AA",
  "createdAt": "2026-09-17T14:22:01Z",
  "completedAt": null
}

### What it means
- id — type: string (UUID) — always present — example: 3fa85f64-5717-4562-b3fc-2c963f66afa6. Uniquely identifies the audit; the client uses this to poll or cancel it.
- url — type: string (URI) — always present — example: https://example.com/checkout. The page that was audited; echoed back so the client can match a result to a request.
- status — type: string, one of the AuditStatus enum — always present — example: queued. Tells the client whether to keep polling or read results.
- conformanceLevel — type: string, one of A / AA / AAA — always present, defaults to AA if the client didn't specify one — example: AA. Which WCAG strictness level was tested against.
- createdAt — type: string (timestamp) — always present — example: 2026-09-17T14:22:01Z. When the audit was submitted.
- completedAt — type: string (timestamp) or null — present but null while queued/running, populated once finished — example: null or 2026-09-17T14:23:40Z. Lets the client compute how long the audit took, or confirm it's not done yet.

## AuditStatus

AuditStatus:
  type: string
  enum: [queued, running, completed, failed]
  description: >
    queued   -> running   (a worker picks up the job)
    running  -> completed (audit finished successfully)
    running  -> failed    (URL unreachable, timeout, or render error)
    queued   -> (deleted) (client cancels before it starts; DELETE only allowed here)

### What it means
- AuditStatus — the four states and the rules governing them:

State	Can move to	What the developer does
queued	running, or deleted via DELETE	Wait, or cancel if no longer needed
running	completed, failed	Keep polling; nothing else to do yet
completed	(terminal)	Read completedAt and the audit results
failed	(terminal)	Check the failure reason; resubmit if the cause was transient (e.g. timeout)
Only queued → deletable. Once a worker picks it up (running), it's committed; DELETE on a running, completed, or failed audit returns 409 Conflict — this is the rule Step 4.5's DELETE endpoint enforces.
completed and failed are terminal — no further transitions, which is why the polling loop in the guide stops on either one.

## ConformanceLevel:
    ConformanceLevel:
        A: Default;
        AA:
        AAA:
        
## CreateAuditRequest
- url is required and must be HTTPS; conformanceLevel is optional.

## Errors
- Which error codes each endpoint can return and why
