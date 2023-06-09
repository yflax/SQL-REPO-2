What issues will you address by cleaning the data?
> ensure the datatypes match with the column type ex.4
> ensure data makes sense given the context/ name of column ex.1
> change unitprice to standard currency format (not cents) ex.2
> find the difference between unitprice and productprice and when either equals 0 ex3
> change 'time' column to varchar and then to time format and make sure that when minute or second exceeds 59 it carries over to the next section (ie.60 seconds =>1 minute and 00 for seconds) ex.5
> change timeonsite in analytics  from varchar to integer ex.6 
> change visitstartime in analytics to timestamp ex.7
> shorten and change format of fullvisitor id in ex.8 *note to self for the future :  could accomplished the same thing with far less queries ie.more concise
> change productquantity from varchar to integer ex.12
> clean the v2productcategoru and v2productname columns by removing the words 'Google' and 'YouTube'  and 'Androrid which have no connection to the columns


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
select * from all_sessions -- in hindsight could have done this with one piece of code as opposed to three chuncks of code;

update  all_sessions
 set totaltransactionrevenue= (totaltransactionrevenue/100000) -- unlikely that totaltransaction revenue is in the millions so divided it

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

-- noticing that the all the fullvisitorids have three numbers in the beginning and unnecessary zeros after the decimal. the following will remove zeros after the deciam point
UPDATEall_sessions
SET fullvisitorid = regexp_replace(fullvisitorid::text, '(\.[0-9]*?)0*$', '\1')::numeric

-- so it could be simplified further by deviding it by 1 billion so that its contains only non-zero numbers . so divided and removed zeros after decimal point again
update all_sessions
set fullvisitorid=(fullvisitorid/100000000);

UPDATE your_table
SET fullvisitorid = ROUND(fullvisitorid, 2) -- to allow only two digits after the decimal place 

9) -- noticed city 'New York' with country 'Canada' so:
  Update all_sessions 
SET country = 'USA' where city = 'New York'
10) -- fill '(not available in demo dataset )' values for city with the mode for cities when the country is canada
select count(city), city from all_sessions where country = 'Canada' group by city order by count(city) desc -- returns 'not avail as mode , and 'toronto' as city value the appears secind most frequently so:
update all_sessions
set city = 'Toronto' where city= 'not available in demo dataset' and country = 'Canada' 
-- now will do the same for USA and other countries
select count(city), city,country from all_sessions where country = 'Mexico' group by  country, city order by count(city) desc 

update all_sessions
set city = 'Mexico City' where country= 'Mexico' and city = 'not available in demo dataset'-- "mexico City" and so on... for Dominican Republic (which had no city value at all)I chose "santo domingo" which is the capital. since it is a developing country the inferrence was made that it is unlikely there are shipments made to villages/cities other than the capital

update all_sessions set country = 'Dominican Republic' where city = 'Santo Domingo' -- santo dom was in Taiwan
--didnt realize there were so many countries so stopped in the middle
11) CREATE TABLE _empty_values_to_be_logged AS
SELECT v2productname , v2productcategory,
FROM all_sessions

12) 
ALTER TABLE all_sessions
ALTER COLUMN productquantity TYPE numeric(10,2) USING ROUND(productquantity::numeric, 2);

13) UPDATE all_sessions
SET v2productcategory = REPLACE(REPLACE(v2productcategory, 'Google ', ''), 'YouTube', ''),
    v2productname = REPLACE(REPLACE(v2productname, 'Google ', ''), 'YouTube', '')
WHERE v2productcategory LIKE '% Google %' OR v2productcategory LIKE '%YouTube%' OR v2productname LIKE '% Google %' OR v2productname LIKE '%YouTube%';
-- then noticed the word android too so:
UPDATE all_sessions
SET v2productcategory = REPLACE(v2productcategory, 'Android ', ''),
    v2productname = REPLACE(v2productname, 'Android ', '')
WHERE v2productcategory LIKE '%Android%' OR v2productname LIKE '%Android%';

14) -- noticed some categories are not set 
select v2productcategory, v2productname from all_sessions where v2productcategory = '(not set)'

SELECT v2productcategory
FROM all_sessions
WHERE v2productname = 'Women''s Short Sleeve Hero Tee White';

update all_sessions
set v2productcategry ='Home/Apparel/Women''s/Women''s-T-Shirts/' where v2productname ='Women''s Short Sleeve Hero Tee White'








