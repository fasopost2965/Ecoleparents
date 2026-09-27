# 03 — Architecture

**Statut :** proposition, à relire par une deuxième IA avant validation.

## 1. Stack

Voir [`decisions/0001-application-separee-pwa.md`](decisions/0001-application-separee-pwa.md).

| Brique | Choix |
|---|---|
| Framework | Laravel 13 (PHP 8.3+) |
| Administration | Filament 5 (garde `web`) |
| Espace parents | Blade + Livewire (fournis avec Filament), garde **`parent`** séparée ; Tailwind avec les jetons du thème |
| PWA | `manifest.webmanifest` + service worker écrits à la main (pas de paquet) |
| Base Ecoleparents | MySQL, connexion `mysql` |
| Smart School | MySQL, connexion `smartschool`, lecture seule |
| Planning | MySQL, connexion `planning`, lecture seule |
| Planning Évaluations | HTTPS, flux iCal par classe (contrat P1) ; lecture du flux avec le client HTTP de Laravel et le lecteur iCal à choisir 🔍 (question à Amadou avant la phase 5) |
| Rôles | spatie/laravel-permission + Filament Shield |
| Journal technique | spatie/laravel-activitylog |
| PDF (fiches d'accès) | mpdf/mpdf (arabe RTL, déjà validé dans Planning Évaluations) |
| QR code | 🔍 bibliothèque à valider (question à Amadou) — celle de Planning Évaluations si elle existe |
| Tests | Pest |

---

## 2. Vue d'ensemble

```
                         ┌──────────────────── Ecoleparents (Laravel) ────────────────────┐
 Téléphone du parent ──► │ Espace parents (PWA, garde `parent`)                         │
                         │        │                                                      │
 Direction/Secrétariat ─►│ Administration (Filament, garde `web`)                       │
                         │        │                                                      │
                         │        ▼                                                      │
                         │  Services métier (App\Domain\…)  ──►  Base Ecoleparents          │
                         │        │                              (comptes, annonces,    │
                         │        ▼                               devoirs, correspondances)│
                         │  Connecteurs (seuls points d'accès aux sources)               │
                         └──┬────────────────┬──────────────────┬───────────────────┬───┘
                            ▼                ▼                  ▼                   ▼
                  Smart School (LS)   Planning (LS)   Planning Évaluations   Source des devoirs
                  familles, élèves,   emplois du      (flux iCal publié      (ADR 0007 🟡)
                  paiements           temps           par classe)
                                         LS = lecture seule
```

---

## 3. Modules

| Module (espace de noms) | Rôle | Accède à |
|---|---|---|
| `App\Domain\SchoolData` | **Connecteur Smart School** : familles, élèves, classes, paiements | Connexion `smartschool` uniquement |
| `App\Domain\Timetable` | Connecteur Planning, **repris de Planning Évaluations** (même interface `TimetableSource`) + copie locale | Connexion `planning` |
| `App\Domain\Assessments` | Lecture du flux publié de Planning Évaluations | HTTPS |
| `App\Domain\Homework` | Interface `HomeworkSource` ; implémentation MVP selon l'ADR 0007 | Base Ecoleparents (MVP proposé) |
| `App\Domain\Families` | Comptes, rattachements, codes, fiches d'accès, sessions d'appareils | Base Ecoleparents + `SchoolData` |
| `App\Domain\Announcements` | Annonces, cibles, lectures | Base Ecoleparents |
| `App\Domain\ParentView` | **Seul point d'entrée de l'espace parents** : assemble les données d'un enfant **après** le contrôle de cloisonnement (R2) | Tous les modules ci-dessus |

**Règle :** aucun contrôleur ni composant Livewire de l'espace parents n'appelle directement un connecteur. Tout passe par `ParentView`, qui reçoit le **compte connecté** et un **identifiant d'enfant**, et renvoie 404 si l'enfant n'est pas rattaché au compte.

### Contrats des connecteurs (interfaces PHP)

```php
/** Smart School — même esprit que SchoolDataSource d'Ecolepay (ADR 0002). */
interface SchoolDataSource
{
    /** Familles candidates : un élément par parent de garde, avec ses enfants actifs du périmètre. */
    public function families(int $sessionId, array $classScope): Collection;   // FamilyDto[]

    /** Élèves actifs d'une famille Smart School (pour la resynchronisation). */
    public function studentsOfFamily(string $familyKey, int $sessionId): Collection; // StudentDto[]

    /** Situation mois par mois d'un élève : scolarité et transport, tarif, réglé ou non. */
    public function paymentStatus(int $studentId, int $sessionId): PaymentStatusDto;

    /** Session scolaire en cours. */
    public function currentSession(): SessionDto;
}

/** Planning — interface existante dans Planning Évaluations, reprise telle quelle. */
interface TimetableSource { /* classes(), subjects(), teachers(), assignments(), weeklySlots() */ }

/** Planning Évaluations — contrat de lecture P1. */
interface PublishedAssessmentsSource
{
    /** Contrôles publiés d'une classe, avec le n° et la date de version. */
    public function forClass(ClassMapping $mapping): PublishedAssessmentsDto;
}

/** Devoirs — ADR 0007. */
interface HomeworkSource
{
    public function forClass(int $localClassId, CarbonImmutable $from, CarbonImmutable $to): Collection; // HomeworkDto[]
}
```

**Connecteur Smart School unique :** Amadou a validé qu'une **seule porte** vers Smart School serve toutes ses applications. Tant que cette porte n'existe pas comme paquet partagé, Ecoleparents **reprend le code d'Ecolepay** (mêmes requêtes de paiement, mêmes tests de comparaison) dans `App\Domain\SchoolData`, sans l'adapter. Extraction en paquet commun : voir `backlog.md` 🔍.

---

## 4. Modèle de données (base Ecoleparents)

**Aucun montant n'est stocké. Aucune donnée d'élève au-delà de l'identité** (R13, R24).

| Table | Champs clés | Notes |
|---|---|---|
| `users`, `roles`, `permissions`… | — | Direction, Secrétariat (spatie/laravel-permission) |
| `families` | id, **smartschool_family_key** (unique : `parent_id` 🔍), guardian_name, status (`to_verify`, `verified`, `sheet_printed`, `handed_over`, `active`, `suspended`), verified_by, verified_at | Identité du parent de garde uniquement (pas de téléphone copié dans le MVP) |
| `family_students` | family_id, student_id | Rattachement confirmé par le Secrétariat |
| `students` | id, **smartschool_student_id** (unique), first_name, last_name, local_class_id, is_active, last_synced_at | **Identité uniquement** (R24) |
| `parent_accounts` | id, family_id (unique), **login** (unique, lisible, ex. `LV-4827`), **code_hash**, code_generated_at, code_generated_by, handed_over_at, handed_over_to, handed_over_by, locale (`ar`, `fr`, `en`), activated_at, last_login_at, status | Le code n'est **jamais** stocké en clair (R4) |
| `parent_devices` | id, parent_account_id, token_hash, user_agent_summary, created_at, last_seen_at, expires_at, revoked_at | Une ligne par appareil connecté ; révocation ciblée ou totale (R5, R7) |
| `parent_logins` | id, parent_account_id, succeeded, ip_hash, user_agent_summary, created_at | Journal des connexions, purgé après 12 mois (R22) |
| `local_classes` | id, name (`1AC-G1`…), smartschool_class_id, smartschool_section_id, planning_class_id, **evaluations_feed_token** (chiffré), is_in_scope | Correspondance des classes (A5) |
| `timetable_*` | Copie locale de Planning (même schéma que Planning Évaluations) | Rafraîchie par la synchronisation |
| `announcements` | id, title_ar, body_ar, title_fr, body_fr, title_en, body_en, status (`draft`, `published`, `withdrawn`), published_at, published_by, edited_after_publication_at, created_by | Arabe obligatoire (R16) |
| `announcement_targets` | announcement_id, local_class_id (null = tout le périmètre) | |
| `announcement_reads` | announcement_id, parent_account_id, read_at | Badge « Nouveau » |
| `homework` | id, local_class_id, subject_label, instructions, given_on, due_on, created_by | **Seulement si** l'ADR 0007 retient la saisie Secrétariat |
| `calendar_days` | date, nature (`school_day`, `holiday`, `vacation`…), label | 🔍 copie du calendrier officiel (import du CSV de Planning Évaluations) — ou lu via le contrat P2 plus tard |
| `settings` | key, value | Périmètre, durée des sessions, horaires d'accueil, textes fixes validés |

---

## 5. Connexion des familles (ADR 0003)

- **Identifiant** : court, lisible, sans ambiguïté (préfixe + 4 à 5 chiffres, ex. `LV-4827`).
- **Code** : 8 caractères aléatoires, sans caractères ambigus (pas de `0/O`, `1/l/I`), généré par `random_bytes`, **haché** (`Hash::make`) ; affiché une seule fois, sur la fiche PDF.
- **Garde `parent`** distincte de la garde `web` : un parent ne peut jamais ouvrir l'administration, et inversement.
- **Appareil mémorisé** : après la connexion, un jeton d'appareil aléatoire (cookie `HttpOnly`, `Secure`, `SameSite=Lax`, 90 jours) ; en base, seule son empreinte (`parent_devices.token_hash`).
- **Régénération** : nouveau code → `parent_devices.revoked_at` sur tous les appareils du compte (R5).
- **Limitation** : `RateLimiter` Laravel, 5 échecs par identifiant / 15 min, et un plafond par adresse IP (R6).

---

## 6. Calculs

| Donnée | Calcul |
|---|---|
| Familles proposées | Élèves actifs du périmètre regroupés par compte parent Smart School (`parent_id` 🔍) ; à défaut, par téléphone du parent de garde, **toujours confirmé par le Secrétariat** (ADR 0004) |
| Paiements | Identique au portail des rapports et à Ecolepay (`01-recherche.md` § 2), lu à l'affichage, cache court (5 min) par élève |
| Aujourd'hui / demain | Nature du jour (calendrier officiel) puis séances de la classe ce jour-là (copie Planning) |
| Contrôles | Flux publié de la classe, filtré sur la date du jour ; mis en cache jusqu'à la prochaine lecture (15 min) avec son n° de version |
| Activation (A1) | Comptes avec `activated_at` / familles du périmètre |

---

## 7. PWA et hors ligne

- Service worker : met en cache l'**interface** (CSS, JS, polices, icônes) et les **dernières réponses** des écrans Accueil, Emploi du temps, Contrôles, Devoirs, Annonces, avec leur date.
- **Jamais** les paiements (R21) : leurs réponses portent `Cache-Control: no-store` et sont exclues du service worker.
- Hors ligne : bandeau « Hors connexion — informations du … » (R20).
- À la déconnexion ou à la révocation : le cache du service worker est vidé.

---

## 8. Sécurité

- **Cloisonnement** (R2) : `ParentView` + une Policy par ressource ; tests d'accès croisé sur chaque écran.
- **Lecture seule des sources** (R1) : `SET SESSION TRANSACTION READ ONLY` sur `smartschool` et `planning` ; tests Pest qui prouvent qu'une écriture échoue.
- Jeton du flux Planning Évaluations **chiffré** en base (clé de l'application).
- Pages parents : HTTPS, `noindex`, `Cache-Control: private` (et `no-store` pour les paiements), protection CSRF.
- Journal technique (activitylog) sur les comptes, codes, remises, annonces, devoirs, paramètres (R26).
- Sauvegarde de `APP_KEY` : sans elle, les jetons chiffrés sont perdus (leçon de Planning Évaluations).

---

## 9. Compatibilité avec la future application unifiée

- Même stack, mêmes paquets, même thème qu'Ecolepay et Planning Évaluations.
- Métier dans `App\Domain\…`, déplaçable tel quel.
- Les connecteurs `SchoolDataSource` et `TimetableSource` sont **les mêmes** que dans les autres modules : dans l'application unifiée, ils deviendront des appels internes.

---

## 10. Risques techniques

| Risque | Parade |
|---|---|
| Un parent voit l'enfant d'un autre (bug d'identifiant) | `ParentView` unique, Policies, tests croisés sur chaque écran, revue de code dédiée avant la mise en ligne |
| Fratries mal rattachées dans Smart School | Vérification humaine obligatoire avant la remise du code (ADR 0004) |
| Codes partagés ou photographiés | Régénération en un clic, révocation des appareils, journal des connexions |
| Planning Évaluations indisponible | Dernière version en cache affichée avec sa date |
| Écart de chiffres avec le portail | Test de comparaison automatique sur les élèves du collège |
| Hébergement mutualisé : pas de tâches permanentes | Synchronisations par cron (`schedule:run`), aucune file d'attente permanente |
| Téléphone partagé dans la famille | Paiements hors cache, déconnexion en un clic dans Réglages |
