What issues will you address by cleaning the data?
> ensure the datatypes match with the column type ex.4
> ensure data makes sense given the context/ name of column ex.1
> change unitprice to standard currency format (not cents) ex.2
> find the difference between unitprice and productprice and when either equals 0 ex3




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