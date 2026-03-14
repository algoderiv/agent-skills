# Connect Charges

To accept a payment on a Connect platform, create a charge. The charge type determines fund distribution, statement appearance, and refund/chargeback liability.

## Charge Type Comparison

| | Direct | Destination | Separate Charges & Transfers |
|-|--------|-------------|------------------------------|
| Fund flow | Customer → Connected Account | Customer → Platform → Connected Account | Customer → Platform → Multiple Accounts |
| Statement | Connected account | Platform | Platform |
| Refund liability | Connected account | Platform | Platform |
| Cross-region | No restriction | Regional limits | Regional limits |
| Use case | SaaS platforms | Marketplaces | Complex splits |

## Direct Charges

Create charges directly on a connected account. The connected account is the merchant of record.

**Requirements:** Connected account must have `card_payments` capability active.

```python
import stripe

# Create a PaymentIntent on the connected account
stripe.PaymentIntent.create(
    amount=2000,
    currency="usd",
    payment_method_types=["card"],
    application_fee_amount=200,  # Platform fee
    stripe_account="acct_connected_xxx",  # Connected account ID
)
```

```javascript
const paymentIntent = await stripe.paymentIntents.create({
  amount: 2000,
  currency: 'usd',
  payment_method_types: ['card'],
  application_fee_amount: 200,
}, {
  stripeAccount: 'acct_connected_xxx',
});
```

```bash
curl https://api.stripe.com/v1/payment_intents \
  -u sk_test_xxx: \
  -H "Stripe-Account: acct_connected_xxx" \
  -d amount=2000 \
  -d currency=usd \
  -d "payment_method_types[]"=card \
  -d application_fee_amount=200
```

**Fee handling:** Platform can choose whether Stripe debits processing fees from the connected account or the platform.

## Destination Charges

Create charges on the platform and immediately transfer funds to a connected account.

```python
stripe.PaymentIntent.create(
    amount=2000,
    currency="usd",
    payment_method_types=["card"],
    transfer_data={"destination": "acct_connected_xxx"},
    application_fee_amount=200,
)
```

```javascript
const paymentIntent = await stripe.paymentIntents.create({
  amount: 2000,
  currency: 'usd',
  payment_method_types: ['card'],
  transfer_data: {
    destination: 'acct_connected_xxx',
  },
  application_fee_amount: 200,
});
```

```bash
curl https://api.stripe.com/v1/payment_intents \
  -u sk_test_xxx: \
  -d amount=2000 \
  -d currency=usd \
  -d "payment_method_types[]"=card \
  -d "transfer_data[destination]"=acct_connected_xxx \
  -d application_fee_amount=200
```

**Stripe debits fees from the platform's balance.**

## Separate Charges and Transfers

Create charges on your platform and split funds between multiple connected accounts. Decouples charge and transfer operations.

```python
# Step 1: Create charge on platform
payment_intent = stripe.PaymentIntent.create(
    amount=10000,
    currency="usd",
    payment_method_types=["card"],
)

# Step 2: Transfer to multiple connected accounts
stripe.Transfer.create(
    amount=7000,
    currency="usd",
    destination="acct_seller_xxx",
    source_transaction="ch_xxx",  # Link to original charge
)

stripe.Transfer.create(
    amount=2000,
    currency="usd",
    destination="acct_driver_xxx",
    source_transaction="ch_xxx",
)
```

```bash
# Create transfer
curl https://api.stripe.com/v1/transfers \
  -u sk_test_xxx: \
  -d amount=7000 \
  -d currency=usd \
  -d destination=acct_seller_xxx \
  -d source_transaction=ch_xxx
```

**Use cases:**
- One charge → multiple recipients (marketplace with seller + delivery)
- Many charges → one recipient (aggregated payouts)
- Charge timing before knowing destination
- Payment splitting across multiple parties

## Refund Handling

| Charge Type | Refund Source |
|-------------|-------------|
| Direct | Debits connected account |
| Destination | Debits platform account (can reverse transfer) |
| Separate | Debits platform account (can reverse transfers) |

## Platform Fee (application_fee_amount)

```json
{
  "amount": 2000,
  "currency": "usd",
  "application_fee_amount": 200,
  "transfer_data": {
    "destination": "acct_connected_xxx"
  }
}
```

- Specified in the smallest currency unit (e.g., cents for USD)
- Cannot exceed the charge amount
- Capped at the total transaction amount minus Stripe fees
- Appears in the platform's Application Fees dashboard
