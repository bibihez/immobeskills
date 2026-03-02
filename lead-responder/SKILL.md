---
name: lead-responder
description: >
  Gestion des leads entrants pour agences immobilières belges. Parse les emails 
  Immoweb/Zimmo/Logic-Immo, qualifie les leads, et rédige des réponses automatiques.
  Use when agent says "traite les leads", "réponds au lead", "nouvelle demande", 
  or when an Immoweb/Zimmo inquiry email arrives. Detects language (FR/NL/EN) 
  automatically. Do NOT use for existing client follow-up — use visit-scheduler instead.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [leads, immoweb, zimmo, email, prospection]
---

# Lead Responder — Gestion Leads Entrants

## Sources

| Source | Format | Parsing |
|--------|--------|---------|
| Immoweb | Standardized email | Extract name, email, phone, property ref |
| Zimmo | Standardized email | Extract name, email, phone |
| Logic-Immo | Standardized email | Extract name, email, phone |
| Agency website | Webhook (JSON) | Structured data |
| WhatsApp direct | Free text | NLP extraction |

## Step 1: Parse lead

Extract from email:
- source, lead_name, lead_email, lead_phone, property_ref, property_address, message, received_at

## Step 2: Quick qualification

Check memory:
- Has this lead contacted the agency before? (history)
- Is the property still available?
- Are there already visits scheduled?

## Step 3: Draft response

CRITICAL: Never send without agent validation for the first leads on a property. After 5+ validated responses without modification, propose auto-send mode.

Detect language automatically (FR/NL/EN) and respond accordingly. Do not include price by email unless agent has authorized it.

Standard response includes: availability confirmation, 3 proposed visit slots, agent contact info.

Recurring lead response: reference previous interactions, suggest new matching properties if relevant.

## Step 4: Notify agent

"New [source] lead: [Name] — [Phone] — interested in [Address]. Draft response ready with 3 visit slots. Validate or modify?"

## Step 5: Follow-up

- J+2 no response: automatic follow-up
- J+5: last email with added value
- J+10: mark as cold, keep in pipeline

## Troubleshooting

Error: Cannot detect property from lead email
Cause: Non-standard email format or missing reference
Solution: Ask agent to identify the property manually
