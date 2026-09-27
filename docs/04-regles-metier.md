# 04 — Règles métier

Règles **non négociables**. Chacune est testable (`07-tests.md`). Les modifier demande l'accord explicite d'Amadou et une mise à jour de ce fichier.

## Sources et données

| # | Règle |
|---|---|
| **R1** | **Aucune écriture dans une source** (Smart School, Planning, Planning Évaluations, source externe des devoirs). Double protection : utilisateur MySQL en lecture seule quand l'hébergement le permet, et **toujours** `SET SESSION TRANSACTION READ ONLY` sur les connexions `smartschool` et `planning`. Un test prouve qu'une écriture échoue. |
| **R10** | Un élève **inactif** dans Smart School ou sorti du périmètre **disparaît** du compte à la synchronisation suivante. Une famille sans aucun enfant actif est **suspendue** automatiquement. Rien n'est supprimé : l'historique reste. |
| **R11** | L'**emploi du temps** est celui de la classe dans Planning. Il n'est **jamais saisi** dans Ecoleparents. La date de la dernière synchronisation est affichée. |
| **R12** | Les **contrôles** affichés sont **uniquement ceux de la version publiée** de Planning Évaluations, avec le n° et la date de cette version. Jamais de brouillon. |
| **R13** | Les **paiements** suivent le **même calcul** que le portail des rapports et Ecolepay : un mois enregistré comme payé dans Smart School est **réglé**, remise comprise. **Aucun montant n'est stocké** dans Ecoleparents : lecture au moment de l'affichage (cache de 5 minutes au plus). |
| **R18** | Les **devoirs** viennent de la source décidée (ADR 0007). Ils sont **par classe**, jamais personnels à un élève. Chacun a une date donnée et une date à rendre. |
| **R24** | **Minimisation** : la copie locale d'un élève se limite à son identité (prénom, nom, classe, statut). Pas de date de naissance, d'adresse, de numéro d'identité, de note ni d'absence. |

## Cloisonnement et accès

| # | Règle |
|---|---|
| **R2** | Un parent ne voit **que les enfants rattachés à son compte**. Chaque accès est vérifié **côté serveur** ; un identifiant d'enfant reçu du navigateur n'est jamais cru. Accès à un autre enfant → **404** (on ne confirme pas son existence). |
| **R3** | **Aucune donnée d'un autre élève** n'apparaît dans l'espace parents : pas de liste de classe, pas de nom de camarade, pas de téléphone de professeur. |
| **R4** | Connexion par **identifiant + code remis par l'école**. Code aléatoire de 8 caractères sans caractère ambigu, **stocké haché**, **affiché une seule fois** (sur la fiche d'accès). Jamais journalisé, jamais réaffiché. |
| **R5** | **Régénérer un code** révoque l'ancien **et déconnecte tous les appareils** du compte. Motif obligatoire, journalisé. |
| **R6** | **5 échecs** de connexion pour un identifiant → blocage de **15 minutes**. Plafond aussi par adresse IP. Message d'erreur neutre, qui ne révèle pas si l'identifiant existe. |
| **R7** | Un appareil reste connecté **90 jours** (réglable). La famille peut se déconnecter à tout moment ; la Direction et le Secrétariat peuvent révoquer tous les appareils d'un compte. |
| **R8** | **Un compte par famille**, au nom du **parent de garde**. Les enfants sont proposés depuis Smart School, puis **confirmés par le Secrétariat** avant toute génération de code. |
| **R9** | La **remise** de la fiche d'accès est enregistrée (date, remise à qui, par qui). Le code n'est **jamais envoyé par WhatsApp, SMS ou e-mail** dans le MVP : remise en main propre. |
| **R23** | Les **permissions** sont stockées en base (Filament Shield) et vérifiées côté serveur. Aucun rôle codé en dur. La garde `parent` ne donne jamais accès à l'administration. |

## Ton et présentation

| # | Règle |
|---|---|
| **R14** | **Paiements, ton respectueux** : « Réglé » / « À régler ». Jamais « impayé », « retard », « dette », « dû » ; pas de rouge d'alerte, pas de compte à rebours, pas de relance dans l'espace parents (les rappels relèvent d'Ecolepay). |
| **R15** | **Aucun paiement en ligne**, aucun reçu généré. Le reçu qui fait foi est celui de Smart School, remis à l'accueil. |
| **R16** | Une **annonce** a obligatoirement une version **arabe** ; français et anglais sont facultatifs. Le parent la lit dans sa langue, sinon en arabe. Ciblage : tout le périmètre ou une ou plusieurs classes. Seule une personne qui a la permission `PublishAnnouncement` publie ou retire. |
| **R17** | Une annonce **modifiée après publication** porte la mention visible « modifiée le … ». Toute modification est journalisée. Une annonce retirée disparaît de l'espace parents. |
| **R19** | **Trois langues** : arabe par défaut, en RTL complet ; français ; anglais. Le choix est mémorisé par compte. Chiffres occidentaux (0-9) ; dates et horaires toujours lisibles de gauche à droite. |
| **R20** | Chaque bloc d'information affiche sa **fraîcheur** (« mis à jour le … »). Hors ligne, la dernière version est montrée **comme telle**, jamais comme si elle était à jour. |
| **R21** | Les **paiements ne sont jamais mis en cache** sur le téléphone (service worker, cache HTTP) : `Cache-Control: no-store`. Hors ligne, la page Paiements invite à se reconnecter. |

## Suivi, sécurité, conformité

| # | Règle |
|---|---|
| **R22** | Chaque **connexion** (réussie ou non) est journalisée : date, appareil résumé, empreinte de l'IP. Visible par la Direction. **Conservation : 12 mois**, puis purge automatique. |
| **R25** | **Aucune ouverture aux parents** (mise en production réelle) tant que la question **CNDP / loi 09-08** n'est pas tranchée et que le texte d'information des familles n'est pas validé. Case bloquante de la liste de mise en production. |
| **R26** | Toute action de l'administration est **journalisée** (activitylog) avec son auteur : familles vérifiées, codes générés et régénérés, remises, suspensions, annonces, devoirs, correspondances, paramètres. |

*Numérotation : les règles sont regroupées par thème, les numéros restent fixes (R1 à R26) pour être cités partout.*
