What issues will you address by cleaning the data?
> ensure the datatypes match with the column type
> ensure data makes sense given the context/ name of column ex.1
> 




Queries:
Below, provide the SQL queries you used to clean your data.
1)SELECT stocklevel FROM sales_report
where stocklevel < 0 ,
SELECT *from sales_report
where total_ordered <=  0 ,
