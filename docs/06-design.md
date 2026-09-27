# 06 — Design

**Statut :** système choisi. Les maquettes (Stitch) viennent **après** la validation de `03-architecture.md`, **avant** tout code d'écran (consigne d'Amadou : « tous les écrans doivent d'abord être faits avec Stitch »).

## Système de design : « Haute Banque & Grand Livre »

**Décision :** Ecoleparents reprend le **même système** que le portail `livre-des-comptes-la-victoire`, Ecolepay et Planning Évaluations.

- **Référence Stitch :** asset `assets/5d7567ad0c4440c8a9c6671305c6d74e` (projet « Financial Report Dashboard », id `14254095045585116271`).
- **Référence code :** `planning-evaluations/resources/css/filament/admin/theme.css` (jetons `--pe-*`, vérifiés sur le site en production).
- **Règle :** ne jamais réinterpréter ce design de mémoire.

| Élément | Valeur |
|---|---|
| Primaire | Encre `#111413` (et non l'ocre) |
| Secondaire (accents) | Ocre `#7f560c` |
| Fond | Parchemin `#faf9f5` — **vérifier le contraste** : Amadou a demandé de corriger le « fond sablé » et les contrastes faibles sur ses outils |
| Succès | Vert profond `#15573e` sur `#edf4f0` |
| Doré | `#d4af37`, **décoration uniquement, jamais pour du texte** |
| Titres | EB Garamond · Interface : Inter · Dates et chiffres : JetBrains Mono (chiffres tabulaires) |
| Arabe | Noto Naskh Arabic (écran) ; **XB Riyaz** dans les PDF (mpdf 8 ne lit pas les tables GPOS de Noto Naskh — constaté dans Planning Évaluations) |
| Angles | Droits · Ombres : aucune, traits fins |

## Adaptation à l'espace parents

**Nom affiché aux familles :** l'icône et le titre de l'application (manifeste PWA : `name`, `short_name`) portent **« La Victoire »** et le logo de l'école, jamais « Ecoleparents ». Pour un autre client, ce sera le nom de son école (paramètre).

L'administration garde le rendu « registre » des autres outils. **L'espace parents est différent** : un public large, sur téléphone, parfois peu à l'aise avec le numérique, souvent en arabe. Le système est gardé (couleurs, polices, angles droits), avec ces ajustements :

1. **Téléphone d'abord.** Chaque écran est conçu à 360 px de large, puis étendu. Déclinaison mobile maximale (règle d'Amadou pour tous ses outils).
2. **Lisibilité avant densité.** Texte courant **16 px minimum**, arabe **17–18 px** ; interligne ~1,5 ; zones de toucher de **44 px** minimum. Contraste WCAG AA vérifié sur le parchemin.
3. **Arabe en premier.** Les maquettes sont faites **d'abord en arabe (RTL)**, puis vérifiées en français et en anglais. Mise en page réellement inversée (navigation, flèches, alignements), pas seulement les mots traduits.
4. **Couleurs des paiements** (R14) : « Réglé » en vert profond, « À régler » en **encre sur fond neutre** — jamais de bordeaux ni de rouge. Chaque état porte aussi un mot (lisible sans la couleur).
5. **Chaleur sans décoration inventée.** Salutation sobre (« السلام عليكم » / « Bonjour »), prénom de l'enfant en titre. Pas de sceau, de matricule, de mentions officielles inventées.
6. **États vides bienveillants** : « Pas de devoirs pour les prochains jours » plutôt qu'une liste vide.
7. **Fraîcheur visible** : « mis à jour le … » discret sous chaque bloc (R20) ; bandeau hors ligne.

## Fiche d'accès (PDF A5)

Règles PDF validées par Amadou pour tous ses documents :

- **12 pt minimum** (texte arabe : 1 à 2 pt de plus), interligne ~1,4 ;
- doré réservé à la décoration, jamais au texte ;
- **test systématique en impression noir et blanc** ;
- identifiant et code en JetBrains Mono, **très gros** (20 pt), sans caractère ambigu ;
- recto : arabe · verso : français (ou deux colonnes, à trancher sur maquette) ;
- QR code vers l'adresse de l'espace parents, instructions d'installation Android et iPhone en 3 étapes illustrées ;
- mention « Ce code est personnel. En cas de perte, adressez-vous au secrétariat. »

## Écrans à maquetter en priorité (Stitch)

**Méthode confirmée dans Planning Évaluations :** décrire l'écran en français avec le **contenu réel** (vraies classes, vraies matières), passer `designSystem: assets/5d7567ad0c4440c8a9c6671305c6d74e`, puis **retirer tout ce que Stitch invente** (mentions ministérielles, salles, signataires, données hors périmètre). Vigilance accrue sur les **écrans publics** : c'est là que Stitch invente le plus. La génération peut être très lente : ne jamais relancer avant d'avoir vérifié qu'elle n'est pas en cours.

1. **P2 — Accueil** (mobile, arabe) — l'écran qui décide de l'adoption
2. **P1 — Connexion** (mobile, arabe)
3. **P6 — Paiements** (mobile, arabe) — le ton se juge sur maquette (R14)
4. **Fiche d'accès PDF** (A5, arabe / français)
5. **A2 — Familles et comptes** (desktop)

Puis, au fil des phases : P3, P4, P5, P7, P8, A1, A3, A4, A5.

## Continuité avec les autres outils

| Élément | Autres outils | Ecoleparents |
|---|---|---|
| Administration | Navigation en haut, contenu centré, cartouches de KPI | Identique |
| Pages publiques de Planning Évaluations | Blade pur, bilingue FR/AR, sélecteur de langue en en-tête | Même logique, trois langues |
| Logo | Chargé depuis `groupelavictoire.com` dans Planning Évaluations | **Fichier local** dans le dépôt (leçon notée dans le backlog de Planning Évaluations) |
