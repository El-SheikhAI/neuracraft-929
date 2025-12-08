# NeuraCraft API Response Codes

This document describes the standard HTTP response codes used by the NeuraCraft API. These codes help you understand the outcome of your requests and troubleshoot any issues.

## Standard Response Codes

### Success (2xx)

| Code | Name           | Description                                 | Resolution |
|------|----------------|---------------------------------------------|------------|
| 200  | OK             | Successful request. Response body contains requested data. | None needed. |
| 201  | Created        | New resource successfully created (e.g., pipeline configuration). | Check `Location` header for new resource URL. |
| 202  | Accepted       | Request accepted for asynchronous processing. | Use provided `task_id` to monitor operation status. |

### Client Errors (4xx)

| Code | Name             | Description                                 | Resolution |
|------|------------------|---------------------------------------------|------------|
| 400  | Bad Request      | Invalid request syntax or parameters.        | Validate request format and parameters against API documentation. |
| 401  | Unauthorized     | Missing or invalid authentication credentials. | Verify API key/credentials in `Authorization` header. |
| 403  | Forbidden        | Authenticated user lacks permission for resource. | Check your role/permissions or contact your workspace admin. |
| 404  | Not Found        | Resource doesn't exist at specified path.     | Verify resource ID and URL path. |
| 429  | Too Many Requests | Rate limit exceeded.                         | Wait before retrying. Check `Retry-After` header if provided. |

### Server Errors (5xx)

| Code | Name                  | Description                                 | Resolution |
|------|-----------------------|---------------------------------------------|------------|
| 500  | Internal Server Error | Unexpected server-side error.               | Retry request. If persistent, report with `X-Request-ID` header value. |
| 503  | Service Unavailable   | Temporary system maintenance or overload.   | Retry after waiting. Check status.neuracraft.com for outages. |

---

## Custom Plugin Response Codes (8xxx)

Plugins may extend NeuraCraft's functionality and return custom status codes in the `8000-8999` range. Refer to your plugin-specific documentation for details.

**Format:**  
`8AAA`, where `AAA` is a plugin-specific numeric identifier (e.g., `8001`).

**Example:**
```json
{
  "error": {
    "code": 8102,
    "message": "PDF Export Plugin: Failed to generate document due to invalid template."
  }
}
```

**General Troubleshooting Tips:**
1. Include `X-Request-ID` when reporting errors
2. Validate all request parameters using the API schemas
3. Verify plugin compatibility with your NeuraCraft version

---

**Next Steps:**  
[Read our error handling guide →](../best-practices/error-handling.md)