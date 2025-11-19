## Authentication

NeuraCraft provides two primary methods for API authentication to ensure secure access to your automation pipelines. Choose the approach that best fits your integration requirements.

---

### API Key Authentication
Ideal for server-to-server communication or script-based integrations.

1. **Get Your API Key**:
   - Navigate to **Settings > API Keys** in your NeuraCraft Dashboard.
   - Click **Generate New Key** and copy the generated key. (Store this securely—it won't be shown again.)

2. **Include the Key in Requests**:
   Add the `X-API-Key` header to every API call:

   ```bash
   curl -X GET "https://api.neuracraft.com/v1/pipelines" \
     -H "X-API-Key: your_api_key_here"
   ```

   ```python
   import requests
   headers = {"X-API-Key": "your_api_key_here"}
   response = requests.get("https://api.neuracraft.com/v1/pipelines", headers=headers)
   ```

---

### OAuth 2.0 (Client Credentials Flow)
Recommended for third-party integrations or delegated access control.

1. **Register Your Application**:
   - In the Dashboard, go to **Settings > OAuth Apps** and create a new application.
   - Note your `client_id` and `client_secret`.

2. **Request an Access Token**:
   ```bash
   curl -X POST "https://api.neuracraft.com/oauth/token" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "client_id=your_client_id" \
     -d "client_secret=your_client_secret" \
     -d "grant_type=client_credentials" \
     -d "scope=pipeline:read workflow:execute"
   ```

3. **Use the Token in API Requests**:
   Include the bearer token in the `Authorization` header:

   ```bash
   curl -X GET "https://api.neuracraft.com/v1/workflows" \
     -H "Authorization: Bearer your_access_token_here"
   ```

---

### Scopes & Permissions
When using OAuth, specify scopes to limit access. Available scopes include:

| Scope                 | Description                                    |
|-----------------------|------------------------------------------------|
| `pipeline:read`       | View pipelines and executions                 |
| `pipeline:write`      | Create or modify pipelines                    |
| `workflow:execute`    | Trigger workflow executions                   |
| `plugin:manage`       | Install, update, or remove plugins            |
| `admin:all`           | Full system access (use with caution)         |

---

### Handling Errors
Common authentication-related errors:

| Status Code | Error Code         | Resolution Steps                             |
|-------------|--------------------|----------------------------------------------|
| `401`       | `invalid_api_key`  | Verify the API key is correct and unexpired  |
| `401`       | `invalid_token`    | Refresh your OAuth token or re-authenticate  |
| `403`       | `insufficient_scope` | Request additional scopes in your token     |

---

Need help? Contact support@neuracraft.com or visit our [Help Center](https://support.neuracraft.com).