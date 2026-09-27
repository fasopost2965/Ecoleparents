# ADR 0007 — Source des devoirs à la maison

**Date :** 2026-09-27 · **Statut :** 🟡 Proposée — Amadou ne sait pas encore qui écrira les devoirs ni où

## Contexte

Les devoirs font partie du MVP, mais aucune application ne les contient aujourd'hui. Options détaillées dans `01-recherche.md` § 6 : Cahier Journal (déployé, pas encore utilisé par les profs), saisie par le Secrétariat, comptes professeurs.

## Décision proposée

- Interface **`HomeworkSource`** : l'espace parents ne sait pas d'où viennent les devoirs.
- **MVP : saisie par le Secrétariat** dans l'administration (écran A4, `ManualHomeworkSource`), avec duplication vers les groupes du même niveau.
- **Plus tard : `CahierJournalHomeworkSource`**, en lecture seule, quand les profs utiliseront le Cahier Journal (pilote avec un prof volontaire déjà décidé) — si ce dernier a un champ « devoirs » (🔍).
- Comptes professeurs dans Ecoleparents : **non** (backlog), pour ne pas créer un deuxième outil de saisie pour les profs.

## À trancher par Amadou avant la phase 8

1. Le Secrétariat a-t-il le temps de saisir les devoirs du collège ? Sinon, les devoirs sortent du MVP.
2. Comment les profs transmettent-ils les devoirs au Secrétariat (papier, WhatsApp, cahier) ?
3. Le Cahier Journal doit-il prévoir un champ « devoirs » dès maintenant ?

## Conséquences

- Le choix de la source ne change aucun écran parent.
