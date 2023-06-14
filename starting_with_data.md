Question 1: On which date (or dates if tied) was the highest number of visitors recorded?

SQL Queries:SELECT COUNT(*) AS visit_count, date
FROM (
  SELECT DISTINCT visitid, date
  FROM analytics
) AS subquery
GROUP BY date
ORDER BY visit_count DESC
LIMIT 1;

Answer: ![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/abc00c6b-687b-487f-adef-c8cfd97ef9c4)
May-16-2017 , 611 visitor




Question 2: What is the average amount of time that each visitor stays on the site?

SQL Queries:	
select visitid ,avg(timeonsite) from (select  distinct visitid, timeonsite
			 from analytics
			)as subquery
			group by visitid

Answer:![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/cd0e49bc-e1c1-4df6-8719-7b3f41a89136)




Question 3: What is the most common channel through which visitors heard about the website?

SQL Queries:select distinct channelgrouping , count (channelgrouping) over (partition by channelgrouping) from analytics order by count desc
			
			
			

Answer:![image](https://github.com/yflax/SQL-REPO-2/assets/127430738/a78c1b03-f64b-4b1a-89b7-07fc78e11b9e)




Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
