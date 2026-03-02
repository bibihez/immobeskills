---
name: annonce-generator
description: >
  Rédaction d'annonces immobilières conformes à la législation belge par région.
  Vérifie les mentions légales obligatoires (PEB, prix, surface, bodemattest, asbestattest).
  Use when agent says "rédige l'annonce", "crée le listing", "publie l'annonce", 
  "schrijf een advertentie", or when dossier is complete and ready to list.
  Do NOT use if mandatory documents are missing — check dossier-tracker first.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [annonce, listing, immoweb, compliance, marketing]
---

# Annonce Generator — Rédaction Annonces Conformes

## Pre-flight check

CRITICAL: Before generating, verify via dossier-tracker that all mandatory documents are received. An ad without PEB/EPC is illegal.

## Step 1: Collect property data

From dossier: address, type, surface, bedrooms, bathrooms, floor, elevator, garden, terrace, garage, PEB score/label/number, RC, price, charges, construction year.

## Step 2: Generate ad

Structure: catchy title (max 80 chars) + engaging description (150-250 words) + characteristics + legal mentions.

Adapt language to property region: FR for Brussels/Wallonia, NL for Flanders. Bilingual if agent requests.

## Step 3: Compliance check

Consult `references/legal-requirements.md` for mandatory mentions per region.

Before publishing, verify:
- PEB/EPC mentioned with score + label + certificate number
- Price displayed
- Surface indicated
- RC mentioned
- [Flanders] Bodemattest referenced
- [Flanders] Asbestattest mentioned if building pre-2001
- [Flanders] Watertoets if flood risk zone
- No discriminatory language
- No "starting from" pricing (forbidden)

## Step 4: Multi-platform formatting

Adapt format for: Immoweb (structured fields), Zimmo, Logic-Immo, Agency website (free format, more storytelling).

CRITICAL: Always submit to agent for validation before publication.

## Troubleshooting

Error: PEB number missing
Solution: Block publication, alert agent that PEB is mandatory for advertising
