Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries: SELECT totalTransactionRevenue, country, city
FROM all_sessions
WHERE totalTransactionRevenue > '0  ' 
  AND city <> 'not available in demo dataset'
GROUP BY country, totalTransactionRevenue, city
ORDER BY totalTransactionRevenue, country, city ;




Answer: "103770000"	"United States"	"San Bruno"




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries: select  a.city, a.country, round(avg(sk.total_ordered), 2) as Rounded_Avg
from all_sessions a
join sales_by_sku sk on a.Productsku = sk.productsku
where  city <> 'not available in demo dataset'	
	and city <> '(not set)'
group by a.city, a.country, a.fullvisitorid
order by avg(sk.total_ordered) desc;



Answer: "Seoul"	"South Korea"	456.00
"San Francisco"	"United States"	456.00
"Mountain View"	"United States"	456.00
"Santa Clara"	"United States"	456.00
"Boston"	"United States"	456.00
"Palo Alto"	"United States"	456.00
"Mountain View"	"United States"	456.00
"San Jose"	"United States"	456.00



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries: select  a.city, a.country, a.v2productcategory,  a.fullvisitorid
from all_sessions a
join sales_by_sku sk on a.Productsku = sk.productsku
where  city <> 'not available in demo dataset'	
	and city <> '(not set)'
	and v2productcategory <> '(not set)'
group by a.city, a.country, a.fullvisitorid, a.v2productcategory
order by country, city

Answer: from the given data, the types of products are broken down into 6. Electronics: here's a consistent presence of electronics in the orders, indicating that visitors are interested in electronic devices or gadgets.
Apparel: Both men's and women's apparel is ordered, suggesting a diverse range of clothing products are popular among visitors.  Accessories: This category includes headgear, stickers, and housewares, indicating a demand for various accessories
Bags: Bags are another popular category, including limited supply bags and specific designs like Men's Outerwear
Nest Products: intrest in home automation, and technology
Office and Drinkware: consistent demand and need for office accesories and gadgets 





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

with  products_1 as (select sum(p.orderedquantity) as total_quantity ,p.name, a.country, a.city
from products p
join all_sessions a on p.name = a.v2productname
where city <> 'not available in demo dataset'	
    AND city <> '(not set)'
    AND v2productcategory <> '(not set)'
group by a.country, a.city, p.name
) 
select max(total_quantity) as max_total_quantity, products_1.city, products_1.country
from products_1
group by products_1.city, products_1.country



Answer:
499	"San Francisco"	"France"
1184	"Mexico City"	"Mexico"
3071	"Rome"	"Italy"
180	"Colombo"	"Sri Lanka"
168	"Goose Creek"	"United States"
56	"Prague"	"Czechia"
845	"Longtan District"	"Taiwan"
3682	"San Bruno"	"United States"
306	"Cupertino"	"United States"
4004	"Los Angeles"	"United States"
390	"Washington"	"United States"
769	"Stockholm"	"Sweden"
2442	"Bangkok"	"Thailand"
3071	"Riyadh"	"Saudi Arabia"
1033	"Ann Arbor"	"United States"




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

CREATE TEMPORARY TABLE revenue_table AS
SELECT s.country, 
		s.city, 
		s.fullvisitorId, 
		p.sku, 
		SUM(a.unit_price * p.orderedQuantity) AS revenue
FROM 
		analytics a
JOIN 
		all_sessions s ON a.visitId = s.visitId
JOIN 
		sales_report sr ON sr.productSKU = s.productSKU
JOIN 
		products p ON sr.stockLevel = p.stockLevel
WHERE 
		a.revenue IS NULL
GROUP BY 
		s.country,
		s.city, 
		s.fullvisitorId, 
		p.sku
  
SELECT DISTINCT revenue, country, city
FROM revenue_table
 where city <> 'not available in demo dataset'	
    AND city <> '(not set)'

Answer:
- Sales patterns differ significantly between cities, especially notable in Australia's Brisbane
- Argentina seems to have consistent demand across cities, whereas Australia's sales are more varied, possibly due to a diverse product range or market fluctuations.
- Argentina, Rosario: Significant sales, potentially indicating a specific popular product or market demand.






