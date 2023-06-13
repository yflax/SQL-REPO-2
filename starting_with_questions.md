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




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







