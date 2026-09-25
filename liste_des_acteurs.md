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