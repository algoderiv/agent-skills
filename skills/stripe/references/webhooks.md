# Webhooks

Webhooks deliver real-time event notifications via JSON payloads to registered HTTPS endpoints.

## Event Formats

1. **Snapshot Events** - Include complete object state in `data.object`
2. **Thin Events** - Minimal data; retrieve full details via `fetchRelatedObject()` or API

## Creating a Webhook Handler

Requirements:
- Accept POST requests with JSON body
- Return `2xx` status code **immediately** (before complex processing)
- Use HTTPS in production (HTTP ok for local dev)

### Node.js (Express)

```javascript
const stripe = require('stripe')('sk_test_xxx');
const express = require('express');
const app = express();

const endpointSecret = 'whsec_xxx';

app.post('/webhook', express.raw({type: 'application/json'}), (req, res) => {
  const sig = req.headers['stripe-signature'];
  let event;

  try {
    event = stripe.webhooks.constructEvent(req.body, sig, endpointSecret);
  } catch (err) {
    console.log(`Signature verification failed: ${err.message}`);
    return res.status(400).send(`Webhook Error: ${err.message}`);
  }

  switch (event.type) {
    case 'payment_intent.succeeded':
      const paymentIntent = event.data.object;
      console.log(`PaymentIntent ${paymentIntent.id} succeeded`);
      break;
    case 'payment_method.attached':
      const paymentMethod = event.data.object;
      console.log(`PaymentMethod ${paymentMethod.id} attached`);
      break;
    default:
      console.log(`Unhandled event type ${event.type}`);
  }

  res.status(200).json({received: true});
});

app.listen(4242, () => console.log('Running on port 4242'));
```

### Python (Flask)

```python
import stripe
from flask import Flask, request, jsonify

app = Flask(__name__)
stripe.api_key = 'sk_test_xxx'
endpoint_secret = 'whsec_xxx'

@app.route('/webhook', methods=['POST'])
def webhook():
    payload = request.data
    sig_header = request.headers.get('Stripe-Signature')

    try:
        event = stripe.Webhook.construct_event(
            payload, sig_header, endpoint_secret
        )
    except ValueError:
        return 'Invalid payload', 400
    except stripe.SignatureVerificationError:
        return 'Invalid signature', 400

    if event['type'] == 'payment_intent.succeeded':
        payment_intent = event['data']['object']
        print(f"Payment succeeded: {payment_intent['id']}")
    elif event['type'] == 'payment_intent.payment_failed':
        payment_intent = event['data']['object']
        error = payment_intent.get('last_payment_error', {})
        print(f"Payment failed: {error.get('message')}")

    return jsonify(success=True), 200
```

### Ruby (Sinatra)

```ruby
require 'stripe'
require 'sinatra'

Stripe.api_key = 'sk_test_xxx'
endpoint_secret = 'whsec_xxx'

set :port, 4242

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError
    status 400
    return
  rescue Stripe::SignatureVerificationError
    status 400
    return
  end

  case event.type
  when 'payment_intent.succeeded'
    payment_intent = event.data.object
    puts "Succeeded: #{payment_intent.id}"
  end

  status 200
end
```

## Signature Verification

The `Stripe-Signature` header contains:
```
t=1492774577,v1=5257a869e7ecebeda32affa62cdca3fa51cad7e77a0e56ff536d0ce8e108d8bd
```

**Manual verification steps:**
1. Extract `t` (timestamp) and `v1` (signature) from header
2. Prepare signed payload: `{timestamp}.{request_body}`
3. Compute HMAC-SHA256 using endpoint secret as key
4. Compare computed signature with header signature (constant-time)
5. Reject if timestamp is too old (default 5-minute tolerance)

## Local Testing with Stripe CLI

```bash
# Forward all events
stripe listen --forward-to localhost:4242/webhook

# Forward specific events
stripe listen --events payment_intent.created,payment_intent.succeeded \
  --forward-to localhost:4242/webhook

# Forward thin events
stripe listen --forward-thin-to localhost:4242/webhook --thin-events "*"

# Trigger a test event
stripe trigger payment_intent.succeeded
stripe trigger payment_intent.payment_failed
```

## Creating Event Destinations via API

```bash
curl -X POST https://api.stripe.com/v2/core/event_destinations \
  -H "Authorization: Bearer sk_test_xxx" \
  -H "Stripe-Version: 2024-12-18.acacia" \
  --json '{
    "name": "My webhook",
    "type": "webhook_endpoint",
    "event_payload": "thin",
    "enabled_events": ["v1.billing.meter.error_report_triggered"],
    "webhook_endpoint": {
      "url": "https://example.com/webhook"
    }
  }'
```

Accounts support up to 16 event destinations.

## Retry Behavior

- **Live mode:** Retries for up to 3 days with exponential backoff
- **Sandbox:** 3 retries over hours
- **Manual retry:** Dashboard "Resend" button (up to 15 days) or CLI (up to 30 days)

## Event Ordering

Stripe does **not** guarantee delivery order. Design systems to handle out-of-order events. Use API retrieval for missing objects.

## Best Practices

1. **Return 2xx immediately** before complex processing
2. **Verify signatures** using official libraries
3. **Handle duplicates** - Log processed event IDs
4. **Listen only to needed events** - Reduce server load
5. **Process asynchronously** - Use queues for concurrent delivery spikes
6. **Exempt from CSRF** - Webhook routes need no CSRF protection
7. **Use HTTPS** - Required for production endpoints
8. **Roll secrets periodically** - Dashboard → Webhooks → Roll secret
9. **Prevent replay attacks** - Verify timestamp within tolerance window
