# Ecoleparent

**L'Espace Parent de l'école La Victoire** (nom affiché aux familles sur leur téléphone : « La Victoire ») : une application web installable sur le téléphone (PWA), où chaque famille retrouve, pour chacun de ses enfants, l'emploi du temps, le calendrier des contrôles, les devoirs, l'état des paiements et les annonces de l'école — en arabe, en français ou en anglais.

Il **rassemble** ce que les autres outils de l'école savent déjà (Smart School, Planning, Planning Évaluations), **en lecture seule**. Il ne ressaisit rien.

**Premier client :** Groupe La Victoire (Tétouan) · **Pilote :** le collège (1AC-G1, 1AC-G2, 2AC).
**Statut :** 📝 Documentation en cours de validation — aucune ligne de code avant la validation d'Amadou.

## Stack prévue

Laravel 13 · Filament 5 (espace Direction/Secrétariat) · Blade + Livewire (espace parents, PWA) · MySQL (base propre + sources en lecture seule) · Pest.
Voir [`docs/decisions/0001-application-separee-pwa.md`](docs/decisions/0001-application-separee-pwa.md).

## Documentation du projet

| Fichier | Contenu |
|---|---|
| [`AGENTS.md`](AGENTS.md) | Règles pour toute IA qui travaille dans ce dépôt |
| [`docs/DEMARRAGE.md`](docs/DEMARRAGE.md) | **Message à coller au début de chaque session de travail** |
| [`docs/continuite.md`](docs/continuite.md) | **État du projet — à lire en premier** |
| [`docs/00-vision.md`](docs/00-vision.md) | Problème, promesse, périmètre |
| [`docs/01-recherche.md`](docs/01-recherche.md) | Sources de données, faits vérifiés et à vérifier, données personnelles |
| [`docs/02-specifications.md`](docs/02-specifications.md) | Écrans parents et administration, permissions, parcours |
| [`docs/03-architecture.md`](docs/03-architecture.md) | Connecteurs, modèle de données, sécurité |
| [`docs/04-regles-metier.md`](docs/04-regles-metier.md) | Règles non négociables R1 à R26 |
| [`docs/05-plan-mvp.md`](docs/05-plan-mvp.md) | Phases et critères de sortie |
| [`docs/06-design.md`](docs/06-design.md) · [`07-tests.md`](docs/07-tests.md) · [`08-deploiement.md`](docs/08-deploiement.md) | Design, tests, mise en production |
| [`docs/donnees-initiales.md`](docs/donnees-initiales.md) | Périmètre du pilote : classes, correspondances entre applications |
| [`docs/decisions/`](docs/decisions/) | Décisions prises ou proposées (ADR) |
| [`docs/backlog.md`](docs/backlog.md) | Tout ce qui sort du MVP |
| [`prompts/`](prompts/) | Prompts prêts à l'emploi pour les agents de code |

## Dépôts liés

| Dépôt | Ce que Ecoleparent en lit |
|---|---|
| `fasopost2965/e-victoire` (Smart School) | Familles, élèves, classes, paiements |
| `fasopost2965/TimeTable` (Planning) | Emplois du temps des classes |
| `fasopost2965/planning-evaluations` | Calendrier des contrôles **publié** (contrat de lecture P1/P2 de son backlog) |
| `fasopost2965/ecolepay` | Même socle, même connecteur Smart School, même calcul des paiements |
| `fasopost2965/cahier-journal-la-victoire` | Source possible des devoirs (🔍, ADR 0007) |

## Prochaine étape

Voir [`docs/continuite.md`](docs/continuite.md). **Pour démarrer une session : coller le message de [`docs/DEMARRAGE.md`](docs/DEMARRAGE.md).**
