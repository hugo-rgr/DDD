# Modèle de domaine

## Entités

| Entité      | Description métier                                                                                                                                                                                                                                                                                                                      | Identifiant métier |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| Reservation | Représente un blocage temporaire de places pour un événement donné. Elle garantit la disponibilité pendant une durée limitée et peut évoluer vers une commande confirmée. Elle porte le cycle de vie principal du processus d’achat (création, expiration, confirmation). Elle est responsable de la cohérence des quantités réservées. | ReservationId      |
| Commande    | Représente l’engagement ferme d’achat issu d’une réservation valide. Elle centralise le montant total, le statut de la vente et le lien avec le paiement. Une commande évolue dans le temps (en attente, confirmée, annulée). Elle constitue la trace officielle de la transaction commerciale.                                         | CommandeId         |
| Billet      | Représente le droit d’accès unique à un événement. Il est généré uniquement après confirmation de commande. Chaque billet est traçable et associé à une place ou catégorie. Il possède un cycle de vie incluant génération et contrôle à l’entrée.                                                                                      | BilletId           |

---

## Objets Valeur

| Objet Valeur      | Description métier                                                                                                                                                                                 | Propriétés principales        |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| DureeReservation  | Représente la durée maximale de validité d’une réservation. Elle détermine le temps pendant lequel des places sont bloquées. Elle est immuable pour garantir la cohérence temporelle du processus. | ValeurTemps                   |
| StatutReservation | Représente l’état métier d’une réservation. Il décrit son cycle de vie et garantit qu’une réservation suit des transitions valides.                                                                | EnCours, Confirmée, Expirée   |
| StatutCommande    | Représente l’état métier d’une commande. Il permet de suivre l’évolution de la transaction commerciale.                                                                                            | EnAttente, Confirmée, Annulée |

---

## Diagramme UML (conceptuel)

```
+------------------------------------------------+
|                   Reservation                  |
+------------------------------------------------+
| ReservationId                                  |
| Quantite                                       |
| DateExpiration                                 |
+------------------------------------------------+
| StatutReservation : StatutReservation          |
| DureeReservation  : DureeReservation           |
+------------------------------------------------+
                | 0..1 génère
                v
+------------------------------------------------+
|                    Commande                   |
+------------------------------------------------+
| CommandeId                                     |
| MontantTotal                                   |
+------------------------------------------------+
| StatutCommande : StatutCommande                |
+------------------------------------------------+
                | 1..* génère
                v
+------------------------------------------------+
|                     Billet                     |
+------------------------------------------------+
| BilletId                                        |
+------------------------------------------------+


+-----------------------+
|   StatutReservation   |
+-----------------------+
| EnCours               |
| Confirmée             |
| Expirée               |
+-----------------------+

+-----------------------+
|   DureeReservation    |
+-----------------------+
| ValeurTemps           |
+-----------------------+

+-----------------------+
|   StatutCommande      |
+-----------------------+
| EnAttente             |
| Confirmée             |
| Annulée               |
+-----------------------+

```