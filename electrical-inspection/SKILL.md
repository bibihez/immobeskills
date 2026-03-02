---
name: electrical-inspection
description: >
  Réservation de contrôle de conformité électrique pour vente immobilière en Belgique.
  Gère Vinçotte (API B2B), BTV, SGS, Certinergie par email. Use when agent says 
  "contrôle électrique", "keuring elektriciteit", "inspection électrique", or when 
  dossier-tracker detects missing electrical certificate. Covers all 3 regions.
  Do NOT use for PEB certificates — use peb-booking instead.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [electrique, keuring, vincotte, inspection, conformite]
---

# Contrôle Électrique — Inspection Booking

## Legal context

Mandatory in all 3 regions for any sale. If conforming: valid 25 years. If non-conforming: buyer has 18 months to bring to standard. Seller must provide report at compromis signing.

## Inspection bodies

| Organization | Coverage | Method | Contact |
|-------------|----------|--------|---------|
| Vinçotte | All Belgium | API (Vinçotte Connect) | brussels@vincotte.be |
| BTV | All Belgium | Email/web | Via website |
| SGS | All Belgium | Email | sgs.brussels.sgsssb@sgs.com |
| Certinergie | All Belgium | Online booking | certinergie.be |
| OCB | Wallonia | Email | dep.ele@ocb.be |

## Step 1: Try Vinçotte Connect API (priority)

```bash
curl -X POST "$VINCOTTE_API_URL/orders" \
  -H "Authorization: Bearer $VINCOTTE_API_KEY" \
  -d '{
    "type": "electrical_inspection",
    "address": "<full address>",
    "contact_name": "<seller name>",
    "contact_phone": "<phone>",
    "preferred_dates": ["2026-03-10", "2026-03-12"],
    "property_type": "house|apartment",
    "requestor": "<agency name>"
  }'
```

## Step 2: Email fallback

If Vinçotte API unavailable, send booking email:

```
Subject: Demande contrôle électrique — [Address]

Bonjour,

Contrôle de conformité électrique pour :
- Adresse : [full address]
- Type : [house/apartment]
- Disponibilités : [dates]
- Contact sur place : [seller name] — [phone]

Merci de confirmer date et tarif.

Cordialement,
[Agent] — [Agency]
```

## Step 3: Handle confirmation

1. Scan inbox for response
2. Extract date + time + price
3. Present to agent: "Electrical inspection [date] at [time] by Vinçotte, [price]EUR. OK?"
4. If confirmed: calendar event + notify seller

## Typical costs

| Property type | Price range |
|--------------|-------------|
| Apartment | 100-150 EUR |
| House | 150-250 EUR |
| Large property | 200-350 EUR |

## Troubleshooting

Error: Vinçotte API returns 401
Cause: API key expired or invalid
Solution: Fall back to email method, alert agent to renew API credentials
