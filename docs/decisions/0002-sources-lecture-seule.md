# ADR 0002 — Toutes les sources en lecture seule, derrière des connecteurs

**Date :** 2026-09-27 · **Statut :** ✅ Acceptée (principe validé par Amadou pour tous ses outils)

## Contexte

Ecoleparent rassemble des données qui ont déjà un propriétaire : Smart School (familles, élèves, paiements), Planning (emplois du temps), Planning Évaluations (contrôles publiés). Amadou a validé une **porte d'accès unique** vers Smart School, partagée par toutes ses applications, et le principe « lecture seule, derrière un connecteur » appliqué dans Ecolepay et Planning Évaluations.

## Décision

- Chaque source est lue **uniquement** par son connecteur (`SchoolDataSource`, `TimetableSource`, `PublishedAssessmentsSource`, `HomeworkSource`).
- **Aucune écriture** dans une source (R1) ; double protection sur MySQL (utilisateur `SELECT` si possible, `SET SESSION TRANSACTION READ ONLY` toujours).
- Les connecteurs Smart School et Planning **reprennent le code existant** (Ecolepay, Planning Évaluations) sans l'adapter, en attendant un paquet commun.
- Ecoleparent **ne demande rien** à Planning Évaluations au-delà de son contrat P1 (flux iCal publié par classe).

## Conséquences

- Démarrer Ecoleparent **ne relance pas** Planning Évaluations (décision notée dans le backlog de celui-ci, 27/09/2026).
- Si une source change, seul son connecteur change.
- Ecoleparent ne peut rien corriger dans une source : une erreur (fratrie, classe) se corrige dans Smart School ou Planning.
