# HTTP Fundamentals

## Methods
| Methods | Purpose   | Has request body | Changes data on server?
|----------|----------|-----------------| ----------------------|
| GET | Retrieve a resource | Yes| Yes |          
| POST | Sends data to server, usually causes change on server | Yes | No |  
| DELETE | Asks server to delete a resource | No | No  |  
| HEAD | requests resource's metadata in headers only | No | Yes |

## Status codes
| Code | Meaning | When it is returned | What developer does next |
|------|---------|---------------------|--------------------------|
| 200 | OK | The request succeeded and the response body contains the requested data | Pull the needed fields from the response body |
| 202 | Accepted | The request was accepted, but processing isn't finished | Use the ID or URL in the response to check the status until the task finishes |
| 204 | No Content | The request succeeded and there is no data to send back, such as after a DELETE | Treat it as success; don't try to read a response body |
| 400 | Bad Request | The server couldn't process the request because it was built wrong, such as malformed JSON | Read the error message, fix the request, and resend |
| 401 | Unauthorized | The request had no valid credentials, such as a missing or expired token | Add a valid token to the `Authorization` header, or get a new one |
| 404 | Not Found | The endpoint or resource doesn't exist, such as a wrong path or an unknown ID | Check the endpoint path and the ID |
| 409 | Conflict | The request conflicts with the resource's current state, such as deleting an audit that is already running | Check the resource's current state and retry only if it allows the action |
| 422 | Unprocessable Content | The server could read the request, but a value in it is invalid, such as `limit=abc` | Change the value to the expected type or format and resend |
| 429 | Too Many Requests | The client sent too many requests in a set amount of time | Wait the number of seconds in the `Retry-After` header, then retry |
| 500 | Internal Server Error | Something failed on the server, not in the request | Retry later; if it keeps failing, report it to the API provider |

https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/202

## JSON annotation
```json
Source: open https://openlibrary.org/search.json?q=harry+potter&limit=1 in Firefox and read the fields.

```

### How to spot each JSON type

| Looks like | JSON type |
|------------|-----------|
| `"text in quotes"` | string |
| bare digits, like `1997` | number |
| `true` or `false` | boolean |
| `[ ]` | array |
| `{ }` | object |

### Open Library response fields

| Field | JSON type | English meaning |
|-------|-----------|-----------------|
| `numFound` | number | Total number of books matching the search |
| `docs` | array | The list of books that matched the search |
| `docs[0]` | object | One book's details, such as title, author, and publish year |
| `docs[0].title` | string | The book's title |
| `docs[0].author_name` | array | The list of authors for that book |
| `docs[0].first_publish_year` | number | The year the book was first published |
| `docs[0].has_fulltext` | boolean | Whether Open Library has a readable copy of the book |