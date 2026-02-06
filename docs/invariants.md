# Agrégats et invariants

## Agrégat 1 : Reservation

**Racine de l’agrégat**  
Reservation

**Éléments internes à l’agrégat**
- Reservation (entité racine)
- StatutReservation (objet valeur)
- DureeReservation (objet valeur)

### Invariants

| Invariant                                            | Description métier                                                                                                                                                            | Conséquence si non respecté                                                |
|------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| Une réservation expirée ne peut pas être confirmée   | Une réservation dont la date d’expiration est dépassée doit rester dans l’état *Expirée*. Toute tentative de confirmation doit être refusée pour éviter une vente illégitime. | Risque de vendre des places qui ne sont plus réservables ou déjà libérées. |
| La quantité réservée doit être strictement positive  | Une réservation doit contenir au moins une place. Une réservation vide n’a pas de sens métier.                                                                                | Création de réservations incohérentes et instables dans le système.        |
| Une réservation confirmée ne peut plus être modifiée | Une fois qu’une réservation devient confirmée (passage en commande), ses données (quantité, catégorie) ne doivent plus changer.                                               | Incohérence entre réservation, commande et billets générés.                |

---

## Agrégat 2 : Commande

**Racine de l’agrégat**  
Commande

**Éléments internes à l’agrégat**
- Commande (entité racine)
- StatutCommande (objet valeur)

### Invariants

| Invariant                                                      | Description métier                                                                                                                                      | Conséquence si non respecté                     |
|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| Une commande confirmée doit être associée à un paiement validé | Le passage à l’état *Confirmée* n’est possible que si un paiement est validé. Cela garantit que les billets ne sont générés que pour des ventes payées. | Génération de billets sans paiement réel.       |
| Une commande annulée ne peut plus être confirmée               | Une commande annulée est un état final. Elle ne peut plus revenir à un état actif.                                                                      | Cycle de vie incohérent et erreurs financières. |
| Le montant total doit correspondre à la quantité et au tarif   | Le montant total de la commande doit refléter exactement le nombre de places et leur prix.                                                              | Erreurs de facturation ou pertes financières.   |

---

## Schéma UML des agrégats (conceptuel)

```
┌───────────────────────────────────────────────────────┐
│                AGRÉGAT : Reservation                  │
│                                                       │
│  +-----------------------------------------------+    │
│  |            Reservation  <<Aggregate Root>>    |    │
│  +-----------------------------------------------+    │
│  | ReservationId                                 |    │
│  | Quantite                                      |    │
│  | DateExpiration                                |    │
│  +-----------------------------------------------+    │
│          |                         |                  │
│          |                         |                  │
│          v                         v                  │
│  +---------------------+     +---------------------+  │
│  |  StatutReservation  |     |  DureeReservation   |  │
│  |   <<Value Object>>  |     |   <<Value Object>>  |  │
│  +---------------------+     +---------------------+  │
│                                                       │
└───────────────────────────────────────────────────────┘
                          | 0..1
                          |
                          v 1
┌───────────────────────────────────────────────────────┐
│                 AGRÉGAT : Commande                    │
│                                                       │
│  +-----------------------------------------------+    │
│  |         Commande  <<Aggregate Root>>          |    │
│  +-----------------------------------------------+    │
│  | CommandeId                                    |    │
│  | MontantTotal                                  |    │
│  +-----------------------------------------------+    │
│                         |                             │
│                         v                             │
│              +---------------------+                  │
│              |  StatutCommande     |                  │
│              |   <<Value Object>>  |                  │
│              +---------------------+                  │
│                                                       │
└───────────────────────────────────────────────────────┘

```