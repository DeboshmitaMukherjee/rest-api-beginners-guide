
# Understanding HTTP Status Codes

## Introduction

When a client sends a request to a REST API, the server returns an HTTP response. One important part of that response is the **HTTP status code**, which indicates the general result of the request.

For example, a status code can indicate that a request succeeded, the requested resource could not be found, or the server encountered an error.

Understanding status codes helps developers interpret API responses, identify problems, and troubleshoot requests more effectively.

## 1. What Are HTTP Status Codes?

HTTP status codes are three-digit numbers included in HTTP responses.

For example:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

In this response:

- `200` is the status code.
- `OK` is the reason phrase.
- `Content-Type` indicates the format of the response body.

Status codes are grouped into five categories based on the type of response.

## 2. The Five Categories of Status Codes

| Code range | Category | Meaning |
|---|---|---|
| 1xx | Informational | The request is still in progress or additional protocol information is being provided |
| 2xx | Success | The request was successfully received, understood, and handled |
| 3xx | Redirection | Further action is needed to complete the request |
| 4xx | Client error | The request has a problem or cannot be fulfilled as sent |
| 5xx | Server error | The server failed to fulfill an apparently valid request |

The exact meaning of a response depends on the status code and the API's behavior.

## 3. Success Status Codes (2xx)

Success codes indicate that the server successfully handled the request.

### 200 OK

The request succeeded.

Example: A client requests a user's details.

```http
GET /users/123
```

Response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "id": 123,
  "name": "Alex Morgan"
}
```

### 201 Created

The request succeeded and created a new resource.

Example: A client creates a user with a POST request.

```http
POST /users
Content-Type: application/json
```

```json
{
  "name": "Alex Morgan",
  "email": "alex@example.com"
}
```

The server may return `201 Created` along with the new resource's details.

### 202 Accepted

The server accepted the request for processing, but processing may not be complete yet.

This status is useful for operations that run in the background.

### 204 No Content

The request succeeded, but the response does not contain a body.

Example: An API successfully processes a DELETE request and returns `204 No Content`.

## 4. Client Error Status Codes (4xx)

Client error codes indicate that the request could not be fulfilled as sent. The response body may provide more information about the problem.

### 400 Bad Request

The server cannot process the request because it considers the request invalid.

**Possible causes:**
- Missing required fields.
- Invalid JSON syntax.
- Incorrect query parameters.

**What to check:** Review the request body, parameters, and error message.

### 401 Unauthorized

The request lacks valid authentication credentials.

**Possible causes:**
- An API key is missing.
- An access token is invalid.
- A token has expired.

**What to check:** Verify the credentials and follow the API's authentication instructions.

### 403 Forbidden

The server understood the request but refuses to authorize it.

**Possible causes:**
- The account lacks the required permissions.
- The user does not have access to the requested resource.

**What to check:** Confirm that the authenticated account has permission to perform the operation.

**Remember:** A `401` generally relates to authentication, while a `403` generally relates to permission. Some APIs may handle these cases differently.

### 404 Not Found

The requested resource or endpoint could not be found.

**Possible causes:**
- The URL is incorrect.
- The resource ID does not exist.
- The resource has been removed.

**What to check:** Verify the endpoint path, resource ID, and API documentation.

### 405 Method Not Allowed

The endpoint does not support the HTTP method used in the request.

**Example:** Sending `DELETE` to an endpoint that only supports `GET`.

**What to check:** Confirm which methods the endpoint supports.

### 409 Conflict

The request conflicts with the current state of the resource.

For example, an API may reject an attempt to create a username that already exists.

**What to check:** Review the response details and resolve the conflict before retrying.

### 415 Unsupported Media Type

The server does not support the format of the request body.

**Possible cause:** The client sends JSON but uses an incorrect `Content-Type` header.

**What to check:** Confirm the expected content type. For JSON requests, APIs commonly use:

```http
Content-Type: application/json
```

### 422 Unprocessable Content

The server understands the request format but cannot process the submitted instructions or data.

For example, the request may contain an email address that fails the API's validation rules.

**What to check:** Review the response body for field-level validation errors.

### 429 Too Many Requests

The client has sent too many requests within a given period.

**What to check:**
- Review the API's rate limits.
- Look for a `Retry-After` header.
- Wait before retrying.
- Use an appropriate retry strategy.

## 5. Server Error Status Codes (5xx)

Server error codes indicate that the server failed to complete a request.

### 500 Internal Server Error

The server encountered an unexpected condition.

**What to check:** Review the response body, request details, and server logs if you have access to them.

### 502 Bad Gateway

A server acting as a gateway or proxy received an invalid response from an upstream server.

**What to check:** The problem may involve an upstream service or gateway configuration.

### 503 Service Unavailable

The server is temporarily unable to handle the request.

Possible reasons include maintenance or temporary overload.

**What to check:** Look for service-status information and any `Retry-After` header. Retry only when appropriate.

### 504 Gateway Timeout

A gateway or proxy did not receive a timely response from an upstream server.

**What to check:** The upstream service may be slow or unavailable. Check the service status and retry according to the API's guidance.

## 6. How to Troubleshoot an API Response

When an API request fails, follow these steps:

1. **Read the status code.** Identify whether the response indicates success, a client error, or a server error.
2. **Inspect the response body.** The API may provide an error message or additional details.
3. **Check the request URL.** Verify the endpoint and resource ID.
4. **Review the HTTP method.** Make sure the endpoint supports the method you used.
5. **Verify authentication.** Check whether the request requires a valid API key or access token.
6. **Validate the request data.** Confirm that required fields, data types, and headers are correct.
7. **Check rate limits and service status.** Look for retry instructions or service outages.
8. **Retry carefully.** Do not repeatedly resend a request that may create duplicate records or perform an operation more than once.

## 7. Example: Troubleshooting a Failed Request

Suppose you send this request:

```http
GET /users/999
Authorization: Bearer YOUR_ACCESS_TOKEN
```

The API responds:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json
```

```json
{
  "error": "User not found"
}
```

Here is how to investigate:

- Confirm that the endpoint is spelled correctly.
- Check whether user `999` exists.
- Verify that you are using the correct environment, such as testing or production.
- Read the API documentation to understand how missing resources are handled.

The `404` response does not necessarily mean the entire API is unavailable. It indicates that the requested resource or endpoint was not found.

## 8. Best Practices

- **Read the full response.** Do not rely on the status code alone when the API provides additional error details.
- **Handle errors in your application.** Display useful messages instead of exposing raw technical errors to end users.
- **Avoid unnecessary retries.** Repeated requests can create duplicate operations or worsen rate-limit problems.
- **Follow retry guidance.** Respect `Retry-After` headers and the API's documented limits.
- **Protect sensitive information.** Avoid including access tokens, passwords, or personal data in logs.
- **Consult the API documentation.** An API may define specific error responses for its endpoints.

## Summary

HTTP status codes help clients understand how a server handled a request. The `2xx` category indicates success, `3xx` indicates redirection, `4xx` indicates a client error, and `5xx` indicates a server error.

Learning to interpret these codes makes it easier to test APIs, diagnose failures, and build applications that handle responses correctly.

## Related Guides

- [Getting Started with REST APIs](getting-started.md) — Learn the basics of REST APIs.
- [HTTP Methods](http-methods.md) — Learn how HTTP methods work.
- [Requests and Responses](requests-and-responses.md) — Understand API requests and responses.
- [API Authentication](authentication.md) — Understand how APIs verify client identity.
- [Troubleshooting REST APIs](troubleshooting.md) — Learn how to investigate common API errors.

[Back to the README](README.md)
