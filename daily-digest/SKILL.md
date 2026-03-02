---
name: daily-digest
description: >
  Digest matinal et relances automatiques pour agents immobiliers belges.
  Compile le statut de tous les dossiers, leads, et rendez-vous chaque matin à 9h.
  Use when agent says "quoi de neuf", "résumé", "qu'est-ce qui est en cours", 
  "digest", "morning briefing", or triggered automatically via cron at 9:00 AM.
  Do NOT use for specific dossier queries — use dossier-tracker instead.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [digest, relance, suivi, cron, operations]
---

# Daily Digest and Follow-ups

## Morning digest (9:00 AM daily)

Compile and send via WhatsApp/Signal:

1. ACTIVE DOSSIERS: count + each with completion % and what's missing
2. PENDING LEADS: count + leads awaiting response with days since last contact
3. TODAY'S AGENDA: all calendar events (PEB visits, property viewings, notary meetings)
4. FOLLOW-UPS SENT: automatic follow-ups executed today
5. ALERTS: expiring certificates, overdue documents, deadlines approaching

## Automatic follow-up execution

Before compiling the digest, check all active dossiers and execute pending follow-ups. See dossier-tracker for follow-up timing rules.

## Inbox monitoring (continuous)

Scan inbox for:
- Commune responses (keywords: "renseignements urbanistiques", "RU", commune names)
- Certifier confirmations
- Documents from sellers
- Lead responses

When an expected document arrives:
1. Match to active dossier in memory
2. Update document status
3. Notify agent: "[Document] received for [Address]. Dossier at [X]%."

## Examples

Example 1: Morning digest
"Good morning [Name] — March 2nd summary. 4 active dossiers. Rue de Flandre: 85% (missing: soil). Avenue Louise: 100%, listed 14 days. 2 pending leads. 3 appointments today. I followed up with Ixelles commune (J+35 on RU)."
