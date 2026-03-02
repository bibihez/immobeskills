---
name: soil-attestation
description: >
  Demande d'attestation de sol pour vente immobilière en Belgique. Gère les 3 régions :
  BDES Bruxelles Environnement (IRISbox), OVAM bodemattest Flandre (via Athumi), 
  BDES SPAQuE Wallonie. Use when agent says "attestation sol", "bodemattest", 
  "certificat sol", "pollution", or when dossier-tracker detects missing soil certificate.
  Do NOT use for Flanders if athumi-connector already returned bodemattest data.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [sol, bodemattest, pollution, bdes, ovam, environnement]
---

# Attestation Sol — Soil Certificate (3 regions)

## Brussels — BDES (Bruxelles Environnement)

| Field | Detail |
|-------|--------|
| Authority | Bruxelles Environnement (IBGE/BIM) |
| Portal | bdesol.brussels / IRISbox |
| Cost | ~50 EUR |
| Delay | 5-15 business days |
| Validity | Transaction-specific |

### Workflow

1. Check free pollution map first: https://geodata.environnement.brussels/
2. If Category 0 (clean): proceed to request
3. If polluted: ALERT agent immediately (major impact on sale)
4. Submit via IRISbox (requires itsme authentication)

CRITICAL: No direct API available. Automation limited to:
- Pre-filling request info
- Generating clear instructions for agent
- Tracking receipt by email

Message to agent:
"Attestation sol for [address] must be requested via IRISbox. Pre-filled info: Parcel [capakey], Commune [commune], Cost ~50EUR. I cannot auto-submit (itsme required). Request here: [link]. I will track receipt by email."

## Flanders — Bodemattest (OVAM)

| Field | Detail |
|-------|--------|
| Authority | OVAM |
| Cost | 55-72 EUR |
| Delay | Instant for clean parcels |
| Mandatory | Yes, for all Flemish sales |

If athumi-connector already called: bodemattest is included in the response. No separate action needed.

If separate request needed: submit via OVAM portal at ovam.be.

Check result: "niet verontreinigd" (not contaminated) = OK. If contaminated, sanering process may be required.

## Wallonia — BDES (SPAQuE / SPW Environnement)

| Field | Detail |
|-------|--------|
| Authority | SPW Environnement |
| Portal | sol.environnement.wallonie.be |
| Cost | 40-100 EUR |
| Delay | Variable |

Same limitation as Brussels: no public API. Guide agent through portal.

## Pollution alert protocol

If any parcel flagged as polluted in any region:

"ALERT SOL — Parcel [capakey] at [address] is flagged as potentially contaminated. Implications: complementary soil study may be required, seller must inform buyer, ad must mention status, sale price will be impacted. Recommendation: consult environmental expert before listing."

## Troubleshooting

Error: Pollution map data outdated
Solution: Always request the official certificate, map is for preliminary check only

Error: IRISbox submission fails
Solution: Guide agent to submit manually, provide all pre-filled data
