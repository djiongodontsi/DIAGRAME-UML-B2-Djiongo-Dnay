sequenceDiagram
    title Facturer le départ
    actor Rec as Réceptionniste
    participant Sys as Système
    participant Pay as Service de paiement
    actor Client

    Rec->>Sys: 1. Ouvrir le dossier du séjour
    Sys->>Sys: 2. Rassembler les consommations
    Sys->>Sys: 3. Calculer la facture

    alt 2a. Consommation manquante
        Rec->>Sys: Saisir la consommation
        note right of Sys: Reprise à l'étape 3
    end

    alt Montant validé
        Rec->>Pay: 6-7. Encaisser
        Pay->>Rec: 8. Paiement confirmé
        Rec->>Sys: 9. Clôturer le séjour
        Sys->>Rec: Chambre libérée
    end