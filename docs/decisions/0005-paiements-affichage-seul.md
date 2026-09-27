# ADR 0005 — Paiements : affichage seul, même calcul que le portail

**Date :** 2026-09-27 · **Statut :** 🟡 Proposée (le calcul est acquis ; le vocabulaire et le niveau de détail sont à valider)

## Contexte

- Le portail des rapports et Ecolepay calculent déjà la situation mois par mois (scolarité, transport), à partir de Smart School ; un mois enregistré comme payé est réglé, remise comprise (ADR 0003 d'Ecolepay).
- Les rappels de paiement sont le rôle d'Ecolepay (numéro de la caissière, ton très respectueux).
- Afficher la situation aux familles est délicat culturellement : il faut informer sans jamais réclamer.

## Décision proposée

- **Même calcul**, repris du code d'Ecolepay, vérifié par un test de comparaison avec le portail (R13).
- **Affichage seul** : « Réglé » / « À régler » par mois, tarif du mois, total restant. Pas de relance, pas de couleur d'alerte, pas de compte à rebours (R14).
- Pas de paiement en ligne ni de reçu (R15).
- Jamais dans le cache du téléphone (R21).

## Points à valider par Amadou

- Afficher les **montants** (tarif et total) ou seulement les états ? Proposition : oui, les montants, comme au guichet.
- Afficher les **remises** comme telles ? Proposition : non, un mois réglé est réglé.
- Texte fixe sous le tableau, dans les trois langues.

## Conséquences

- Toute évolution du calcul se fait d'abord dans le connecteur commun, puis se reporte ici avec le même test.
