---
name: hurl-api-testing
description: Test REST and GraphQL APIs using the Hurl CLI. Use when the user needs to send HTTP requests, test API endpoints, validate response status/headers/bodies, chain requests with captured values, or run API test suites. Drives the hurl command-line tool to execute .hurl files with built-in assertions.
---

# API Testing with Hurl

Hurl is a command-line tool that runs HTTP requests defined in a simple plain-text format. Each `.hurl` file contains one or more requests with optional assertions and variable captures — making it ideal for API testing, integration testing, and request chaining.

## Quick start

```bash
# run a single hurl file
hurl api-test.hurl
# run in test mode (shows pass/fail summary)
hurl --test api-test.hurl
# run with verbose output (shows full request/response)
hurl --verbose api-test.hurl
# run all hurl files in a directory
hurl --test tests/*.hurl
```

## Hurl file syntax

A `.hurl` file contains one or more **entries**. Each entry is an HTTP request followed by optional response assertions.

### Basic request and assertions

```hurl
# Simple GET with status check
GET http://localhost:3000/api/products
HTTP 200
[Asserts]
header "Content-Type" contains "application/json"
jsonpath "$.products" count > 0
jsonpath "$.products[0].name" exists
```

### POST with JSON body

```hurl
POST http://localhost:3000/api/products
Content-Type: application/json
{
  "name": "Test Product",
  "price": 29.99,
  "category": "electronics"
}
HTTP 201
[Asserts]
jsonpath "$.id" exists
jsonpath "$.name" == "Test Product"
jsonpath "$.price" == 29.99
```

### Variable capture and chaining

```hurl
# Create a resource and capture its ID
POST http://localhost:3000/api/products
Content-Type: application/json
{
  "name": "Chained Product",
  "price": 19.99
}
HTTP 201
[Captures]
product_id: jsonpath "$.id"

# Use the captured ID in the next request
GET http://localhost:3000/api/products/{{product_id}}
HTTP 200
[Asserts]
jsonpath "$.name" == "Chained Product"

# Clean up
DELETE http://localhost:3000/api/products/{{product_id}}
HTTP 204
```

### Authentication

```hurl
# Login and capture token
POST http://localhost:3000/api/auth/login
Content-Type: application/json
{
  "email": "test@example.com",
  "password": "password123"
}
HTTP 200
[Captures]
auth_token: jsonpath "$.token"

# Use token in subsequent request
GET http://localhost:3000/api/profile
Authorization: Bearer {{auth_token}}
HTTP 200
[Asserts]
jsonpath "$.email" == "test@example.com"
```

## Commands

### Running tests

```bash
# run a single file
hurl request.hurl
# test mode — pass/fail summary, no response body output
hurl --test request.hurl
# run multiple files
hurl --test tests/auth.hurl tests/products.hurl tests/orders.hurl
# glob pattern
hurl --test tests/**/*.hurl
# verbose — full request/response details
hurl --verbose request.hurl
# very verbose — includes wire-level details
hurl --very-verbose request.hurl
```

### Variables

```bash
# pass variables from command line
hurl --variable base_url=http://localhost:3000 request.hurl
hurl --variable user=admin --variable pass=secret auth.hurl
# use environment variable file
hurl --variables-file vars.env request.hurl
```

### Output control

```bash
# output response body only (default when not --test)
hurl request.hurl
# output to file
hurl --output response.json request.hurl
# include headers in output
hurl --include request.hurl
# JSON report for CI
hurl --test --report-json report/ tests/*.hurl
# JUnit report
hurl --test --report-junit report.xml tests/*.hurl
```

### Request options

```bash
# follow redirects
hurl --location request.hurl
# set timeout (seconds)
hurl --connect-timeout 5 --max-time 30 request.hurl
# ignore SSL certificate errors
hurl --insecure request.hurl
# use a proxy
hurl --proxy http://proxy:8080 request.hurl
# limit max redirects
hurl --max-redirs 5 request.hurl
```

### Retry and delay

```bash
# retry failed requests (useful for eventual consistency)
hurl --retry 3 request.hurl
# delay between retries (milliseconds)
hurl --retry --retry-interval 1000 request.hurl
# delay between requests in a file (milliseconds)
hurl --delay 500 request.hurl
```

## Assertion reference

```hurl
[Asserts]
# Status
status == 200
status >= 200
status < 300

# Headers
header "Content-Type" == "application/json; charset=utf-8"
header "Content-Type" contains "json"
header "Cache-Control" exists
header "X-Custom" not exists

# Body — jsonpath
jsonpath "$.name" == "value"
jsonpath "$.count" > 0
jsonpath "$.count" >= 10
jsonpath "$.items" count == 5
jsonpath "$.active" == true
jsonpath "$.data" exists
jsonpath "$.error" not exists
jsonpath "$.name" startsWith "Test"
jsonpath "$.name" contains "Product"
jsonpath "$.name" matches "^[A-Z].*"
jsonpath "$.tags" includes "featured"

# Body — raw
body contains "success"
body startsWith "{"

# Response time (milliseconds)
duration < 1000
duration < 500

# SHA-256 of response body
sha256 == hex,abc123...;

# Certificate
certificate "Expire-Date" daysAfterNow > 30
```

## Example: Full CRUD test suite

```hurl
# --- CREATE ---
POST http://localhost:3000/api/products
Content-Type: application/json
{
  "name": "Hurl Test Product",
  "price": 49.99
}
HTTP 201
[Captures]
product_id: jsonpath "$.id"
[Asserts]
jsonpath "$.name" == "Hurl Test Product"
duration < 500

# --- READ ---
GET http://localhost:3000/api/products/{{product_id}}
HTTP 200
[Asserts]
jsonpath "$.id" == {{product_id}}
jsonpath "$.name" == "Hurl Test Product"
jsonpath "$.price" == 49.99

# --- UPDATE ---
PUT http://localhost:3000/api/products/{{product_id}}
Content-Type: application/json
{
  "name": "Updated Product",
  "price": 59.99
}
HTTP 200
[Asserts]
jsonpath "$.name" == "Updated Product"
jsonpath "$.price" == 59.99

# --- DELETE ---
DELETE http://localhost:3000/api/products/{{product_id}}
HTTP 204

# --- VERIFY DELETED ---
GET http://localhost:3000/api/products/{{product_id}}
HTTP 404
```

## Example: Error handling tests

```hurl
# Missing required field
POST http://localhost:3000/api/products
Content-Type: application/json
{
  "price": 29.99
}
HTTP 400
[Asserts]
jsonpath "$.error" exists

# Unauthorized access
GET http://localhost:3000/api/admin/users
HTTP 401

# Not found
GET http://localhost:3000/api/products/nonexistent-id
HTTP 404
```

## Specific topics

* **Variable capture and chaining** [references/variable-capture.md](references/variable-capture.md)
* **Authentication patterns** [references/authentication.md](references/authentication.md)
* **CI integration** [references/ci-integration.md](references/ci-integration.md)
