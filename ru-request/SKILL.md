---
name: ru-request
description: >
  Demande de Renseignements Urbanistiques pour la vente immobilière en Belgique.
  Gère le fallback PDF+email pour les 4 communes bruxelloises hors IRISbox 
  (Evere, Forest, Koekelberg, Watermael-Boitsfort) et toutes les communes wallonnes.
  Use when agent says "lance le RU", "demande urbanistique", "renseignements urbanistiques"
  for a property NOT on IRISbox. Routes to irisbox-ru for the 15 other Brussels communes.
  Do NOT use for Flanders — use athumi-connector instead.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [urbanisme, ru, bruxelles, wallonie, commune]
---

# Renseignements Urbanistiques — Fallback et Wallonie

## Routing

| Région | Méthode | Skill |
|--------|---------|-------|
| Bruxelles (15 communes IRISbox) | Navigation IRISbox + itsme | irisbox-ru |
| Bruxelles (4 hors IRISbox) | PDF + email | CE SKILL |
| Flandre | API Athumi/VIP | athumi-connector |
| Wallonie (~250 communes) | PDF + email | CE SKILL |

## Bruxelles — Communes sur IRISbox (15/19)

Déléguer au skill `irisbox-ru` : Anderlecht, Auderghem, Berchem-Sainte-Agathe, Bruxelles-Ville, Etterbeek, Ganshoren, Ixelles, Jette, Molenbeek, Saint-Gilles, Saint-Josse, Schaerbeek, Uccle, Woluwe-Saint-Lambert, Woluwe-Saint-Pierre.

## Bruxelles — Fallback (4 communes hors IRISbox)

Communes concernées : Evere, Forest, Koekelberg, Watermael-Boitsfort.

### Step 1: Routing commune

Consulter `references/communes-hors-irisbox.md` pour l'email, IBAN, et montant de chaque commune.

### Step 2: Génération du formulaire PDF

```bash
python scripts/generate-ru-form.py \
  --address "<adresse>" \
  --capakey "<capakey>" \
  --owner "<nom proprietaire>" \
  --agent-bce "<BCE>" \
  --commune "<code_postal>"
```

Output attendu : PDF pré-rempli + QR code SEPA pour paiement.

### Step 3: Descriptif sommaire (si vente)

Générer un descriptif incluant :
- Description littérale du bien
- Caractéristiques façades et toitures
- Destination des constructions
- Nombre d'unités de logement et places de parking

CRITICAL: Le descriptif doit être validé par l'agent avant soumission. Il engage la responsabilité légale.

### Step 4: Soumission par email

Après validation agent + preuve de paiement, envoyer à l'email de la commune avec en pièces jointes : formulaire PDF, preuve paiement, mandat, descriptif.

### Step 5: Suivi

- Rappel automatique à J+30 si pas de réponse
- Scanner inbox pour emails de la commune
- Mettre à jour le statut dans dossier-tracker
- Notifier l'agent quand le RU est reçu

## Wallonie (~250 communes)

Même logique que le fallback Bruxelles. Consulter `references/communes-wallonie.md` pour les données par commune. Priorité : Namur, Liège, Charleroi, Mons, Wavre, Waterloo, Ottignies-LLN.

## Troubleshooting

Error: Commune non trouvée dans la base
Cause: Code postal non mappé ou commune wallonne pas encore ajoutée
Solution: Alerter l'agent, fournir les infos connues, demander de compléter manuellement

Error: Email de la commune bounce
Cause: Adresse email obsolète
Solution: Réessayer dans 1h. Si échec, alerter l'agent avec le numéro de téléphone de la commune.

## Examples

Example 1: Bien à Forest (hors IRISbox)
User says: "Lance le RU pour Chaussée de Bruxelles 15, 1190 Forest"
Actions:
1. Détecter Forest (1190) = hors IRISbox
2. Charger données commune depuis references/
3. Générer PDF + QR paiement
4. Attendre validation agent
5. Soumettre par email
Result: "RU soumis à Forest. Délai estimé : 4-8 semaines. Relance auto à J+30."
