# 02 — Spécifications

## Rôles

| Rôle | Qui | Accès |
|---|---|---|
| **Parent** | Parent de garde, un compte par famille | Espace parents (PWA), identifiant + code remis par l'école |
| **Secrétariat** | Secrétaire | Administration (Filament) |
| **Direction** | Amadou, directrice | Administration (Filament), tous les droits |

Les droits exacts sont des **permissions en base** (Filament Shield), jamais codées en dur (R23). Voir § Permissions.

## Vocabulaire

**Espace parents** (textes à valider par Amadou dans les trois langues avant la phase concernée) :

| Notion | Français | Jamais |
|---|---|---|
| Mois non encore payé | **À régler** | impayé, en retard, dette, dû |
| Mois payé | **Réglé** | — |
| Contrôle | **Contrôle** (Contrôle 1, 2, 3) | examen, devoir surveillé |
| Travail à la maison | **Devoirs** | — |
| Information de l'école | **Annonce** | notification, alerte |
| Fraîcheur | « Mis à jour le 12/10 à 08 h 30 » | — |

**Administration** : famille, compte, fiche d'accès, code, remise, correspondance des classes, annonce (brouillon / publiée / retirée).

---

# Espace parents (PWA, téléphone d'abord)

Commun à tous les écrans : en-tête avec le **sélecteur d'enfant** (prénom + classe) quand la famille a plusieurs enfants, le **sélecteur de langue** (ع / FR / EN), et une barre de navigation en bas : Accueil · Emploi du temps · Contrôles · Devoirs · Plus (Paiements, Annonces, Réglages).

## P1 — Connexion

- Champs : **identifiant** et **code** (tels qu'imprimés sur la fiche d'accès).
- Langue choisie avant la connexion (arabe par défaut).
- Message d'erreur **neutre**, qui ne dit pas si l'identifiant existe (R6).
- Après 5 échecs : « Réessayez dans 15 minutes, ou contactez le secrétariat. »
- Lien « Code perdu ? » → texte : se présenter au secrétariat (pas de réinitialisation en ligne dans le MVP).
- Une fois connecté, l'appareil **reste connecté** (R7).

## P2 — Accueil (par enfant)

| Bloc | Contenu |
|---|---|
| **Aujourd'hui / Demain** | Séances de la journée (horaire, matière) ; « Pas de cours » les jours fériés et de vacances (calendrier officiel lu dans Planning Évaluations 🔍) |
| **Prochains contrôles** | Les 3 prochains, les 7 prochains jours mis en avant |
| **Devoirs** | À rendre dans les prochains jours |
| **Annonce** | La plus récente non lue |
| **Paiements** | Une ligne : « Tout est réglé » ou « 2 mois à régler » → lien vers P6. Ton neutre (R14) |

Chaque bloc affiche sa date de mise à jour (R20).

## P3 — Emploi du temps

- Semaine de la classe (lundi → vendredi), vue jour par défaut sur téléphone, bascule semaine.
- Par séance : horaire, matière, professeur (nom seul, **à valider**, `01-recherche.md` § 3).
- Source : Planning (R11). Si la classe n'a pas de correspondance dans Planning : « Emploi du temps bientôt disponible ».

## P4 — Contrôles

- Liste chronologique des contrôles **publiés** de la classe : date, jour, matière, numéro.
- Mention « Version n° X du … » (R12).
- Dates « À confirmer » affichées telles quelles.
- Bouton « Ajouter à l'agenda du téléphone » → le lien iCal de la classe (déjà fourni par Planning Évaluations).

## P5 — Devoirs

- Par classe, du plus proche au plus lointain : matière, consigne, donné le, à rendre le.
- Devoirs passés repliés (les 2 dernières semaines).
- Jamais de devoir personnel à un élève (R18).

## P6 — Paiements

- Deux sections : **Scolarité** et **Transport** (si l'élève est inscrit au transport).
- Mois de septembre à juin : **Réglé** / **À régler**, avec le tarif du mois.
- Un seul total en bas : « Reste à régler : X DH ». Pas de couleur d'alerte, pas de compte à rebours (R14).
- Phrase fixe (texte à valider) : « Pour toute question, le secrétariat est à votre disposition. » + horaires d'accueil.
- **Jamais hors ligne** : sans connexion, « Consultez cette page une fois connecté à Internet » (R21).
- Pas de paiement en ligne, pas de reçu (R15).

## P7 — Annonces

- Liste, de la plus récente à la plus ancienne ; badge « Nouveau » sur les non lues.
- Chaque annonce : titre, texte, date, « modifiée le … » si c'est le cas (R17).
- Affichée dans la langue de la famille ; si elle n'existe pas dans cette langue, en arabe (R16).

## P8 — Réglages

- Langue.
- « Installer l'application sur mon téléphone » (explications Android / iPhone).
- Mes enfants (prénoms, classes) — lecture seule ; « Une erreur ? Contactez le secrétariat. »
- Informations sur les données personnelles (texte à valider, R25).
- Se déconnecter de cet appareil.

---

# Administration (Filament, en français)

## A1 — Tableau de bord du pilote

| Bloc | Contenu |
|---|---|
| Adoption | Familles du périmètre · fiches imprimées · codes remis · comptes activés (1ʳᵉ connexion) · % d'activation |
| Usage | Familles connectées cette semaine · par classe |
| Synchronisations | Dernière lecture de Smart School et de Planning ; élèves sans famille ; classes sans correspondance |
| Annonces | Brouillons en attente, dernières publiées |

## A2 — Familles et comptes

- Liste des familles du périmètre (parent de garde, enfants, classes), **proposées depuis Smart School** (ADR 0004).
- État : *à vérifier* → *vérifiée* → *fiche imprimée* → *code remis* → *active* ; *suspendue*.
- Actions : **Vérifier** (rattachement des enfants confirmé) · **Générer la fiche d'accès** (PDF A5 : identifiant, code, QR vers l'adresse, instructions d'installation en arabe et français) · **Noter la remise** (date, remis à qui) · **Régénérer le code** (motif obligatoire, R5) · **Suspendre / réactiver**.
- Le code n'est visible **qu'une fois**, sur la fiche générée (R4).
- Dernière connexion de la famille (R22).

## A3 — Annonces

- Rédaction : titre et texte en **arabe (obligatoire)**, français et anglais (facultatifs).
- Cible : tout le collège, ou une ou plusieurs classes.
- États : brouillon → publiée → retirée. Publier et retirer demandent la permission `PublishAnnouncement`.
- Aperçu tel que le verra le parent, dans les trois langues, RTL pour l'arabe.

## A4 — Devoirs (si l'ADR 0007 retient la saisie Secrétariat)

- Saisie rapide : classe(s), matière, consigne, date donnée, date à rendre.
- Duplication vers les autres groupes du niveau (1AC-G1 → 1AC-G2).
- Modification et suppression tracées (R26).

## A5 — Correspondance des classes

- Une ligne par classe du périmètre : classe Smart School ↔ classe Planning ↔ lien de classe Planning Évaluations (jeton du flux iCal, **stocké chiffré**).
- Tant qu'une classe n'a pas sa correspondance, la section concernée affiche « bientôt disponible » aux parents.

## A6 — Paramètres

- Périmètre du pilote (classes incluses) · durée de connexion d'un appareil (90 jours par défaut) · horaires d'accueil du secrétariat · textes fixes validés · conservation du journal des connexions (12 mois).

## A7 — Utilisateurs et rôles

- Comptes Direction / Secrétariat, rôles et permissions (Shield). Création d'un compte **depuis l'écran**, pas en ligne de commande.

---

## Permissions (proposées)

| Action | Direction | Secrétariat |
|---|---|---|
| Voir le tableau de bord (A1) | ✅ | ✅ |
| Vérifier une famille, générer et remettre une fiche (A2) | ✅ | ✅ |
| Régénérer un code, suspendre un compte | ✅ | ✅ (motif obligatoire) |
| Rédiger une annonce | ✅ | ✅ |
| Publier / retirer une annonce | ✅ | ❌ |
| Saisir les devoirs (A4) | ✅ | ✅ |
| Correspondance des classes (A5), paramètres (A6) | ✅ | ❌ |
| Utilisateurs et rôles (A7) | ✅ | ❌ |
| Voir le journal des connexions | ✅ | ❌ |

## Parcours type

1. **Préparation (Secrétariat)** : la synchronisation propose les familles du collège → le secrétariat vérifie chaque famille (enfants rattachés) → génère les fiches d'accès → les remet en main propre (réunion de parents, accueil) et note la remise.
2. **Première connexion (Parent)** : scanne le QR de la fiche → choisit sa langue → saisit identifiant et code → installe l'application sur l'écran d'accueil.
3. **Usage courant (Parent)** : ouvre l'application le soir → voit les séances de demain, le contrôle de Maths jeudi, les devoirs de Français.
4. **Annonce (Direction)** : rédige « Sortie pédagogique du 12/03 » en arabe et en français, cible 1AC-G1 et 1AC-G2, publie.
5. **Code perdu (Parent → Secrétariat)** : le parent se présente → le secrétariat régénère le code (l'ancien ne marche plus, les appareils sont déconnectés) → nouvelle fiche remise.
