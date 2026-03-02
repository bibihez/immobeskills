---
name: notary-liaison
description: >
  Coordination avec le notaire pour compromis et acte authentique en Belgique.
  Assemble le package notaire, gère les communications, suit les conditions suspensives.
  Use when agent says "envoie au notaire", "prépare le compromis", "package notaire",
  "acte authentique", or when offer is accepted and dossier needs to go to notary.
  Do NOT use before offer acceptance — use offer-manager first.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [notaire, compromis, acte, fednot, transaction]
---

# Notary Liaison — Coordination Notariale

## Context

In Belgium, the notary (usually buyer's choice) drafts the compromis de vente and acte authentique. The agent must provide a complete dossier. Notaries use IBOT (Fednot) internally for their own research.

## Step 1: Verify dossier completeness

CRITICAL: Check via dossier-tracker that ALL mandatory documents are present. If incomplete, alert agent with missing items before contacting notary.

## Step 2: Assemble notary package

All sale dossier documents plus:
- Signed accepted offer
- Buyer ID
- Seller ID
- Buyer financing info (if applicable)
- Marital status of both parties
- Power of attorney if representative

## Step 3: Email to notary

Structured email with: all documents listed with checkmarks, seller info, buyer info, agreed price, deposit amount (typically 10%), and request for compromis signing date.

Consult `references/notary-email-template.md` for the full template.

## Step 4: Post-compromis tracking

After compromis signed:
- Track suspensive condition period (typically 4 months)
- Remind about deadlines (buyer financing, condition lifting)
- Respond to notary's supplementary document requests
- Prepare for acte authentique day

## Troubleshooting

Error: Notary requests document not in dossier
Cause: Regional requirement missed or conditional document needed
Solution: Check compliance-checker for the specific requirement, obtain document, resend
