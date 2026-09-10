# Variable Capture and Chaining

Hurl supports capturing values from responses and reusing them in subsequent requests within the same file.

## Capture sources

```hurl
POST http://localhost:3000/api/auth/login
Content-Type: application/json
{
  "email": "user@example.com",
  "password": "secret"
}
HTTP 200
[Captures]
# From JSON response body
token: jsonpath "$.token"
user_id: jsonpath "$.user.id"

# From response headers
content_type: header "Content-Type"
request_id: header "X-Request-Id"

# From cookies
session: cookie "session_id"

# From body (regex)
csrf: body regex "name=\"csrf\" value=\"([^\"]+)\""

# Duration of the request
response_time: duration
```

## Using captured variables

```hurl
# Variables are referenced with double curly braces
GET http://localhost:3000/api/users/{{user_id}}
Authorization: Bearer {{token}}
HTTP 200
```

## Combining CLI variables with captures

```bash
# Pass base URL and credentials from command line
hurl --variable base_url=http://localhost:3000 \
     --variable email=test@example.com \
     --variable password=secret \
     auth-flow.hurl
```

```hurl
# Use CLI variables for setup, capture for chaining
POST {{base_url}}/api/auth/login
Content-Type: application/json
{
  "email": "{{email}}",
  "password": "{{password}}"
}
HTTP 200
[Captures]
token: jsonpath "$.token"

GET {{base_url}}/api/profile
Authorization: Bearer {{token}}
HTTP 200
```
