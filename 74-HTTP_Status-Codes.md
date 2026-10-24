
# 74 - HTTP Status Codes

## 1. What Are HTTP Status Codes?

HTTP status codes are three-digit numbers returned by a web server to indicate the result of a client's HTTP request.

They tell the client whether a request was successful, whether additional action is required, or whether an error occurred.

HTTP status codes are commonly used in:
- Web development
- REST APIs
- Backend applications
- Client-server communication
- Troubleshooting HTTP requests and responses

## 2. Categories of HTTP Status Codes

HTTP status codes are divided into five categories.

| Category | Range | Meaning |
|---|---|---|
| 1xx | 100–199 | Informational |
| 2xx | 200–299 | Success |
| 3xx | 300–399 | Redirection |
| 4xx | 400–499 | Client errors |
| 5xx | 500–599 | Server errors |

## 3. Informational Status Codes (1xx)

These codes indicate that the request has been received and the process is continuing.

| Status Code | Name | Description |
|---|---|---|
| 100 | Continue | The client can continue sending the request. |
| 101 | Switching Protocols | The server agrees to switch protocols. |
| 102 | Processing | The server is processing the request but has not completed it. |
| 103 | Early Hints | The server sends preliminary information before the final response. |

**Example:** A client receives `100 Continue` before sending the remaining content of a request.

## 4. Success Status Codes (2xx)

These codes indicate that the request was successfully received, understood, and processed.

| Status Code | Name | Description |
|---|---|---|
| 200 | OK | The request was successful. |
| 201 | Created | A new resource was successfully created. |
| 202 | Accepted | The request was accepted for processing but has not yet been completed. |
| 203 | Non-Authoritative Information | The response metadata may have been modified by a proxy. |
| 204 | No Content | The request succeeded, but there is no response content. |
| 205 | Reset Content | The client should reset the document view that initiated the request. |
| 206 | Partial Content | The server returns only part of the requested resource. |
| 207 | Multi-Status | The response contains information about multiple operations, commonly with WebDAV. |
| 208 | Already Reported | Previously reported members are not reported again in a WebDAV response. |
| 226 | IM Used | The server fulfilled a GET request using instance manipulations. |

### Common Examples

- `200 OK`: Fetching user details successfully.
- `201 Created`: Creating a new user through an API.
- `202 Accepted`: Submitting a background job for processing.
- `204 No Content`: Deleting a resource successfully without returning a response body.
- `206 Partial Content`: Downloading a portion of a large file.

## 5. Redirection Status Codes (3xx)

These codes indicate that the client needs to take additional action, commonly by using another URL or following a redirect.

| Status Code | Name | Description |
|---|---|---|
| 300 | Multiple Choices | Multiple representations are available for the requested resource. |
| 301 | Moved Permanently | The resource has permanently moved to another URL. |
| 302 | Found | The resource is temporarily available at another URL. |
| 303 | See Other | The client should retrieve another resource, usually using GET. |
| 304 | Not Modified | The cached resource has not changed and can be reused. |
| 305 | Use Proxy | Reserved; should not be used. |
| 306 | Unused | No longer used; reserved. |
| 307 | Temporary Redirect | The resource is temporarily available elsewhere, preserving the request method. |
| 308 | Permanent Redirect | The resource has permanently moved, preserving the request method. |

### Common Examples

- `301 Moved Permanently`: A website permanently changes its URL.
- `302 Found`: A user is temporarily redirected to another page.
- `304 Not Modified`: A browser can use its cached copy of a resource.
- `307 Temporary Redirect`: A POST request is redirected without changing its method.

### 301 vs 302

| Feature | 301 | 302 |
|---|---|---|
| Meaning | Permanent redirect | Temporary redirect |
| Intended use | Resource moved permanently | Resource moved temporarily |
| Typical example | Permanent website URL change | Temporary maintenance redirect |

### 307 vs 308

Both codes explicitly preserve the HTTP request method during redirection.

- `307`: Temporary redirect.
- `308`: Permanent redirect.

## 6. Client Error Status Codes (4xx)

These codes indicate that the request has a problem, such as invalid input, missing authentication, insufficient permissions, or a resource that does not exist.

| Status Code | Name | Description |
|---|---|---|
| 400 | Bad Request | The server cannot process the request because it considers the request invalid. |
| 401 | Unauthorized | Valid authentication credentials are missing or invalid. |
| 402 | Payment Required | Reserved for future use; some services use it for payment-related restrictions. |
| 403 | Forbidden | The server refuses to fulfill the request. |
| 404 | Not Found | The requested resource could not be found. |
| 405 | Method Not Allowed | The HTTP method is not supported for the resource. |
| 406 | Not Acceptable | The server cannot provide a response matching the client's acceptable formats. |
| 407 | Proxy Authentication Required | Authentication with a proxy is required. |
| 408 | Request Timeout | The server timed out waiting for the request. |
| 409 | Conflict | The request conflicts with the current state of the resource. |
| 410 | Gone | The resource is no longer available and this is likely permanent. |
| 411 | Length Required | The request must include a valid Content-Length header. |
| 412 | Precondition Failed | A request condition was not satisfied. |
| 413 | Content Too Large | The request content exceeds the server's accepted limit. |
| 414 | URI Too Long | The requested URI is too long for the server to process. |
| 415 | Unsupported Media Type | The request content format is not supported. |
| 416 | Range Not Satisfiable | The requested range of a resource cannot be provided. |
| 417 | Expectation Failed | The server cannot meet the requirements in the Expect header. |
| 418 | Unused | Reserved and not intended for general HTTP use. |
| 421 | Misdirected Request | The request was directed to a server unable to produce a response. |
| 422 | Unprocessable Content | The request content is understood but cannot be processed. |
| 423 | Locked | The resource is locked; used by WebDAV. |
| 424 | Failed Dependency | The request failed because a dependent operation failed. |
| 425 | Too Early | The server is unwilling to process a request that might be replayed. |
| 426 | Upgrade Required | The client must switch to a different protocol. |
| 428 | Precondition Required | The server requires conditional requests. |
| 429 | Too Many Requests | The client has sent too many requests within a given period. |
| 431 | Request Header Fields Too Large | The request headers are too large for the server to process. |
| 451 | Unavailable For Legal Reasons | Access is denied for legal reasons. |

### Common Examples

- `400 Bad Request`: An API receives malformed JSON.
- `401 Unauthorized`: A client sends an invalid or missing access token.
- `403 Forbidden`: An authenticated user attempts an action they are not allowed to perform.
- `404 Not Found`: A requested user ID does not exist.
- `405 Method Not Allowed`: A client sends POST to an endpoint that only supports GET.
- `409 Conflict`: A request attempts to create a username that already exists.
- `413 Content Too Large`: An uploaded file exceeds the configured size limit.
- `415 Unsupported Media Type`: An endpoint expects JSON but receives an unsupported content type.
- `422 Unprocessable Content`: A request contains semantically invalid data.
- `429 Too Many Requests`: A client exceeds an API rate limit.

## 7. Server Error Status Codes (5xx)

These codes indicate that the server failed to fulfill an apparently valid request because of a server-side problem or an issue involving an upstream service.

| Status Code | Name | Description |
|---|---|---|
| 500 | Internal Server Error | An unexpected server-side error occurred. |
| 501 | Not Implemented | The server does not support the functionality required to fulfill the request. |
| 502 | Bad Gateway | A gateway or proxy received an invalid response from an upstream server. |
| 503 | Service Unavailable | The server is temporarily unable to handle the request. |
| 504 | Gateway Timeout | A gateway or proxy did not receive a timely response from an upstream server. |
| 505 | HTTP Version Not Supported | The server does not support the HTTP version used in the request. |
| 506 | Variant Also Negotiates | A server configuration error occurred during transparent content negotiation. |
| 507 | Insufficient Storage | The server cannot store the information needed to complete the request; used by WebDAV. |
| 508 | Loop Detected | The server detected an infinite loop while processing a request; used by WebDAV. |
| 510 | Not Extended | Further extensions are required to fulfill the request. |
| 511 | Network Authentication Required | The client must authenticate to gain network access. |

### Common Examples

- `500 Internal Server Error`: An unhandled exception occurs in a backend application.
- `502 Bad Gateway`: A reverse proxy receives an invalid response from the backend.
- `503 Service Unavailable`: A service is temporarily unavailable during maintenance or overload.
- `504 Gateway Timeout`: A proxy does not receive a response from the backend within the allowed time.

## 8. Important HTTP Status Codes to Remember

The following codes are especially common in web development, APIs, and DevOps troubleshooting.

| Status Code | Meaning | Typical Situation |
|---|---|---|
| 200 | OK | Successful request |
| 201 | Created | Resource created |
| 204 | No Content | Successful request with no response body |
| 301 | Moved Permanently | Permanent redirect |
| 302 | Found | Temporary redirect |
| 304 | Not Modified | Cached resource can be reused |
| 400 | Bad Request | Invalid request |
| 401 | Unauthorized | Authentication required or failed |
| 403 | Forbidden | Access denied |
| 404 | Not Found | Resource missing |
| 405 | Method Not Allowed | Unsupported HTTP method |
| 409 | Conflict | Conflict with current resource state |
| 422 | Unprocessable Content | Invalid request data |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected backend error |
| 502 | Bad Gateway | Invalid upstream response |
| 503 | Service Unavailable | Service temporarily unavailable |
| 504 | Gateway Timeout | Upstream response timed out |

## 9. HTTP Status Codes in REST APIs

HTTP status codes communicate the outcome of API requests to clients.

For example, consider an API endpoint:

`GET /api/users/10`

Possible responses:

- `200 OK`: User details were returned successfully.
- `401 Unauthorized`: Authentication is missing or invalid.
- `403 Forbidden`: The client is not allowed to access the user.
- `404 Not Found`: The user does not exist.
- `500 Internal Server Error`: An unexpected server-side error occurred.

For a resource creation request:

`POST /api/users`

Possible responses:

- `201 Created`: The user was created successfully.
- `400 Bad Request`: The request is malformed.
- `409 Conflict`: The new resource conflicts with an existing resource.
- `422 Unprocessable Content`: The request data fails validation.

Choosing the correct status code helps clients understand what happened and how to respond.

## 10. HTTP Status Codes in DevOps

HTTP status codes are useful when diagnosing web servers, reverse proxies, containerized applications, and deployed services.

| Status Code | Possible Cause | What to Check |
|---|---|---|
| 200 | Service is responding successfully | Verify the response content and application behavior. |
| 301 / 302 | Redirect is configured | Check redirect rules and destination URLs. |
| 400 | Invalid request | Inspect request headers, parameters, and payload. |
| 401 / 403 | Authentication or authorization issue | Check credentials, permissions, and access policies. |
| 404 | Incorrect route or missing resource | Check application routes and server configuration. |
| 429 | Rate limiting | Check rate-limit rules and request frequency. |
| 500 | Application error | Inspect application logs and stack traces. |
| 502 | Upstream service problem | Check backend health, proxy configuration, and connectivity. |
| 503 | Service unavailable | Check service health, capacity, deployments, and maintenance state. |
| 504 | Upstream timeout | Check backend response time, network connectivity, and timeout settings. |

### Example: Troubleshooting a 502 Bad Gateway

Suppose an application runs behind a reverse proxy such as Apache or Nginx.

If the proxy cannot obtain a valid response from the backend, it may return `502 Bad Gateway`.

Troubleshooting steps:

1. Check whether the backend service is running.
2. Verify the backend IP address and port.
3. Confirm that the proxy can reach the backend.
4. Inspect application and proxy logs.
5. Check container status and network connectivity if the application uses Docker.
6. Restart a failed service only after identifying the likely cause.

A `502` response does not necessarily mean the proxy itself is broken. The backend or the connection between the two services may be responsible.

## 11. How to Check HTTP Status Codes

### Using curl

Use the `-I` option to request response headers:

```bash
curl -I https://example.com
```

Example output:

```text
HTTP/2 200
content-type: text/html
```

The `200` indicates that the request was successful.

To display only the HTTP status code:

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://example.com
```

Example output:

```text
200
```

To inspect a response with headers and body:

```bash
curl -i https://example.com
```

To follow redirects and inspect the final response:

```bash
curl -L -I https://example.com
```

### Using Browser Developer Tools

1. Open the website in a browser.
2. Open Developer Tools.
3. Select the **Network** tab.
4. Reload the page.
5. Select a request.
6. Inspect the status code in the request details or status column.

This is useful for checking document requests, API calls, JavaScript files, CSS files, and images.

## 12. Common Misunderstandings

### 401 vs 403

- **401 Unauthorized:** Valid authentication credentials are missing or invalid.
- **403 Forbidden:** The server refuses access, even if the client is authenticated.

### 404 vs 410

- **404 Not Found:** The resource cannot be found, and its availability in the future is not established.
- **410 Gone:** The resource is no longer available and is likely to remain unavailable.

### 500 vs 502 vs 503 vs 504

- **500:** An unexpected error occurred within the server handling the request.
- **502:** A gateway received an invalid response from an upstream service.
- **503:** The service is temporarily unable to handle requests.
- **504:** A gateway did not receive a timely response from an upstream service.

### 200 vs 201 vs 204

- **200 OK:** The request succeeded, commonly with a response body.
- **201 Created:** The request successfully created a resource.
- **204 No Content:** The request succeeded, but the response has no content.

### Status Codes Are Not Always Proof of Application Success

A `200 OK` response means the HTTP request succeeded at the protocol level. It does not guarantee that every business operation succeeded.

For example, an application may return `200 OK` with a JSON response describing a failed operation. Well-designed APIs should use HTTP status codes consistently and avoid misleading responses.

## 13. Best Practices

- Use status codes that accurately represent the result of a request.
- Return `201 Created` when a request creates a resource.
- Return `204 No Content` when the operation succeeds and no response body is needed.
- Use `400` or `422` appropriately for invalid requests, depending on the API's conventions.
- Distinguish authentication failures (`401`) from access restrictions (`403`).
- Use `404` when the requested resource cannot be found.
- Use `429` when rate limits are exceeded.
- Avoid exposing sensitive internal details in `500` error responses.
- Check application and reverse-proxy logs when troubleshooting `5xx` errors.
- Remember that HTTP status codes are defined by the HTTP specification, but some applications and APIs may use additional conventions.

## 14. Summary

HTTP status codes describe the outcome of HTTP requests.

- **1xx:** Informational responses.
- **2xx:** Successful responses.
- **3xx:** Redirection responses.
- **4xx:** Client error responses.
- **5xx:** Server error responses.

Understanding these codes is essential for developing APIs, debugging web applications, monitoring services, and troubleshooting production deployments.
