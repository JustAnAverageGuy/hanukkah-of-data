#     Hanukkah of Data
## Noah's Rug (5784)
[https://hanukkah.bluebird.sh/5784/](https://hanukkah.bluebird.sh/5784/) 

### 0

*Answer*: `5777`

Just checked google

It is also the zip password

### 1

*Answer*: `826-636-2286`

Wrote a python script to get data from sqlite db and then converted last names into phone numbers (T9 encoding), then compared.

This should be possible in pure sqlite, but I shudder at the thought of it. (because I don't do sql)

Investigator Name: `Sam Tannenbaum`

### 2

*Answer*: `332-274-4185`

Query:
```SQL
SELECT DISTINCT * FROM customers NATURAL JOIN (
    SELECT customerid FROM orders NATURAL JOIN (
        SELECT orderid 
        FROM orders_items items 
        WHERE items.sku IN (SELECT sku FROM products WHERE desc LIKE '%cleaner%')
    ) 
    WHERE DATETIME(ordered) < DATETIME("2018-01-01 00:00:00")
) 
WHERE name LIKE 'J% P%';
```

Cleaner Name: `Joshua Peterson`

### 3
*Answer*: `917-288-9635`

Query:
```SQL
CREATE TEMPORARY TABLE p2 as ... -- PART 2 QUERY 

SELECT * from customers WHERE 
     '06-21' < STRFTIME('%m-%d',birthdate)        -- cancer
AND STRFTIME('%m-%d', birthdate) < '07-23'        -- cancer
AND STRFTIME("%Y", birthdate) % 12 = '1903' % 12  -- year of rabbit
AND citystatezip in (select citystatezip from p2) -- neighbour of p2 guy
```
Neighbor Name: `Robert Morton`

### 4
*Answer*: `607-231-3605`

Query:
```SQL
CREATE TEMPORARY TABLE customer_id_counts  as 
SELECT customerid, count(*) as cnt 
FROM orders 
WHERE orderid IN (SELECT orderid FROM orders_items WHERE SUBSTR(sku, 1,3 ) == 'BKY' )
AND STRFTIME("%Y", shipped) <= '2020' -- "more than three years ago"
AND STRFTIME('%H', shipped) < '05'    -- reached at 5 am, collected it earlier 
AND STRFTIME("%H", shipped) > '02'    -- probably unnecessary, but anyways, earliest morning is like from 3 am
GROUP BY customerid;

SELECT * FROM customers NATURAL JOIN (
    SELECT * FROM customer_id_counts
    WHERE cnt >= (SELECT MAX(cnt) FROM customer_id_counts)
); -- Get the customer which has done this the most times
```

Reparier Name: `Renee Harmon`

### 5
*Answer*: `631-507-6048`

Query:
```SQL
CREATE TEMPORARY TABLE customers_from_staten_buy_catf AS
SELECT customerid, COUNT(*) cnt FROM orders
WHERE
    orderid IN (
        SELECT orderid FROM orders_items
        WHERE sku IN ( SELECT sku FROM products WHERE DESC LIKE '%cat%')
    )
    AND customerid IN (
        SELECT customerid FROM customers
        WHERE citystatezip LIKE 'staten island%'
    )
GROUP BY customerid
ORDER BY cnt DESC;

SELECT * FROM customers
WHERE customerid IN (
    SELECT customerid FROM customers_from_staten_buy_catf a
    ORDER BY a.cnt DESC -- don't judge me, I am tired
    LIMIT 1
);
-- also I don't do SQL
```

Cat-lady Name: `Nicole Wilson`

### 6
*Answer*: `585-838-9161`

Query:
```SQL
select *
from
  customers
  NATURAL JOIN (
    select
      customerid,
      sum(cost_to_noah - total) noah_loss
    from
      orders
      NATURAL JOIN (
        select
          orderid,
          sum(qty * wholesale_cost) cost_to_noah
        from ( orders_items NATURAL JOIN products)
        GROUP BY orderid
      )
    GROUP BY
      customerid -- total loss a customer has caused to noah
    ORDER BY
      noah_loss desc
    LIMIT
      1
  );
```

Bargain Hunter's Name: `Sherri Long`
Customer Id: `4167`

### 7
*Answer*: `838-335-7157`

Query:
```SQL
CREATE TEMPORARY TABLE p6 AS ...; -- previous query, but only get customerid

CREATE TEMPORARY TABLE colored_items AS
SELECT
  sku,
  substr ("desc", 1, instr ("desc", '(') -1) color_stripped_des,
  shipped
FROM
  orders
  NATURAL JOIN orders_items
  NATURAL JOIN products
WHERE
  customerid IN p6 AND sku LIKE 'COL%';

SELECT
  customerid,
  "name",
  birthdate,
  phone,
  shipped
FROM
  customers
  NATURAL JOIN (
    SELECT
      l.customerid,
      l.shipped,
      l.sku,
      l."desc"
    FROM
      (
        orders
        NATURAL JOIN orders_items
        NATURAL JOIN products
      ) l,
      colored_items r
    WHERE
      date(l.shipped) = date(r.shipped)
      AND l.sku LIKE 'COL%'
      AND instr (l."desc", r.color_stripped_des)
      AND l.sku != r.sku
  )
LIMIT
  10;
```

I don't know why it wasn't anyone else from the following, 
maybe youngest ?
| customerid | name             | birthdate  | phone        | shipped             |
|:------------:|:------------------:|:------------:|:--------------:|:---------------------:|
| 5783       | Carlos Myers     | 1986-04-27 | 838-335-7157 | 2018-12-31 12:26:39 |
| 7474       | Alex Evans       | 1983-08-02 | 914-514-2194 | 2018-12-31 13:02:52 |
| 2487       | Herbert Phillips | 1966-06-26 | 516-849-7413 | 2021-10-07 15:00:00 |
| 1162       | Jeffrey Johnson  | 1984-08-06 | 516-810-7590 | 2022-04-23 15:31:27 |

Ex Boyfriend's Name: `Carlos Myers`
Customer Id: `5783`

### 8
*Answer*: `212-547-3518`

Query:
```SQL
CREATE TEMPORARY TABLE cust_item AS
SELECT DISTINCT
  customerid,
  sku
FROM
  orders NATURAL JOIN orders_items
WHERE
  sku LIKE 'col%';

SELECT
  *
FROM
  customers cc
WHERE
  NOT EXISTS (
    SELECT sku FROM products
    WHERE
      sku LIKE 'COL%'
      AND (cc.customerid, sku) NOT IN cust_item
  );
```

Collecter's Name: `James Smith`
Customer Id: `3808`
