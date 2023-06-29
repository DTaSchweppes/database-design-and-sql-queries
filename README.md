# database-design-and-sql-queries
Сущности
1. Номенклатура (наименование, кол-во, цена)
2. Каталог номенклатуры/Дерево категорий.
Необходимо хранить данные о категориях товара, при этом сами категории могут
иметь неограниченный уровень вложенности

Схема данных категорий номенклатуры должна безболезненно позволять добавлять
категории любого уровня вложенности. На этапе проектирования максимальный
уровень вложенности неизвестен.

3. Клиенты (наименование, адрес)
4. Заказы покупателей. Необходимо предусмотреть возможность делать заказ из
разного набора товаров.
Продумать схему БД, бизнес логику описывать не требуется.

(Скриншот из pgAdmin)
![image](https://github.com/DTaSchweppes/database-design-and-sql-queries/assets/45369246/b785d681-fc25-4fc6-8a36-ae5ab92222d2)

**Запросы:**

**Получение информации о сумме товаров заказанных под каждого клиента
(Наименование клиента, сумма):**
```SQL
SELECT Customers.Name AS client_name, SUM(Products.Price * OrderDetails.Quantity) AS sum
FROM Customers
JOIN Orders ON Customers.ID = Orders.CustomerID
JOIN OrderDetails ON Orders.ID = OrderDetails.OrderID
JOIN Products ON OrderDetails.ProductID = Products.ID
GROUP BY Customers.Name;
```
![image](https://github.com/DTaSchweppes/database-design-and-sql-queries/assets/45369246/056d6f5a-af36-4fb8-bb1b-94b418534351)



**Найти количество дочерних элементов первого уровня вложенности для
категорий номенклатуры:**
```SQL
SELECT Categories.ID, Categories.Name, COUNT(*) AS Count
FROM Categories
LEFT JOIN Categories AS ChildCategories ON Categories.ID = ChildCategories.ParentCategoryID
WHERE Categories.ParentCategoryID IS NULL
GROUP BY Categories.ID, Categories.Name;
```

![image](https://github.com/DTaSchweppes/database-design-and-sql-queries/assets/45369246/cffd02c5-6555-4708-9ae9-7b4c36229525)
