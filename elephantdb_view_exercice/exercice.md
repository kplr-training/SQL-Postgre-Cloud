# VIEW - Exercice

1. Compléter le code suivant pour qu'il crée les tables **Customers** et **Orders** :

```sql
-- Créer la base de données exoview
CREATE DATABASE IF NOT EXISTS exoview;

-- Utiliser la base de donnée exoview
USE exoview;

-- Créer la table Customers
CREATE TABLE Customers (
-- Remplir ici
);

-- Insérer les données dans la table Customers
INSERT INTO Customers (id, name, email, city) VALUES
(1, 'John', 'john@gmail.com', 'New York'),
(2, 'Sara', 'sara@gmail.com', 'Paris'),
-- Remplir ici
;

-- Créer la table Orders
CREATE TABLE Orders (
-- Remplir ici
);

-- Insérer les données dans la table Orders
INSERT 
-- Remplir ici
;
```

Customers : 
| id | name | email | city |
| --- | --- | --- | --- |
| 1 | John | <john@gmail.com> | New York |
| 2 | Sara | <sara@gmail.com> | Paris |
| 3 | Mark | <mark@gmail.com> | London |
| 4 | Jane | <jane@gmail.com> | Berlin |
| 5 | Mike | <mike@gmail.com> | Madrid |

Orders :
| id | customer_id | product | price |
| --- | --- | --- | --- |
| 1 | 1 | TV | 1000 |
| 2 | 2 | Phone | 500 |
| 3 | 1 | Headset | 100 |
| 4 | 3 | Computer | 1500 |
| 5 | 2 | Headset | 50 |
| 6 | 1 | Phone | 800 |
| 7 | 4 | Computer | 2000 |
| 8 | 5 | TV | 1200 |
| 9 | 1 | Camera | 600 |

Enregistrer le code dans un fichier portant l'extension .sql et exécutez-le dans ElephantSQL pour créer la base de données exoview, créer les tables et insérer les données.

2. Créez une vue appelée **CustomerOrders** qui affiche le nom, l'adresse électronique, la ville et le nombre de commandes de chaque client. (Indice : utiliser left join)
3. Récupérer toutes les lignes de la vue **CustomerOrders**.
4. Créez une vue appelée **HighSpendingCustomers** qui affiche le nom, l'adresse électronique et le montant total dépensé pour chaque client ayant dépensé plus de 1000 $.
5. Récupérer toutes les lignes de la vue **HighSpendingCustomers**.
6. Créez une vue appelée **TopSellingProducts** qui affiche le produit et le nombre de fois où il a été vendu, trié par ordre décroissant du nombre de fois où chaque produit a été vendu.
7. Récupérer toutes les lignes de la vue **TopSellingProducts**.
8. Créez une vue appelée **OrdersByCity** qui affiche la ville et le nombre de commandes passées par les clients de cette ville. (indice : utiliser left join)
9. Récupérer toutes les lignes de la vue **OrdersByCity**.
10. Créez une vue appelée **AverageOrderValue** qui affiche la valeur moyenne des commandes pour chaque client.
11. Récupérer toutes les lignes de la vue **AverageOrderValue**.
