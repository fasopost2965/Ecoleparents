# ADR 0009 — Hébergement sur le même compte Hostinger

**Date :** 2026-09-27 · **Statut :** 🟡 Proposée (même choix que Planning Évaluations, ADR 0008 de celui-ci)

## Contexte

Smart School, Planning et Planning Évaluations sont sur le **même compte Hostinger Business** (mutualisé). Ecoleparent doit lire Smart School et Planning en permanence. Un VPS existe dans un autre compte.

## Options

| Option | Pour | Contre |
|---|---|---|
| **Même compte mutualisé** | Bases lues en local, rien d'exposé sur Internet ; procédure de déploiement déjà rodée | Pas de tâches permanentes (notifications push temps réel impossibles) ; ressources partagées |
| VPS | Maîtrise totale, prêt pour les notifications | Ouvrir Smart School et Planning à distance ; administration plus lourde |

## Décision proposée

**Même compte mutualisé**, sous-domaine **{{SOUS_DOMAINE}}** (proposé : `parents.groupelavictoire.com`).

## Conséquences

- Procédure de `08-deploiement.md`, copiée de Planning Évaluations.
- Le passage au VPS se reposera avec les notifications push (backlog), sans changer d'adresse.
