[liste_des_acteurs.md](https://github.com/user-attachments/files/32571575/liste_des_acteurs.md)
# 1 – Je liste les acteurs

| Acteur | Nature | Ce qu'il attend du système |
| :--- | :--- | :--- |
| **Client** | Humain | - **consulter** les disponibilités (dates, hôtel, catégorie, capacité) ;<br>- **réserver** une chambre pour une période et un nombre d'occupants ;<br>- **verser** des arrhes (10 % min) si arrivée > J+8 ;<br>- **annuler** lui-même sa réservation ;<br>- **obtenir** un remboursement dépendant du délai d'annulation. |
| **Agent de voyage** | humain, externe (partenaire). | Peut voir les réservations du client, une relation commerciale de type partenaire (facturation, ou multi-réservations). |
| **Gérant** | Humain | - **administrer** les hôtels (création, paramétrage) ;<br>- **administrer** les catégories de chambres (capacité, confort) ;<br>- **administrer** les chambres (numéros, affectation à une catégorie) ;<br>- **administrer** les tarifs ;<br>- **consulter** le taux d'occupation par catégorie sur une période. |
| **Réceptionniste** | Humain | - **enregistrer** l'arrivée (remise des clés, relevé compteur téléphonique) ;<br>- **saisir** les consommations (restaurant, bar, téléphone) ;<br>- **facturer** le départ ;<br>- **éditer** les arrivées prévues du matin. |
| **Service de paiement externe** | Systèmes externes | encaisser les paiements (arrhes, factures) et exécuter les remboursements. |
| **Temps** | Systèmes externes | - **déclencher** l'annulation automatique à J-8 des réservations non confirmées (arrhes non versées) ;<br>- **déclencher** l'édition quotidienne des arrivées prévues chaque matin |

---

L'agent de voyage est modélisé comme un acteur distinct. Parce que pour le client, il agit pour lui-même ; l'agent agit pour autrui et engage sa responsabilité professionnelle via l'agence de voyage. Ils n'ont ni les mêmes droits ni les mêmes obligations.


# 2 - Le diagramme de cas d'utilisation

flowchart TD
    Client[" Client"]
    Agent[" Agent de voyage"]
    Gér[Gérant]
    Récep[" Réceptionniste"]
    Paiement[" Paiement"]
    Temps[" Temps (horloge)"]
    UC1["Consulter les disponibilités"]
    UC2["Réserver une chambre"]
    UC3["Payer des arrhes"]
    UC4["Annuler une réservation"]
    UC5["Rembourser le client"]
    UC6["Annuler les réserv. non confirmées (J-8)"]
    UC7["Enregistrer l'arrivée"]
    UC8["Saisir les consommations"]
    UC9["Facturer le départ"]
    UC10["Encaisser un paiement"]
    UC11["Éditer les arrivées du jour"]
    UC12["Consulter le taux d'occupation"]
    UC13["Administrer hôtels / chambres"]
    UC14["Administrer catégories / tarifs"]
    Agent --> Client
    Client --> UC1
    Client --> UC2
    Client --> UC4
    Récep --> UC7
    Récep --> UC8
    Récep --> UC9
    Récep --> UC11
    Gér --> UC12
    Gér --> UC13
    Gér --> UC14
    Temps --> UC6
    Temps --> UC11
    UC2 -.->|include| UC3
    UC4 -.->|include| UC5
    UC9 -.->|include| UC10
    UC6 -.->|extend| UC2
    UC3 --> Paiement
    UC5 --> Paiement
    UC10 --> Paiement


    # Cas d'utilisation n1: Réserver une chambre

## Identification
- **Acteur principal** : Client (ou Agent de voyage, par généralisation)
- **Acteurs secondaires** : Service de paiement externe ; Temps (pour l'échéance J-8)
- **Cas inclus** : Payer des arrhes
- **Cas étendant** : Annuler les réservations non confirmées (J-8)

## Précondition 
Le client a accès au service (site Internet ou agence partenaire) et a saisi
un hôtel, une période et un nombre d'occupants.

## Postcondition
Une réservation confirmée existe pour une chambre, une période et un nombre
d'occupants compatibles avec la capacité. Si l'arrivée est à plus de 8 jours,
les arrhes (10 % minimum) sont encaissées. La chambre n'est plus disponible
sur ces nuits.

## Scénario nominal
1. Le client saisit les critères (hôtel, dates, nombre d'occupants).
2. Le système affiche les chambres disponibles compatibles avec la capacité.
3. Le client sélectionne une chambre et une période.
4. Le système vérifie qu'aucune réservation n'existe sur ces nuits
   (règle : une chambre n'est jamais réservée deux fois sur une même nuit).
5. Le système calcule le montant des arrhes (10 % minimum si arrivée > J+8).
6. Le client déclenche le paiement des arrhes.
7. Le système transmet la demande au service de paiement externe.
8. Le service de paiement confirme l'encaissement.
9. Le système enregistre la réservation et notifie le client.

## Alternatives
- **2a. Aucune chambre disponible** : le système propose d'autres dates ou
  hôtels, retour à l'étape 1.
- **4a. Conflit détecté (double réservation)** : le système signale
  l'indisponibilité et retourne à l'étape 2.
- **6a. Arrivée à J+8 ou moins** : aucune arrhe exigée, passage direct à
  l'étape 9.
- **8a. Paiement refusé** : le système annule la demande, informe le client,
  retour à l'étape 6.

## Règle automatique associée
Si les arrhes ne sont pas versées avant J-8, le cas « Annuler les réservations
non confirmées (J-8) » s'exécute automatiquement (relation extend) et libère
la chambre pour avoir plusieurs autres disponibilitées.


# Cas d'utilisation  n2: Facturer le départ

## Identification
- **Acteur principal** : Réceptionniste
- **Acteurs secondaires** : Service de paiement externe
- **Cas inclus** : Encaisser un paiement

## Précondition
Le client est enregistré (arrivée saisie dans le systeme) et son séjour est en cours ou
terminé. Les consommations ont été saisies (ou sont saisissables à la volée au fur et a mesure que la reservation se poursuit).

## Postcondition
La facture est éditée, le paiement encaissé, la chambre libérée et remise
en disponibilité.

## Scénario nominal
1. Le réceptionniste ouvre le dossier du séjour du client.
2. Le système agrège les consommations .
3. Le système calcule la facture :
   - nuitées , nombre d'occupants, tarif de la catégorie,
   - prestations consommées,
   - taxe de séjour.
4. Le système affiche le détail au réceptionniste.
5. Le client valide le montant.
6. Le réceptionniste déclenche l'encaissement.
7. Le système transmet la demande au service de paiement externe.
8. Le service de paiement confirme l'encaissement.
9. Le système édite la facture, libère la chambre et clôture le séjour.

## Alternatives
- **2a. Consommation manquante** : le réceptionniste la saisit
  (cas « Saisir les consommations ») avant de reprendre à l'étape 3.
- **5a. Le client conteste un poste** : le réceptionniste vérifie, corrige la
  ligne concernée, retour à l'étape 4.
- **8a. Paiement refusé** : le système conserve le dossier ouvert, propose un
  autre moyen de paiement, retour à l'étape 6.
