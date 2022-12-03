# DEFORESTATION-Exploration

## Introduction
Introduction
You’re a data analyst for ForestQuery, a non-profit organization, on a mission to reduce deforestation around the world and which raises awareness about this important environmental topic.

Your executive director and her leadership team members are looking to understand which countries and regions around the world seem to have forests that have been shrinking in size, and also which countries and regions have the most significant forest area, both in terms of amount and percent of total area. The hope is that these findings can help inform initiatives, communications, and personnel allocation to achieve the largest impact with the precious few resources that the organization has at its disposal.

You’ve been able to find tables of data online dealing with forestation as well as total land area and region groupings, and you’ve brought these tables together into a database that you’d like to query to answer some of the most important questions in preparation for a meeting with the ForestQuery executive team coming up in a few days. Ahead of the meeting, you’d like to prepare and disseminate a report for the leadership team that uses complete sentences to help them understand the global deforestation overview between 1990 and 2016.

### CREATE VIEW
```
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

