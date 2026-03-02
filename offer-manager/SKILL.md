---
name: offer-manager
description: >
  Gestion des offres d'achat et négociations pour transactions immobilières belges.
  Analyse les offres, prépare les communications vendeur/acheteur, gère les contre-offres.
  Use when agent says "offre reçue", "nouvelle offre", "contre-offre", "négociation",
  or when a buyer submits a written offer on a listed property.
  Do NOT use before a property is listed — use dossier-tracker to check status first.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [offre, negociation, acheteur, vendeur, prix]
---

# Offer Manager — Gestion des Offres

## Step 1: Receive and structure offer

Extract: property address, buyer name, offer amount, conditions (financing, technical visit, etc.), validity period, date received.

## Step 2: Analyze

Compare with: asking price (calculate discount %), other active offers on same property, estimated market price.

Present to agent: "Offer received for [Address]: [Amount]EUR (-[X]% vs asking [Y]EUR). Conditions: [list]. Validity: [X] days. [If other offers exist: Also have offer of [Amount2]EUR from [Name2].]"

## Step 3: Seller communication

Draft for agent to send to seller with offer details and professional recommendation.

## Step 4: Counter-offer handling

If seller counter-offers: prepare communication to buyer, track back-and-forth.

## Step 5: Acceptance

If offer accepted:
1. Update dossier status to OFFRE_ACCEPTEE
2. Trigger notary-liaison workflow
3. Notify all parties
4. Remove listing from portals
