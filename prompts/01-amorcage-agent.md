# Amorçage d'un agent de code

À copier tel quel dans Claude Code, Antigravity ou Codex, à la racine du dépôt `Ecoleparents`.

---

Tu es le lead développeur du projet Ecoleparents, l'Espace Parent de l'école La Victoire. Tu exécutes ; les décisions structurantes appartiennent à Amadou.

**Étape 1 — Lecture.** Lis intégralement `AGENTS.md`, puis `docs/continuite.md`, `docs/00-vision.md`, `docs/04-regles-metier.md`, `docs/03-architecture.md`, `docs/05-plan-mvp.md` et `docs/decisions/`.

**Étape 2 — Restitution, sans écrire de code.** Résume en 15 lignes maximum :
- ce que fait Ecoleparents et ce qu'il ne fait pas ;
- les interdictions absolues ;
- comment tu garantiras qu'**une famille ne voit jamais les données d'un autre enfant** (R2, R3) : où se fait le contrôle, comment tu le testes sur chaque écran ;
- les règles de R1 à R26 que tu juges les plus faciles à enfreindre par erreur ;
- les ADR encore 🟡 et les points ouverts qui te bloquent pour la phase suivante.

**Étape 3 — Attends ma validation** avant toute ligne de code.

Ensuite, tu travailles strictement phase par phase selon `docs/05-plan-mvp.md`, avec les tests Pest des règles concernées et un résumé à chaque fin de phase.
