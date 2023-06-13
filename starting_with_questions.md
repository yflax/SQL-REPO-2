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


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







