-- Import Statement

CREATE TABLE counties_2000 (
  geo_name varchar(90), -- County/state name,
  state_us_abbreviation varchar(2), -- State/U.S. abbreviation
  state_fips varchar(2), -- State FIPS code
  county_fips varchar(3), -- County code
  p0010001 integer, -- Total population
  p0010002 integer, -- Population of one race
  p0010003 integer, -- White Alone
  p0010004 integer, -- Black or African American alone
  p0010005 integer, -- American Indian and Alaska Native alone
  p0010006 integer, -- Asian alone
  p0010007 integer, -- Native Hawaiian and Other Pacific Islander alone
  p0010008 integer, -- Some Other Race alone
  p0010009 integer, -- Population of two or more races
  p0010010 integer, -- Population of two races
  p0020002 integer, -- Hispanic or Latino
  p0020003 integer -- Not Hispanic or Latino
);

SELECT * FROM counties_2000;

SELECT * FROM counties;

SELECT 
  c2000.geo_name,
  c2010.state_us_abbreviation AS state,
  c2010.p0010001 AS pop_2010,
  c2000.p0010001 AS pop_2000,
  c2010.p0010001 - c2000.p0010001 AS raw_change,
  round( (CAST(c2010.p0010001 AS numeric(8,1)) - c2000.p0010001) / c2000.p0010001 * 100, 1 ) AS pct_change
FROM counties c2010 
INNER JOIN us_counties_2000 c2000 ON c2010.state_fips = c2000.state_fips AND c2010.county_fips = c2000.county_fips AND c2010.p0010001 != c2000.p0010001
ORDER BY pct_change DESC;

ALTER TABLE counties_2000
  ADD COLUMN cname varchar(60);

ALTER TABLE counties_2000
  DROP COLUMN cname;

UPDATE counties_2000

SET cname = split_part(geo_name,',', 1)

SELECT * FROM counties_2000
