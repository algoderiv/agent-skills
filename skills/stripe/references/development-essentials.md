# Development Essentials

## SDKs

### Server-Side

| Language | Package | Install |
|----------|---------|---------|
| Node.js | `stripe` | `npm install stripe` |
| Python | `stripe` | `pip install stripe` |
| Ruby | `stripe` | `gem install stripe` |
| Go | `stripe-go` | `go get github.com/stripe/stripe-go/v82` |
| Java | `stripe-java` | Maven: `com.stripe:stripe-java` |
| PHP | `stripe-php` | `composer require stripe/stripe-php` |
| .NET | `Stripe.net` | `dotnet add package Stripe.net` |

### Web (Client-Side)

| Package | Install |
|---------|---------|
| `@stripe/stripe-js` | `npm install @stripe/stripe-js` |
| `@stripe/react-stripe-js` | `npm install @stripe/react-stripe-js` |

### Mobile

| Platform | Package |
|----------|---------|
| iOS | `StripePaymentSheet` (Swift Package Manager) |
| Android | `com.stripe:stripe-android` (Gradle) |
| React Native | `@stripe/stripe-react-native` |

### SDK Versioning
Stripe uses semantic versioning. Major version increments correspond to breaking API changes.

## Expanding Objects

Use `expand` to include related objects in API responses instead of just IDs.

```bash
# Expand customer on a charge
curl https://api.stripe.com/v1/charges/ch_xxx \
  -u sk_test_xxx: \
  -d "expand[]"=customer \
  -G

# Nested expansion
curl https://api.stripe.com/v1/charges/ch_xxx \
  -u sk_test_xxx: \
  -d "expand[]"="payment_intent.customer" \
  -G
```

```python
charge = stripe.Charge.retrieve(
    'ch_xxx',
    expand=['customer', 'payment_intent.customer']
)
# Access expanded object
print(charge.customer.email)  # Instead of just customer ID
```

```javascript
const charge = await stripe.charges.retrieve('ch_xxx', {
  expand: ['customer', 'payment_intent.customer'],
});
```

**Rules:**
- Maximum depth: 4 levels
- List requests: prefix with `data.` (e.g., `data.customer`)
- Multiple expansions: pass array
- Deep expansions on large lists may be slow

## Pagination

Stripe uses cursor-based pagination on all list endpoints.

### Parameters

| Parameter | Description |
|-----------|-------------|
| `limit` | Number of objects (1-100, default 10) |
| `starting_after` | Cursor for next page (object ID) |
| `ending_before` | Cursor for previous page (object ID) |

```bash
# First page
curl https://api.stripe.com/v1/charges?limit=3 \
  -u sk_test_xxx:

# Next page (use last object's ID)
curl https://api.stripe.com/v1/charges?limit=3&starting_after=ch_xxx \
  -u sk_test_xxx:
```

```python
# Auto-pagination in Python
for charge in stripe.Charge.list(limit=100).auto_paging_iter():
    print(charge.id)
```

```javascript
// Auto-pagination in Node.js
for await (const charge of stripe.charges.list({limit: 100})) {
  console.log(charge.id);
}
```

### Response Structure

```json
{
  "object": "list",
  "url": "/v1/charges",
  "has_more": true,
  "data": [
    {"id": "ch_xxx", ...},
    {"id": "ch_yyy", ...}
  ]
}
```

## Idempotent Requests

Safely retry requests without performing the same operation twice.

```bash
curl https://api.stripe.com/v1/charges \
  -u sk_test_xxx: \
  -H "Idempotency-Key: unique-key-123" \
  -d amount=2000 \
  -d currency=usd \
  -d source=tok_visa
```

```python
stripe.PaymentIntent.create(
    amount=2000,
    currency="usd",
    idempotency_key="unique-key-123",
)
```

```javascript
await stripe.paymentIntents.create(
  { amount: 2000, currency: 'usd' },
  { idempotencyKey: 'unique-key-123' }
);
```

**Rules:**
- Keys must be unique per request
- Maximum 255 characters
- Stripe caches results for 24 hours
- Reusing a key with different parameters returns `IdempotencyError`
- SDKs can auto-generate keys with automatic retries enabled

## API Versioning

- API versions are dates: `2024-12-18.acacia`
- Set version in Dashboard or per-request:

```bash
curl https://api.stripe.com/v1/charges \
  -u sk_test_xxx: \
  -H "Stripe-Version: 2024-12-18.acacia"
```

- Webhook event objects use the API version set at event creation time
- Upgrading is one-way; test thoroughly before upgrading

## Metadata

Attach key-value data to most Stripe objects:

```bash
curl https://api.stripe.com/v1/charges \
  -u sk_test_xxx: \
  -d amount=2000 \
  -d currency=usd \
  -d "metadata[order_id]"=12345 \
  -d "metadata[customer_email]"=user@example.com
```

```python
stripe.PaymentIntent.create(
    amount=2000,
    currency="usd",
    metadata={
        "order_id": "12345",
        "customer_email": "user@example.com",
    },
)
```

**Rules:**
- Up to 50 keys per object
- Key names: max 40 characters
- Values: max 500 characters
- Not used for Stripe operations (for your own use)
- Searchable in Dashboard

## Request IDs

Every API response includes a unique request ID:

```
Request-Id: req_xxx
```

Include this ID when contacting Stripe support for debugging.

## Rate Limits

- Stripe applies rate limits on API requests
- Returns `429 Too Many Requests` when exceeded
- Implement exponential backoff for retries
- Contact support for rate limit increases if needed

## Development Tools

### Stripe CLI
```bash
# Install
brew install stripe/stripe-cli/stripe

# Login
stripe login

# Listen for webhooks
stripe listen --forward-to localhost:4242/webhook

# Trigger events
stripe trigger payment_intent.succeeded

# Make API calls
stripe charges list --limit 3
```

### Workbench
In-Dashboard tool for debugging and management.

### VS Code Extension
`Stripe for VS Code` - Integration directly in your editor.

### Postman Collection
Available at Stripe's GitHub for API exploration.
