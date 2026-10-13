
# REST API Beginner's Guide

A beginner-friendly guide to understanding REST APIs, HTTP methods, requests and responses, status codes, authentication, and troubleshooting.

## Overview

REST APIs allow applications to exchange data over HTTP. They provide a structured way for clients to request information from servers and perform operations on resources.

This documentation introduces the main concepts of REST APIs through simple explanations, practical HTTP examples, JSON samples, and reference tables.

Use this guide to:

- Understand how REST APIs work.
- Learn how HTTP methods interact with resources.
- Read API requests and responses.
- Interpret common HTTP status codes.
- Understand authentication and credential security.
- Identify and troubleshoot common API errors.

The documentation is organized by topic. Start with the fundamentals, continue to the API reference, and use the security and troubleshooting guide when you need to investigate errors.

## Documentation

| Guide | Description |
|---|---|
| [Getting Started](getting-started.md) | Learn API fundamentals, REST concepts, resources, endpoints, and request flow. |
| [API Reference](api-reference.md) | Explore HTTP methods, request structure, JSON formats, and HTTP status codes. |
| [Security and Troubleshooting](security-and-troubleshooting.md) | Understand authentication, protect credentials, and investigate common API errors. |

## Quick Start

The following example shows how a client can retrieve information about a user.

### Request

```http
GET /users/123
Accept: application/json
```

- `GET` specifies the operation.
- `/users/123` identifies the requested resource.
- `Accept: application/json` indicates that the client expects a JSON response.

### Successful Response

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

The `200 OK` status indicates that the request succeeded. The JSON body contains the returned user information.

This is an illustrative example. Actual endpoints, authentication requirements, and response formats depend on the API.

## Who Is This Guide For?

This documentation is intended for:

- Beginners learning REST APIs
- Students exploring web development
- Junior developers learning to work with APIs
- Readers building their understanding of technical documentation

No advanced API knowledge is required to get started.

## How to Use This Documentation

1. **Start with Getting Started:** Learn what APIs are and how clients and servers communicate.
2. **Explore the API Reference:** Understand HTTP methods, request and response formats, and status codes.
3. **Review Security and Troubleshooting:** Learn how authentication works and how to investigate common request failures.

You can also use the documentation table above to open a specific topic directly.

## Documentation Format and Tools

This project uses:

- **Markdown** to structure and format documentation.
- **GitHub** to host the guides and track changes.
- **HTTP examples** to demonstrate API requests and responses.
- **JSON examples** to explain structured API data.
- **Tables and code blocks** to make technical information easier to scan.

## About This Project

This portfolio project demonstrates technical writing skills through clear information architecture, plain-language explanations, consistent formatting, practical examples, and task-focused guidance.

The goal is to make introductory API concepts easier to understand without overwhelming readers with unnecessary technical detail.

**Author:** Deboshmita Mukherjee
