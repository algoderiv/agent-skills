# API Keys & Authentication

## API Base URL

```
https://api.stripe.com
```

The Stripe API is REST-based with form-encoded request bodies and JSON-encoded responses.

## Key Types

Stripe accounts include four default API keys:

| Key | Prefix | Usage | Environment |
|-----|--------|-------|-------------|
| Sandbox secret | `sk_test_` | Server-side testing | Server only |
| Sandbox publishable | `pk_test_` | Client-side testing | Browser/mobile |
| Live secret | `sk_live_` | Server-side production | Server only |
| Live publishable | `pk_live_` | Client-side production | Browser/mobile |
| Restricted | `rk_test_` / `rk_live_` | Limited-permission server | Server only |

**Example test keys:**
```
Secret:      sk_test_xxxx
Publishable: pk_test_xxxx
```

## Authentication Methods

### Bearer Token (recommended)

```bash
curl https://api.stripe.com/v1/charges \
  -H "Authorization: Bearer sk_test_xxxx"
```

### HTTP Basic Auth

```bash
curl https://api.stripe.com/v1/charges \
  -u sk_test_xxxx:
```

The colon after the key prevents curl from asking for a password.

### SDK Authentication

```python
# Python
import stripe
stripe.api_key = "sk_test_xxx"
```

```javascript
// Node.js
const stripe = require('stripe')('sk_test_xxx');
```

```ruby
# Ruby
Stripe.api_key = 'sk_test_xxx'
```

```go
// Go
stripe.Key = "sk_test_xxx"
```

```java
// Java
Stripe.apiKey = "sk_test_xxx";
```

```php
// PHP
$stripe = new \Stripe\StripeClient("sk_test_xxx");
```

```csharp
// .NET
StripeConfiguration.ApiKey = "sk_test_xxx";
```

## Secret Key Rules

- **Server-side only.** Never expose in browsers, mobile apps, or client-side code.
- Anyone with the key can make API calls as your account.
- Store in a secrets vault or encrypted environment variables.
- Never commit to source control or config files in version control.
- Never share via email, chat, or unencrypted channels.

## Publishable Key Rules

- Safe for client-side (browser or mobile app).
- Used to securely collect payment information with Stripe Elements or Checkout.
- Cannot make most API calls - only for tokenizing payment details.

## Restricted API Keys

Provide customizable, limited access to the API:
- Granular read/write permissions on specific resources
- Follow same server-side security as secret keys
- Reduce impact of a compromised key

## Organization API Keys

Access multiple Stripe business accounts. Requires:
- `Stripe-Context` header identifying the target account
- `Stripe-Version` header for consistency
- No publishable key variant

```bash
curl https://api.stripe.com/v1/balance \
  -u $ORG_SECRET_KEY: \
  -H "Stripe-Version: 2024-12-18.acacia" \
  -H "Stripe-Context: acct_1R3fqDP6919yCiFv"
```

## Key Management

### Create
Access Dashboard → API keys tab → Create. Copy immediately (cannot retrieve later).

### Rotate
Generates replacement key and revokes original. Specify expiration timing for graceful rotation.

### IP Restrictions
Apply to live keys using IPv4 CIDR notation (e.g., `100.10.38.0/24`).

## Security Best Practices

1. Store secret keys in secrets vault or encrypted environment variables
2. Use restricted keys to minimize blast radius
3. Apply IP restrictions on live keys
4. Rotate keys when team members leave
5. Use test keys for all sandbox development
6. Replace test keys with live ones only when going to production
