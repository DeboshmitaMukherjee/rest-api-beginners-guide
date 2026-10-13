
# Getting Started with REST APIs

## Introduction

REST APIs allow different applications to communicate and exchange data over the web. They help applications request information from servers and receive responses in a standard format.

For example, when you open a weather app, it may use an API to retrieve the latest weather information from a server.

This guide explains the basic concepts of REST APIs, how they work, and what you need to understand before making API requests.

## What Is an API?

API stands for **Application Programming Interface**. It acts as a connection between two software applications, allowing them to exchange information without needing to know how each application works internally.

For example, a weather application can send a request to a weather service's API and receive information about the current temperature.

### How an API Works

```text
Application
     |
     | API Request
     v
   Server
     |
     | API Response
     v
Application
```

The application sends a request, the server processes it, and the application receives a response.

## What Is a REST API?

REST stands for **Representational State Transfer**. A REST API is an API that follows REST architectural principles and commonly uses HTTP to exchange data between clients and servers.

A client can be a web browser, mobile application, or another software system. The server receives the request, performs the required operation, and returns a response.

REST APIs commonly use JSON (JavaScript Object Notation) to exchange structured data.

### Example of a REST API Endpoint

An endpoint is a specific URL where an API resource can be accessed.

```http
GET /users/123
```

In this example:

- `GET` is the HTTP method used to retrieve information.
- `/users/123` is the endpoint path.
- `123` identifies the user whose information is requested.

A successful response might look like this:

```json
{
  "id": 123,
  "name": "John Smith",
  "email": "john@example.com"
}
```

This JSON object contains the requested user's ID, name, and email address.

*Note: This is an illustrative example, not a live API endpoint.*

## How REST APIs Work

A typical REST API interaction follows these steps:

1. **The client sends a request.** The request specifies the resource or action needed.
2. **The server receives the request.** It checks the request and processes it.
3. **The server performs the operation.** Depending on the request, it may retrieve, create, update, or delete data.
4. **The server returns a response.** The response includes a status code and may contain data or an error message.
5. **The client handles the response.** The application uses the result to display information or determine what to do next.

Understanding this flow makes it easier to read API documentation and troubleshoot unexpected results.

## Key REST API Concepts

| Concept | Description |
|---|---|
| Client | The application that sends an API request. |
| Server | The system that receives and processes the request. |
| Endpoint | The URL path used to access a resource. |
| HTTP method | Specifies the intended operation, such as retrieving or updating data. |
| Request | The information sent by the client to the server. |
| Response | The result returned by the server. |
| Status code | A number indicating the outcome of a request. |
| JSON | A common format for exchanging structured data. |

## What You Need Before Using a REST API

Before making requests to an API, check its documentation for the following information:

- **Base URL:** The main address used to access the API.
- **Endpoints:** The available paths for accessing resources.
- **HTTP methods:** The methods supported by each endpoint.
- **Request parameters:** The information required to make a request.
- **Authentication:** Whether an API key, access token, or another credential is required.
- **Response format:** The structure and format of the returned data.
- **Status codes:** The codes that indicate success or explain errors.

API documentation helps you understand these details before you start working with an endpoint.

## Best Practices for Beginners

- Read the API documentation before sending requests.
- Start with a simple request, such as retrieving data with `GET`.
- Check the status code and response body to understand the result.
- Use valid request parameters and the correct data format.
- Keep API keys and access tokens private.
- Use a testing tool such as Postman to send requests and inspect responses.

## Summary

REST APIs provide a standard way for applications to communicate over HTTP. By understanding endpoints, requests, responses, HTTP methods, and status codes, beginners can build a strong foundation for working with APIs.

Start by exploring a documented public API and sending a simple request. Then examine the response to see how the concepts in this guide work in practice.

## Related Guides

- [API Reference](api-reference.md) — Learn about HTTP methods, request and response structures, JSON, and status codes.
- [Security and Troubleshooting](security-and-troubleshooting.md) — Learn about authentication, credential protection, and common API problems.

## Additional Resources

- [MDN Web Docs: An Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [MDN Web Docs: Introduction to Web APIs](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction)
