# 00 — Vision

## Le problème

À La Victoire, l'information utile aux familles existe déjà, mais **dispersée et hors de leur portée** :

| Information | Où elle est aujourd'hui | Comment la famille l'obtient |
|---|---|---|
| Emploi du temps de la classe | Application **Planning** (en production) | Gabarit imprimé ; le lien parents n'a jamais été partagé |
| Calendrier des contrôles | **Planning Évaluations** (en production depuis le 27/09/2026) | Lien et PDF prévus pour les professeurs et les classes, rien pour les parents |
| Devoirs à la maison | Cahier de l'élève | Par l'enfant, quand il y pense |
| État des paiements | **Smart School** | En venant à l'accueil, ou quand la caissière appelle depuis le fixe de l'école |
| Annonces (sorties, fêtes, réunions) | WhatsApp de la directrice, papiers dans le cartable | Selon les cas |

Conséquences :

- les familles ne savent pas **quand** leur enfant a un contrôle, ni **quoi** réviser ;
- les questions arrivent sur le **WhatsApp personnel de la directrice** ;
- un parent qui veut savoir où il en est de ses paiements doit **se déplacer ou attendre un appel** ;
- les parents sont **majoritairement arabophones** ; certains lisent le français, quelques-uns l'anglais — les papiers en une seule langue ne touchent pas tout le monde.

L'école a une licence de l'application mobile Smart School, **non déployée** : elle n'a jamais été ouverte aux familles (voir `01-recherche.md` § 5 et ADR 0001).

## La promesse

> **Tout ce qui concerne mon enfant, au même endroit, sur mon téléphone, dans ma langue.**

## Pour qui

| Utilisateur | Ce qu'il fait |
|---|---|
| **Parent** (parent de garde, un compte par famille) | Consulte, pour chacun de ses enfants : emploi du temps, contrôles, devoirs, paiements, annonces. Choisit sa langue. **Ne saisit rien** dans le MVP |
| **Secrétariat** | Vérifie les familles, génère et imprime les fiches d'accès, les remet, répond aux codes perdus ; rédige les annonces (et, selon l'ADR 0007, les devoirs) |
| **Direction** (Amadou, directrice) | Publie les annonces, suit l'adoption du pilote, règle les paramètres |
| **Élève** | Pas d'accès propre dans le MVP |

## Périmètre du MVP

### Dedans

1. **Comptes des familles** : un compte par parent de garde, enfants rattachés depuis Smart School, **identifiant + code remis par l'école** sur une fiche imprimée (ADR 0003, 0004).
2. **Accueil** par enfant : aujourd'hui et demain, prochains contrôles, devoirs à venir, dernière annonce, résumé des paiements.
3. **Emploi du temps** de la classe, lu dans Planning.
4. **Contrôles** : version publiée de Planning Évaluations (ADR 0006).
5. **Devoirs à la maison** : source à décider ; saisie par le Secrétariat proposée pour le MVP (ADR 0007 🟡).
6. **Paiements** : scolarité et transport mois par mois, même calcul que le portail des rapports et Ecolepay, **affichage seul** (ADR 0005).
7. **Annonces** de l'école, pour tout le collège ou une classe, en arabe (obligatoire), français et anglais.
8. **Trois langues** : arabe par défaut (RTL complet), français, anglais (ADR 0008).
9. **PWA** : installable sur l'écran d'accueil, consultable hors ligne avec la date de mise à jour (sauf les paiements, R21).
10. **Administration** (Filament) : familles et codes, annonces, correspondance des classes, tableau de bord du pilote, paramètres, utilisateurs et rôles.

### Dehors (voir `backlog.md`)

- Notifications (push, WhatsApp, e-mail)
- Paiement en ligne, reçus
- Messagerie parent ↔ école
- Absences, notes, bulletins
- Documents (certificats, autorisations de sortie) — relève d'Ecoledoc
- Compte élève, comptes professeurs
- Application native (Android, iPhone)
- Primaire et préscolaire

## Critères de réussite du pilote (collège)

Chiffres **proposés**, à valider par Amadou (🔍) :

- **Au moins 70 %** des familles du collège ont activé leur compte (première connexion) dans les **3 semaines** qui suivent la remise des fiches.
- **Au moins la moitié** des familles se connectent au moins une fois par semaine pendant le mois suivant.
- **Zéro incident de cloisonnement** : aucune famille n'a vu les données d'un autre enfant.
- Les paiements affichés sont **identiques** au portail des rapports pour 100 % des élèves du collège (test de comparaison, `07-tests.md`).
- Moins de questions de routine (emploi du temps, dates de contrôle) sur le WhatsApp de la directrice — **mesure qualitative**, à son avis.

**Date de lancement proposée** : début du 2ᵉ semestre (**semaine 18, lundi 1er février 2027**), quand le calendrier des contrôles du 2ᵉ semestre vient d'être publié dans Planning Évaluations (🔍 à valider).
