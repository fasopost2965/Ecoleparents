# 01 — Recherche

**Statut :** en cours. Les faits confirmés sont marqués ✅, ceux à vérifier 🔍. Rien de ce qui est 🔍 ne doit être codé comme une certitude.

---

## 1. Terrain (La Victoire)

| Sujet | Ce qu'on sait | Statut |
|---|---|---|
| Langues des parents | Majoritairement arabophones ; certains lisent le français ; quelques-uns anglophones | ✅ |
| Canal actuel | Le numéro WhatsApp connu des parents est celui de la directrice ; la caissière appelle depuis le fixe de l'école | ✅ |
| Ton | Très poli et respectueux, selon les valeurs islamiques et la culture conservatrice du Nord du Maroc | ✅ |
| Nom de l'école | « La Victoire de l'Enseignement Privé » ; en communication « École La Victoire, Tétouan » ; le nom arabe n'est jamais utilisé dans les documents | ✅ |
| Téléphones des parents | Mixtes Android / iPhone, Android majoritaire | ✅ (décision Espace Parent) |
| Collège | Ouvert cette année : 1AC-G1, 1AC-G2, 2AC (seule classe de 2ᵉ année avec des élèves) | ✅ |
| Nombre de familles du collège | — | 🔍 requête § 2 |
| Familles avec plusieurs enfants (collège + primaire) | — | 🔍 requête § 2 ; décide si le compte montre aussi les enfants du primaire (ADR 0004) |
| Qui écrit les devoirs aujourd'hui, et où | Pas encore décidé | 🔍 ADR 0007 |

---

## 2. Smart School (familles, élèves, paiements)

Hébergé sur le **compte Hostinger Business** de `groupelavictoire.com`, comme Planning et Planning Évaluations ✅. Base lue **en local**, en lecture seule.

### Paiements ✅

Même logique que le portail `livre-des-comptes-la-victoire` et qu'Ecolepay (`ecolepay/docs/01-recherche.md` § 2) :

- **Scolarité** : un mois est payé s'il existe une ligne `student_fees_deposite` pour le couple (dossier de l'élève, mois). Chaîne : `student_session` → `student_fees_master` → `fee_groups_feetype` → `student_fees_deposite`. Groupes dont le nom contient « colarit ».
- **Transport** : `route_pickup_point` (zone, tarif) → `transport_feemaster` (mois) → `student_transport_fees` → `student_fees_deposite.student_transport_fee_id`. Mois de septembre à juin.
- **Remises** : un mois enregistré comme payé est réglé, même si le montant est inférieur au tarif (ADR 0003 d'Ecolepay).
- Seuls les élèves `students.is_active = 'yes'`.

### Familles et comptes parents 🔍

Structure de la **version standard** de Smart School, à confirmer :

| Donnée | Champ standard |
|---|---|
| Parent de garde | `students.guardian_is` (`father`, `mother`, `other`), `guardian_name`, `guardian_phone` |
| Compte parent Smart School (fratrie) | `students.parent_id` → `users.id` (rôle `parent`) : les frères et sœurs partagent le même `parent_id` |
| Classe et section | `student_session` → `classes`, `sections` |

**Requête de vérification** (lecture seule, sur la base Smart School) :

```sql
-- 1. Colonnes réellement présentes
SHOW COLUMNS FROM students
WHERE Field IN ('parent_id','guardian_is','guardian_name','guardian_phone','is_active');

-- 2. Nom exact des classes et sections du collège
SELECT c.id, c.class, s.id AS section_id, s.section
FROM classes c JOIN class_sections cs ON cs.class_id = c.id JOIN sections s ON s.id = cs.section_id
ORDER BY c.class, s.section;

-- 3. Familles du collège et fratries (y compris hors collège)
SELECT st.parent_id, COUNT(*) AS enfants, GROUP_CONCAT(DISTINCT c.class) AS classes
FROM students st
JOIN student_session ss ON ss.student_id = st.id
JOIN classes c ON c.id = ss.class_id
WHERE st.is_active = 'yes' AND st.parent_id IS NOT NULL AND st.parent_id <> 0
GROUP BY st.parent_id
HAVING SUM(c.class LIKE '%AC%' OR c.class LIKE '%APIC%' OR c.class LIKE '%oll%') > 0;
```

*(Filtre du collège dans la requête 3 : à ajuster au vrai nom des classes trouvé en 2.)*

Coller les résultats ici, puis mettre à jour `03-architecture.md` et `donnees-initiales.md`.

**Session scolaire :** filtrer sur la session en cours (`student_session.session_id`), à confirmer avec la table `sessions` 🔍.

---

## 3. Planning (emplois du temps) ✅

Vérifié en SSH le 26/09/2026 pendant le cadrage de Planning Évaluations (`planning-evaluations/docs/01-recherche.md` § 3) :

- Même compte Hostinger ; base MySQL **locale** ; lue en lecture seule (`SET SESSION TRANSACTION READ ONLY`, un seul utilisateur MySQL possible en mutualisé).
- Tables lues : `classes`, `subjects`, `teachers`, `requirements` (affectations), `assignments` (séances par jour, créneau `p1`…`p6`, demi-créneau), `schedule_presets` (grille horaire du cycle `college`).
- Identifiants des classes du collège : `COLG1` (1AC-G1), `COLG2` (1AC-G2), `COL2EG1` (2AC).
- Le code d'accès est déjà écrit : `App\Domain\Timetable\PlanningDatabaseSource` dans Planning Évaluations. **ParentsApp le reprend tel quel** (même interface `TimetableSource`).

**Point à trancher 🔍 :** faut-il afficher le **nom du professeur** dans l'emploi du temps des parents ? Proposition : oui, le nom seul (jamais le téléphone). À valider (R3).

---

## 4. Planning Évaluations (contrôles) ✅

- En production : `https://evaluations.groupelavictoire.com`.
- Seule la **version publiée** sort du module (sa règle R17).
- Chaque classe a un **lien de partage à jeton**, avec un flux d'agenda iCal `/p/{jeton}.ics` (UID stable par contrôle, `SEQUENCE` = n° de version). C'est le **contrat P1** de son backlog : ParentsApp peut l'utiliser **sans aucun changement** dans Planning Évaluations.
- Un point de lecture JSON par classe (**contrat P2**) est prévu dans son backlog, s'il devient nécessaire ; il demande l'accord d'Amadou et un ADR **côté Planning Évaluations**.

**Décision :** ADR [`0006`](decisions/0006-controles-version-publiee.md).

---

## 5. Application mobile Smart School 🔍

L'école a une licence de l'application mobile Smart School, **non déployée**.

| À vérifier | Pourquoi |
|---|---|
| Ce qu'elle montre aux parents (paiements, absences, notes, devoirs ?) et si on peut masquer des modules | Si elle montre des données que l'école ne veut pas ouvrir (notes non validées, absences), c'est un risque |
| Langues (arabe RTL ?) | Parents majoritairement arabophones |
| Si elle affiche l'emploi du temps et les contrôles | Non : ces données sont dans Planning et Planning Évaluations, pas dans Smart School |

Conclusion provisoire : elle ne peut pas **rassembler** les sources de l'école (Planning, Planning Évaluations). C'est la raison d'être de ParentsApp (ADR 0001).

---

## 6. Devoirs à la maison 🔍

| Option | Pour | Contre |
|---|---|---|
| **Cahier Journal** (`cahier-journal-la-victoire`, déployé sur `journal.groupelavictoire.com`, React + Express + Prisma) | Les profs notent déjà la séance ; les devoirs y auraient leur place naturelle | **Pas encore utilisé par les profs** ; 🔍 existe-t-il un champ « devoirs » ? Quel accès en lecture (base, API) ? |
| **Saisie par le Secrétariat** dans ParentsApp | Aucune dépendance, disponible tout de suite | Charge de travail pour le Secrétariat ; dépend des profs qui transmettent |
| **Comptes professeurs** dans ParentsApp | À la source | Nouveaux comptes, nouveau modèle de sécurité, formation |

**Proposition :** ADR [`0007`](decisions/0007-source-des-devoirs.md) 🟡 — interface `HomeworkSource`, saisie Secrétariat pour le MVP, bascule vers le Cahier Journal quand les profs l'utiliseront.

---

## 7. PWA ✅

- Une PWA exige **HTTPS** et un **service worker** ; elle s'installe sur l'écran d'accueil.
- **Android (Chrome)** : proposition d'installation par le navigateur.
- **iPhone (Safari)** : installation manuelle (« Partager » → « Sur l'écran d'accueil ») — à expliquer sur la fiche d'accès.
- Notifications push : **hors MVP**. Sur iPhone, elles n'existent que pour une PWA installée sur l'écran d'accueil ; à étudier avant de les promettre (backlog).
- Décision déjà prise par Amadou : **PWA d'abord** (zéro coût, aucun store) ; Android prioritaire si une application native devient nécessaire ; Apple écarté pour l'instant.

---

## 8. Données personnelles 🔍

- ParentsApp **ouvre à des parents**, via Internet, des données sur des **mineurs** (nom, classe) et sur la **situation de paiement** des familles. C'est plus sensible qu'un outil interne.
- **Loi 09-08** et **CNDP** au Maroc : 🔍 vérifier si l'école doit déclarer ce traitement ou compléter sa déclaration **avant l'ouverture aux parents**. Bloquant pour la mise en production (R25).
- Minimisation : identité uniquement (R24), pas de copie des montants (R13), paiements hors du cache du téléphone (R21), journal des connexions conservé 12 mois (R22).
- Mention d'information à prévoir sur la fiche d'accès et dans l'application (qui traite, quelles données, à qui s'adresser) — texte à valider.

---

## 9. Décisions issues de la recherche

| Décision | Fiche | Statut |
|---|---|---|
| Application séparée, PWA en Blade + Livewire | [`0001`](decisions/0001-application-separee-pwa.md) | 🟡 technique du front à valider |
| Sources en lecture seule, derrière des connecteurs | [`0002`](decisions/0002-sources-lecture-seule.md) | ✅ |
| Connexion : identifiant + code remis par l'école | [`0003`](decisions/0003-connexion-code-remis.md) | ✅ |
| Un compte par famille (parent de garde) | [`0004`](decisions/0004-un-compte-par-famille.md) | 🟡 |
| Paiements : affichage seul, même calcul que le portail | [`0005`](decisions/0005-paiements-affichage-seul.md) | 🟡 |
| Contrôles : version publiée de Planning Évaluations | [`0006`](decisions/0006-controles-version-publiee.md) | ✅ |
| Source des devoirs | [`0007`](decisions/0007-source-des-devoirs.md) | 🟡 |
| Arabe, français, anglais | [`0008`](decisions/0008-trois-langues.md) | ✅ |
| Hébergement sur le même compte | [`0009`](decisions/0009-hebergement.md) | 🟡 |
| Pilote : le collège | [`0010`](decisions/0010-pilote-college.md) | ✅ |
