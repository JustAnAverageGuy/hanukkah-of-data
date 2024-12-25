#     Hanukkah of DataNoah's Rug (5784)
[https://hanukkah.bluebird.sh/5784/](https://hanukkah.bluebird.sh/5784/) 

### 0

*Answer*: `5777`

It is also the zip password


### 1

*Answer*: `826-636-2286`

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
    ORDER BY a.cnt DESC
    LIMIT 1
);
```

Cat-lady Name: `Nicole Wilson`
