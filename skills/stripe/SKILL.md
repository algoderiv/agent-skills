---
name: stripe
description: Stripe payment platform integration guide covering API authentication, webhooks, error handling, testing, Connect platforms/marketplaces, SDKs, and development best practices. This skill should be used when building payment integrations with Stripe, implementing checkout flows, managing subscriptions, handling webhooks, creating Connect platforms, processing payouts, or debugging Stripe API errors.
---

# Stripe Development & Integration

Stripe is a payment infrastructure platform providing APIs and tools for internet commerce. This skill covers the core development workflows: API authentication, payment processing, webhook handling, error management, testing strategies, and building multi-party platforms with Stripe Connect.

## When to Use This Skill

This skill should be triggered when:
- Integrating Stripe payments (checkout, subscriptions, invoices)
- Setting up API keys and authentication
- Implementing webhook endpoints and signature verification
- Handling Stripe errors and decline codes
- Testing with sandbox/test cards
- Building platforms or marketplaces with Stripe Connect
- Managing connected accounts, charges, transfers, and payouts
- Using Stripe SDKs (Node.js, Python, Ruby, Go, Java, PHP, .NET)
- Debugging Stripe API issues

## Quick Reference

### API Base URL

```
https://api.stripe.com
```

### Authentication

```bash
# Secret key (server-side only)
curl https://api.stripe.com/v1/charges \
  -u sk_test_xxxx:

# Or via Authorization header
curl https://api.stripe.com/v1/charges \
  -H "Authorization: Bearer sk_test_xxxx"
```

### Key Prefixes

| Mode | Secret Key | Publishable Key | Restricted Key |
|------|-----------|-----------------|----------------|
| Sandbox | `sk_test_` | `pk_test_` | `rk_test_` |
| Live | `sk_live_` | `pk_live_` | `rk_live_` |

### Available SDKs

| Platform | Package |
|----------|---------|
| Node.js | `stripe` |
| Python | `stripe` |
| Ruby | `stripe` |
| Go | `github.com/stripe/stripe-go` |
| Java | `com.stripe:stripe-java` |
| PHP | `stripe/stripe-php` |
| .NET | `Stripe.net` |
| React (web) | `@stripe/react-stripe-js` |
| iOS | `StripePaymentSheet` |
| Android | `com.stripe:stripe-android` |
| React Native | `@stripe/stripe-react-native` |

### Common Test Cards

| Card | Number | PaymentMethod |
|------|--------|---------------|
| Visa | `4242424242424242` | `pm_card_visa` |
| Visa (debit) | `4000056655665556` | `pm_card_visa_debit` |
| Mastercard | `5555555555554444` | `pm_card_mastercard` |
| Amex | `378282246310005` | `pm_card_amex` |
| Declined | `4000000000000002` | `pm_card_chargeDeclined` |
| Fraud block | - | `pm_card_radarBlock` |
| Expired | - | `pm_card_chargeDeclinedExpiredCard` |

Use any future expiry date, any 3-digit CVC (4 for Amex), any other values.

### Create a Payment Intent (curl)

```bash
curl https://api.stripe.com/v1/payment_intents \
  -u sk_test_xxx: \
  -d amount=2000 \
  -d currency=usd \
  -d "payment_method_types[]"=card
```

### Connect Charge Types

| Type | Use Case | Fund Flow |
|------|----------|-----------|
| Direct charges | SaaS platforms | Customer → Connected Account → Platform fee |
| Destination charges | Marketplaces | Customer → Platform → Connected Account |
| Separate charges & transfers | Complex splits | Customer → Platform → Multiple accounts |

### Webhook Signature Verification (Node.js)

```javascript
const stripe = require('stripe')('sk_test_xxx');
const endpointSecret = 'whsec_xxx';

app.post('/webhook', (req, res) => {
  const sig = req.headers['stripe-signature'];
  let event;
  try {
    event = stripe.webhooks.constructEvent(req.body, sig, endpointSecret);
  } catch (err) {
    return res.status(400).send(`Webhook Error: ${err.message}`);
  }
  // Handle event
  res.status(200).json({received: true});
});
```

### Error Types

| Exception Class | HTTP | Cause |
|----------------|------|-------|
| `CardError` | 402 | Payment declined / card issue |
| `InvalidRequestError` | 400 | Wrong parameters or invalid state |
| `AuthenticationError` | 401 | Bad API key |
| `RateLimitError` | 429 | Too many requests |
| `APIConnectionError` | - | Network failure |
| `APIError` | 500 | Stripe server error |
| `IdempotencyError` | 400 | Mismatched idempotency key |
| `PermissionError` | 403 | Restricted key lacks permissions |
| `SignatureVerificationError` | - | Invalid webhook signature |

## References

- `api-keys-auth.md` - API keys, authentication, security best practices, key management
- `error-handling.md` - Error types, exception handling patterns, test cards for errors
- `webhooks.md` - Webhook setup, signature verification, event handling, testing, best practices
- `testing.md` - Sandbox environment, test card numbers by brand/country, PaymentMethods, going live
- `connect-overview.md` - Connect platform design, account types, charge types, onboarding, payouts
- `connect-charges.md` - Direct/destination/separate charges, platform fees, fund flow
- `development-essentials.md` - SDKs, expanding objects, pagination, idempotency, versioning, metadata
