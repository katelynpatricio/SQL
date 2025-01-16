-- This data analyzes SNAP participation across U.S. counties in 2000 and 2010, examining trends in participation, the relationship between population growth and SNAP participation, and the potential impact of water scarcity on SNAP rates.

-- 1. How many people receive SNAP benefits in 2000? In 2010? What proportion of the population receives them in each time period?

SELECT 
  SUM(cs.snap_2000) as total_snap_2000,
  ROUND((SUM(cs.snap_2000)/SUM(c2.totalpop)) * 100,2) as proportion
FROM counties_2000 c2
JOIN county_snap cs ON c2.state_fips = cs.state_fips AND c2.county_fips = cs.county_fips

SELECT 
  SUM(cs.snap_2010) AS total_snap_2010,
  ROUND((SUM(cs.snap_2010)/SUM(c.totalpop)) * 100,2) as proportion
FROM counties c
JOIN county_snap cs ON c.state_fips = cs.state_fips AND c.county_fips = cs.county_fips

-- 2. Which states and counties have the highest proportion of SNAP participants?

SELECT 
  c2.state_us_abbreviation as state, 
  geo_name as county,
  cs.snap_2000 * 100/c2.totalpop as proportion
FROM counties_2000 c2
JOIN county_snap cs ON c2.state_fips = cs.state_fips AND c2.county_fips = cs.county_fips
WHERE cs.snap_2000 IS NOT NULL
ORDER BY proportion desc

SELECT 
  c.state_us_abbreviation as state, 
  c.geo_name as county,
  cs.snap_2010 * 100/c.totalpop as proportion
FROM counties c
JOIN county_snap cs ON c.state_fips = cs.state_fips AND c.county_fips = cs.county_fips
WHERE cs.snap_2010 IS NOT NULL
ORDER BY proportion desc

-- 3. Did any counties see a disconnect between population growth and SNAP participation growth?
SELECT 
  CORR(c.totalpop - c2.totalpop, cs.snap_2010 cs.snap_2000) as overall_pop,
  CORR((c.totalpop - c.white_1),(c2.totalpop - c2.white_1)) as non_white_pop
FROM county_snap cs
JOIN counties c ON cs.state_fips = c.state_fips AND cs.county_fips = c.county_fips
JOIN counties_2000 c2 ON c.state_fips = c2.state_fips AND c.state_fips = c2.state_fips
WHERE c2.totalpop > 0 AND cs.snap_2000 > 0

-- 5. Is SNAP participation higher in states where there is waterscarcity?

SELECT 
  c.state_us_abbreviation as state, ROUND(SUM(c.area_water)/SUM(c.totalpop),2) as water_per_person, 
  cs.snap_2010 as snap_2010
FROM counties c
JOIN county_snap cs ON c.state_fips = cs.state_fips AND c.county_fips = cs.county_fips
WHERE snap_2010 IS NOT NULL
GROUP BY state, snap_2010
ORDER BY snap_2010 asc
