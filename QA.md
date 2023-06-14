What are your risk areas? Identify and describe them.
Potential QA RISKS:
### data untrustowrthiness. The information about the ecommerce website could have been pulled from a website many years ago that is no longer relevant . the data may have been changed by someone after the information was pulled , resulting in false information.
### quality of tests. My test were limited in their capacity to detect every single potential quality flaw
### missing , duplicate data . although i tried cleaning missing or duplicate values , I could not completely get rid of duplicate or null values resluting in lowered data quality.


QA Process:
Describe your QA process and include the SQL queries used to execute it.
THe first stp of my QA process included simply reviewing the data to ensure completeness. It was not complete, nor was everything unique , accurate and conforming to conventional formatting. My first attemot at fixing all these quality assurance dimensions occured during the cleaning process , as I transformed columns that were the wrong type  , and trimmed ids to numbers the conform to standard id format. ie. changed time clomuns to time or timezone or hour format - depending on context. For detialed code see cleaning data repo.

Later I ran some QA tests like :
### ex1) select pageviews from all_sessions where pageviews < = 0

### ex2) select total_ordered from sales_by_sku where total_ordered <= 0  -- here, the data failed the test , because rows were returned when it does not make sense for someone to have order information for an orderd with 0 or less items ordered (unless total_ordered refers to the product name and tells us if this product was ever ordered)


