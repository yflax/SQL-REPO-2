Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT city, country, total_revenue, RANK() OVER (ORDER BY total_revenue DESC) AS revenue_rank
FROM (
  SELECT city, country,COALESCE(SUM("totaltransactionrevenue"),0) AS total_revenue
  FROM all_sessions
  GROUP BY city, country
) AS subquery
ORDER BY total_revenue DESC
LIMIT 10;


Answer: ![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/01894008-0e88-4250-9bf9-0b2e4137bdf3)
"New York"	"United States"	66909.100	1
"San Francisco"	"United States"	15643.200	2
"Sunnyvale"	"United States"	9922.300	3
"Atlanta"	"United States"	8544.400	4
"Palo Alto"	"United States"	6080.000	5
"Tel Aviv-Yafo"	"Israel"	6020.000	6
"Mountain View"	"United States"	4833.600	7
"Los Angeles"	"United States"	4794.800	8
"Chicago"	"United States"	4495.200	9
"Sydney"	"Australia"	3580.000	10





**Question 2: What is the average number of products ordered from visitors in each city and country?**

SELECT city, country, Avg(productquantity)AS Average_product_quantity
FROM all_sessions 
where productquantity is not null
GROUP BY city, country


Answer:
![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/afe14939-add5-4a2a-938a-14e6b6269884)
"Madrid"	"Spain"	10.0000000000000000
"Salem"	"United States"	8.0000000000000000
"New York"	"United States"	7.4444444444444444
"Atlanta"	"United States"	4.0000000000000000
"Houston"	"United States"	2.0000000000000000
"Columbus"	"United States"	1.00000000000000000000
"Dallas"	"United States"	1.00000000000000000000
"Detroit"	"United States"	1.00000000000000000000
"Dublin"	"Ireland"	1.00000000000000000000
"Helsinki"	"Finland"	1.00000000000000000000
"Los Angeles"	"United States"	1.00000000000000000000
-- when done w/ order by Avg(productquantity) desc



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
1)SELECT city, country, v2productcategory, order_count
FROM (
  SELECT city, country, v2productcategory, COUNT(*) AS order_count,
         ROW_NUMBER() OVER (PARTITION BY city, country ORDER BY COUNT(*) DESC) AS rank
  FROM all_sessions
  GROUP BY city, country, v2productcategory
) AS subquery
WHERE rank = 1
ORDER BY city, country;  -- found the product category with the most orders for each city and country , and displayed the count .

2) SELECT city, country, COUNT(DISTINCT v2productcategory) AS num__unique_product_categories
FROM all_sessions
GROUP BY city, country; --find the number of unique categories for orders made in each city and country
2)B) COUNT(DISTINCT v2productcategory) AS num__unique_product_categories
FROM all_sessions



Answer: 1) ![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/be8a4176-2fee-4371-bea4-de88b2a932cf)
"Accra"	"Ghana"	"Home/Apparel/Men's/Men's-T-Shirts/"	2
"Adelaide"	"Australia"	"Home/Apparel/Men's/Men's-Outerwear/"	1
"Ahmedabad"	"India"	"Home/Shop by Brand//"	5
"Akron"	"United States"	"Home/Apparel/Men's/"	1
"Alexandria"	"Egypt"	"Home/Apparel/Men's/Men's-T-Shirts/"	1
"Amman"	"Jordan"	"Home/Bags/"	1
"Amsterdam"	"Netherlands"	"Home/Shop by Brand//"	37
"Amsterdam"	"United States"	"Home/Apparel/Men's/"	1
"Ann Arbor"	"United States"	"Home/Apparel/Men's/Men's-T-Shirts/"	9
"Antalya"	"Turkey"	"(not set)"	1
"Antwerp"	"Belgium"	"Home/Apparel/Men's/Men's-T-Shirts/"	16

>> Pattern discovered: THe most popular categories from each city and country were Men's Apparel/Clothing categories.

2)![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/18a62143-1f58-4fc4-91d8-036d4462028a)
"Accra"	"Ghana"	2
"Adelaide"	"Australia"	1
"Ahmedabad"	"India"	7
"Akron"	"United States"	1
"Alexandria"	"Egypt"	1
"Amman"	"Jordan"	1
"Amsterdam"	"Netherlands"	32
"Amsterdam"	"United States"	1
"Ann Arbor"	"United States"	23
"Antalya"	"Turkey"	1
"Antwerp"	"Belgium"	23
2)B) ![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/178173ae-c3a4-41b8-a309-aaa9dfe9aacc)
74

>> Pattern discovered: the number of distinct order categories from each city and country varies with some cities having inly one type of order category and others having 32 or more different categories.
>> The average number of distinct categories is 74 but it is not an indication of the central tendency becasue there is high variability amongst the result set( of unique categories per city and country).








**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries: --finds the count of each product name for each city and country 
1)SELECT city, country, COUNT(v2productname) OVER (PARTITION BY city, country) AS num_of_product_bought, v2productname
FROM all_sessions; 
-- this is good for a detailed overview of each product name bought from each city and country , but there are quite a lot to scroll through so:
 
2) Select max(num_of_product_bought) as most_popular_product,
city, country, v2productname
from (SELECT city, country, COUNT(v2productname) OVER (PARTITION BY city, country) AS num_of_product_bought, v2productname
FROM all_sessions
order by num_of_product_bought desc) as subquery
group by city, country , v2productname
order by most_popular_product desc ; --gives the most popular product for each city in each country but it is also quite lengthy as there are many 'ties'

3) --gives total sales profit for each product in each city and each country
  SELECT al.City, al.Country, al.v2productName,
   sum(al.productprice*sr.total_ordered) as product_total_sales
   from all_session al
   join sales_report sr on sr.name =v2productname
    group by al.v2productname, al.city, al.country
	having sum(al.productprice*sr.total_ordered) > 0
   order by product_total_sales desc



Answer: 
1)![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/0348b5d8-6829-4a5e-93a5-87294906f9c8)
"Accra"	"Ghana"	3	" Men's Vintage Tank"
"Accra"	"Ghana"	3	" Luggage Tag"
"Accra"	"Ghana"	3	"Men's 100% Cotton Short Sleeve Hero Tee White"
"Adelaide"	"Australia"	1	"Men's Watershed Full Zip Hoodie Grey"
"Ahmedabad"	"India"	12	"Canvas Tote Natural/Navy"
"Ahmedabad"	"India"	12	" RFID Journal"
"Ahmedabad"	"India"	12	"Youth Short Sleeve T-shirt Royal Blue"
"Ahmedabad"	"India"	12	" Trucker Hat"
"Ahmedabad"	"India"	12	"Laptop and Cell Phone Stickers"
"Ahmedabad"	"India"	12	"Trucker Hat"
"Ahmedabad"	"India"	12	"Snapback Hat Black"
"Ahmedabad"	"India"	12	" Wool Heather Cap Heather/Black"
"Ahmedabad"	"India"	12	"Women's  Short Sleeve Hero Tee Black"
"Ahmedabad"	"India"	12	"Tri-blend Hoodie Grey"
"Ahmedabad"	"India"	12	" Leatherette Notebook Combo"

2)![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/61073e91-cccb-48a6-be5f-529e4ca6d95f)
4835	"New York"	"United States"	"Onesie Gold"
4835	"New York"	"United States"	"Men's Pullover Hoodie Grey"
4835	"New York"	"United States"	" Leatherette Notebook Combo"
4835	"New York"	"United States"	"Women's Short Sleeve V-Neck Tee Stone"
4835	"New York"	"United States"	"Glass Water Bottle with Black Sleeve"
4835	"New York"	"United States"	"Tube Power Bank"
4835	"New York"	"United States"	"Gift Card - $25.00"
4835	"New York"	"United States"	"Infant Short Sleeve Tee Royal Blue"
4835	"New York"	"United States"	"Ballpoint Pen Blue"
4835	"New York"	"United States"	"Infant Short Sleeve Tee Cornflower Blue"
4835	"New York"	"United States"	"Keyboard DOT Sticker"
4835	"New York"	"United States"	"Waze Pack of 9 Decal Set" 
>> Pattern observed : New York, USA has the highest sales volume and mony of its products are tied

3) ![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/95ab2852-60d8-4aac-a820-19ad3db25c90)
"New York"	"United States"	" Spiral Journal with Pen"	78300
"New York"	"United States"	" Hard Cover Journal"	55860
"London"	"United Kingdom"	" Spiral Journal with Pen"	39150
"New York"	"United States"	"Leatherette Journal"	37642
"New York"	"United States"	"26 oz Double Wall Insulated Bottle"	32592
"London"	"United Kingdom"	" Hard Cover Journal"	30870
"Berlin"	"Germany"	" Spiral Journal with Pen"	20880
"New York"	"United States"	" Men's Short Sleeve Hero Tee Charcoal"	20034
"New York"	"United States"	"Clip-on Compact Charger"	17700
"New York"	"United States"	"22 oz  Bottle Infuser"	15700
"Bengaluru"	"India"	" Spiral Journal with Pen"	15660
"New York"	"United States"	" RFID Journal"	12274
 >> office/writing products are the most popular type 






**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
1) select city, country, count(*)
							from(select distinct city, country, v2productname 
                from all_sessions
) as subquery
group by city ,country 
order by count desc

2) SELECT al.City, al.Country, al.v2productName,
   sum(al.productprice*sr.total_ordered) as product_total_sales
   from all_session al
   join sales_report sr on sr.name =v2productname
    group by al.v2productname, al.city, al.country
	having sum(al.productprice*sr.total_ordered) > 0
   order by product_total_sales desc




Answer:
* Before we interpret the results of our queries - we need to step back and deine "impact" . Is impact the biggest diversity of products ordered (variety)? Is it the city/country with the highest order volume? Is it the city/country whith the biggest profit ? 
1)![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/68386540-968d-4478-a01b-03c179c135a9)
-- finds the cities with the biggest variety of products sold
>> NEW YORK has the biggest impact in terms of variety of products sold

2) ![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/b0dbfcc8-ecf2-417a-af0a-498cca41de99)
-- finds total revenue for each product in each city/country
>> NEW York has the biggest impact in terms of revenue per product






