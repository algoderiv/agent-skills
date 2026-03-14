# Stripe Connect Overview

Connect enables platforms and marketplaces to manage payments and move money between multiple parties.

## Core Concepts

- **Platform**: Your application that uses Connect
- **Connected Account**: A person or business accepting payments or receiving payouts through your platform
- **Charge**: A payment created on behalf of or for a connected account

## Account Types (Legacy v1)

> Stripe recommends using controller properties instead of account types for new integrations.

| Factor | Standard | Express | Custom |
|--------|----------|---------|--------|
| Integration effort | Lowest | Low | Significantly higher |
| Liability | Connected account | Platform | Platform |
| Dashboard access | Full Stripe Dashboard | Express Dashboard (limited) | None (build your own) |
| Charge types | Direct only | All types | All types |
| Onboarding | Stripe handles | Stripe handles | Platform or Stripe |
| Compliance | Automatic | Automatic | Manual |
| Additional cost | No | Yes | Yes |

### Standard
Best for platforms with experienced online businesses. Connected account has direct relationship with Stripe, full Dashboard access, and processes charges independently.

### Express
Best for marketplaces (rideshare, home rental). Stripe handles onboarding; platform controls charge types and payout settings. Express Dashboard for basic account management.

### Custom
Best for fully white-labeled experiences. Platform handles all interactions, support, and compliance. Almost invisible to account holder.

**Important:** After creation, you cannot change an account's type or country.

## Onboarding Approaches

### Stripe-Hosted
Send users to Stripe's onboarding flow. Quick launch, co-branded, 46+ countries, 14 languages.

### Embedded
Embed themeable onboarding UI in your app. Users stay on-site.

### API-Based
Build custom flows via Stripe APIs. Requires significant engineering resources and ongoing compliance maintenance.

## Charge Types

### Direct Charges
- Payment made directly to connected account
- Connected account appears on customer's statement
- Best for SaaS platforms
- Connected account needs `card_payments` capability

```bash
# Create direct charge on connected account
curl https://api.stripe.com/v1/payment_intents \
  -u sk_test_xxx: \
  -H "Stripe-Account: acct_connected" \
  -d amount=2000 \
  -d currency=usd \
  -d "payment_method_types[]"=card \
  -d application_fee_amount=200
```

### Destination Charges
- Charge on platform, immediate transfer to connected account
- Platform appears on customer's statement
- Best for marketplaces

```bash
# Create destination charge
curl https://api.stripe.com/v1/payment_intents \
  -u sk_test_xxx: \
  -d amount=2000 \
  -d currency=usd \
  -d "payment_method_types[]"=card \
  -d "transfer_data[destination]"=acct_connected \
  -d application_fee_amount=200
```

### Separate Charges and Transfers
- Charge on platform, transfer to one or many connected accounts
- Decouples charge timing from transfer
- Best for complex payment splitting

```bash
# Step 1: Create charge on platform
curl https://api.stripe.com/v1/payment_intents \
  -u sk_test_xxx: \
  -d amount=2000 \
  -d currency=usd \
  -d "payment_method_types[]"=card

# Step 2: Transfer to connected account(s)
curl https://api.stripe.com/v1/transfers \
  -u sk_test_xxx: \
  -d amount=1500 \
  -d currency=usd \
  -d destination=acct_connected \
  -d source_transaction=ch_xxx
```

## Fee Management

### Platform Liability Model
Stripe collects fees from your platform account. You control processing fees via `application_fee_amount`.

### Connected Account Liability Model
Stripe collects fees directly from connected accounts. Optional application fees at charge time.

## Payouts

### Default Behavior
Charges accumulate in connected account balance and are paid out on a daily rolling basis.

### Payout Options
- **Scheduled payouts** - Configure frequency (daily, weekly, monthly)
- **Manual payouts** - Trigger manually via API
- **Instant payouts** - Settle funds instantly (typically within 30 minutes)
- **Fund retention** - Keep funds in platform balance

### Payout Webhook Events
| Event | Description |
|-------|-------------|
| `payout.created` | Payout initiated |
| `payout.updated` | Payout status changed |
| `payout.paid` | Payout succeeded |
| `payout.failed` | Payout failed |

Failed payouts automatically disable the associated external account.

## Negative Balance Responsibility

### Platform Liability
Platform monitors risk, establishes fraud remediation flows. Recommended for advanced marketplaces with dedicated resources.

### Stripe Liability
Stripe monitors risk signals, implements interventions, recovers negative balances. Requires embedded components for onboarding, account management, and notification banners.
