---
name: athumi-connector
description: >
  Connecteur API Athumi/VIP (Vastgoedinformatieplatform) pour les transactions 
  immobilières en Flandre. Récupère en un seul appel : stedenbouwkundige inlichtingen, 
  bodemattest OVAM, watertoets, droits de préemption. Use when property is in Flanders 
  (postcodes 1500-1999, 3000-3999, 8000-9999), agent asks for "infos urbanistiques", 
  "bodemattest", "stedenbouwkundige inlichtingen", or "vastgoedinlichtingen".
  Do NOT use for Brussels or Wallonia properties.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [flandre, athumi, vip, ovam, bodemattest, urbanisme]
---

# Athumi VIP Connector — Flandre

## Why this matters

In Flanders, Athumi/VIP centralizes what Brussels requires per-commune requests for. One API call replaces 300+ individual communes. This is the key differentiator for Flemish expansion.

## Product: Vastgoedinlichtingen voor overdracht

One request returns:
- Stedenbouwkundige inlichtingen (urban planning info)
- Zonage et destination
- Permis délivrés
- Infractions urbanistiques
- Droits de préemption (voorkooprechten)
- Bodemattest OVAM (soil attestation)
- Watertoets (flood risk VMM)

## Prerequisites

Environment variables required:
- ATHUMI_CLIENT_ID
- ATHUMI_CLIENT_SECRET

Register at https://www.athumi.be/ to obtain credentials.

## Step 1: Authenticate

```bash
TOKEN=$(curl -s -X POST "https://auth.athumi.be/oauth/token" \
  -d "client_id=$ATHUMI_CLIENT_ID" \
  -d "client_secret=$ATHUMI_CLIENT_SECRET" \
  -d "grant_type=client_credentials" \
  | jq -r '.access_token')
```

## Step 2: Request vastgoedinlichtingen

```bash
curl -s "https://api.athumi.be/vip/vastgoedinlichtingen" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "capakey": "<capakey>",
    "type": "overdracht",
    "requestor": {
      "name": "<agency name>",
      "role": "vastgoedmakelaar",
      "ipi_number": "<IPI number>"
    }
  }'
```

Expected output: JSON with stedenbouwkundig, bodemattest, and watertoets data.

## Step 3: Post-processing

After receiving data:
1. Store all info in the property dossier via dossier-tracker
2. Mark "urbanisme", "sol", and "inondation" documents as received
3. If overtredingen (infractions) found: ALERT the agent immediately
4. If voorkooprecht (preemption) active: ALERT the agent
5. If overstromingsgevoelig (flood risk): flag for mandatory ad mention

## Troubleshooting

Error: capakey_not_found
Cause: Wrong cadastral reference
Solution: Redo cadastral lookup with correct address

Error: unauthorized
Cause: Token expired
Solution: Re-authenticate

Error: parcel_not_flemish
Cause: Postal code outside Flanders
Solution: Redirect to ru-request (Brussels/Wallonia)

## Examples

Example 1: Standard Flemish property
User says: "Nouveau bien Veldstraat 12, 9000 Gent"
Actions:
1. Detect Gent (9000) = Flanders
2. Authenticate with Athumi
3. Request vastgoedinlichtingen with capakey
4. Parse results
Result: "Athumi data received: residential zone, no infractions, clean soil (OVAM-2026-XXXXX), no flood risk, no preemption. Dossier updated."
