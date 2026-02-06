# Context Map — Plateforme de Billetterie

## Schéma général

```
[ContexteEvenement]
        |
        | (Published Language)
        v
[ContexteReservation] ---> (Customer / Supplier) ---> [ContexteCommandePaiement]
        |
        | (ACL)
        v
[ContexteBilletterie]
        |
        | (Published Language)
        v
[ContexteNotification]
```

---

## 2. Relations et patterns

| Contexte source     | Contexte cible           | Pattern de relation  | Justification métier                                                                                                                                                                                                                                  |
|---------------------|--------------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContexteEvenement   | ContexteReservation      | Published Language   | Le contexte Réservation consomme les informations d’événement (capacités, catégories, politiques d’annulation) via un langage standardisé. Les structures échangées sont documentées et stables, évitant un couplage fort entre les modèles internes. |
| ContexteReservation | ContexteCommandePaiement | Customer / Supplier  | Le contexte Réservation dépend du paiement pour finaliser une vente. Il agit comme client et impose ses besoins métier (validation, refus). Le contexte Paiement fournit un service nécessaire mais non différenciant.                                |
| ContexteReservation | ContexteBilletterie      | Anticorruption Layer | Le contexte Billetterie protège son modèle interne de billet en traduisant les données issues des commandes. Cette couche empêche la propagation des concepts internes de réservation ou de paiement dans la logique de billetterie.                  |
| ContexteBilletterie | ContexteNotification     | Published Language   | Les notifications sont déclenchées à partir d’événements métiers standardisés (BilletGénéré, CommandeConfirmée). Le modèle est simple et partagé sans logique métier complexe.                                                                        |

---

## 3. Intégrations techniques envisagées

| Type d’intégration                    | Contextes impliqués                            | Cas d’usage                                                                                       |
|---------------------------------------|------------------------------------------------|---------------------------------------------------------------------------------------------------|
| API REST                              | ContexteReservation → ContexteCommandePaiement | Création et suivi d’une transaction de paiement pour une commande issue d’une réservation valide. |
| Événements métier (publish/subscribe) | ContexteReservation → ContexteBilletterie      | Déclenchement de la génération des billets après confirmation de commande.                        |
| Événements métier                     | ContexteBilletterie → ContexteNotification     | Envoi automatique d’emails contenant les billets électroniques au client.                         |

---
