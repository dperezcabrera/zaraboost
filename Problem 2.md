## 2. Calculating incomes from shipping costs

Given the tables COUNTRIES, DELIVERY_TYPES, ITEM_REFERENCES, ORDERS y ORDER_ITEMS, and using my MySQL as a database manager.

<br/>

### Tables:

```sql
create table countries(
   id int not null,
   iso char(2) not null,
   name varchar(80) not null,
   nicename varchar(80) not null,
   iso3 char(3) default null,
   numcode smallint default null,
   phonecode int not null,
   primary key(id)
);

create table delivery_types(
   id int not null,
   name varchar(80) not null,
   primary key(id)
); 

create table item_references(
   id int,
   price numeric(9,2)
);

create table orders(
   id int,
   country_id int,
   delivery_type_id int
); 

create table order_items(
  id int,
  order_id int,
  item_reference_id int,
  units int
);
```
<br/>

Calculate income from shipping costs with a single SQL sentence that returns a number in a column named delivery_incomes.
 

It should be noted that:

* All orders from SPAIN (`'iso = ES'`) do not pay shipping costs.
* For the other countries:
  * If the order's amount is equal to or greater than €50 do not pay shipping costs.
  * If the order's amount is less than €50 it will pay €3.95 if the delivery type is `'ORDINARY DELIVERY'` and €7.5 it is `'URGENT DELIVERY'`
  * The amount will be approximated to two decimals.

The query should work with any dataset.

<br/>
<br/>
<br/>

### Solution:


```sql
SELECT SUM(dt.delivery_income) as delivery_incomes
FROM 
    orders o
    JOIN countries c ON o.country_id = c.id AND c.iso <> 'ES'
    JOIN (
        SELECT 
            order_id, 
            ROUND( SUM(oi.units * ir.price), 2) AS amount  
        FROM order_items oi
        JOIN item_references ir ON oi.item_reference_id = ir.id
        GROUP BY oi.order_id
        HAVING amount < 50) oi ON oi.order_id = o.id
    JOIN (
        SELECT id, 
            CASE 
                WHEN name = 'ORDINARY DELIVERY' THEN 3.95 
                WHEN name = 'URGENT DELIVERY' THEN 7.5 
                ELSE 0 
            END as delivery_income
        FROM delivery_types) dt ON dt.id = o.delivery_type_id;

```

### Solution 2:

```sql
SELECT 
    SUM(CASE 
        WHEN c.iso = 'ES' THEN 0 -- No shipping cost for orders from Spain
        WHEN order_total >= 50 THEN 0 -- No shipping cost for orders >= €50
        WHEN dt.name = 'ORDINARY DELIVERY' THEN 3.95 -- Shipping cost for ORDINARY DELIVERY
        WHEN dt.name = 'URGENT DELIVERY' THEN 7.5  -- Shipping cost for URGENT DELIVERY
        ELSE 0
    END) AS delivery_incomes
FROM 
    orders o
    JOIN countries c ON o.country_id = c.id
    JOIN delivery_types dt ON o.delivery_type_id = dt.id
    LEFT JOIN (
        SELECT
            order_id,
            ROUND(SUM(price * units), 2) AS order_total
        FROM 
            order_items oi
            JOIN item_references ir ON oi.item_reference_id = ir.id
        GROUP BY
            order_id
    ) AS order_totals ON o.id = order_totals.order_id;

```

