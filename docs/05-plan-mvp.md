# 05 — Plan du MVP

Chaque phase se termine par : tests verts, résumé, commit, mise à jour de `continuite.md`. **Pas de phase suivante sans validation d'Amadou.**

**Objectif de calendrier (proposé 🔍) :** ouverture aux familles du collège au **début du 2ᵉ semestre, lundi 1er février 2027** (semaine 18), juste après la publication du calendrier des contrôles du S2 dans Planning Évaluations.

**Contrainte connue :** la v1.1 de Planning Évaluations est prévue pendant les vacances du 6 au 13 décembre 2026, pour la réunion de janvier. Ne pas planifier de phase lourde de ParentsApp sur cette semaine.

| Période (proposée) | Phases |
|---|---|
| Octobre 2026 | 0, 1, 2 |
| Novembre 2026 | 3, 4, 5 |
| Décembre 2026 (hors semaine du 6 au 13) | 6, 7, 8 |
| Janvier 2027 | 9, 10 (recette, CNDP, fiches d'accès, déploiement) |

---

## Phase 0 — Installation (manuelle, par Amadou)

- Projet Laravel 13, Filament 5, dépendances (liste complète demandée **en un seul message**, comme pour Ecolepay).
- Base MySQL locale du projet. Connexions `smartschool` et `planning` **déclarées**, en lecture seule, pointant d'abord vers une copie locale ou vide.
- Thème « Haute Banque & Grand Livre » repris de Planning Évaluations (voir `06-design.md`).
- Dépôt poussé sur GitHub.

**Sortie :** `php artisan test` passe ; la page de connexion de l'administration s'affiche avec le thème ; une écriture sur `smartschool` et sur `planning` est refusée (R1).

---

## Phase 1 — Socle

- Rôles Direction et Secrétariat, permissions en base (Shield), écran de création des comptes (A7).
- Journal technique (activitylog).
- Table `settings` et écran Paramètres (A6).
- **Garde `parent`** et gabarit de l'espace parents : en-tête, sélecteur de langue ع / FR / EN, RTL complet en arabe, barre de navigation basse, manifeste PWA (sans service worker encore).
- Fichiers de traduction `lang/{ar,fr,en}` ; **tous les textes destinés aux familles listés dans un seul fichier à valider** par Amadou.

**Règles testées :** R19, R23, R26.
**Sortie :** un compte Secrétariat ne peut pas ouvrir les Paramètres, même par l'URL ; la page de connexion parents s'affiche correctement dans les trois langues, en RTL pour l'arabe.

---

## Phase 2 — Connecteur Smart School et familles

**Préalable :** requêtes de vérification de `01-recherche.md` § 2 faites et collées ; ADR 0004 validée.

- `App\Domain\SchoolData` : code d'Ecolepay repris (même interface), familles, élèves, session en cours.
- Tables `families`, `family_students`, `students`, `local_classes` (partie Smart School).
- Synchronisation (tâche planifiée + bouton) ; élève inactif retiré, famille suspendue (R10).
- Écran A2 : familles proposées, **vérification** par le Secrétariat.

**Règles testées :** R1, R8, R10, R24.
**Sortie :** toutes les familles du collège sont proposées ; le nombre correspond à la requête manuelle de `01-recherche.md` ; aucune donnée hors identité n'est copiée.

---

## Phase 3 — Comptes, fiches d'accès et connexion

- Tables `parent_accounts`, `parent_devices`, `parent_logins`.
- Génération du code, **fiche d'accès PDF A5** (arabe + français, QR code, instructions d'installation), remise enregistrée.
- Écran P1 (connexion), appareil mémorisé, déconnexion, régénération, suspension, limitation des tentatives.
- `App\Domain\ParentView` (squelette) et **premier test de cloisonnement** : un parent qui demande l'enfant d'une autre famille reçoit 404.
- Purge automatique du journal des connexions après 12 mois.

**Règles testées :** R2, R4, R5, R6, R7, R9, R22, R23.
**Sortie :** un compte de test se connecte avec sa fiche ; le même code, régénéré, ne marche plus et l'appareil est déconnecté ; 6ᵉ tentative bloquée ; aucun code en clair en base ni dans les journaux (recherche automatique dans les tests).

---

## Phase 4 — Paiements

- `paymentStatus()` du connecteur (scolarité, transport).
- Écran P6, vocabulaire validé, `Cache-Control: no-store`.
- **Test de comparaison** avec le portail des rapports sur tous les élèves du collège.

**Règles testées :** R2, R13, R14, R15, R21.
**Sortie :** 100 % des élèves du collège ont la même situation que dans le portail ; aucun des mots interdits (R14) n'apparaît dans les trois langues (test automatique sur les fichiers de traduction et le rendu).

---

## Phase 5 — Emploi du temps

- `TimetableSource` repris de Planning Évaluations (`PlanningDatabaseSource` + copie locale).
- Écran A5 (correspondance des classes : Smart School ↔ Planning).
- Import du calendrier officiel (CSV de Planning Évaluations) pour la nature des jours.
- Écran P3.

**Règles testées :** R1, R2, R3, R11.
**Sortie :** l'emploi du temps affiché pour chaque classe du collège est identique à celui de l'application Planning ; les jours fériés et de vacances affichent « Pas de cours ».

---

## Phase 6 — Contrôles

- Connecteur `PublishedAssessmentsSource` sur le flux iCal de chaque classe (contrat P1), jeton chiffré (A5).
- Écran P4, bouton « Ajouter à l'agenda ».

**Règles testées :** R2, R12, R20.
**Sortie :** après une nouvelle publication dans Planning Évaluations, le changement apparaît dans ParentsApp au plus tard 15 minutes après, avec le nouveau n° de version ; un brouillon n'apparaît jamais.

---

## Phase 7 — Annonces

- Tables `announcements`, `announcement_targets`, `announcement_reads`.
- Écran A3 (rédaction, aperçu trilingue, publication) et P7.

**Règles testées :** R2, R16, R17, R26.
**Sortie :** une annonce ciblée sur 1AC-G1 n'est pas visible d'une famille de 2AC ; une annonce modifiée porte la mention ; une annonce sans version arabe ne peut pas être publiée.

---

## Phase 8 — Devoirs

**Préalable :** ADR 0007 tranchée par Amadou.

- `HomeworkSource` et son implémentation retenue ; écran A4 si saisie Secrétariat ; écran P5.

**Règles testées :** R2, R18, R26.
**Sortie :** un devoir saisi pour 1AC-G1 et dupliqué vers 1AC-G2 apparaît pour les familles des deux classes, et pour elles seules.

---

## Phase 9 — Accueil, hors ligne, tableau de bord

- Écran P2 (Accueil), P8 (Réglages, installation).
- Service worker : cache de l'interface et des dernières réponses (sauf paiements), bandeau hors ligne, vidage à la déconnexion.
- Écran A1 (tableau de bord du pilote).

**Règles testées :** R2, R20, R21.
**Sortie :** en mode avion, l'application s'ouvre, montre la dernière version datée, et la page Paiements invite à se reconnecter ; les indicateurs de A1 correspondent à un calcul fait à la main.

---

## Phase 10 — Recette, conformité, déploiement, pilote

- Recette de `07-tests.md` (dont la **revue de cloisonnement** dédiée).
- Question CNDP tranchée, texte d'information des familles validé (R25).
- Déploiement selon `08-deploiement.md`.
- Impression des fiches d'accès, remise aux familles du collège (réunion de parents ou accueil).
- **Pilote** : ouverture le 1er février 2027 (proposé), mesure des critères de `00-vision.md` pendant 4 semaines.

**Sortie :** critères de réussite de `00-vision.md` mesurés et présentés à Amadou.
