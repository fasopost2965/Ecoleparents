# AGENTS.md — Règles pour toute IA qui travaille dans ce dépôt

> Lis ce fichier en entier avant toute action. Il s'applique à Claude, ChatGPT, Gemini, Antigravity, Codex ou tout autre agent.

## 1. Ce qu'est ce projet

**Ecoleparent** est l'**Espace Parent** de l'école La Victoire (Tétouan). Une application web installable sur le téléphone (PWA) où chaque famille, connectée avec un **code remis par l'école**, retrouve pour chacun de ses enfants :

- l'**emploi du temps** de sa classe (lu dans l'application Planning) ;
- le **calendrier des contrôles publié** (lu dans Planning Évaluations) ;
- les **devoirs à la maison** (source à décider, ADR 0007) ;
- l'**état des paiements** de scolarité et de transport (lu dans Smart School) ;
- les **annonces** de l'école (écrites dans Ecoleparent par la direction).

Un petit espace d'administration (Filament) sert à la Direction et au Secrétariat : comptes des familles, codes, annonces, correspondances entre applications.

**Ce qui compte le plus :**

1. **Une famille ne voit jamais les données d'un autre enfant** (R2, R3). C'est la règle la plus grave du projet : elle est testée à chaque écran.
2. **Les sources ne sont jamais modifiées** (R1). Ecoleparent rassemble, il ne ressaisit rien.
3. **Le ton envers les familles** : respectueux, dans les valeurs islamiques et la culture du Nord du Maroc. Sur les paiements, on **informe**, on ne réclame jamais (R14).

C'est un module de la future application de gestion scolaire, construit sur le même socle qu'Ecolepay et Planning Évaluations.

## 2. Documents à lire avant toute action, dans cet ordre

1. `docs/continuite.md` — où en est le projet
2. `docs/00-vision.md` — le pourquoi et le périmètre
3. `docs/04-regles-metier.md` — **les règles non négociables**
4. `docs/03-architecture.md` — la structure technique
5. `docs/05-plan-mvp.md` — la phase en cours et ses critères de sortie
6. `docs/decisions/` — les décisions prises (✅, à ne pas rediscuter) ou proposées (🟡, à valider par Amadou avant de coder ce qu'elles touchent)

## 3. Stack imposée

- **Laravel 13**, PHP 8.3 minimum
- **Filament 5** pour l'administration (Direction, Secrétariat)
- **Blade + Livewire** (déjà fournis avec Filament) pour l'espace parents, avec un manifeste et un service worker écrits à la main (PWA) — ADR 0001 🟡
- **MySQL** : une base propre + des connexions **en lecture seule** vers Smart School et Planning
- **Pest** pour les tests
- spatie/laravel-permission, Filament Shield, spatie/laravel-activitylog (comme Ecolepay)

**Toute autre bibliothèque (y compris un framework JavaScript, un paquet PWA ou de notifications push) ou tout changement de stack = arrêt et question à Amadou.**

## 4. Interdictions absolues

- ❌ **Écrire dans une source** : Smart School, Planning, Planning Évaluations, ou la source des devoirs si elle est externe. Aucune requête INSERT, UPDATE, DELETE ou migration sur ces connexions (R1).
- ❌ **Faire confiance à un identifiant venu du navigateur.** Chaque accès à un élève est vérifié côté serveur contre les enfants du compte connecté (R2).
- ❌ **Afficher une donnée d'un autre élève** : liste de classe, nom d'un camarade, résultat d'un autre enfant (R3).
- ❌ **Stocker un code d'accès en clair**, le journaliser, ou l'envoyer par WhatsApp ou SMS (R4, R9).
- ❌ **Stocker un montant dû.** Les paiements sont lus dans Smart School au moment de l'affichage (R13).
- ❌ **Employer les mots « impayé », « retard », « dette »**, un compte à rebours ou du rouge d'alerte dans l'espace parents (R14).
- ❌ **Proposer un paiement en ligne ou générer un reçu** (R15).
- ❌ **Mettre les paiements dans le cache hors ligne** du téléphone (R21).
- ❌ **Copier plus que l'identité** d'un élève (nom, classe, statut) : pas de date de naissance, d'adresse, de notes ni d'absences (R24).
- ❌ **Montrer un brouillon de Planning Évaluations** : seule la version publiée existe pour les familles (R12).
- ❌ **Coder des permissions en dur.** Elles sont stockées en base et vérifiées côté serveur (R23).
- ❌ **Écrire un texte destiné aux parents** (message, libellé d'état, notification) sans validation d'Amadou.
- ❌ **Sortir du périmètre** de `docs/00-vision.md`. Toute idée nouvelle va dans `docs/backlog.md`.

## 5. Quand t'arrêter et demander

Tu t'arrêtes et tu poses la question **avant** d'agir si la décision touche :

- le modèle de données ;
- une règle métier ;
- la sécurité, la connexion des familles, les permissions ;
- les données personnelles (ce qui est copié, affiché, conservé) ;
- la stack ou une nouvelle dépendance ;
- l'accès à une source (Smart School, Planning, Planning Évaluations, devoirs) ;
- un texte lu par les familles.

Tu tranches seul, et tu le signales dans ton résumé, pour ce qui est facile à défaire : nommage, organisation interne d'une classe, détail d'affichage dans l'administration.

## 6. Comment travailler

- **Une phase à la fois**, dans l'ordre de `docs/05-plan-mvp.md`. Jamais plusieurs phases groupées.
- **Avant de coder une phase :** restitue en quelques lignes ce que tu vas faire et les règles métier concernées.
- **Prouve, n'affirme pas.** Chaque règle de `docs/04-regles-metier.md` touchée par la phase a un test Pest. Donne le résultat des tests.
- **Tests de cloisonnement (R2, R3) sur chaque écran parent**, sans exception : un parent qui tente d'ouvrir un autre enfant reçoit une page 404.
- **Jamais de `RefreshDatabase`** sur une base partagée : utiliser `DatabaseTransactions` (leçon de Planning Évaluations, 26/09/2026).
- **Pas de correctif hors sujet** sans l'annoncer d'abord.
- **Fin de phase :** résumé (fait, reste à faire, décisions prises), commit, et mise à jour de `docs/continuite.md`.

## 7. Langue

- Code, noms de tables et de colonnes : **anglais**.
- Documentation et administration (Filament) : **français**.
- **Espace parents : arabe (par défaut, RTL complet), français, anglais**, au choix de la famille (ADR 0008, R19).
