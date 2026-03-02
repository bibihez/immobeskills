---
name: dossier-tracker
description: >
  Suivi des documents obligatoires pour la vente immobilière en Belgique.
  Gère la checklist par région (Bruxelles/Flandre/Wallonie), calcule le pourcentage 
  de complétion, envoie des relances automatiques, et bloque la mise en vente si 
  documents manquants. Use when agent asks "où en est le dossier", "statut", 
  "documents manquants", "dossier complet ?", or when any document status changes.
  Do NOT use for document generation — use the specific document skills instead.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [dossier, tracking, documents, compliance, checklist]
---

# Dossier Tracker — Suivi Documents de Vente

## Document statuses

NOT_STARTED > REQUESTED > IN_PROGRESS > RECEIVED > VALIDATED > EXPIRED

## Checklists by region

Consult `references/checklists.md` for the complete document matrix per region including mandatory/conditional rules.

## Completion calculation

```
required_docs = filter by region + conditions (copropriété, année construction, citerne)
received_docs = documents with status RECEIVED or VALIDATED
percentage = (received_docs / required_docs) * 100
```

## Automatic follow-up logic

### Communes (RU)
- J+30: Polite email follow-up
- J+45: Second follow-up + notify agent
- J+60: Alert agent with "call required"

### Certifiers (PEB, electrical)
- J+3: Follow-up if no appointment confirmation
- J+7: Second email + notify agent

### Sellers (missing documents)
- J+7: Polite reminder via email/WhatsApp
- J+14: Second reminder + notify agent
- J+21: Alert agent for personal intervention

### Leads (no response)
- J+2: Follow-up "still interested?"
- J+5: Last email with added value
- J+10: Mark as "cold", keep in CRM

## Communication templates

Status request from agent:
"Dossier [address] — [X]%. [List each doc with status icon]. Action required: [next steps]."

Document received:
"[Doc name] for [address] received! [Summary]. Dossier now at [X]%."

Dossier complete:
"Dossier COMPLETE for [address]! All mandatory documents collected. Ready to list."

## Troubleshooting

Error: Document marked received but file missing
Cause: Status updated without file upload
Solution: Ask agent to upload the actual document

Error: Completion stuck at 100% but status not advancing
Cause: Conditional documents not evaluated
Solution: Check building year (asbestos), copropriété status, oil tank presence
