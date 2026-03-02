---
name: compliance-checker
description: >
  Vérification de conformité régionale pour transactions immobilières belges.
  Matrice complète des obligations par région (Bruxelles/Flandre/Wallonie), 
  règles conditionnelles (amiante, copropriété, citerne), droits d'enregistrement.
  Use when checking if a dossier is compliant, agent asks "qu'est-ce qu'il faut en 
  Flandre/Wallonie/Bruxelles", or before any status transition in the dossier workflow.
  Do NOT use for document generation — use specific document skills instead.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [compliance, regional, legal, belgique, amiante, copropriete]
---

# Regional Compliance — Conformité Belgique

## Quick reference

Consult `references/regional-matrix.md` for the full obligation matrix per region.

## Conditional rules

### Asbestos (Flanders only)
IF region == flanders AND construction_year less than 2001
THEN asbestattest = MANDATORY
Block listing if not received. Alert: "Pre-2001 building in Flanders — asbestos certificate mandatory."

### Oil tank
IF property.oil_tank == true
THEN oil_tank_certificate = MANDATORY
Add to checklist in dossier-tracker.

### Co-ownership
IF property.copropriete == true
THEN syndic_documents = MANDATORY
Include: charge statements, AG minutes, reserve fund, co-ownership rules.

### Flood risk (Flanders)
IF region == flanders
THEN check flood zone via Athumi/VMM
IF flood_risk == true THEN mandatory_ad_mention = true
Alert: "Property in flood zone — mandatory mention in listing."

## Registration duties

| Category | Brussels | Flanders | Wallonia |
|----------|----------|----------|----------|
| Standard rate | 12.5% | 3% (since 2025) | 12.5% |
| Single home | 200k EUR abatement | 3% | Reduced rate |
| Modest | 200k EUR abatement | 1% | Reduced rate |

Note: Flemish rates changed in 2025. Always verify current conditions.

## Regulatory updates

The system must be updated when:
- New regional obligations (e.g., asbestattest 2022)
- Rate changes (e.g., Flemish registration duties 2025)
- New digital platforms (e.g., Athumi evolutions)
- Municipal form modifications

Proactively flag changes affecting active dossiers.

## Troubleshooting

Error: Document required but not in checklist
Cause: Conditional rule not triggered (missing building year, copropriété status, etc.)
Solution: Ask agent to confirm property characteristics, update dossier, recalculate checklist
