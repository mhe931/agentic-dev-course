# Authentication Patterns

Common authentication testing patterns with Hurl.

## Bearer token flow

```hurl
# Login
POST http://localhost:3000/api/auth/login
Content-Type: application/json
{
  "email": "admin@example.com",
  "password": "admin123"
}
HTTP 200
[Captures]
token: jsonpath "$.token"

# Authenticated request
GET http://localhost:3000/api/admin/dashboard
Authorization: Bearer {{token}}
HTTP 200

# Verify unauthenticated access is rejected
GET http://localhost:3000/api/admin/dashboard
HTTP 401

# Verify expired/invalid token is rejected
GET http://localhost:3000/api/admin/dashboard
Authorization: Bearer invalid-token-here
HTTP 401
```

## API key authentication

```hurl
GET http://localhost:3000/api/data
X-API-Key: {{api_key}}
HTTP 200

# Missing API key
GET http://localhost:3000/api/data
HTTP 401

# Invalid API key
GET http://localhost:3000/api/data
X-API-Key: invalid-key
HTTP 403
```

## Cookie-based session

```hurl
# Login — server sets session cookie
POST http://localhost:3000/api/login
Content-Type: application/json
{
  "username": "testuser",
  "password": "password"
}
HTTP 200
[Captures]
session_cookie: cookie "session_id"

# Use session cookie
GET http://localhost:3000/api/profile
Cookie: session_id={{session_cookie}}
HTTP 200

# Logout
POST http://localhost:3000/api/logout
Cookie: session_id={{session_cookie}}
HTTP 200

# Session should be invalid after logout
GET http://localhost:3000/api/profile
Cookie: session_id={{session_cookie}}
HTTP 401
```

## Role-based access

```hurl
# Login as regular user
POST http://localhost:3000/api/auth/login
Content-Type: application/json
{
  "email": "user@example.com",
  "password": "user123"
}
HTTP 200
[Captures]
user_token: jsonpath "$.token"

# Regular user can access own profile
GET http://localhost:3000/api/profile
Authorization: Bearer {{user_token}}
HTTP 200

# Regular user cannot access admin endpoint
GET http://localhost:3000/api/admin/users
Authorization: Bearer {{user_token}}
HTTP 403
```
