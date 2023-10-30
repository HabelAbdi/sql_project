What are your risk areas? Identify and describe them.


Data Quality: Inaccurate, incomplete, or inconsistent data can lead to incorrect conclusions. The columns are either completely empty or have little to no information regarding what the picture is showing. 

Managing Assumptions: assumptions made during analysis can be very hard to moderate as some of the columns have little to no context regarding the information being presented except its column name. Incorrect assumptions can lead to incorrect interpretations of the data.

Scope Creep: Be clear about the project scope. Expanding the scope without proper planning can lead to delays and resource constraints.

Communication: Inadequate communication of findings or misinterpretation of results can lead to incorrect actions being taken. for example, not being able to find the top sales or products in certain cities would cause a misdirection in resources at a company if this data was being used, as some of the cities are not there but some are not even the correct ones linked to their country. 

Resource Limitations: Limited time and expertise can impact the depth and quality of the analysis.




QA Process:
Describe your QA process and include the SQL queries used to execute it.
Below, provide the SQL queries you used to clean your data.

This query is meant to take all the empty cells that are supposed to be filled with revenues from users and get rid of them for me to be able to make more accurate findings and answer better questions.

(DELETE FROM all_sessions
WHERE totaltransactionrevenue IS NULL;

select totaltransactionrevenue from all_sessions)

this query was supposed to take every cell that populated into pgadmin saying it was not able to give the rea, date due to it being a demo, and just replacing the cell completely for now to be able to make other calculations regarding top countries for products and product revenues.

(DELETE FROM all_sessions
WHERE city = 'not available in demo dataset'
and city = 'not set'

select city from all_sessions)

this query is to transform the data type to an hour and minute timestamp to be able to better understand how long the user would stay on a site regarding purchasing an item and specifically where they would browse on the website. 

SELECT 
    fullvisitorid,
    TO_CHAR(TO_TIMESTAMP(timeonsite), 'HH:MM') AS updated_timeonsite
FROM 
    all_sessions;
