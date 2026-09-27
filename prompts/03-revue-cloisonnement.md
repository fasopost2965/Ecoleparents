# Revue de cloisonnement (R2, R3)

À donner à une **deuxième IA** (ou une deuxième personne) qui n'a pas écrit le code, avant la mise en ligne (phase 10).

---

Tu relis le dépôt `ecoleparent` avec un seul objectif : **une famille connectée peut-elle, d'une façon ou d'une autre, voir une donnée d'un enfant qui n'est pas le sien ?**

Lis `AGENTS.md`, `docs/04-regles-metier.md` (R2, R3, R21) et `docs/03-architecture.md` § 3 et § 8. Puis :

1. **Liste toutes les routes** de la garde `parent` et **tous les composants Livewire** de l'espace parents.
2. Pour chacun : d'où vient l'identifiant de l'enfant ? Passe-t-il par `App\Domain\ParentView` avant toute lecture ? Une propriété Livewire publique peut-elle être modifiée depuis le navigateur pour pointer vers un autre enfant ?
3. Vérifie les **réponses mises en cache** (cache applicatif, service worker, en-têtes HTTP) : une réponse d'une famille peut-elle être servie à une autre ?
4. Vérifie que **les paiements** ne sont jamais dans le cache du téléphone (R21).
5. Vérifie qu'**aucune donnée d'un autre élève** (nom, classe, liste) n'apparaît dans les rendus, les réponses JSON ou les messages d'erreur (R3).
6. Vérifie que la garde `parent` n'ouvre **jamais** `/admin`, et inversement.

Rends un tableau : route ou composant · risque constaté (aucun / faible / grave) · preuve (fichier et ligne) · correction proposée. **Ne corrige rien toi-même.**
