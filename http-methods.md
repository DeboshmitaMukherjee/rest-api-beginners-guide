# HTTP Methods in REST APIs

## Introduction

HTTP methods define the action a client wants to perform when communicating with a REST API. For example, a client might request information about a user, create a new product, update an existing record, or delete a resource.

Understanding HTTP methods helps developers send the correct requests and understand how an API handles data.

This guide explains the most commonly used HTTP methods, with simple examples and practical request formats.

## 1. What Are HTTP Methods?

An HTTP method tells the API server what operation the client wants to perform on a resource.

A **resource** is the item an API works with, such as a user, product, order, or article.

For example:

```http
GET /users/123
```

In this request:

- `GET` is the HTTP method.
- `/users/123` is the endpoint identifying a specific user.
- `123` is the user's ID.

The client is asking the server to return information about that user.

## 2. Common HTTP Methods

The following table summarizes the HTTP methods commonly used in REST APIs.

| Method | Purpose | Example |
|---|---|---|
| GET | Retrieve a resource | `GET /users/123` |
| POST | Create a resource or submit data for processing | `POST /users` |
| PUT | Replace a resource | `PUT /users/123` |
| PATCH | Partially update a resource | `PATCH /users/123` |
| DELETE | Delete a resource | `DELETE /users/123` |
| HEAD | Retrieve response headers without the response body | `HEAD /users/123` |
| OPTIONS | Discover supported communication options | `OPTIONS /users` |

The exact behavior of an endpoint depends on how the API is designed and documented.

## 3. GET: Retrieve Data

The `GET` method requests information from a server. It is commonly used to retrieve a list of resources or details about a specific resource.

### Example: Get a User

```http
GET /users/123
```

A successful response might look like this:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "id": 123,
  "name": "Alex Morgan",
  "email": "alex@example.com"
}
```

**When to use GET:**
- Retrieve a user's profile.
- View a list of products.
- Search for available records.

GET requests should not be used to create, update, or delete resources.

## 4. POST: Create or Submit Data

The `POST` method sends data to a server. It is often used to create a new resource or submit information for processing.

### Example: Create a User

```http
POST /users
Content-Type: application/json
```

Request body:

```json
{
  "name": "Alex Morgan",
  "email": "alex@example.com"
}
```

If the API creates the user successfully, it might return:

```http
HTTP/1.1 201 Created
Content-Type: application/json
```

```json
{
  "id": 124,
  "name": "Alex Morgan",
  "email": "alex@example.com"
}
```

The server has created a new user and assigned the resource an ID.

**When to use POST:**
- Register a new user.
- Submit an order.
- Send information to a processing endpoint.

POST is not generally idempotent. Sending the same request multiple times may create multiple resources or trigger the same operation repeatedly.

## 5. PUT: Replace a Resource

The `PUT` method creates or replaces the state of a resource at a known URL, depending on the API's design.

### Example: Update a User

```http
PUT /users/123
Content-Type: application/json
```

Request body:

```json
{
  "name": "Alex Morgan",
  "email": "alex.morgan@example.com"
}
```

The server processes the submitted representation according to the endpoint's rules.

**When to use PUT:**
- Replace a user's profile details.
- Submit a complete representation of an existing resource.

A PUT request is generally expected to include the complete representation that the endpoint uses for replacement. Check the API documentation to understand which fields are required.

## 6. PATCH: Partially Update a Resource

The `PATCH` method updates part of an existing resource. Unlike a typical PUT request, it does not necessarily require the client to send every field.

### Example: Update Only an Email Address

```http
PATCH /users/123
Content-Type: application/json
```

Request body:

```json
{
  "email": "alex.morgan@example.com"
}
```

The API updates the email address while leaving other fields unchanged, assuming that is the endpoint's documented behavior.

**When to use PATCH:**
- Change a user's email address.
- Update a product's price.
- Modify a single setting.

The exact format of a PATCH request depends on the API. Some APIs accept a simple JSON object, while others require a specific patch format.

## 7. DELETE: Remove a Resource

The `DELETE` method requests the removal of a resource.

### Example: Delete a User

```http
DELETE /users/123
```

A successful response might be:

```http
HTTP/1.1 204 No Content
```

A `204 No Content` response indicates that the request succeeded and there is no response body to return.

**When to use DELETE:**
- Remove a user account.
- Delete a product record.
- Remove an item from a collection.

The actual result depends on the API. Some systems use soft deletion, which marks a resource as deleted instead of immediately removing it permanently.

## 8. HEAD and OPTIONS

Two additional HTTP methods can be useful when working with APIs.

### HEAD

`HEAD` works similarly to `GET`, but the server does not return a response body.

Example:

```http
HEAD /users/123
```

It can help a client check response headers or resource availability without downloading the full representation. The endpoint must support HEAD for this request to be useful.

### OPTIONS

`OPTIONS` asks the server about the communication options available for a resource.

Example:

```http
OPTIONS /users
```

The server may indicate supported methods in an `Allow` response header:

```http
HTTP/1.1 200 OK
Allow: GET, POST, OPTIONS
```

The actual response and supported methods depend on the server's configuration.

OPTIONS requests are also relevant to CORS preflight checks made by web browsers.

## 9. Safe and Idempotent Methods

Two HTTP concepts help explain how methods behave when requests are repeated.

### Safe methods

A safe method is intended to retrieve information without changing the resource's state.

`GET`, `HEAD`, and `OPTIONS` are defined as safe methods.

### Idempotent methods

An idempotent method has the same intended effect on the server when the same request is repeated, assuming the relevant conditions remain the same.

| Method | Safe? | Idempotent? |
|---|---|---|
| GET | Yes | Yes |
| HEAD | Yes | Yes |
| OPTIONS | Yes | Yes |
| POST | No | Not generally |
| PUT | No | Yes |
| PATCH | No | Depends on the operation |
| DELETE | No | Yes |

For example, repeating a DELETE request should leave the resource deleted. However, the response to a repeated request may differ: the API might return `404 Not Found` after the resource has already been removed.

Idempotency describes the intended effect, not necessarily an identical response every time.

## 10. Common HTTP Method Errors

Using the wrong method can cause an API request to fail.

| Status code | Possible reason | What to check |
|---|---|---|
| 400 Bad Request | The request is invalid | Check the request body and parameters |
| 401 Unauthorized | Authentication is missing or invalid | Check the API key or access token |
| 404 Not Found | The endpoint or resource does not exist | Verify the URL and resource ID |
| 405 Method Not Allowed | The endpoint does not support the method | Check the API documentation for supported methods |
| 415 Unsupported Media Type | The request format is not supported | Check the `Content-Type` header |

These are common explanations, not the only possible causes. Consult the API's error response and documentation for details.

## 11. Best Practices

Follow these practices when working with HTTP methods:

- **Choose the correct method.** Use GET to retrieve data, POST to submit or create data, PUT to replace a resource, PATCH to update part of a resource, and DELETE to request removal.
- **Read the API documentation.** Endpoints may have specific requirements for methods, fields, and request formats.
- **Check request headers.** When sending JSON, APIs commonly expect `Content-Type: application/json`.
- **Inspect the response.** Check the status code and response body to understand the result.
- **Handle errors carefully.** Do not assume that every request succeeds.
- **Protect sensitive operations.** Use appropriate authentication and authorization before modifying or deleting data.

## Summary

HTTP methods describe the actions a client wants an API to perform. GET retrieves data, POST submits data, PUT replaces a resource, PATCH partially updates it, and DELETE requests its removal. HEAD and OPTIONS provide additional ways to inspect resources and supported communication options.

Choosing the correct method and following the API's documentation makes requests easier to understand, test, and troubleshoot.

## Related Guides

- [Getting Started with REST APIs](getting-started.md) — Learn the basics of REST APIs.
- [Requests and Responses](requests-and-responses.md) — Understand how API requests and responses work.
- [HTTP Status Codes](status-codes.md) — Learn what API status codes mean.
- [API Authentication](authentication.md) — Understand how APIs verify client identity.
- [Troubleshooting REST APIs](troubleshooting.md) — Find and resolve common API errors.

[Back to the README](README.md)
