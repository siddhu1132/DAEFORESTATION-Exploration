# DEFORESTATION-Exploration

## Introduction
Introduction
You’re a data analyst for ForestQuery, a non-profit organization, on a mission to reduce deforestation around the world and which raises awareness about this important environmental topic.

Your executive director and her leadership team members are looking to understand which countries and regions around the world seem to have forests that have been shrinking in size, and also which countries and regions have the most significant forest area, both in terms of amount and percent of total area. The hope is that these findings can help inform initiatives, communications, and personnel allocation to achieve the largest impact with the precious few resources that the organization has at its disposal.

You’ve been able to find tables of data online dealing with forestation as well as total land area and region groupings, and you’ve brought these tables together into a database that you’d like to query to answer some of the most important questions in preparation for a meeting with the ForestQuery executive team coming up in a few days. Ahead of the meeting, you’d like to prepare and disseminate a report for the leadership team that uses complete sentences to help them understand the global deforestation overview between 1990 and 2016.

### CREATE VIEW
```sql

DROP VIEW IF EXISTS forestation;

CREATE VIEW forestation  
AS
(SELECT f.country_code, f.country_name, f.year, f.forest_area_sqkm,
(l.total_area_sq_mi*2.59) AS total_area_sqkm,
r.region, r.income_group,
((SUM(forest_area_sqkm)/SUM(total_area_sq_mi*2.59))*100) AS percent_forest 
FROM forest_area AS f
JOIN land_area AS l
ON f.country_code = l.country_code
AND f.year = l.year
JOIN regions AS r
ON r.country_code = l.country_code
GROUP BY 1,2,3,4,5,6,7);
 
SELECT * 
FROM forestation;

```
## PART 1 : GLOBAL SITUATION

- _What was the total forest area (in sq km) of the world in 1990? Please keep in mind that you can use the country record denoted as "World" in the region table.?_
- 
```sql
WITH total_forest_area_1990 AS 
(SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 1990 AND country_name = 'World')

SELECT * FROM total_forest_Area_1990;
```
- _What was the total forest area (in sq km) of the world in 2016? Please keep in mind that you can use the country record in the table is denoted as "World"?_
- 
```sql
WITH total_forest_area_2016 AS 
(SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 2016 AND country_name = 'World')

SELECT * FROM total_forest_Area_2016;
```
-_What was the change (in sq km) in the forest area of the world from 1990 to 2016?_

```sql
SELECT 
(SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 1990 AND country_name = 'World') - 
(SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 2016 AND country_name = 'World') AS change_in_forest_area 
FROM forestation
LIMIT 1;
```

-_What was the percent change in forest area of the world between 1990 and 2016?_

```sql
SELECT 
(((SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 1990 AND country_name = 'World') -
(SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 2016 AND country_name = 'World')) /
(SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 1990 AND country_name = 'World'))*100 AS percent_change_in_forest_area
FROM forestation
LIMIT 1;
```

-_If you compare the amount of forest area lost between 1990 and 2016, to which country's total area in 2016 is it closest to?_

```sql
SELECT country_name, total_area_sqkm, 
ABS((total_area_sqkm - (SELECT 
(SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 1990 AND country_name = 'World') - 
(SELECT SUM(forest_area_sqkm) AS total_forest_area_sqkm
FROM forest_area
WHERE year = 2016 AND country_name = 'World')))) AS forest_area_lost
FROM forestation
WHERE year = 2016
GROUP BY 1,2
ORDER BY 3
LIMIT 1;
```
