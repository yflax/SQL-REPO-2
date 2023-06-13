Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT city, country, SUM(totaltransactionrevenue)AS Total_Revenue
FROM all_sessions where totaltransactionrevenue is not null
GROUP BY city, country
ORDER BY Total_Revenue DESC
LIMIT 10;



Answer: ![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/87864f63-79bc-4e5c-ad0d-81386d3ef7fc)
"city"	"country"	"total_revenue"
"New York"	"United States"	60925.600
"San Francisco"	"United States"	15643.200
"Sunnyvale"	"United States"	9922.300
"Atlanta"	"United States"	8544.400
"Palo Alto"	"United States"	6080.000
"Tel Aviv-Yafo"	"Israel"	6020.000
"New York"	"USA"	5983.500
"Mountain View"	"United States"	4833.600
"Los Angeles"	"United States"	4794.800
"Chicago"	"United States"	4495.200





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







