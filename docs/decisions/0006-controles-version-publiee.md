# ADR 0006 — Contrôles : version publiée de Planning Évaluations (contrat P1)

**Date :** 2026-09-27 · **Statut :** ✅ Acceptée (décision prise avec Amadou lors de la revue de Planning Évaluations)

## Contexte

Planning Évaluations publie des versions figées du calendrier des contrôles ; chaque classe a un lien à jeton et un flux iCal `/p/{jeton}.ics` qui ne montrent que la version publiée. Son backlog définit deux contrats de lecture pour l'Espace Parent :

- **P1** : lire le flux iCal de chaque classe, **sans changement** dans Planning Évaluations ;
- **P2** : un point de lecture JSON par classe, à ajouter plus tard (accord d'Amadou + ADR côté Planning Évaluations).

## Décision

- Ecoleparent démarre avec **P1**. Le jeton de chaque classe est saisi dans l'écran A5 et **stocké chiffré**.
- Lecture toutes les 15 minutes au plus ; en cas d'échec, la dernière version reste affichée avec sa date (R20).
- Passage à **P2** seulement si P1 ne suffit pas (par exemple pour le calendrier officiel des jours fériés, aujourd'hui importé en CSV).

## Conséquences

- Aucune dépendance de calendrier entre les deux projets.
- Régénérer le lien d'une classe dans Planning Évaluations oblige à mettre à jour le jeton dans A5 (à écrire dans la notice).
- Le lecteur iCal (bibliothèque ou code maison) est à valider par Amadou avant la phase 6.
