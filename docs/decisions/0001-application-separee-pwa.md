# ADR 0001 — Application séparée, PWA en Blade + Livewire

**Date :** 2026-09-27 · **Statut :** ✅ Application séparée et PWA (décisions d'Amadou) · 🟡 technique du front parents (Blade + Livewire) à valider

## Contexte

- Amadou veut un Espace Parent « maison » qui **rassemble** les données de ses autres outils (Smart School, Planning, Planning Évaluations) ; sa vision est non monolithique : un outil par usage, les outils communiquant entre eux.
- Décision déjà prise : **démarrer en PWA** (web installable, zéro coût, aucun store) ; Android prioritaire si une application native devient nécessaire ; Apple écarté pour l'instant.
- L'application mobile Smart School (licence existante, non déployée) ne peut pas rassembler Planning et Planning Évaluations (`01-recherche.md` § 5).
- Ses nouveaux projets partent sur Laravel (socle commun).

## Options pour l'espace parents

| Option | Pour | Contre |
|---|---|---|
| **Blade + Livewire** dans la même application Laravel | Déjà fourni avec Filament : aucune dépendance nouvelle ; une seule application à déployer ; même logique que les pages publiques de Planning Évaluations (Blade pur) | Moins « application » qu'un front JavaScript complet ; hors ligne limité à ce que fait le service worker |
| React (SPA) + API Laravel | Amadou connaît React ; expérience très fluide | Deux projets à maintenir, une API à sécuriser (cloisonnement plus exposé), `npm run build` plus lourd |
| Filament pour les parents | Tout dans un seul outil | Filament est fait pour des outils de travail, pas pour des familles sur téléphone |

## Décision

- **Application Laravel séparée** (pas un module d'Ecolepay ni de Planning Évaluations), même socle et même thème.
- Administration en **Filament 5**, espace parents en **Blade + Livewire**, garde `parent` séparée, **PWA** (manifeste + service worker écrits à la main).

## Conséquences

- Aucune nouvelle dépendance pour le front.
- Si l'expérience hors ligne ou la fluidité ne suffisent pas au pilote, le passage à React reste possible derrière le même `ParentView` (backlog).
