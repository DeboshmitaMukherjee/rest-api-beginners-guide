# HTTP Methods in REST APIs

HTTP methods define the type of operation a client wants to perform on a resource.

REST APIs commonly use HTTP methods such as `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`. Each method has a specific purpose and helps clients communicate their intended action to the server.

Understanding these methods is essential when working with REST APIs.

## HTTP Methods at a Glance

| Method | Purpose | Example |
|---|---|---|
| `GET` | Retrieve data | `GET /users/123` |
| `POST` | Create a new resource | `POST /users` |
| `PUT` | Replace an existing resource | `PUT /users/123` |
| `PATCH` | Partially update a resource | `PATCH /users/123` |
| `DELETE` | Remove a resource | `DELETE /users/123` |

The method is usually combined with an API endpoint to describe the requested operation.

For example:

```http
GET /users/123
