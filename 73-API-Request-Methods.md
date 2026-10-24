# 73. API Request Methods

## 1. Introduction

HTTP request methods define the intended operation a client wants to perform on a resource hosted by a server.

When a client communicates with a backend API, it uses an HTTP method to indicate what action it wants to perform.

For example, a client may want to retrieve user information, create a new record, update existing data, or delete a resource.

HTTP methods help clients and servers communicate consistently.

### Common HTTP Request Methods

| Method | General Purpose |
|---|---|
| GET | Retrieve a resource |
| POST | Submit data or request an operation |
| PUT | Create or replace a resource at a known target URI |
| PATCH | Partially modify a resource |
| DELETE | Remove a resource |
| HEAD | Retrieve response headers without the response body |
| OPTIONS | Discover communication options supported by a resource |

These methods have standardized semantics, but the actual behavior depends on the server's implementation.

---

## 2. Understanding Resources and Endpoints

Before learning HTTP methods, it is useful to understand resources and endpoints.

A **resource** is an identifiable piece of information or functionality managed by an application.

Examples include:

- A user.
- A product.
- An order.
- A blog post.
- A database record exposed through an API.

An **endpoint** is an address through which a client accesses a particular API resource or operation.

### Example API Endpoints

```text
/users
/users/101
/products
/products/25
/orders
/orders/5001
```

In this example:

- `/users` represents a user collection.
- `/users/101` represents a particular user.
- `/products` represents a product collection.
- `/products/25` represents a particular product.
- `/orders` represents an order collection.

The HTTP method helps indicate the intended operation on the resource.

For example:

```http
GET /users
```

Requests the user collection.

```http
GET /users/101
```

Requests a particular user.

```http
POST /users
```

Typically requests the creation of a new user.

The server determines how each request is processed.

---

## 3. GET Method

The GET method requests a representation of a specified resource.

It is commonly used to retrieve information without intentionally changing the resource's state.

### Common Uses

- Retrieving user information.
- Listing products.
- Viewing order details.
- Searching available records.
- Retrieving application configuration that is safe to expose.

### Example Request

```http
GET /users HTTP/1.1
Host: api.example.com
Accept: application/json
```

This request asks the server to return the user collection.

### Example Response Body

```json
{
  "users": [
    {
      "id": 101,
      "name": "Moin"
    },
    {
      "id": 102,
      "name": "Aamir"
    }
  ]
}
```

The actual response depends on the available records and API implementation.

### GET with Query Parameters

Query parameters can be used to filter, sort, or paginate results.

Example:

```http
GET /products?category=laptops&limit=10
```

This request asks for products matching the specified category, with a limit parameter.

The API must implement these parameters for them to have an effect.

### Important Characteristics

- GET is a safe method according to HTTP semantics.
- GET is idempotent.
- A GET request generally does not require a request body.
- GET responses may be cacheable depending on the request, response headers, and application behavior.

**Safe** means the client is not requesting a change to the resource's state.

**Idempotent** means that repeating the same request has the same intended effect as performing it once.

For example, retrieving a user's profile repeatedly should not create or modify the profile.

---

## 4. POST Method

The POST method asks the target resource to process the representation included in the request according to the resource's own semantics.

It is commonly used to create resources, submit forms, or initiate operations.

### Common Uses

- Creating a user account.
- Submitting a registration form.
- Creating an order.
- Submitting a comment.
- Initiating a payment operation.
- Uploading information to a service.

### Example Request

```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Accept: application/json
```

Request body:

```json
{
  "name": "Moin",
  "email": "moin@example.com"
}
```

This request could create a new user if the API implements that behavior.

### Example Response Body

```json
{
  "id": 103,
  "name": "Moin",
  "email": "moin@example.com"
}
```

The server may return the newly created resource or another representation of the operation's result.

### Important Characteristics

- POST commonly submits data for processing.
- POST requests often include a request body.
- POST is not inherently idempotent.
- Repeating a POST request may create multiple resources or initiate an operation more than once.

For example, submitting the same order request twice could create two orders if the application does not implement duplicate protection.

For operations such as payments or order creation, applications may use idempotency keys or other safeguards to prevent unintended duplicate processing.

---

## 5. PUT Method

The PUT method requests that the target resource be created or replaced with the state represented by the request, according to HTTP semantics and the server's implementation.

It is commonly used when the client knows the target resource's URI.

### Common Uses

- Replacing a user's profile representation.
- Replacing a product's editable information.
- Creating a resource at a known URI when the API supports it.

### Example Request

```http
PUT /users/101 HTTP/1.1
Host: api.example.com
Content-Type: application/json
```

Request body:

```json
{
  "name": "Moin Rafi",
  "email": "moin@example.com"
}
```

This request asks the server to replace the representation of the resource identified by `/users/101`.

The exact behavior depends on the API's resource model and validation rules.

### PUT vs POST

| Feature | PUT | POST |
|---|---|---|
| General purpose | Create or replace the target resource's state | Submit data for processing |
| Target | Usually a known resource URI | Often a collection or processing endpoint |
| Idempotent | Yes, by definition | Not inherently |
| Common example | Replace a user's profile | Create a new user |

### Important Characteristics

- PUT is idempotent.
- Repeating an equivalent PUT request should have the same intended effect as performing it once.
- PUT is generally used for creating or replacing resource state.
- PUT should not automatically be treated as a partial update method.

If the API expects a complete resource representation, omitted fields may be reset, removed, or handled according to the API's rules.

Always check the API documentation to understand the expected request body.

---

## 6. PATCH Method

The PATCH method requests a partial modification of a resource.

Unlike a replacement-oriented PUT request, PATCH is commonly used when the client wants to change only selected fields.

### Common Uses

- Updating a user's display name.
- Changing a product's price.
- Updating an order's permitted status.
- Modifying selected profile settings.

### Example Request

```http
PATCH /users/101 HTTP/1.1
Host: api.example.com
Content-Type: application/json
```

Request body:

```json
{
  "name": "Moin Rafi"
}
```

This example asks the server to update the user's name without explicitly supplying the email field.

Whether the field is changed successfully depends on the API's implementation and validation rules.

### PUT vs PATCH

| Feature | PUT | PATCH |
|---|---|---|
| General purpose | Create or replace resource state | Partially modify resource state |
| Request body | Often represents the desired complete resource state | Describes the intended modification |
| Typical use | Replace a profile representation | Update one or more selected fields |
| Idempotency | Idempotent | Not guaranteed to be idempotent |

PATCH is not automatically idempotent. Its behavior depends on the patch operation.

For example, setting a field to a fixed value can be idempotent, while an operation that increments a number may not be.

### Common PATCH Formats

APIs may use different formats for PATCH requests, including:

- JSON Merge Patch.
- JSON Patch.
- Custom JSON structures defined by the API.

The server and client must agree on the patch format.

---

## 7. DELETE Method

The DELETE method requests the removal of the association between a target URI and its current functionality.

In common resource-oriented APIs, it is used to delete a resource.

### Common Uses

- Deleting a user account.
- Removing a product.
- Deleting a comment.
- Removing an order draft.
- Deleting an application record.

### Example Request

```http
DELETE /users/101 HTTP/1.1
Host: api.example.com
```

This request asks the server to delete the user resource identified by `/users/101`.

### Possible Response

A successful deletion might return a response without a body:

```http
HTTP/1.1 204 No Content
```

Alternatively, the server may return a response containing a confirmation message or other information.

The exact response depends on the API design.

### Important Characteristics

- DELETE is idempotent according to HTTP semantics.
- Repeating the request should have the same intended effect as performing it once.
- The response to repeated requests does not have to be identical.
- Deletion may be implemented as a permanent deletion, soft deletion, or another application-specific operation.

For example, the first request might delete a record, while a later request reports that the record no longer exists.

Idempotency concerns the intended effect, not necessarily the response status or response body.

---

## 8. HEAD Method

The HEAD method is similar to GET, but the server does not return the response body.

It is commonly used when a client needs response metadata without retrieving the complete representation.

### Common Uses

- Checking whether a resource is accessible.
- Inspecting response headers.
- Checking metadata before downloading a file.
- Examining cache-related information.

### Example Request

```http
HEAD /files/report.pdf HTTP/1.1
Host: api.example.com
```

The server may return:

```http
HTTP/1.1 200 OK
Content-Type: application/pdf
Content-Length: 245760
```

The example illustrates response headers. Actual values depend on the resource and server.

### Important Characteristics

- HEAD does not return a response body.
- It is safe.
- It is idempotent.
- The returned headers generally correspond to those a GET request would return, although some differences are permitted by HTTP semantics.

HEAD can be useful for inspecting a resource without transferring its complete content.

---

## 9. OPTIONS Method

The OPTIONS method requests information about the communication options available for a target resource or server.

It can help clients discover supported methods or other communication capabilities.

### Common Uses

- Discovering supported HTTP methods.
- Inspecting communication options.
- Supporting Cross-Origin Resource Sharing (CORS) preflight requests in browsers.

### Example Request

```http
OPTIONS /users HTTP/1.1
Host: api.example.com
```

### Example Response

```http
HTTP/1.1 204 No Content
Allow: GET, POST, OPTIONS
```

This illustrative response indicates that the server reports GET, POST, and OPTIONS as supported methods for the target resource.

The actual supported methods and response format depend on the server.

### OPTIONS and CORS

CORS stands for Cross-Origin Resource Sharing.

Browsers use CORS rules to determine whether frontend JavaScript running on one origin can access resources on another origin.

For certain cross-origin requests, the browser sends an OPTIONS preflight request to check whether the intended request is permitted.

The server may return headers such as:

```http
Access-Control-Allow-Origin: https://frontend.example.com
Access-Control-Allow-Methods: GET, POST, PATCH, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
```

These headers are illustrative. Actual values should match the application's security requirements.

CORS is a browser-enforced access mechanism, not a replacement for authentication or authorization.

---

## 10. Safe and Idempotent HTTP Methods

Two important properties of HTTP methods are safety and idempotency.

### Safe Methods

A safe method is intended for retrieving information rather than requesting a change to the resource's state.

Safe methods include:

- GET
- HEAD
- OPTIONS

A server may still perform incidental activities, such as logging requests or updating metrics. These do not change the method's intended safe semantics.

### Idempotent Methods

An idempotent method has the same intended effect when the same request is performed multiple times as when it is performed once.

The following methods are idempotent:

- GET
- HEAD
- PUT
- DELETE
- OPTIONS

POST is not inherently idempotent, and PATCH is not guaranteed to be idempotent.

### Comparison Table

| Method | Safe | Idempotent |
|---|---|---|
| GET | Yes | Yes |
| POST | No | No, not inherently |
| PUT | No | Yes |
| PATCH | No | Not guaranteed |
| DELETE | No | Yes |
| HEAD | Yes | Yes |
| OPTIONS | Yes | Yes |

### Why Does Idempotency Matter?

Idempotency is important when dealing with retries, network failures, and distributed systems.

For example, a client may send a request but fail to receive the response because the network connection is interrupted.

If the client retries the request, the consequences depend on the method and application behavior.

Repeating a PUT request that sets a resource to the same state should have the same intended effect. Repeating a POST request that creates an order could create another order unless duplicate protection is implemented.

Understanding these properties helps developers design reliable APIs and retry behavior.

---

## 11. Practical Examples Using cURL

cURL can be used to send HTTP requests from a terminal.

The following examples assume that an API is available at `http://localhost:8000` and supports the specified endpoints.

Replace the URLs with the actual address of your application.

### Example 1: GET Request

Retrieve a user:

```bash
curl http://localhost:8000/users/101
```

This sends a GET request to retrieve the specified user.

### Example 2: POST Request

Create a user:

```bash
curl -X POST http://localhost:8000/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Moin","email":"moin@example.com"}'
```

This sends a JSON request body to the API.

### Example 3: PUT Request

Replace a user's representation:

```bash
curl -X PUT http://localhost:8000/users/101 \
  -H "Content-Type: application/json" \
  -d '{"name":"Moin Rafi","email":"moin@example.com"}'
```

This example assumes the API accepts the supplied representation for PUT.

### Example 4: PATCH Request

Update a user's name:

```bash
curl -X PATCH http://localhost:8000/users/101 \
  -H "Content-Type: application/json" \
  -d '{"name":"Moin Rafi"}'
```

This example assumes the API supports partial updates using the supplied JSON format.

### Example 5: DELETE Request

Delete a user:

```bash
curl -X DELETE http://localhost:8000/users/101
```

This requests deletion of the specified user.

### Example 6: HEAD Request

Inspect response headers:

```bash
curl -I http://localhost:8000/users/101
```

This sends a HEAD request.

### Example 7: OPTIONS Request

Inspect communication options:

```bash
curl -i -X OPTIONS http://localhost:8000/users
```

The server must implement or support the requested behavior for the response to be meaningful.

**Important:** These commands demonstrate client-side requests. They do not create the API endpoints. The backend application must implement the corresponding functionality.

---

## 12. Choosing the Appropriate HTTP Method

The appropriate method depends on the operation the client wants to perform.

| Requirement | Typical Method | Example |
|---|---|---|
| Retrieve all users | GET | `/users` |
| Retrieve one user | GET | `/users/101` |
| Create a user | POST | `/users` |
| Replace a user's representation | PUT | `/users/101` |
| Update selected user fields | PATCH | `/users/101` |
| Delete a user | DELETE | `/users/101` |
| Retrieve response headers | HEAD | `/users/101` |
| Discover communication options | OPTIONS | `/users` |

The endpoint design and supported operations depend on the API.

A well-designed API should use HTTP methods consistently with their intended semantics.

---

## 13. Common Mistakes

### Mistake 1: Using GET to Modify Data

GET is intended for safe retrieval.

Using GET to perform operations such as deleting a record or transferring money can cause problems with caching, automated requests, browser behavior, and other HTTP mechanisms.

Use a method appropriate to the operation.

### Mistake 2: Assuming PUT and PATCH Are Identical

PUT is generally used to create or replace a resource representation.

PATCH is used to request partial modification.

Their behavior should be documented by the API.

### Mistake 3: Assuming POST Can Never Be Retried

POST is not inherently idempotent, but an application can implement duplicate protection or idempotency keys.

Whether retrying is safe depends on the operation and API contract.

### Mistake 4: Assuming DELETE Always Removes Data Permanently

An application may implement soft deletion, archival, or another deletion policy.

The HTTP method alone does not define the application's internal storage behavior.

### Mistake 5: Assuming Every API Supports Every Method

An API may support only a subset of HTTP methods for a resource.

Clients should follow the API documentation and handle unsupported operations appropriately.

### Mistake 6: Sending the Wrong Request Body Format

If an API expects JSON, the client should send valid JSON and use an appropriate Content-Type header.

For example:

```http
Content-Type: application/json
```

The body must also follow the schema expected by the backend.

---

## 14. Key Takeaways

- HTTP methods indicate the intended operation on a resource.
- GET retrieves information.
- POST submits data or requests processing.
- PUT creates or replaces resource state.
- PATCH requests partial modification.
- DELETE requests resource deletion.
- HEAD retrieves response headers without a response body.
- OPTIONS requests information about communication options.
- Safe methods are intended not to change resource state.
- Idempotent methods have the same intended effect when repeated.
- cURL can be used to test API endpoints from a terminal.
- The backend determines the actual supported operations and validates incoming requests.

---

## 15. Conclusion

HTTP request methods provide a standardized way for clients and servers to communicate their intended operations.

Understanding these methods is essential when developing backend applications, testing APIs, integrating external services, and troubleshooting application behavior.

For DevOps engineers, knowledge of HTTP methods is useful when testing deployed services, troubleshooting application endpoints, configuring integrations, and validating backend functionality.

### Related Topics

Continue with the following documents:

- `72-API-HTTP-Request-and_Response.md` — HTTP requests and responses.
- `74-HTTP_Status-Codes.md` — HTTP status codes and their meanings.
- `75-API-Architecture-Types.md` — Different API architectural styles.
- `76-Fast-API-Intro.md` — Introduction to FastAPI.
- `77-fastapi-hello-world.md` — Building a first FastAPI application.