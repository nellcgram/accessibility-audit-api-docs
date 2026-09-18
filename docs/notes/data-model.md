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

## AuditStatus