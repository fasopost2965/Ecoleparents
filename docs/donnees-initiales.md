# Données initiales — périmètre du pilote

## Classes du collège (2026-2027)

| Nom court | Nom officiel | Planning (`classes.id`) | Smart School (classe / section) | Planning Évaluations (lien de classe) |
|---|---|---|---|---|
| 1AC-G1 | 1APIC G1 | `COLG1` ✅ | 🔍 requête `01-recherche.md` § 2 | 🔍 jeton à copier depuis l'écran 7 de Planning Évaluations |
| 1AC-G2 | 1APIC G2 | `COLG2` ✅ | 🔍 | 🔍 |
| 2AC | — | `COL2EG1` ✅ | 🔍 | 🔍 |

3AC et le second groupe de 2AC (`COL2EG2`) : hors périmètre, pas d'élèves cette année ✅.

Source des identifiants Planning : `planning-evaluations/config/evaluations.php`.

## Matières évaluées (pour les contrôles)

EIS, Maths, PC, SVT, Arabe, Français, HG, Info, Anglais (source : Planning Évaluations). L'emploi du temps des parents montre **toutes** les séances de la classe, y compris EPS et éducation artistique.

## Calendrier officiel 2026-2027

Fichier déjà transcrit et vérifié : `planning-evaluations/docs/donnees/calendrier-2026-2027.csv`. À importer tel quel pour la nature des jours (phase 5).

**Point ouvert hérité :** vacances intermédiaires de mai 2027 — le CSV dit 02 → 09/05/2027, plusieurs sources de presse disent 09 → 16/05/2027. À confirmer par Amadou sur le document papier.

## À compléter (phase 2)

- Nombre de familles du collège, fratries, familles avec des enfants au primaire (requête 3 de `01-recherche.md` § 2).
- Session scolaire Smart School en cours (`sessions.id`).
