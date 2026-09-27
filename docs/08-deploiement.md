# 08 — Déploiement

**Proposition :** ADR [`0009`](decisions/0009-hebergement.md) 🟡 — même compte Hostinger Business (mutualisé) que Smart School, Planning et Planning Évaluations. Sous-domaine : **{{SOUS_DOMAINE}}** (proposé : `parents.groupelavictoire.com`, à choisir par Amadou).

## Architecture

```
Compte Hostinger Business (mutualisé, u707543112)
├── Smart School (groupelavictoire.com)              → base Smart School
├── /rapports                                        → portail livre-des-comptes
├── planning.groupelavictoire.com                    → Planning (sa base MySQL)
├── evaluations.groupelavictoire.com                 → Planning Évaluations
└── {{SOUS_DOMAINE}}                                 → Ecoleparents (Laravel)
                                                        ├── base Ecoleparents (nouvelle)
                                                        ├── lecture locale Smart School (lecture seule)
                                                        ├── lecture locale Planning (lecture seule)
                                                        └── flux iCal publiés de Planning Évaluations (HTTPS)
```

Tout est **local** au même compte : aucune base MySQL exposée sur Internet.

## Vérifications préalables

| Point | Pourquoi |
|---|---|
| PHP 8.3 ou plus, Composer, SSH | Laravel 13 (déjà vérifiés pour Planning Évaluations ✅) |
| Sous-domaine libre, aucun ancien dossier ni enregistrement DNS | Éviter les restes d'anciens projets |
| **HTTPS actif** | **Obligatoire pour une PWA** (service worker) |
| Nouvelle base MySQL | Données propres ; vérifier le nombre de bases autorisé par le forfait |
| Utilisateur MySQL en lecture seule sur Smart School et Planning | R1 ; en mutualisé c'est souvent impossible (constaté pour Planning) → protection par `SET SESSION TRANSACTION READ ONLY`, testée en production |
| Tâche Cron hPanel toutes les minutes | Synchronisations, purge du journal des connexions, sauvegardes |
| **Question CNDP tranchée, texte d'information validé** | R25 — bloquant avant l'ouverture aux familles |

## Mise en ligne

Même procédure que Planning Évaluations (`planning-evaluations/docs/08-deploiement.md`, « Installation réelle ») :

1. Créer le sous-domaine, activer HTTPS.
2. Créer la base et son utilisateur (mot de passe uniquement dans `.env`).
3. Code dans `~/domains/groupelavictoire.com/apps/ecoleparents` (hors `public_html`) ; racine web = lien symbolique vers `public/`.
4. Clé de déploiement GitHub **en lecture seule**.
5. `composer install --no-dev --optimize-autoloader`.
6. `npm run build` **en local** (pas de Node.js sur le serveur), puis envoi de `public/build/`.
7. `.env` : `APP_ENV=production`, `APP_DEBUG=false`, `APP_LOCALE=fr` (administration), `APP_TIMEZONE=Africa/Casablanca`, connexions `smartschool` et `planning`.
8. `php artisan migrate --force` (base Ecoleparents uniquement), `php artisan filament:assets`, `php artisan optimize`.
9. Tâche Cron hPanel : `/usr/bin/php …/apps/ecoleparents/artisan schedule:run` chaque minute.
10. Première synchronisation Smart School et Planning ; correspondance des classes (A5).
11. Comptes Direction et Secrétariat (depuis l'écran A7).

## Sauvegardes

- Sauvegarde quotidienne de la base (script `mysqldump` comme Planning Évaluations), 30 jours, copie régulière hors du serveur.
- **`APP_KEY` sauvegardée à part** : elle chiffre les jetons des flux de Planning Évaluations.
- Restauration testée avant l'ouverture aux familles.
- Les codes des familles sont **hachés** : une sauvegarde ne permet pas de les retrouver (c'est voulu). En cas de perte de la base, les codes sont régénérés et les fiches réimprimées.

## Ouverture aux familles

1. Recette complète (`07-tests.md`), revue de cloisonnement faite.
2. Toutes les familles du collège vérifiées (A2).
3. Fiches d'accès imprimées, testées en noir et blanc.
4. Remise en main propre (réunion de parents ou accueil), remise notée pour chaque famille.
5. Pendant les deux premières semaines : le Secrétariat aide les familles qui n'arrivent pas à installer l'application.

## Checklist finale

- [ ] Hébergement tranché (ADR 0009) et sous-domaine choisi.
- [ ] HTTPS actif, `APP_DEBUG=false`.
- [ ] Écriture refusée sur `smartschool` et `planning` (R1, test exécuté sur le serveur).
- [ ] Pages parents `noindex` ; paiements en `no-store` (R21) vérifiés dans le navigateur.
- [ ] Tâche planifiée active, synchronisations réussies, purge du journal programmée.
- [ ] Sauvegarde active, restauration testée, `APP_KEY` sauvegardée.
- [ ] Test de référence des paiements réussi sur les données réelles.
- [ ] Revue de cloisonnement faite et consignée.
- [ ] **CNDP / loi 09-08 tranchée, texte d'information validé (R25).**
- [ ] Comptes Direction et Secrétariat créés.
