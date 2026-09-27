# ADR 0004 — Un compte par famille, au nom du parent de garde

**Date :** 2026-09-27 · **Statut :** 🟡 Proposée — dépend des vérifications de `01-recherche.md` § 2

## Contexte

- Ecolepay contacte **le parent de garde** (son ADR 0002) : c'est la personne habilitée à répondre au nom de l'enfant.
- Smart School rattache en principe les frères et sœurs à un même compte parent (`parent_id` 🔍).
- Des familles ont peut-être des enfants au collège **et** au primaire (🔍).

## Options

| Option | Pour | Contre |
|---|---|---|
| **Un compte par famille** (parent de garde), tous ses enfants | Une seule fiche, un seul code ; cohérent avec Ecolepay | Un seul code pour les deux parents (s'ils le partagent, c'est leur choix) |
| Un compte par parent (père et mère) | Chacun son accès | Double gestion des codes ; situations de séparation délicates à arbitrer |
| Un compte par élève | Simple à générer | Plusieurs codes par famille, confusion |

## Décision proposée

- **Un compte par famille**, au nom du parent de garde.
- Enfants **proposés** depuis Smart School (`parent_id`, à défaut même téléphone du parent de garde), **confirmés par le Secrétariat** avant la génération du code (R8).
- Pendant le pilote, **seuls les enfants du collège** apparaissent ; les frères et sœurs du primaire s'ajouteront quand le primaire entrera dans le périmètre (🔍 à confirmer par Amadou : les montrer dès le pilote ?).

## Conséquences

- Situations familiales délicates (séparation, garde contestée) : le Secrétariat peut ne pas remettre de code et en parler à la direction ; aucune règle automatique.
