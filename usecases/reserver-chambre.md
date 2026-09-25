sequenceDiagram
    title Réserver une chambre

    actor Client
    participant Sys as "Système"
    participant Pay as "Service de paiement"

    Client->>Sys: 1. Saisir les critères
    Sys-->>Client: 2. Afficher les chambres libres
    Client->>Sys: 3. Choisir une chambre
    Sys->>Sys: 4. Vérifier qu'elle est libre

    alt 4a. Chambre déjà réservée
        Sys-->>Client: Signaler le conflit
    else Chambre libre
        Sys->>Sys: 5. Calculer les arrhes

        alt 6a. Arrivée à J+8 ou moins
            note right of Sys: Pas d'arrhes
        else Arrivée à plus de 8 jours
            Sys-->>Client: Demander les arrhes
            Client->>Pay: 6. Payer les arrhes
            Pay-->>Sys: 8. Paiement confirmé
        end

        Sys->>Sys: 9. Enregistrer la réservation
        Sys-->>Client: Confirmer la réservation
    end