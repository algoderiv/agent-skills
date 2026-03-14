# Error Handling

## Error Response Structure

```json
{
  "error": {
    "type": "card_error",
    "code": "card_declined",
    "message": "Your card was declined.",
    "param": "payment_method",
    "doc_url": "https://docs.stripe.com/error-codes/card-declined",
    "request_log_url": "https://dashboard.stripe.com/logs/req_xxx"
  }
}
```

Fields:
- `type` - Error category
- `code` - Specific error code
- `message` - Human-readable description
- `param` - The parameter that caused the error
- `doc_url` - Link to documentation
- Request ID (`req_xxx`) - Unique identifier for debugging

## Error Types

### CardError (HTTP 402)
Payment declined or card issue.

**Three categories:**

1. **Fraud block** (Stripe Radar blocked it)
   - Check: `charge.outcome.type == 'blocked'`
   - Action: Ask customer for alternative payment method

2. **Issuer decline** (`code == "card_declined"`)
   - Action: Consult decline codes documentation

3. **Other card errors**
   - Action: Check `error.code` for specific guidance

**Test cards for payment errors:**
- `pm_card_radarBlock` - Blocked by fraud detection
- `pm_card_visa_chargeDeclined` - Declined by issuer
- `pm_card_chargeDeclinedExpiredCard` - Expired card
- `pm_card_chargeDeclinedProcessingError` - Processing error

### InvalidRequestError (HTTP 400)
Wrong parameters, invalid state, or malformed request.
- Check `error.param` for the problematic parameter
- Follow `error.doc_url` for details

### AuthenticationError (HTTP 401)
Invalid API key or AK/SK.
- Verify correct API key is being used
- Check key hasn't been rotated or revoked

### RateLimitError (HTTP 429)
Too many API calls.
- Implement exponential backoff retry logic
- Contact support for rate limit increases

### APIConnectionError
Network failure between your server and Stripe.
- **Treat result as indeterminate** (don't assume success or failure)
- Use idempotency keys for safe retries
- Enable automatic retries in SDK
- Retrieve object to check actual status

### APIError (HTTP 500)
Something went wrong on Stripe's end (rare).
- Treat result as indeterminate
- Rely on webhooks for outcome

### IdempotencyError (HTTP 400)
Mismatched idempotency key reuse.
- Only reuse idempotency keys for identical requests
- Keys must be under 255 characters

### PermissionError (HTTP 403)
Restricted key lacks required permissions.
- Don't use restricted keys for services they can't access

### SignatureVerificationError
Invalid webhook signature.
- Respond with `400 Bad Request`
- Verify using correct endpoint secret (not API key)
- Check webhook signature docs

## Error Handling Code Examples

### Python
```python
import stripe

try:
    stripe.PaymentIntent.create(
        amount=2000,
        currency="usd",
        payment_method="pm_card_visa",
    )
except stripe.CardError as e:
    print(f"Card error: {e.user_message}")
    print(f"Code: {e.code}")
except stripe.InvalidRequestError as e:
    print(f"Invalid request: {e.user_message}")
except stripe.AuthenticationError:
    print("Authentication failed - check API key")
except stripe.RateLimitError:
    print("Rate limited - retry with backoff")
except stripe.APIConnectionError:
    print("Network error - retry with idempotency key")
except stripe.StripeError as e:
    print(f"Stripe error: {e.http_status} {e.code} {e.user_message}")
```

### Node.js
```javascript
try {
  await stripe.paymentIntents.create({
    amount: 2000,
    currency: 'usd',
    payment_method: 'pm_card_visa',
  });
} catch (error) {
  switch (error.type) {
    case 'StripeCardError':
      console.log(`Card error: ${error.message}`);
      break;
    case 'StripeInvalidRequestError':
      console.log(`Invalid request: ${error.message}`);
      break;
    case 'StripeAuthenticationError':
      console.log('Check API key');
      break;
    case 'StripeRateLimitError':
      console.log('Rate limited');
      break;
    case 'StripeAPIError':
      console.log('Stripe server error');
      break;
    case 'StripeConnectionError':
      console.log('Network error');
      break;
    default:
      console.log(`Unknown error: ${error.message}`);
  }
}
```

### Ruby
```ruby
begin
  Stripe::PaymentIntent.create(params)
rescue Stripe::CardError => e
  puts "Card error: #{e.error.message}"
rescue Stripe::InvalidRequestError => e
  puts "Invalid request: #{e.message}"
rescue Stripe::AuthenticationError
  puts "Check API key"
rescue Stripe::RateLimitError
  puts "Rate limited"
rescue Stripe::APIConnectionError
  puts "Network error"
rescue Stripe::StripeError => e
  puts "Status: #{e.http_status}, Code: #{e.code}"
end
```

### Go
```go
result, err := paymentintent.New(params)
if err != nil {
    if stripeErr, ok := err.(*stripe.Error); ok {
        switch stripeErr.Type {
        case stripe.ErrorTypeCard:
            fmt.Printf("Card error: %s\n", stripeErr.Msg)
        case stripe.ErrorTypeInvalidRequest:
            fmt.Printf("Invalid request: %s\n", stripeErr.Msg)
        default:
            fmt.Printf("Error: %s\n", stripeErr.Msg)
        }
    }
}
```

## Webhook Error Handling

Monitor webhook events for async error detection:

```python
@app.route('/webhook', methods=['POST'])
def webhook():
    event = stripe.Webhook.construct_event(
        request.data, request.headers['Stripe-Signature'], endpoint_secret
    )

    if event['type'] == 'payment_intent.payment_failed':
        pi = event['data']['object']
        error = pi.get('last_payment_error', {})
        print(f"Payment failed: {error.get('type')} - {error.get('message')}")

    return '', 200
```

## Objects That Store Failure Info

| Object | Attribute | Value Type |
|--------|-----------|-----------|
| PaymentIntent | `last_payment_error` | Error object |
| SetupIntent | `last_setup_error` | Error object |
| Invoice | `last_finalization_error` | Error object |
| SetupAttempt | `setup_error` | Error object |
| Payout | `failure_code` | Payout failure code |
| Refund | `failure_reason` | Refund failure code |
