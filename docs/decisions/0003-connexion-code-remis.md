# ADR 0003 — Connexion par identifiant + code remis par l'école

**Date :** 2026-09-27 · **Statut :** ✅ Acceptée (choix d'Amadou)

## Contexte

L'espace parents montre des données sensibles (situation de paiement, identité de mineurs). Il faut un accès simple pour des familles parfois peu à l'aise avec le numérique, sans coût d'envoi.

## Options

| Option | Pour | Contre |
|---|---|---|
| **Code remis par l'école** (fiche imprimée) | Coût nul ; remise en main propre = on sait qui reçoit le code | Fiche perdue ou photographiée ; passage au secrétariat en cas de perte |
| Code à usage unique par WhatsApp / SMS | Pratique | Service d'envoi payant ou API non officielle (interdite : risque de bannissement, cf. Ecolepay) ; numéros parfois faux |
| Lien personnel sans compte (comme les professeurs) | Très simple | Un lien transféré expose les paiements |

## Décision

**Identifiant + code remis par l'école**, sur une fiche d'accès imprimée, remise en main propre (R4 à R9). L'appareil reste connecté 90 jours.

## Conséquences

- Écran A2 (fiches, remises, régénération) et journal des connexions dans le MVP.
- Code perdu = passage au secrétariat (pas de réinitialisation en ligne dans le MVP).
- Un code à usage unique par WhatsApp officiel pourra être étudié plus tard (backlog).
