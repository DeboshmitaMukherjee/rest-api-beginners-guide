
# HTTP Status Codes in REST APIs

HTTP status codes tell a client what happened when a server received and processed an API request.

When working with REST APIs, checking the status code is one of the easiest ways to understand whether a request succeeded or encountered a problem.

For example, a response containing `200 OK` usually means the request was successful, while `404 Not Found` means the requested resource could not be found.

This guide explains the main categories of HTTP status codes, commonly used codes, practical examples, and ways to troubleshoot errors.

## What Are HTTP Status Codes?

An HTTP status code is a three-digit number returned by a server in response to an HTTP request.

The code provides information about the result of the request. Depending on the result, the server may also return response headers and a response body containing data or an error message.

For example, a request to retrieve a user might look like this:

~~~http
GET /users/123
~~~

A successful response could be:

~~~http
HTTP/1.1 200 OK
Content-Type: application/json
~~~

Response body:

~~~json
{
  "id": 123,
  "name": "John Smith",
  "email": "john@example.com"
}
~~~

The status code `200` indicates that the request succeeded. The response body contains the requested user's information.

## Categories of HTTP Status Codes

HTTP status codes are divided into five categories. The first digit indicates the general type of result.

| Code Range | Category | Meaning |
|---|---|---|
| `100–199` | Informational | The request has been received and the process is continuing |
| `200–299` | Success | The request was successfully received, understood, and processed |
| `300–399` | Redirection | Further action is needed, often to access a different URL |
| `400–499` | Client Error | The request contains a problem or cannot be fulfilled as sent |
| `500–599` | Server Error | The server failed to complete an apparently valid request |

Most beginners working with REST APIs will frequently encounter success codes in the `200` range and error codes in the `400` and `500` ranges.

## 2xx: Success Status Codes

A `2xx` status code indicates that the request was successfully processed. The exact meaning depends on the specific code.

### 200 OK

The `200 OK` status code indicates that the request succeeded.

It is commonly returned when retrieving information from an API.

Example request:

~~~http
GET /products/45
~~~

Example response:

~~~http
HTTP/1.1 200 OK
Content-Type: application/json
~~~

~~~json
{
  "id": 45,
  "name": "Wireless Keyboard",
  "price": 39.99
}
~~~

**When you see it:** The API successfully processed the request. Check the response body to access the returned information.

### 201 Created

The `201 Created` status code indicates that the request successfully created a new resource.

It is commonly returned after a successful `POST` request.

Example request:

~~~http
POST /users
Content-Type: application/json
~~~

Request body:

~~~json
{
  "name": "Sarah Lee",
  "email": "sarah@example.com"
}
~~~

Example response:

~~~http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /users/125
~~~

~~~json
{
  "id": 125,
  "name": "Sarah Lee",
  "email": "sarah@example.com"
}
~~~

**When you see it:** The server has created a resource. The response may include the new resource's details or a `Location` header identifying it.

### 204 No Content

The `204 No Content` status code indicates that the request succeeded, but the server has no response body to return.

It is commonly used after a successful deletion or update.

Example request:

~~~http
DELETE /users/125
~~~

Example response:

~~~http
HTTP/1.1 204 No Content
~~~

**When you see it:** The operation succeeded. Do not expect a response body containing JSON.

## 3xx: Redirection Status Codes

A `3xx` status code indicates that additional action may be needed to complete the request, often because the requested resource is available at a different URL.

### 301 Moved Permanently

The `301 Moved Permanently` status code indicates that a resource has permanently moved to another URL.

A response may include a `Location` header containing the new URL.

~~~http
HTTP/1.1 301 Moved Permanently
Location: https://api.example.com/v2/users
~~~

**When you see it:** Check the new URL and update your request if necessary.

### 304 Not Modified

The `304 Not Modified` status code is used with conditional requests to indicate that a resource has not changed since the version the client has.

The client can reuse its cached copy instead of downloading the resource again.

**When you see it:** The response usually means the cached version can still be used. A `304` response does not normally include a response body.

## 4xx: Client Error Status Codes

A `4xx` status code indicates that something is wrong with the request or that the client cannot access the requested resource as sent.

These codes are particularly useful when troubleshooting API requests.

### 400 Bad Request

The `400 Bad Request` status code indicates that the server cannot process the request because it considers the request invalid.

Possible causes include:

- Missing required fields
- Invalid JSON syntax
- Incorrect parameter values
- Malformed request data

Example request:

~~~http
POST /users
Content-Type: application/json
~~~

Request body with invalid JSON:

~~~json
{
  "name": "Sarah Lee"
  "email": "sarah@example.com"
}
~~~

The comma between the two fields is missing.

**How to troubleshoot it:**

1. Check the request body for syntax errors.
2. Verify that all required fields are included.
3. Check the API documentation for accepted values and formats.
4. Read the error message returned by the server.

### 401 Unauthorized

The `401 Unauthorized` status code indicates that valid authentication credentials are missing or were not accepted.

For example, an API may require an access token.

~~~http
GET /account
Authorization: Bearer your_access_token
~~~

If the token is missing, invalid, or expired, the server may return `401 Unauthorized`.

**How to troubleshoot it:**

1. Check whether the endpoint requires authentication.
2. Verify that the token is included in the request.
3. Check whether the token has expired.
4. Follow the API provider's instructions for obtaining or refreshing credentials.

### 403 Forbidden

The `403 Forbidden` status code indicates that the server understood the request but refuses to authorize it.

The client may be authenticated but lack permission to perform the requested action.

For example, a regular user may not have permission to access an administrator-only endpoint.

**How to troubleshoot it:**

1. Confirm that the correct account or token is being used.
2. Check the permissions or roles assigned to that account.
3. Review the API documentation for access requirements.
4. Contact the API administrator if the required permission is missing.

**Remember:** `401` generally relates to authentication, while `403` generally means the request is understood but access is not permitted.

### 404 Not Found

The `404 Not Found` status code indicates that the server cannot find the requested resource.

Example request:

~~~http
GET /users/9999
~~~

Example response:

~~~http
HTTP/1.1 404 Not Found
Content-Type: application/json
~~~

~~~json
{
  "error": "User not found"
}
~~~

Possible causes include:

- The resource does not exist.
- The resource ID is incorrect.
- The endpoint URL contains a spelling mistake.
- The resource is no longer available.

**How to troubleshoot it:**

1. Check the endpoint and resource ID.
2. Confirm that the resource exists.
3. Verify the URL against the API documentation.
4. Check whether the resource is accessible to your account.

### 405 Method Not Allowed

The `405 Method Not Allowed` status code indicates that the HTTP method is not supported for the requested resource.

For example, an API might allow retrieving users through `GET` but not allow deleting them through the same endpoint.

~~~http
DELETE /reports
~~~

If deletion is not supported for that endpoint, the server may return `405 Method Not Allowed`.

**How to troubleshoot it:**

1. Check which HTTP methods the endpoint supports.
2. Verify that the method matches the operation you want to perform.
3. Review the endpoint documentation.
4. Check the `Allow` response header if the server provides it.

### 409 Conflict

The `409 Conflict` status code indicates that the request conflicts with the current state of the resource.

For example, an API may reject a request to create a username that already exists.

~~~http
POST /users
Content-Type: application/json
~~~

~~~json
{
  "username": "sarahlee"
}
~~~

The API might return `409 Conflict` if that username is already in use.

**How to troubleshoot it:**

1. Read the response body for details about the conflict.
2. Check whether the resource or value already exists.
3. Update the request to resolve the conflict.
4. Retry the request only after addressing the cause.

### 422 Unprocessable Content

The `422 Unprocessable Content` status code indicates that the server understands the request format but cannot process the submitted instructions or data.

For example, an API may reject a registration request because an email address does not meet its validation rules.

~~~json
{
  "email": "not-an-email"
}
~~~

**How to troubleshoot it:**

1. Read the validation errors in the response.
2. Check the required format for each field.
3. Correct the invalid values.
4. Submit the request again.

APIs differ in how they use `400` and `422`. Check the API documentation to understand which code it returns for validation errors.

### 429 Too Many Requests

The `429 Too Many Requests` status code indicates that the client has sent too many requests within a specified period.

APIs may limit requests to protect their services and maintain performance.

**How to troubleshoot it:**

1. Check whether the API has a rate limit.
2. Look for a `Retry-After` header that indicates when to retry.
3. Reduce the frequency of requests.
4. Use a delay between retries rather than repeatedly sending requests immediately.

## 5xx: Server Error Status Codes

A `5xx` status code indicates that the server failed to complete a request.

The request may be valid, but a server-side problem prevents successful processing.

### 500 Internal Server Error

The `500 Internal Server Error` status code indicates that the server encountered an unexpected problem.

Possible causes include:

- An unhandled application error
- A database problem
- An unexpected condition in the server code

**How to troubleshoot it:**

1. Check the response body for any useful error details.
2. Check the API provider's service-status page if one is available.
3. Retry later if the problem appears temporary.
4. Contact the API provider if the error continues.

Avoid repeatedly sending the same request without checking whether the operation may already have been completed.

### 502 Bad Gateway

The `502 Bad Gateway` status code indicates that a server acting as a gateway or proxy received an invalid response from an upstream server.

This can happen when one service depends on another service that is unavailable or responding incorrectly.

**How to troubleshoot it:**

- Check the API provider's status page.
- Retry after a short delay if appropriate.
- Contact the provider if the problem persists.

### 503 Service Unavailable

The `503 Service Unavailable` status code indicates that the server is temporarily unable to handle the request.

Possible causes include maintenance, temporary overload, or service downtime.

**How to troubleshoot it:**

1. Check for maintenance notices or service outages.
2. Look for a `Retry-After` header.
3. Wait before trying again.
4. Avoid sending repeated requests in rapid succession.

### 504 Gateway Timeout

The `504 Gateway Timeout` status code indicates that a gateway or proxy did not receive a timely response from an upstream server.

The request may be taking too long because of server load or a problem with a dependent service.

**How to troubleshoot it:**

- Wait briefly before retrying.
- Check whether the API provider reports an outage.
- Review any request timeout settings you control.
- Contact the provider if the problem continues.

## Common HTTP Status Codes at a Glance

Use this table as a quick reference when reading API responses.

| Status Code | Name | Typical Meaning |
|---|---|---|
| `200` | OK | Request succeeded |
| `201` | Created | Resource was created |
| `204` | No Content | Request succeeded without a response body |
| `301` | Moved Permanently | Resource has moved to a new URL |
| `304` | Not Modified | Cached resource can be reused |
| `400` | Bad Request | Request is invalid |
| `401` | Unauthorized | Authentication is missing or invalid |
| `403` | Forbidden | Access is not permitted |
| `404` | Not Found | Resource could not be found |
| `405` | Method Not Allowed | HTTP method is unsupported for the resource |
| `409` | Conflict | Request conflicts with the resource's current state |
| `422` | Unprocessable Content | Request data cannot be processed |
| `429` | Too Many Requests | Rate limit has been exceeded |
| `500` | Internal Server Error | Unexpected server error |
| `502` | Bad Gateway | Invalid response from an upstream server |
| `503` | Service Unavailable | Server is temporarily unavailable |
| `504` | Gateway Timeout | Upstream server did not respond in time |

These are common uses, but the exact response codes and error details depend on the API.

## How to Troubleshoot an API Status Code

When an API request fails, follow a consistent process instead of guessing at the cause.

### Step 1: Read the Status Code

Start by identifying the returned code.

For example, `401` suggests an authentication problem, while `404` suggests that the requested resource could not be found.

### Step 2: Read the Response Body

Some APIs return an error message that explains the problem.

Example:

~~~json
{
  "error": "Missing required field: email"
}
~~~

This message indicates that the request needs an `email` field.

Not every API provides detailed error messages, so you may need to check its documentation.

### Step 3: Check the Request

Review the HTTP method, endpoint, headers, query parameters, and request body.

Make sure the request follows the API's requirements.

### Step 4: Check Authentication and Permissions

If you receive `401` or `403`, verify your credentials and access permissions.

Do not share access tokens or other sensitive credentials when requesting help.

### Step 5: Retry Carefully

Temporary server errors may resolve themselves, but not every request should be retried immediately.

For example, if a request creates a payment or another important resource, confirm whether the original operation succeeded before sending it again. Follow the API's retry and idempotency guidance.

For rate limits, respect any `Retry-After` header.

## Common Mistakes to Avoid

When working with HTTP status codes, keep the following points in mind:

- **Do not assume every `2xx` response contains JSON.** A `204 No Content` response has no response body.
- **Do not treat `401` and `403` as identical.** Authentication and permission problems require different checks.
- **Do not assume every error is caused by the client.** A `5xx` response indicates a server-side failure.
- **Do not rely only on the status code.** Read the response body and headers when available.
- **Do not retry every failed request immediately.** Some operations may create duplicate records or repeat an action.
- **Do not assume every API uses the same error format.** Always consult the relevant API documentation.

## Summary

HTTP status codes help clients understand the result of an API request.

The five categories are:

- `1xx`: Informational responses
- `2xx`: Successful requests
- `3xx`: Redirection
- `4xx`: Client errors
- `5xx`: Server errors

When troubleshooting an API, check the status code, inspect the response body, verify the request, and consult the API documentation.

Understanding status codes makes it easier to identify problems, interpret responses, and work more effectively with REST APIs.

## What's Next?

Continue with **[Authentication](authentication.md)** to learn how APIs verify a client's identity, use access tokens, and control access to protected resources.
