The data consists of U.S. counties, including details such as total population, racial demographics, housing units, water area, and populations of specific racial or ethnic groups, categorized by state and county.

-- Create a list of the states from greatest to smallest population. Which is 3rd largest/third smallest?

SELECT *
FROM
  (
    SELECT 
      state_us_abbreviation as state,
      SUM(population_count_100_percent) as population
    FROM counties
    GROUP BY state_us_abbreviation
    ORDER BY population desc
    LIMIT 3
  )
ORDER BY population asc
LIMIT 1

SELECT *
FROM
  (
    SELECT 
      state_us_abbreviation as state,
      SUM(population_count_100_percent) as population
    FROM counties
    GROUP BY state_us_abbreviation
    ORDER BY population asc
    LIMIT 3
  )
ORDER BY population desc
LIMIT 1

-- 2. Which county and state have the highest proportion of water?

SELECT 
  state_us_abbreviation as state, 
  geo_name as county, 
  SUM(area_water) as water_proportion
FROM counties
GROUP BY state, county
ORDER BY water_proportion desc
LIMIT 1

SELECT 
  state_us_abbreviation as state, 
  SUM(area_water) as water_proportion
FROM counties
GROUP BY state
ORDER BY water_proportion desc
LIMIT 1

/*3. Which county has the largest Asian population? Is this the same county that has the highest percentage of Asians?*/

SELECT 
  geo_name as county, 
  Asian_1 as asian_pop
FROM counties
GROUP BY county, asian_pop
ORDER BY asian_pop desc
LIMIT 1

SELECT 
  geo_name as county, 
  (Asian_1 * 100/TotalPop) as asian_pop_percent
FROM counties
GROUP BY county,asian_1,totalpop
ORDER BY asian_pop_percent desc
LIMIT 1

-- 4. Which county has the highest percentage minority population?

SELECT 
  county, 
  (minority_pop * 100/totalpop) as minority_percent
FROM
  (
    SELECT 
      geo_name as county, 
      totalpop, 
      (black_1 + amindian_1 + asian_1 + pacific_1 + otherrace_1 + twoormore) as minority_pop
    FROM counties
  )
ORDER BY minority_percent desc
LIMIT 1

/* 5. Look at the highest and lowest counties by non-white population. Does there
appear to be differences in the housing stock to population ratio?*/

SELECT 
  geo_name as county, 
  housing_unit_count_100_percent as housing, 
  (totalpop - white_1) as population
FROM counties
GROUP BY county, housing, totalpop, white_1
ORDER BY population desc

SELECT 
  geo_name as county, 
  housing_unit_count_100_percent as housing, 
  (totalpop - white_1) as population
FROM counties
GROUP BY county, housing, totalpop, white_1
ORDER BY population asc

-- 6. Create a table that is average minority population by state. Which state is lowest?

SELECT 
  state_us_abbreviation as state, 
  round(AVG(black_1 + amindian_1 + asian_1 + pacific_1 + otherrace_1 + twoormore),2) as minority_pop
FROM counties
GROUP BY state
ORDER BY minority_pop asc
LIMIT 1

-- 7. Find the median non-white population by state?

SELECT 
  state_us_abbreviation as state, 
  PERCENTILE_CONT(.5) WITHIN GROUP (ORDER BY (totalpop - white_1)) as non_white_pop
FROM counties
GROUP BY state

-- 8. Which county has the largest population of people who are five or more races. 

SELECT 
  geo_name as county, 
  fiverace
FROM counties
ORDER BY fiverace desc
LIMIT 1
