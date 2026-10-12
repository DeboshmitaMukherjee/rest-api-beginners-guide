# Troubleshooting Common REST API Errors

When working with REST APIs, requests may fail for several reasons. The endpoint might be incorrect, authentication credentials might be missing, or the server might be temporarily unavailable.

Troubleshooting helps you identify the cause of an error and determine what to check next.

This guide explains common API problems, their possible causes, and practical steps for resolving them.

## What Is API Troubleshooting?

API troubleshooting is the process of identifying and resolving problems that occur when a client communicates with an API.

When a request fails, start by checking:

- The HTTP status code.
- The request URL and HTTP method.
- The request headers.
- The request body.
- The authentication credentials.
- The error message returned by the API.

Checking these details systematically can help you identify the problem without making unnecessary changes.

## Common HTTP Status Codes

HTTP status codes provide useful information about the result of an API request.

| Status Code | Meaning | What to Check |
|---|---|---|
| `400 Bad Request` | The request contains invalid information | Check the request body, parameters, and required fields |
| `401 Unauthorized` | Authentication is missing or invalid | Check the API key, access token, or credentials |
| `403 Forbidden` | The client is not allowed to perform the operation | Check permissions and access policies |
| `404 Not Found` | The requested resource or endpoint was not found | Verify the URL and resource ID |
| `405 Method Not Allowed` | The HTTP method is not supported for the endpoint | Check which methods the API supports |
| `409 Conflict` | The request conflicts with the current resource state | Check whether the resource already exists or has changed |
| `415 Unsupported Media Type` | The request uses an unsupported content type | Check the `Content-Type` header |
| `429 Too Many Requests` | The client has exceeded a request limit | Check rate limits and retry instructions |
| `500 Internal Server Error` | The server encountered an unexpected problem | Check the error details and try again later if appropriate |
| `503 Service Unavailable` | The service is temporarily unable to handle the request | Check service status and retry guidance |

The exact status codes and error details depend on the API.

For a broader explanation, see [Status Codes](status-codes.md).

## 1. 400 Bad Request

A `400 Bad Request` response means the server cannot process the request because it considers the request invalid.

### Common Causes

- A required field is missing.
- The JSON body contains invalid syntax.
- A query parameter has an invalid value.
- The request does not follow the API's documented format.

### Example

Incorrect JSON:

```json
{
  "name": "Sarah Lee"
  "email": "sarah@example.com"
}
```

The comma between the `name` and `email` fields is missing.

Correct JSON:

```json
{
  "name": "Sarah Lee",
  "email": "sarah@example.com"
}
```

### How to Fix It

1. Check the request body for syntax errors.
2. Confirm that all required fields are included.
3. Verify the parameter names and values.
4. Compare your request with the API documentation.

## 2. 401 Unauthorized

A `401 Unauthorized` response commonly means the request does not include valid authentication credentials.

### Common Causes

- The access token is missing.
- The API key is incorrect.
- The access token has expired.
- The `Authorization` header is incorrectly formatted.

### Example

A request to a protected endpoint might require:

```http
GET /users/123
Authorization: Bearer your_access_token
```

If the token is missing or invalid, the API may reject the request.

### How to Fix It

1. Confirm which authentication method the API requires.
2. Check that the required header is present.
3. Verify that the credentials are valid and have not expired.
4. Obtain a new token if necessary.

For more details, see [Authentication](authentication.md).

## 3. 403 Forbidden

A `403 Forbidden` response means the server understands the request but refuses to allow the operation.

The client may be authenticated but lack the required permission.

### Common Causes

- The account does not have the necessary role.
- The token does not include the required permissions.
- The requested operation is restricted.
- An access policy blocks the request.

### How to Fix It

1. Check the permissions assigned to the account or application.
2. Verify that the token grants access to the requested resource.
3. Confirm that the operation is permitted for the current user.
4. Contact the API administrator if access should be allowed.

Changing the URL or repeatedly resending the same request will not resolve a genuine permission restriction.

## 4. 404 Not Found

A `404 Not Found` response indicates that the requested resource or endpoint could not be found.

### Common Causes

- The endpoint is misspelled.
- The resource ID does not exist.
- The URL contains an incorrect path.
- The resource has been removed.

### Example

Incorrect endpoint:

```http
GET /user/123
```

Expected endpoint, if defined by the API:

```http
GET /users/123
```

The singular and plural paths may represent different endpoints.

### How to Fix It

1. Compare the URL with the API documentation.
2. Check the spelling and capitalization of the path.
3. Verify that the resource ID is correct.
4. Confirm that the resource exists and is accessible through that endpoint.

## 5. 405 Method Not Allowed

A `405 Method Not Allowed` response indicates that the server does not support the HTTP method used for the requested resource.

### Example

An endpoint may support retrieving users with:

```http
GET /users
```

However, it might not support:

```http
DELETE /users
```

The API may allow deletion only for an individual user endpoint, or it may not support deletion at all.

### How to Fix It

1. Check which HTTP methods the endpoint supports.
2. Verify that you are using the correct method.
3. Confirm that the endpoint is designed for the intended operation.

See [HTTP Methods](http-methods.md) for more information.

## 6. 415 Unsupported Media Type

A `415 Unsupported Media Type` response means the server does not accept the format of the request data.

### Common Cause

The client sends JSON data but specifies an incorrect content type.

For example:

```http
POST /users
Content-Type: text/plain
```

If the API expects JSON, use:

```http
POST /users
Content-Type: application/json
```

Request body:

```json
{
  "name": "Sarah Lee",
  "email": "sarah@example.com"
}
```

### How to Fix It

1. Check the supported request formats.
2. Set the correct `Content-Type` header.
3. Confirm that the request body matches the declared format.

## 7. 429 Too Many Requests

A `429 Too Many Requests` response means the client has sent more requests than the API currently allows.

Many APIs apply rate limits to protect their services and manage traffic.

### Common Causes

- Sending requests too frequently.
- Running repeated requests in a loop.
- Exceeding the account's usage limit.
- Sending too many requests from multiple application processes.

### How to Fix It

1. Check the API's rate-limit documentation.
2. Look for a `Retry-After` response header.
3. Wait before sending another request when instructed.
4. Reduce unnecessary requests.
5. Use an appropriate retry strategy with delays between attempts.

Avoid repeatedly retrying immediately because this can make the problem worse.

## 8. 500 Internal Server Error

A `500 Internal Server Error` response means the server encountered an unexpected condition while processing the request.

The problem may be on the server side rather than in the client request.

### How to Fix It

1. Check whether the API provides an error message or request ID.
2. Confirm that the request follows the documented format.
3. Check the API provider's service-status page, if available.
4. Retry later when appropriate.
5. Contact the API provider if the error continues.

Do not assume that every `500` error can be fixed by changing the request. The API provider may need to investigate the server.

## 9. 503 Service Unavailable

A `503 Service Unavailable` response indicates that the service cannot currently handle the request.

Possible causes include maintenance, temporary overload, or service interruptions.

### How to Fix It

1. Check the service-status page, if one is available.
2. Look for retry instructions in the response.
3. Wait before retrying if the API is temporarily unavailable.
4. Use controlled retries rather than sending repeated requests immediately.

If the service remains unavailable, contact the API provider or check its official announcements.

## Troubleshooting Checklist

Use this checklist when an API request fails.

- [ ] Verify the endpoint URL.
- [ ] Confirm the HTTP method.
- [ ] Check required headers.
- [ ] Validate the JSON request body.
- [ ] Confirm that required parameters are present.
- [ ] Check authentication credentials.
- [ ] Read the HTTP status code.
- [ ] Review the response body for error details.
- [ ] Check rate limits and retry instructions.
- [ ] Confirm whether the API service is available.

Work through the checklist one item at a time. This makes it easier to isolate the cause of the problem.

## Example: Troubleshooting a Failed Request

Suppose you send this request:

```http
GET /users/123
Authorization: Bearer your_access_token
Accept: application/json
```

The API returns:

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json
```

The response body might contain:

```json
{
  "error": "invalid_token",
  "message": "The access token is missing, invalid, or expired."
}
```

Follow these steps:

1. Identify the status code: `401 Unauthorized`.
2. Read the error message.
3. Check that the `Authorization` header is present and correctly formatted.
4. Verify that the token is valid and has not expired.
5. Retry the request with valid credentials.

If the request succeeds, the response may contain the requested user information.

This example demonstrates why checking both the status code and response body is useful.

## Best Practices for API Troubleshooting

### Read the API Documentation

The documentation explains required parameters, supported methods, authentication requirements, and expected responses.

### Check One Thing at a Time

Avoid changing several parts of a request simultaneously. Change one suspected cause, test again, and observe the result.

### Use API Testing Tools

Tools such as Postman or curl can help you inspect requests, headers, status codes, and response bodies.

### Protect Sensitive Information

Do not publish real API keys, passwords, access tokens, or private customer data when sharing error reports.

### Record Useful Error Details

When reporting a problem, include the endpoint, HTTP method, status code, relevant error message, and steps to reproduce the issue. Remove credentials and sensitive information first.

## Summary

REST API errors can occur because of invalid requests, authentication problems, insufficient permissions, rate limits, or server-side issues.

To troubleshoot effectively:

- Identify the status code.
- Read the response body.
- Verify the endpoint and HTTP method.
- Check headers, parameters, and request data.
- Confirm authentication and permissions.
- Follow the API provider's retry instructions.

A systematic troubleshooting process helps you identify likely causes and resolve API problems more efficiently.

## What's Next?

Return to the [REST API Beginner's Guide](README.md) to review the documentation topics and revisit any concepts you want to practice.
