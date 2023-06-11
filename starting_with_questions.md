Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:



Answer:




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







