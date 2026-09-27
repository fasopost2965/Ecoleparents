# Continuité — état du projet

> **À donner en premier à toute nouvelle IA ou nouvelle session.** À régénérer à chaque fin de phase.

**Dernière mise à jour :** 2026-09-27 — documentation initiale rédigée ; **projet en pause** (décision d'Amadou, 27/09/2026) : rien ne démarre avant qu'il le décide.

## Où on en est

| Étape | Statut |
|---|---|
| Cadrage | ✅ Choix d'Amadou du 27/09/2026 : contenu du MVP, connexion par code remis, pilote au collège |
| Recherche | 🟡 Rédigée ; requêtes Smart School à lancer (familles, classes, fratries) ; CNDP à vérifier |
| Spécifications | 🟡 Rédigées (8 écrans parents, 7 écrans d'administration), à valider |
| Architecture | 🟡 Rédigée, à relire par une deuxième IA |
| Règles métier | 🟡 R1 à R26, à valider |
| Plan du MVP | 🟡 Phases 0 à 10, calendrier proposé jusqu'au 1er février 2027 |
| Design | 🟡 Système repris ; maquettes Stitch à faire (arabe d'abord) |
| Nom du projet, dépôt, sous-domaine | ✅ **Nom définitif : Ecoleparents** (validé le 27/09/2026, même famille qu'Ecolepay et Ecoledoc). Sur le téléphone des parents, l'application s'appelle **« La Victoire »** (nom et logo de l'école). Dépôt renommé `fasopost2965/Ecoleparents` le 27/09/2026. Sous-domaine à choisir (`{{SOUS_DOMAINE}}` dans la doc) |
| Développement | Pas commencé |

## Décisions prises

1. **Contenu du MVP** : emploi du temps, calendrier des contrôles, devoirs à la maison, état des paiements, annonces de l'école (Amadou, 27/09/2026)
2. **Connexion** : identifiant + code remis par l'école sur une fiche imprimée (ADR 0003)
3. **Pilote** : le collège uniquement (ADR 0010)
4. **PWA** d'abord, Android prioritaire pour un éventuel natif, Apple écarté pour l'instant (décision antérieure d'Amadou, ADR 0001)
5. **Sources en lecture seule** derrière des connecteurs ; connecteur Smart School unique repris d'Ecolepay (ADR 0002)
6. **Contrôles** : version publiée de Planning Évaluations via son flux iCal par classe (contrat P1), sans changement dans Planning Évaluations (ADR 0006)
7. **Trois langues**, arabe par défaut (ADR 0008)
8. **Nom** : produit **Ecoleparents** ; nom affiché aux familles (icône, titre de l'application) : **« La Victoire »** (27/09/2026)

## Décisions proposées, à valider

| ADR | Sujet | Question à Amadou |
|---|---|---|
| 0001 | Front parents en Blade + Livewire | D'accord pour ne pas partir sur React ? |
| 0004 | Un compte par famille (parent de garde) | Montrer les frères et sœurs du primaire dès le pilote ? |
| 0005 | Paiements : affichage seul | Afficher les montants ou seulement « Réglé / À régler » ? |
| 0007 | Devoirs saisis par le Secrétariat pour le MVP | Le Secrétariat en a-t-il le temps ? Comment les profs transmettent-ils ? |
| 0009 | Même compte Hostinger | Nom du sous-domaine |

## Points ouverts

- **Qui écrit les devoirs, et où** : Amadou ne sait pas encore (27/09/2026). Proposition dans l'ADR 0007.
- **Requêtes Smart School** (`01-recherche.md` § 2) : champs des familles, noms des classes du collège, fratries.
- **CNDP / loi 09-08** : bloquant avant l'ouverture aux familles (R25).
- **Critères de réussite du pilote** (`00-vision.md`) : chiffres proposés (70 % d'activation en 3 semaines…), à valider.
- **Date de lancement** proposée : lundi 1er février 2027 (début du S2).
- **Nom du professeur** dans l'emploi du temps des parents : proposé oui (nom seul).
- **Application mobile Smart School** : vérifier ce qu'elle offre, pour documenter pourquoi on ne l'utilise pas (`01-recherche.md` § 5).
- **Nouvelles dépendances à valider** : lecteur iCal (phase 6), bibliothèque de QR code (phase 3).
- **Vacances de mai 2027** (hérité de Planning Évaluations) : 02–09 ou 09–16/05.

## Liens avec les autres projets

- **Planning Évaluations** : Ecoleparents lit son flux publié (contrat P1). Son backlog note que l'Espace Parent est **hors de sa v2** et ne le relance pas. Si le contrat P2 (JSON) devient nécessaire, la demande se fait **dans Planning Évaluations**, avec un ADR là-bas.
- **Ecolepay** : même connecteur Smart School, même calcul des paiements. Les rappels de paiement restent chez Ecolepay.
- **Cahier Journal** : source possible des devoirs (ADR 0007).

## Prochaine action

1. **Amadou relit** la documentation, tranche les ADR 🟡, choisit le **nom définitif** et le sous-domaine.
2. ✅ Documentation poussée (27/09/2026) ; nom Ecoleparents appliqué partout. Dépôt renommé `Ecoleparents`.
3. Requêtes de vérification Smart School, collées dans `01-recherche.md`.
4. Maquettes Stitch des écrans prioritaires (P2, P1, P6, fiche d'accès, A2), en arabe d'abord.
5. Phase 0 (installation) — pas avant la validation des points 1 à 4.
