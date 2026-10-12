
# Authentication in REST APIs

Authentication is an important part of working with REST APIs. It helps an API verify the identity of a client before allowing access to protected resources.

For example, a weather API may allow anyone to view basic weather information, while a banking API requires valid credentials before users can access account details.

This guide explains how API authentication works, common authentication methods, and best practices for protecting credentials.

## What Is API Authentication?

**Authentication** is the process of verifying the identity of a client making an API request.

A client could be a web application, mobile application, another server, or a developer using an API testing tool.

An API may require authentication to protect sensitive information and prevent unauthorized access.

For example, a client might send this request to retrieve account information:

```http
GET /accounts/123
```

If authentication is required, the API may reject the request because it does not include valid credentials.

The client might need to send an access token:

```http
GET /accounts/123
Authorization: Bearer your_access_token
```

The API validates the token and checks whether the client is allowed to access the requested resource.

**Note:** `your_access_token` is a placeholder, not a real credential.

## Authentication vs. Authorization

Authentication and authorization are related, but they have different purposes.

| Term | Meaning | Example |
|---|---|---|
| Authentication | Verifies identity | Checking a user's login credentials |
| Authorization | Determines access permissions | Allowing a user to view their own account |

For example, a user may successfully authenticate with a banking API but still be denied access to another customer's account.

Authentication confirms identity. Authorization determines what that identity is permitted to do.

## Why Do REST APIs Require Authentication?

APIs may provide access to private information or operations that should not be available to everyone.

Authentication helps API providers:

- Protect private user information.
- Restrict access to sensitive operations.
- Identify applications making requests.
- Reduce unauthorized access.
- Support security monitoring and access control.

The authentication method depends on the API's purpose and security requirements.

## Common API Authentication Methods

REST APIs can use different methods to verify clients. The following are common approaches.

### 1. API Keys

An **API key** is a value used to identify a client or application when it makes an API request.

Example:

```http
GET /products
X-API-Key: your_api_key
```

The API checks the key before processing the request.

API keys are commonly used for:

- Identifying applications.
- Tracking API usage.
- Applying usage limits.
- Controlling access to certain services.

An API key does not automatically verify an individual user's identity. Some APIs use keys mainly to identify an application.

**Security tip:** Keep private API keys confidential. Never publish real keys in a public GitHub repository.

### 2. Bearer Tokens

A **bearer token** is a credential that allows whoever possesses it to access resources permitted by that token.

Bearer tokens are commonly sent in the `Authorization` header.

Example:

```http
GET /users/123
Authorization: Bearer your_access_token
```

The API validates the token and checks whether the request is permitted.

Bearer tokens are often issued after a user or application completes an authentication or authorization process.

**Security tip:** Anyone who obtains a valid bearer token may be able to use it. Protect tokens carefully and never share real tokens in screenshots, logs, or public code.

### 3. OAuth 2.0

**OAuth 2.0** is an authorization framework that allows an application to obtain limited access to resources without needing to handle the user's password directly.

For example, a user might authorize a calendar application to access their calendar information through another service.

A simplified process looks like this:

1. The application requests authorization.
2. The user or an authorized system grants permission.
3. The application obtains an access token.
4. The application sends the token with API requests.
5. The API validates the token and checks the granted permissions.

OAuth 2.0 is commonly used when applications need delegated access to protected resources.

OAuth 2.0 primarily handles authorization. Applications that need standardized user authentication may also use OpenID Connect.

### 4. Basic Authentication

**Basic authentication** sends a username and password in an HTTP `Authorization` header using Base64 encoding.

Example:

```http
Authorization: Basic <encoded-credentials>
```

Base64 is an encoding method, not encryption. It does not protect credentials from being read if communication is intercepted.

For this reason, Basic authentication must only be used over HTTPS. Many modern systems prefer other authentication approaches.

Avoid placing usernames or passwords directly in URLs because URLs may be stored in logs or browser history.

## How Token-Based Authentication Works

Consider an application that needs information from a protected API.

A simplified token-based authentication flow is:

```text
1. Client requests authentication.
2. Client receives an access token.
3. Client sends an API request with the token.
4. API validates the token.
5. API returns data or an error.
```

For example, the client sends:

```http
GET /orders
Authorization: Bearer your_access_token
```

If the token is valid and the client has permission to access the orders, the API may return:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

The response body might contain:

```json
{
  "orders": [
    {
      "id": 501,
      "status": "shipped"
    },
    {
      "id": 502,
      "status": "processing"
    }
  ]
}
```

If the token is missing, invalid, or expired, the API may reject the request. The exact response depends on the API's implementation.

## Where Should Authentication Credentials Be Sent?

Authentication credentials are commonly sent in HTTP headers.

For example:

```http
Authorization: Bearer your_access_token
```

Some APIs use a dedicated header for API keys, while others require a specific authentication flow.

Always follow the API documentation to determine which authentication method and header are required.

Avoid placing sensitive credentials in query parameters unless the API specifically requires it and the associated risks are understood. URLs can be recorded in logs, browser history, and monitoring systems.

## Common Authentication Errors

Authentication problems can prevent an API request from succeeding.

| Status Code | Meaning | What to Check |
|---|---|---|
| `401 Unauthorized` | Authentication is missing or invalid | Check whether credentials or tokens are present and valid |
| `403 Forbidden` | The request is not permitted | Check whether the client has the required permissions |

A `401` response commonly indicates an authentication problem, while a `403` response generally indicates a permission problem.

An API may return an error body with additional details.

Example:

```json
{
  "error": "invalid_token",
  "message": "The access token is missing, invalid, or expired."
}
```

Error formats vary between APIs. Consult the API documentation when troubleshooting.

For more information, see the [Status Codes](status-codes.md) guide.

## Authentication Security Best Practices

Follow these practices when working with API credentials.

### Keep Credentials Private

Never commit real API keys, passwords, or access tokens to a public GitHub repository.

Use clearly marked placeholders in documentation and example code.

### Use HTTPS

HTTPS encrypts data in transit and helps protect credentials during communication between the client and server.

Never send sensitive credentials over an unencrypted HTTP connection.

### Use Appropriate Permissions

Give clients only the permissions they need. Avoid granting unrestricted access when a smaller set of permissions is sufficient.

### Handle Tokens Carefully

Do not share access tokens in screenshots, public issue reports, or logs. Follow the service's instructions for token expiration, storage, and revocation.

### Follow the API Documentation

Different APIs use different authentication methods. Check the documentation for required credentials, headers, token formats, and permissions.

## Practical Example: Sending an Authenticated Request

Suppose an API requires a bearer token to retrieve user information.

The request could look like this:

```http
GET /users/123
Accept: application/json
Authorization: Bearer your_access_token
```

The request contains:

- `GET`: the method used to retrieve information.
- `/users/123`: the endpoint for the requested user.
- `Accept: application/json`: indicates that the client expects JSON.
- `Authorization`: provides the bearer token.

If the token is valid and the client is authorized to access the resource, the API may return:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

The response body might contain:

```json
{
  "id": 123,
  "name": "John Smith"
}
```

This is an illustrative example. Actual endpoints, token requirements, and response formats depend on the API.

## Summary

Authentication helps REST APIs verify clients before processing requests to protected resources.

Common approaches include API keys, bearer tokens, OAuth 2.0, and Basic authentication. Each approach has different uses and security considerations.

When working with authentication:

- Understand the difference between authentication and authorization.
- Use the method required by the API.
- Send credentials in the correct location.
- Protect API keys and access tokens.
- Use HTTPS for requests containing credentials.
- Check error responses when authentication fails.

## What's Next?

Continue with [Troubleshooting](troubleshooting.md) to learn how to identify common API problems, interpret error responses, and resolve request failures.
