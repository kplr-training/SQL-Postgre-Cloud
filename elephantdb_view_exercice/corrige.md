# VIEW - Solution

1. Compléter le code suivant pour qu'il crée les tables **Customers** et **Orders** :

```sql
-- Créer la base de données exoview
CREATE DATABASE IF NOT EXISTS exoview;

-- Utiliser la base de donnée exoview
USE exoview;

-- Create Customers table
CREATE TABLE Customers (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  email VARCHAR(50),
  city VARCHAR(50)
);

-- Insérer les données dans la table Customers
INSERT INTO Customers (id, name, email, city) VALUES
(1, 'John', 'john@gmail.com', 'New York'),
(2, 'Sara', 'sara@gmail.com', 'Paris'),
(3, 'Mark', 'mark@gmail.com', 'London'),
(4, 'Jane', 'jane@gmail.com', 'Berlin'),
(5, 'Mike', 'mike@gmail.com', 'Madrid');

-- Créer la table Orders
CREATE TABLE Orders (
  id INT PRIMARY KEY,
  customer_id INT,
  product VARCHAR(50),
  price INT
);

-- Insérer les données dans la table Orders
INSERT INTO Orders (id, customer_id, product, price) VALUES
(1, 1, 'TV', 1000),
(2, 2, 'Phone', 500),
(3, 1, 'Headset', 100),
(4, 3, 'Computer', 1500),
(5, 2, 'Headset', 50),
(6, 1, 'Phone', 800),
(7, 4, 'Computer', 2000),
(8, 5, 'TV', 1200),
(9, 1, 'Camera', 600);

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

```sql
CREATE VIEW CustomerOrders AS
SELECT c.name, c.email, c.city, COUNT(o.id) AS num_orders
FROM Customers c
LEFT JOIN Orders o ON c.id = o.customer_id
GROUP BY c.id;
```
3. Récupérer toutes les lignes de la vue **CustomerOrders**.
```sql
SELECT * FROM CustomerOrders;
```

4. Créez une vue appelée **HighSpendingCustomers** qui affiche le nom, l'adresse électronique et le montant total dépensé pour chaque client ayant dépensé plus de 1000 $.
```sql
CREATE VIEW HighSpendingCustomers AS
SELECT c.name, c.email, SUM(o.price) AS total_spent
FROM Customers c
JOIN Orders o ON c.id = o.customer_id
GROUP BY c.id
HAVING total_spent > 1000;
```

5. Récupérer toutes les lignes de la vue **HighSpendingCustomers**.
```sql
SELECT * FROM HighSpendingCustomers;
```

6. Créez une vue appelée **TopSellingProducts** qui affiche le produit et le nombre de fois où il a été vendu, trié par ordre décroissant du nombre de fois où chaque produit a été vendu.
```sql
CREATE VIEW TopSellingProducts AS
SELECT product, COUNT(id) AS num_sold
FROM Orders
GROUP BY product
ORDER BY num_sold DESC;
```

7. Récupérer toutes les lignes de la vue **TopSellingProducts**.
```sql
SELECT * FROM TopSellingProducts;
```

8. Créez une vue appelée **OrdersByCity** qui affiche la ville et le nombre de commandes passées par les clients de cette ville. (indice : utiliser left join)
```sql
CREATE VIEW OrdersByCity AS
SELECT c.city, COUNT(o.id) AS num_orders
FROM Customers c
LEFT JOIN Orders o ON c.id = o.customer_id
GROUP BY c.city;
```
9. Récupérer toutes les lignes de la vue **OrdersByCity**.
```sql
SELECT * FROM OrdersByCity;
```

10. Créez une vue appelée **AverageOrderValue** qui affiche la valeur moyenne des commandes pour chaque client.
```sql
CREATE VIEW AverageOrderValue AS
SELECT c.name, AVG(o.price)
```

11. Récupérer toutes les lignes de la vue **AverageOrderValue**.
```sql
SELECT * FROM AverageOrderValue;
```





