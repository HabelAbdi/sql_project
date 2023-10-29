Question 1: 
What are the top ten id's based on the quantity ordered
SQL Queries:
 
select a.fullvisitorid, sku.total_ordered, a.v2productname
from all_sessions a
join sales_by_sku sku on sku.productsku = a.productsku
where sku.total_ordered <> 0
group by a.v2productname, a.fullvisitorid, sku.total_ordered
order by sku.total_ordered desc;


Answer: 
178299450219923401	456	"Ballpoint LED Light Pen"
755785039458158809	456	"Ballpoint LED Light Pen"
822565286032368780	456	"Ballpoint LED Light Pen"
1128053735353413254	456	"Ballpoint LED Light Pen"
1475404838632495633	456	"Ballpoint LED Light Pen"
1585419338148187519	456	"Ballpoint LED Light Pen"
1704048100885896972	456	"Ballpoint LED Light Pen"
1730646281059286092	456	"Ballpoint LED Light Pen"
1884314548586855019	456	"Ballpoint LED Light Pen"
1936834114497180045	456	"Ballpoint LED Light Pen"


Question 2: find the top users who ordered the most time while having the highest time spent on the sight 

SQL Queries:

WITH RankedSessions AS (
    SELECT 
        a.fullvisitorid, 
        MAX(a.timeonsite) AS max_timeonsite,
        MAX(sku.total_ordered) AS max_total_ordered,
        RANK() OVER (ORDER BY MAX(a.timeonsite) DESC) AS timeonsite_rank,
        RANK() OVER (ORDER BY MAX(sku.total_ordered) DESC) AS total_ordered_rank,
        RANK() OVER (ORDER BY MAX(a.timeonsite) DESC, MAX(sku.total_ordered) DESC) AS top_shopper_rank
    FROM 
        all_sessions a
    JOIN 
        sales_by_sku sku ON sku.productsku = a.productsku
    WHERE 
        sku.total_ordered <> 0
        AND a.timeonsite <> 0
    GROUP BY 
        a.fullvisitorid
)
SELECT 
    fullvisitorid,
    max_timeonsite,
    max_total_ordered,
    timeonsite_rank,
    total_ordered_rank,
    top_shopper_rank
FROM 
    RankedSessions
WHERE 
    top_shopper_rank = 1
ORDER BY 
    top_shopper_rank;


Answer:

fulluserid: 2933187673711787156	max_timeonsite: 3937	max_total_ordered: 5	timeonsite_rank: 1	total_ordered_rank: 3296 
top_shopper_rank:	1



Question 3: rank the products based on product price for each country

SQL Queries:
WITH CountryProductPrices AS (
    SELECT 
        country,
        round (AVG(productprice), 2) AS avg_product_price
    FROM 
        all_sessions
    GROUP BY 
        country
)
SELECT 
    country,
    avg_product_price,
    RANK() OVER (ORDER BY avg_product_price DESC) AS country_rank
FROM 
    CountryProductPrices
ORDER BY 
    country_rank;


Answer:
"Maldives"	119990000.00	1
"Palestine"	99990000.00	2
"Botswana"	99990000.00	2
"Barbados"	65990000.00	4
"Bolivia"	64990000.00	5
"Honduras"	62992000.00	6


Question 5: 

SQL Queries:

Answer:
