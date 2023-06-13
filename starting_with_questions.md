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


SELECT distinct (city), country,
avg(totaltransactionrevenue)
from all_sessions
GROUP BY totaltransactionrevenue, city,country


Answer:
"city"	"country"	"avg_revenue_per_city"
"Tel Aviv-Yafo"	"Israel"	602000000.00000000
"Atlanta"	"United States"	427220000.00000000
"Seattle"	"United States"	358000000.00000000
"Sydney"	"Australia"	358000000.00000000
"Sunnyvale"	"United States"	248057500.00000000
"not available in demo dataset"	"United States"	243702400.00000000
"Los Angeles"	"United States"	239740000.00000000
"Palo Alto"	"United States"	202666666.66666667
"Nashville"	"United States"	157000000.000000000000	




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







