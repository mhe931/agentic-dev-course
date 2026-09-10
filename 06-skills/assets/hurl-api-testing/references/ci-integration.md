# CI Integration

Running Hurl tests in continuous integration pipelines.

## Test mode

Always use `--test` in CI — it suppresses response bodies and shows a pass/fail summary:

```bash
hurl --test tests/**/*.hurl
```

Output:

```
tests/auth.hurl: Running [1/3]
tests/auth.hurl: Success (4 request(s) in 120 ms)
tests/products.hurl: Running [2/3]
tests/products.hurl: Success (6 request(s) in 230 ms)
tests/orders.hurl: Running [3/3]
tests/orders.hurl: Success (3 request(s) in 95 ms)
--------------------------------------------------------------------------------
Executed files:  3
Succeeded files: 3 (100.0%)
Failed files:    0 (0.0%)
Duration:        450 ms
```

## Reports

```bash
# JUnit XML (most CI systems)
hurl --test --report-junit report.xml tests/**/*.hurl

# JSON report (for custom processing)
hurl --test --report-json report/ tests/**/*.hurl

# HTML report
hurl --test --report-html report/ tests/**/*.hurl
```

## Variables for environments

Use `--variables-file` to switch between environments:

```bash
# vars/staging.env
base_url=https://staging.example.com
api_key=staging-key-123

# vars/production.env
base_url=https://api.example.com
api_key=prod-key-456
```

```bash
hurl --test --variables-file vars/staging.env tests/**/*.hurl
```

## Retry for eventual consistency

Some API tests need to wait for async operations:

```bash
# Retry failed assertions up to 3 times with 1s delay
hurl --test --retry 3 --retry-interval 1000 tests/async-flow.hurl
```

## GitHub Actions example

```yaml
- name: Run API tests
  run: |
    hurl --test \
      --variable base_url=http://localhost:3000 \
      --report-junit test-results.xml \
      tests/**/*.hurl
```
