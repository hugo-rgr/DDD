# Scénario choisi

Billetterie & réservation

---

# Contexte métier

Le système concerne une plateforme de billetterie en ligne permettant la réservation et l’achat de billets pour des événements (concerts, spectacles, conférences, événements sportifs).

L’organisation met en relation des organisateurs d’événements et des clients finaux. Elle doit gérer la mise en vente des événements, les catégories de places, les quotas disponibles, les paiements et l’émission des billets électroniques.

Les enjeux principaux sont :

- La gestion précise des stocks de places afin d’éviter toute survente.
- La sécurisation des paiements et la traçabilité des transactions.
- La génération fiable et unique des billets électroniques.
- La gestion des annulations et remboursements selon des règles définies.
- La capacité à absorber des pics de trafic lors des ouvertures de ventes.

Le système doit être robuste, cohérent et garantir l’intégrité des données même en cas de forte charge.

---

# Rôles utilisateurs

| Rôle                 | Type            | Description |
|----------------------|-----------------|------------|
| DirecteurCommercial  | Direction       | Définit la stratégie tarifaire, valide les mises en vente et analyse les performances des événements (taux de remplissage, chiffre d’affaires, pics de ventes). |
| AgentBilletterie     | Opérationnel    | Supervise les réservations et traite les demandes d’annulation ou de remboursement. |
| Client               | Client final    | Consulte les événements disponibles, réserve des billets, effectue le paiement et reçoit ses billets électroniques. |
| Artiste              | Opérationnel    | Créer un événement spécifique dans le système, gère les quotas de places, et le soumet à l'agent de billeterie. |

---

# Problématiques métier

1. Éviter la survente lorsqu’un grand nombre de clients réservent simultanément.
2. Gérer une réservation temporaire avant paiement (blocage limité dans le temps).
3. Assurer la cohérence entre paiement validé et émission effective des billets.
4. Gérer les annulations et remboursements selon des conditions métier précises.
5. Garantir l’unicité et la validité des billets lors du contrôle d’accès.

---

# Scénario fil rouge

Un Client consulte la plateforme et sélectionne un événement : un concert prévu dans deux mois. Il choisit une catégorie de place (Carré Or) et demande 2 billets.

Le système vérifie la disponibilité dans cette catégorie. Si des places sont disponibles, une RéservationTemporaire est créée et les places sont bloquées pour une durée limitée (par exemple 10 minutes).

Le Client procède ensuite au paiement. Le système transmet la demande au service de paiement externe.

Si le paiement est validé, la Réservation devient confirmée. Les Billets sont générés avec un identifiant unique et un QR code. Ils sont ensuite envoyés au Client par email.

Si le paiement échoue ou si le délai de réservation expire avant validation, les places sont automatiquement libérées et redeviennent disponibles.

Le jour de l’événement, un agent scanne le billet à l’entrée. Le système vérifie que le billet est valide et qu’il n’a pas déjà été utilisé. Si la vérification est positive, l’accès est autorisé.
