What issues will you address by cleaning the data?
> ensure the datatypes match with the column type ex.4
> ensure data makes sense given the context/ name of column ex.1
> change unitprice to standard currency format (not cents) ex.2
> find the difference between unitprice and productprice and when either equals 0 ex3
> change 'time' column to varchar and then to time format and make sure that when minute or second exceeds 59 it carries over to the next section (ie.60 seconds =>1 minute and 00 for seconds) ex.5
> change timeonsite in analytics  from varchar to integer ex.6 
> change visitstartime in analytics to timestamp ex.7



Queries:
Below, provide the SQL queries you used to clean your data.

1)SELECT stocklevel FROM sales_report
where stocklevel < 0 ,
SELECT *from sales_report
where total_ordered <=  0 

2)UPDATE analytics
SET unitprice = (unitprice/1000000),
UPDATE all_sessions
SET productprice=(productprice/1,000,000)

3) SELECT unitprice, v2productname,productprice from all_sessions al
JOIN analytics a on a.visitid=al.visitid
where productprice <= 0 or unitprice <=0

4) ALTER table all_sessions
   ALTER column totaltransactionrevenue type  numeric
   USING totaltransactionrevenue::numeric
   and then ( because realized too many number after the decimal)
   UPDATE all_sessions
SET totaltransactionrevenues = ROUND(totaltransactionrevenue::numeric, 2);
ALTER TABLE all_sessions
DROP COLUMN totaltransactionrevenue;
ALTER TABLE all_sessions
RENAME COLUMN totaltransactionrevenues TO totaltransactionrevenue
select * from all_sessions

5) ALTER TABLE all_sessions
ALTER COLUMN time
TYPE VARCHAR
USING LPAD(time::VARCHAR, 6, '0');

UPDATE public.all_sessions
SET time = SUBSTR(time, 1, 2) || ':' || SUBSTR(time, 3, 2) || ':' || SUBSTR(time, 5, 2);

UPDATE public.all_sessions
SET time = TIME '00:00:00' + 
           INTERVAL '1 hour' * CAST(SUBSTR(time, 1, 2) AS INTEGER) +
           INTERVAL '1 minute' * CAST(SUBSTR(time, 4, 2) AS INTEGER) +
           INTERVAL '1 second' * CAST(SUBSTR(time, 7, 2) AS INTEGER);
		 
		   
ALTER TABLE public.all_sessions
ALTER COLUMN time TYPE TIME USING time::TIME;

6) ALTER TABLE analytics ADD COLUMN timeonsite_minutes INTEGER;

UPDATE analytics
SET timeonsite_minutes = CAST(timeonsite AS INTEGER);

ALTER TABLE analytics DROP COLUMN timeonsite;

ALTER TABLE analytics RENAME COLUMN timeonsite_minutes TO timeonsite;

7) --create new table with timestamp format
ALTER TABLE analytics ADD COLUMN visitstarttime_new timestamp with time zone;

--input the original visitstarttime values in the new table
UPDATE analytics
SET visitstarttime_new = to_timestamp(visitstarttime)

--drop the original table and rename the new one to the original name
ALTER TABLE analytics DROP COLUMN visitstarttime;
ALTER TABLE analytics RENAME COLUMN visitstarttime_new TO visitstarttime;

8) -- remove the scientific notation from fullvisior id and chnage type to numeric 
ALTER TABLE all_sessions ALTER COLUMN fullvisitorid TYPE numeric USING fullvisitorid::n

-- noticing that the all the fullvisitorids have three numbers in the beginning and zeros 
