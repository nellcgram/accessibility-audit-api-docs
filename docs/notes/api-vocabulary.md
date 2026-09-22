# API Vocabulary

**Sources:** MDN HTTP overview (developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview) and MDN HTTP methods
(developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)

## For each term: 1–2 sentences in your own words, then one example from the Open Library request you already sent.

**API:** what one program lets another program ask for, and how.
- It's like a package being sent in pieces
Ex: https://openlibrary.org/search.json?q=harry+potter 

**Resource::** the "thing" an API deals in (a book, a user, an audit).
Ex: Harry Potter book

:**URL::** the address of a resource. Break https://openlibrary.org/search.json?q=harry+potter into its parts: scheme, host, path, query
string.
- Scheme: https (protocol)
- Host: openlibrary.org (server being contacted)
- Path: /search.json (the resource being requested; Open Library's search API endpoint)
- Query string = ?q=harry+potter (extra info sent to server)
The + represents a space

https://openlibrary.org/search.json?q=harry+potter
└─┬─┘  └──────────────┬──────────────┘ └──────┬──────┘
scheme               host                    path
                                           └───────┬───────┘
                                            query string


:**Endpoint:** a URL path **plus the method used on it** ( GET /search.json )
- "a location where information can be obtained." 
- URLs that reflect something your app can do
-  any RESTful API accessible URI from which you can get, put, post, and delete

:**HTTP request / response::** the request is what the client sends; the response is what the server sends back. Both have a start line,
headers, and an optional body.
Reddit user notes:
- a structured message that a client (such as a browser, mobile app, or API tool) sends to a server to request a resource or trigger an action.
- When asked, what is it, answer: “Which version of HTTP?”
- “A way of asking the server to do something.”
- HTTP means hyper text transfer protocol. it is a protocol to transfer text. 

**HTTP methods:**
GET (read), POST (create or trigger), DELETE (remove) - request methods to indicate the purpose of the request and what is expected if the request is successful.
- GET - represents resource; used to retrieve data, no body
https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/GET 
- HEAD - requests resource's metadata in headers only; has no body
- POST - submits an entity to the specified resource, often causing a change in state or side effects on the server. Has body, indicated by content type header.
https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST 
- DELETE - deletes the specified resource. Has no body.
https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/DELETE
- Safe means read-only, so a method that changes data on the server is NOT safe

- More: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods 

**Headers:** metadata about the request or response ( Content-Type: application/json ).

**Path parameters vs. query parameters:** path is part of the address that identifies one item ( /audits/{auditId} ); query is options
after ? that filter or shape the result ( ?q=harry+potter ).

**Request body / response body:** the data payload itself. GET requests usually have none.

**JSON:** the text format most API bodies use: keys and values in { } , lists in [ ] .

**Status codes:** the three-digit result number. 2xx success, 4xx client mistake, 5xx server problem.

**Authentication and bearer tokens:** how the server knows who is calling. A bearer token is a secret string sent in the Authorization
header.

**Asynchronous operations::** the server accepts the job now and finishes it later; the client checks back (polls) for the result

**The pattern repeats across most REST APIs::** a path is the resource (/audits), a path segment in {} is one specific item (/audits/{auditId}), and the method (GET, POST, DELETE) says what you're doing to it.

## Self-test
" POST /audits sends a JSON body containing a URL, with a bearer token in the header. The server
replies 202 Accepted and an audit ID with status queued . The client then polls GET /audits/{auditId} until status is completed or failed ."

## Endpoint Examples

**Looks like:**
GET  https://api.example.com/v1/audits/3fa85f64-5717-4562-b3fc-2c963f66afa6

**Breaking that apart:**
- https:// — scheme
- api.example.com — host (the server)
- /v1 — API version, part of the base path
- /audits/3fa85f64-5717-4562-b3fc-2c963f66afa6 — the path: /audits is the resource collection, 3fa85f64... is a path parameter identifying one specific audit
- GET — the method, sent separately from the URL (in curl, as -X GET, or implied when there's no body)

**The same resource has multiple endpoints depending on the method:**

POST   /audits                 -> create a new audit
GET    /audits                 -> list all audits
GET    /audits/{auditId}       -> get one specific audit
DELETE /audits/{auditId}       -> cancel one specific audit

Each of those four is a distinct endpoint — same base resource (/audits), different method and sometimes a different path shape (with or without an ID).

**A full request to an endpoint:**
...also carries headers and, for POST, a body:

```
curl -X POST https://api.example.com/v1/audits \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com/checkout"}'
```
- X POST = the method
- The URL after curl = the endpoint
- H lines = headers (auth token, content type)
- d = the request body, JSON in this case