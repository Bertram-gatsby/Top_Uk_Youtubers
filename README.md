The SQL steps i use to clean this data 

/* data cleaning
1. remove unnescessary columns by only selecting the ones we need
2. extract the youtube channel names from the first column
3. rename the column names 
*/


SELECT 
NOMBRE,
total_subscribers,
total_views,
total_videos
FROM Top_uk_youtubers_2024;

---charindex
/*

SELECT CHARINDEX('@', NOMBRE), NOMBRE FROM Top_uk_youtubers_2024;

--SUBSTRING

SELECT SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE))
FROM Top_uk_youtubers_2024;*/

CREATE VIEW view_Top_uk_youtubers_2024
AS

SELECT CAST( SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) -1) AS varchar(100))
AS channel_name,
total_subscribers,
total_views,
total_videos
FROM Top_uk_youtubers_2024;


/* data quality tests
1. the data needs to be 100 records of youtube channels (row count test)
2. the data needs 4 fields (column count test)
3. the data type check 
4. the records must be unique in the dataset (duplicate count check)


row count = 100
column count = 4
channel names = varchar
total_ sub = integer
total views and videos 

duplicate = 0
*/

--row count check


SELECT COUNT(*) AS NO_ROW
FROM view_Top_uk_youtubers_2024;


--column count check

SELECT 
COUNT(*) AS column_count
FROM
INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'view_Top_uk_youtubers_2024';

--data type check

SELECT 
COLUMN_NAME,
DATA_TYPE
FROM
INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'view_Top_uk_youtubers_2024';

---CHECK FOR DUPLICATE 

SELECT channel_name,
       COUNT(*) AS duplicate_count
FROM
view_Top_uk_youtubers_2024
GROUP BY channel_name
HAVING COUNT(*) > 1
