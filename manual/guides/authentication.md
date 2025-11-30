# NeuraCraft Authentication Guide

Welcome to NeuraCraft's authentication guide! Secure access is essential when working with automation pipelines. This document covers all authentication methods supported by our platform.

## Authentication Types at a Glance
- **OAuth 2.0**: Recommended for production systems
- **API Keys**: Simple development access
- **Command-Line Authentication**: Local testing and CI/CD
- **Plugin Authentication**: Custom handlers for specialized workflows

Choose the method that best matches your security requirements and use case.

---

## OAuth 2.0 Implementation
Our recommended method for production environments follows RFC 6749 standards.

### Configuration Steps:
1. **Register Your Client**  
   Navigate to `NeuraCraft Console > Security > OAuth Clients` and create a new client:
   - Redirect URI: `https://your-domain.com/oauth/callback`
   - Grant types: Authorization Code (recommended)

2. **Authorization Flow**  
   Typical web application integration:
   ```http
   GET /oauth/authorize?
     response_type=code&
     client_id=YOUR_CLIENT_ID&
     redirect_uri=REDIRECT_URI&
     scope=pipelines:read pipelines:write&
     state=RANDOM_STRING
   ```

3. **Token Acquisition**  
   Exchange authorization code for tokens:
   ```bash
   curl -X POST https://api.neuracraft.com/oauth/token \
     -d client_id=YOUR_CLIENT_ID \
     -d client_secret=YOUR_CLIENT_SECRET \
     -d code=AUTHORIZATION_CODE \
     -d grant_type=authorization_code \
     -d redirect_uri=REDIRECT_URI
   ```

4. **Token Refresh**  
   Use refresh tokens before expiration (14 days):
   ```json
   {
     "grant_type": "refresh_token",
     "refresh_token": "YOUR_REFRESH_TOKEN",
     "client_id": "YOUR_CLIENT_ID"
   }
   ```

---

## API Key Authentication
Ideal for service accounts and CLI tools.

### Usage Guidelines:
1. **Generate Keys**  
   In your project dashboard:
   ```yaml
   sec_cha_neuracraft:
     auth:
       api_key: neu_sk_prod_abc123xyz456
   ```

2. **Request Authorization**  
   Include in API headers:
   ```http
   Authorization: Bearer neu_sk_prod_abc123xyz456
   ```

3. **Key Rotation Policy**  
   - Rotate quarterly or after personnel changes
   - Use key versioning for zero-downtime rotations:
     ```bash
     neuracraft keys rotate --keep-previous 48h
     ```

---

## Command-Line Authentication
Authenticate via terminal for local development.

### Login Process:
```bash
neuracraft auth login
# Follow browser-based authentication flow

# Verify session status:
neuracraft auth status

# Active sessions list:
neuracraft auth sessions --active
```

### Session Management:
```bash
# Extend session validity
neuracraft auth renew --ttl 72h

# Session termination
neuracraft auth logout --all
```

---

## Plugin Authentication
Extend authentication mechanisms through our plugin architecture.

### Implementation Steps:
1. Create `auth_handler.py`:
   ```python
   from neuracraft.plugins import AuthPlugin

   class CustomAuth(AuthPlugin):
     name = "vault-integration"

     def authenticate(self, request):
       vault_token = request.headers.get("X-Vault-Token")
       return self.verify_vault_signature(vault_token)
   ```

2. Register in `manifest.yaml`:
   ```yaml
   auth_mechanisms:
     - name: vault-integration
       priority: 200
       paths:
         - "/vault/*"
         - "/secrets/**"
   ```

3. Configuration reference:
   ```bash
   # Enable in neuracraft.yaml
   auth:
     plugins:
       vault-integration:
         endpoint: "https://vault.example.com"
         timeout: 2.5
   ```

---

## Security Best Practices
1. **Always use HTTPS** - Plain HTTP is automatically rejected
2. **Principle of Least Privilege** - Restrict token scopes to minimum required permissions
3. **Secret Storage** - Never commit keys to source control
4. **Regular Audits** - Review active sessions monthly via audit logs
5. **Token Expiry** - Set expiration for temporary tokens (max 1 hour for sensitive operations)

```bash
# Audit command examples:
neuracraft audit auth --last-30d
neuracraft audit keys --expired
```

---

## Need Help?
- Explore our [Plugin Development Guide](plugins.md) for custom authentication
- Review [CLI Reference](cli-reference.md) for additional commands
- Contact security@neuracraft.com for urgent credential compromises

Secure your automation pipelines with confidence! 🔒