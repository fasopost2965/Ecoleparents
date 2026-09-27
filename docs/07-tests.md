# 07 — Tests

**Principe :** chaque règle de `04-regles-metier.md` a au moins un test Pest. Les tests utilisent **`DatabaseTransactions`**, jamais `RefreshDatabase` sur une base partagée (leçon de Planning Évaluations).

## Tests automatiques (Pest), une ligne par règle

| Règle | Test |
|---|---|
| R1 | Une requête INSERT sur `smartschool` et sur `planning` échoue ; aucun appel HTTP d'écriture vers Planning Évaluations n'existe dans le code (analyse du code) |
| R2 | **Pour chaque écran parent (P2 à P7)** : famille A connectée, identifiant d'un enfant de la famille B dans l'URL ou la requête Livewire → 404 ; même test avec un identifiant inexistant → 404 identique |
| R3 | Le rendu de chaque écran parent ne contient aucun nom d'élève autre que ceux du compte (jeu de données avec deux familles de la même classe) ; aucun numéro de téléphone |
| R4 | Après génération, `code_hash` ≠ code ; le code n'apparaît dans aucune table, aucun fichier de journal, aucune réponse sauf le PDF ; alphabet sans `0 O 1 l I` |
| R5 | Régénération → l'ancien code échoue ; tous les `parent_devices` du compte sont révoqués ; motif absent → refus |
| R6 | 5 échecs → 6ᵉ tentative bloquée même avec le bon code ; débloquée après 15 min ; message identique pour identifiant inconnu et code faux |
| R7 | Appareil mémorisé valable 90 jours, refusé au 91ᵉ ; « Se déconnecter » révoque l'appareil courant seulement |
| R8 | Impossible de générer un code pour une famille non vérifiée ; rattachement confirmé tracé |
| R9 | Remise sans date ou sans destinataire refusée ; aucune fonction d'envoi du code (WhatsApp, SMS, e-mail) n'existe |
| R10 | Élève passé inactif → absent du compte après synchronisation, historique intact ; famille sans enfant actif → `suspended`, connexion refusée |
| R11 | L'emploi du temps affiché = séances de la classe dans la copie Planning ; aucune ressource Filament ne permet de créer une séance |
| R12 | Flux publié version 3 → affichée « Version n° 3 » ; aucun chemin de code ne lit un brouillon |
| R13 | **Test de référence** (ci-dessous) ; aucune colonne de montant dû dans le schéma |
| R14 | Les mots interdits (« impayé », « retard », « dette », « dû » et leurs équivalents arabes et anglais, liste validée par Amadou) n'apparaissent dans aucun fichier `lang/` ni dans le rendu de P2 et P6 |
| R15 | Aucune route de paiement, aucune génération de reçu |
| R16 | Annonce sans titre ou texte arabe → publication refusée ; parent `fr` sur une annonce sans français → version arabe ; annonce ciblée 1AC-G1 invisible pour 2AC ; Secrétariat → publication refusée (permission) |
| R17 | Modification après publication → mention « modifiée le » + entrée activitylog ; annonce retirée invisible |
| R18 | Devoir d'une classe visible des seules familles de cette classe ; aucun champ élève dans le devoir |
| R19 | `?lang=ar` → `dir="rtl"` ; langue mémorisée sur le compte ; dates en chiffres occidentaux |
| R20 | Chaque bloc de P2 à P7 porte sa date de mise à jour |
| R21 | Réponse de P6 : en-tête `Cache-Control: no-store` ; le service worker exclut la route des paiements (test sur sa liste de routes) |
| R22 | Connexion réussie et échouée journalisées ; IP stockée sous forme d'empreinte ; purge des lignes de plus de 12 mois |
| R23 | Secrétariat → 403 sur Paramètres, Correspondances, Utilisateurs, même par l'URL ; un compte parent n'ouvre pas `/admin` |
| R24 | La table `students` n'a que les colonnes d'identité (test de schéma) |
| R25 | (Manuel) case de la liste de mise en production |
| R26 | Chaque action d'administration listée crée une entrée activitylog avec son auteur |

## Test de référence : paiements

Sur une **copie de la base Smart School** (ou en lecture sur la base réelle, R1) :

- pour **chaque élève actif du collège**, la liste des mois réglés / à régler (scolarité et transport) donnée par ParentsApp est **identique** à celle du portail `livre-des-comptes-la-victoire` et à celle d'Ecolepay, à la même date ;
- le total « reste à régler » est identique ;
- un élève avec remise (mois enregistré à 225 ou 200 DH au lieu de 250 DH) apparaît **réglé** pour ce mois.

## Test de référence : emploi du temps

Pour 1AC-G1, 1AC-G2 et 2AC : les séances affichées sont celles de l'application Planning, vérifiées sur **une journée complète par classe**, à la main.

## Revue de cloisonnement (avant la mise en ligne)

Revue dédiée, **par une deuxième IA ou une deuxième personne**, de toutes les routes et composants Livewire de l'espace parents : chaque accès à un élève passe-t-il par `ParentView` ? Résultat écrit dans `continuite.md`.

## Recette manuelle (phase 10)

| # | Essai | Attendu |
|---|---|---|
| 1 | Scanner le QR d'une fiche d'accès sur un Android | Page de connexion en arabe |
| 2 | Se connecter, installer sur l'écran d'accueil (Android) | Icône de l'école, ouverture plein écran |
| 3 | Même chose sur un iPhone (Safari) | Idem, en suivant les instructions de la fiche |
| 4 | Famille à deux enfants | Sélecteur d'enfant ; chaque écran change avec l'enfant |
| 5 | Mode avion | Dernières informations datées ; Paiements invite à se reconnecter |
| 6 | Changer une date publiée dans Planning Évaluations | Visible dans ParentsApp en moins de 15 min |
| 7 | Régénérer le code d'une famille | Son téléphone est déconnecté |
| 8 | Publier une annonce pour 1AC-G1 | Visible par une famille de 1AC-G1, pas par une famille de 2AC |
| 9 | Lire P6 en arabe, français, anglais | Aucun mot interdit, ton validé |
| 10 | Imprimer une fiche d'accès en noir et blanc | Tout est lisible, code sans ambiguïté |
| 11 | 6 tentatives avec un mauvais code | Blocage de 15 min, message neutre |
| 12 | Élève désactivé dans Smart School | Disparaît du compte après la synchronisation |
