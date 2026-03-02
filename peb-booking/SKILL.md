---
name: peb-booking
description: >
  Réservation de certificat PEB/EPC pour vente immobilière en Belgique. 
  Contacte les certificateurs agréés par email, gère les réponses, et planifie 
  le rendez-vous dans le calendrier. Use when agent says "réserve un PEB", 
  "certificat énergie", "EPC", "energieprestatiecertificaat", or when dossier-tracker 
  detects a missing PEB. Covers all 3 regions.
  Do NOT use for electrical inspections — use electrical-inspection instead.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [peb, epc, certificat, energie, booking]
---

# PEB/EPC Booking — Certificat Energétique

## Step 1: Check for preferred certifiers

Search memory for `agency.preferred_peb_certifiers`. If found, contact favorite first. If not, search certifier database by postal code.

## Step 2: Certifier registries by region

| Region | Registry | URL |
|--------|----------|-----|
| Brussels | Bruxelles Environnement | peb.brussels |
| Flanders | VEKA / Energiesparen.be | energiesparen.be |
| Wallonia | SPW Énergie | energie.wallonie.be |

## Step 3: Send booking emails

Send to 2-3 certifiers in parallel (or favorite only if set):

```
Subject: Demande PEB — [Address], [Postal code] [Municipality]

Bonjour [Certifier name],

Nous souhaitons faire réaliser un certificat PEB pour :
- Adresse : [full address]
- Type : [house/apartment]
- Surface : ~[area]m2
- Disponibilité souhaitée : [proposed dates]

Merci de confirmer disponibilité et tarif.

Cordialement,
[Agent] — [Agency]
[Phone]
```

First responder with availability wins.

## Step 4: Handle responses

- Scan inbox for certifier replies
- Extract: proposed date, price, confirmation
- Present to agent: "[Name] available [date] at [time] for [price]EUR. Confirm?"
- If OK: block calendar slot, confirm to certifier, notify seller

## Step 5: Post-PEB

When certificate received:
- Extract score/label (A to G)
- Store in dossier via dossier-tracker
- Check: PEB is mandatory for publishing ads
- If score F or G: flag to agent (impact on sale price)

## Typical costs

| Property type | Price range |
|--------------|-------------|
| Apartment | 150-250 EUR |
| House | 200-400 EUR |
| Large property | 400-600 EUR |

## Troubleshooting

Error: No certifier responds within 5 days
Solution: Expand search radius, try different certifiers, alert agent

Error: PEB score seems abnormally high/low
Solution: Flag to agent for verification, do not auto-publish

## Examples

Example 1: Standard booking
User says: "Réserve un PEB pour Avenue Louise 142, 1050 Ixelles"
Actions: Check favorites > send emails to 3 certifiers > wait for response
Result: "PEB available Thursday 14/03 at 10h with [Name] for 220EUR. Confirm?"
