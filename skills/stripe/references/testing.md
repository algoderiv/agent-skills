# Testing

## Sandbox Environment

Testing occurs in an isolated sandbox environment. Access sandboxes through the Dashboard account picker. Test mode doesn't affect live data or interact with banking networks.

**Critical:** Never use real card details for testing. The Stripe Services Agreement prohibits testing in live mode using real payment method details.

## Interactive Testing

Use card number `4242 4242 4242 4242` with:
- Any valid future expiry (e.g., 12/34)
- Any 3-digit CVC (4 digits for Amex)
- Any values for other fields

## Server-Side Testing

Use `PaymentMethod` IDs instead of card numbers for better PCI compliance:

```bash
curl https://api.stripe.com/v1/payment_intents \
  -u "sk_test_xxx:" \
  -d amount=500 \
  -d currency=gbp \
  -d payment_method=pm_card_visa \
  -d "payment_method_types[]"=card
```

## Test Cards by Brand

| Brand | Number | PaymentMethod |
|-------|--------|---------------|
| Visa | `4242424242424242` | `pm_card_visa` |
| Visa (debit) | `4000056655665556` | `pm_card_visa_debit` |
| Mastercard | `5555555555554444` | `pm_card_mastercard` |
| Mastercard (2-series) | `2223003122003222` | - |
| Mastercard (debit) | `5200828282828210` | `pm_card_mastercard_debit` |
| Mastercard (prepaid) | `5105105105105100` | `pm_card_mastercard_prepaid` |
| American Express | `378282246310005` | `pm_card_amex` |
| American Express | `371449635398431` | - |
| Discover | `6011111111111117` | `pm_card_discover` |
| Discover | `6011000990139424` | - |
| Discover (debit) | `6011981111111113` | - |
| Diners Club | `3056930009020004` | `pm_card_diners` |
| JCB | `3566002020360505` | `pm_card_jcb` |
| UnionPay | `6200000000000005` | `pm_card_unionpay` |
| UnionPay (debit) | `6200000000000047` | - |

## Geographic Test Cards (Selected)

### Americas
| Country | Number | PaymentMethod |
|---------|--------|---------------|
| US | `4242424242424242` | `pm_card_us` |
| Brazil | `4000000760000002` | `pm_card_br` |
| Canada | `4000001240000000` | `pm_card_ca` |
| Mexico | `4000004840008001` | `pm_card_mx` |

### Europe
| Country | Number | PaymentMethod |
|---------|--------|---------------|
| UK | `4000008260000000` | `pm_card_gb` |
| Germany | `4000002760000016` | `pm_card_de` |
| France | `4000002500000003` | `pm_card_fr` |
| Netherlands | `4000005280000002` | `pm_card_nl` |
| Spain | `4000007240000007` | `pm_card_es` |
| Italy | `4000003800000008` | `pm_card_it` |
| Sweden | `4000007520000008` | `pm_card_se` |
| Switzerland | `4000007560000009` | `pm_card_ch` |

### Asia Pacific
| Country | Number | PaymentMethod |
|---------|--------|---------------|
| Australia | `4000000360000006` | `pm_card_au` |
| Japan | `4000003920000003` | `pm_card_jp` |
| Singapore | `4000007020000003` | `pm_card_sg` |
| India | `4000003560000008` | `pm_card_in` |
| Hong Kong | `4000003440000004` | `pm_card_hk` |

> Note: European Economic Area countries require SCA / 3D Secure for online payments. The geographic test cards simulate successful payments without authentication.

## Decline & Error Test Cards

| Scenario | PaymentMethod |
|----------|---------------|
| Generic decline | `pm_card_visa_chargeDeclined` |
| Insufficient funds | `pm_card_chargeDeclinedInsufficientFunds` |
| Expired card | `pm_card_chargeDeclinedExpiredCard` |
| Incorrect CVC | `pm_card_chargeDeclinedIncorrectCvc` |
| Processing error | `pm_card_chargeDeclinedProcessingError` |
| Fraud (Radar block) | `pm_card_radarBlock` |

## Co-Branded Test Cards

| Brand/Co-brand | Number | PaymentMethod |
|----------------|--------|---------------|
| Cartes Bancaires/Visa | `4000002500001001` | `pm_card_visa_cartesBancaires` |
| Cartes Bancaires/Mastercard | `5555552500001001` | `pm_card_mastercard_cartesBancaires` |
| eftpos/Visa | `4000050360000001` | `pm_card_visa_debit_eftposAuCoBranded` |
| eftpos/Mastercard | `5555050360000080` | `pm_card_mastercard_debit_eftposAuCoBranded` |

## Legacy Tokens

| Brand | Token |
|-------|-------|
| Visa | `tok_visa` |
| Visa (debit) | `tok_visa_debit` |
| Mastercard | `tok_mastercard` |
| Amex | `tok_amex` |
| Discover | `tok_discover` |

## Testing Webhooks Locally

```bash
# Install Stripe CLI and login
stripe login

# Forward events to local server
stripe listen --forward-to localhost:4242/webhook

# Trigger specific events
stripe trigger payment_intent.succeeded
stripe trigger payment_intent.payment_failed
stripe trigger customer.subscription.created
```

## Going Live Checklist

1. Replace test API keys with live keys
2. Store live secret keys securely (environment variables, secrets vault)
3. Never commit keys to version control
4. Verify webhook endpoints use HTTPS
5. Update webhook endpoint secrets for live mode
6. Test with a real small-amount transaction

> **Important:** Don't use testing environments for load testing — you may hit rate limits. Use dedicated load testing instead.
