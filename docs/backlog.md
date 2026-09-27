# Backlog

Tout ce qui n'entre pas dans le périmètre du MVP (`00-vision.md`) atterrit ici. On y revient **après la mesure du pilote**.

Maturité : 🟢 prêt à cadrer · 🟡 à préciser avec Amadou · ⚪ simple idée.

## Après le pilote

| Idée | Origine | Note | Maturité |
|---|---|---|---|
| **Notifications** (nouvelle annonce, contrôle modifié, devoir ajouté) | 2026-09-27 | Push web : sur iPhone, seulement si l'application est installée sur l'écran d'accueil ; demande un service d'envoi et peut-être le VPS (ADR 0009). WhatsApp : API officielle payante uniquement (API non officielles interdites) | 🟡 |
| **Extension au primaire, puis au préscolaire** | 2026-09-27 | Emplois du temps et contrôles du primaire à brancher ; décide aussi l'affichage des fratries (ADR 0004) | 🟡 |
| **Absences** de l'enfant (lues dans Smart School) | 2026-09-27 | Données sensibles : à décider avec la direction | ⚪ |
| **Notes et bulletins** | 2026-09-27 | Recoupe l'idée de saisie des notes calquée sur Massar | ⚪ |
| **Documents** : certificat de scolarité, autorisation de sortie à signer | 2026-09-27 | Relève d'Ecoledoc ; Ecoleparents n'en serait que la vitrine | ⚪ |
| **Messagerie** parent ↔ école | 2026-09-27 | Charge de réponse pour le secrétariat ; à bien encadrer | ⚪ |
| **Réinitialisation du code en ligne** (code à usage unique par WhatsApp officiel) | 2026-09-27 | Évite le passage au secrétariat ; coût par message | ⚪ |
| **Comptes professeurs** (saisie des devoirs à la source) | 2026-09-27 | Écarté du MVP (ADR 0007) ; à comparer avec le Cahier Journal | ⚪ |
| **Transport** : zone, horaire du bus, suivi par le chauffeur | 2026-09-27 | Lien avec l'outil mobile du chauffeur noté dans le backlog d'Ecolepay | ⚪ |
| **Paiement en ligne**, reçus téléchargeables | 2026-09-27 | Exclu du MVP (R15) ; demande un prestataire de paiement | ⚪ |
| **Application Android native** | 2026-09-27 | Seulement si la PWA ne suffit pas (licence Google Play ~25 $ à vie) | ⚪ |
| **Front React** | 2026-09-27 | Si Blade + Livewire ne suffit pas (ADR 0001) | ⚪ |

## Technique, commun à plusieurs projets

| Idée | Note | Maturité |
|---|---|---|
| **Paquet commun du connecteur Smart School** (Ecolepay, Ecoleparents, portail des rapports…) | Porte unique validée par Amadou ; aujourd'hui le code est repris d'Ecolepay | 🟡 |
| **Paquet commun du connecteur Planning** (Planning Évaluations, Ecoleparents) | Même logique | 🟡 |
| **Contrat P2 de Planning Évaluations** (JSON par classe, calendrier officiel inclus) | À demander **dans** Planning Évaluations, avec un ADR là-bas | 🟡 |
| Intégration dans l'application unifiée | Métier déjà isolé dans `App\Domain\…` | ⚪ |
