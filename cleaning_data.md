What issues will you address by cleaning the data?

1. turning numeric null values to '0' or deleting them to update the column
2. turning empty data cells so they do not show up in the updated table
3. cleaning time columns to be more readable


Queries:
Below, provide the SQL queries you used to clean your data.



(DELETE FROM all_sessions
WHERE totaltransactionrevenue IS NULL;

select totaltransactionrevenue from all_sessions)


(DELETE FROM all_sessions
WHERE city = 'not available in demo dataset'
and city = 'not set'

select city from all_sessions)



SELECT 
    fullvisitorid,
    TO_CHAR(TO_TIMESTAMP(timeonsite), 'HH:MM') AS updated_timeonsite
FROM 
    all_sessions;



