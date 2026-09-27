# Démarrage — message à coller au début d'une session de travail

> À coller tel quel au début d'une nouvelle conversation (Claude, Claude Code, ChatGPT, Gemini…).

---

Bonjour. Je suis Amadou, je développe moi-même les outils de gestion de l'école La Victoire à Tétouan.

**Le projet :** Ecoleparents, l'Espace Parent de l'école. Une application web installable sur le téléphone (PWA) où chaque famille, connectée avec un code remis par l'école, retrouve pour chacun de ses enfants l'emploi du temps, les contrôles publiés, les devoirs, l'état des paiements et les annonces, en arabe, français ou anglais. Il lit Smart School, Planning et Planning Évaluations **en lecture seule**. Pilote : le collège.

**Où tout se trouve :**
- Dépôt du projet : https://github.com/fasopost2965/Ecoleparents
- Même socle et même connecteur Smart School : https://github.com/fasopost2965/ecolepay
- Connecteur Planning et flux des contrôles : https://github.com/fasopost2965/planning-evaluations
- Bibliothèque d'idées et méthode : https://github.com/fasopost2965/fasopost-bibliotheque

**Avant toute chose, lis dans `Ecoleparents`, dans cet ordre :**
1. `AGENTS.md`
2. `docs/continuite.md`
3. `docs/00-vision.md`, `docs/04-regles-metier.md`, `docs/02-specifications.md`, `docs/03-architecture.md`, `docs/05-plan-mvp.md`
4. `docs/decisions/` (✅ acceptées, à ne pas rediscuter ; 🟡 proposées, à valider avec moi)

**Ma méthode :**
- Documentation d'abord. On ne code qu'en suivant `05-plan-mvp.md`, une phase à la fois.
- J'économise mes tokens : pour une installation, donne-moi **tout en un seul message** (commandes, versions, ordre), je l'exécute moi-même.
- Tu t'arrêtes et tu me demandes avant toute décision structurante (données, règles, sécurité, connexion des familles, stack, textes lus par les parents).
- Prouve, n'affirme pas : chaque règle métier touchée a un test Pest.

**Ce qui compte le plus :**
- Une famille ne voit **jamais** les données d'un autre enfant (R2, R3).
- Les sources ne sont **jamais** modifiées (R1).
- Sur les paiements, on **informe**, on ne réclame jamais (R14), dans le respect des valeurs islamiques et de la culture du Nord du Maroc.
- Même design que mes autres outils (« Haute Banque & Grand Livre »), adapté au téléphone et à l'arabe.

**Aujourd'hui, je veux :** [à compléter — par exemple : « la liste d'installation de la phase 0 », « les maquettes Stitch de l'accueil », « relire l'architecture »]

Commence par me confirmer en 10 lignes ce que tu as compris du projet et de ses règles non négociables.
