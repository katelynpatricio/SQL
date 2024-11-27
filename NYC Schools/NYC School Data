-- This analysis focuses on proving whether graduation rates are determined by demographic factors or class size in NYC schools. 

-- Setting up needed tables and variables

CREATE TABLE boroavg AS
SELECT 
  borough, 
  ROUND(SUM(number_of_students)/SUM(number_of_sections),2) as b_avg
FROM classsize
GROUP BY borough

SELECT *
FROM boroavg

SELECT *
FROM gradout

SELECT *
FROM classsize

SELECT *
FROM schooldirect

SELECT *
FROM zipcodes

drop table newgrad
CREATE TABLE newgrad AS
SELECT * 
From gradout
WHERE cohort_year = 2014
  AND cohort = '4 year June'
  AND total_grads != 's'
  AND total_grads_cohort != 's'
  AND dropped_out != 's'
  AND total_grads != '0'

ALTER TABLE newgrad
  ALTER COLUMN total_grads TYPE integer USING total_grads::integer,
  ALTER COLUMN total_grads_cohort type numeric USING total_grads_cohort::numeric(5,2),
  ADD COLUMN tot_st numeric (6,2)

UPDATE newgrad
  SET tot_st = round(total_grads/(total_grads_cohort*.01),1) WHERE total_grads != 0

SELECT *
FROM newgrad

UPDATE zipcodes
  set borough = 'Staten Island' WHERE zipcodes.borough = 'Staten'

-- Looking at Demographic against Graduation Rate

SELECT 
  AVG(rate)
FROM
  (
    SELECT 
      demographic_variable, 
      ROUND((SUM(total_grads)/SUM(tot_st))*100,2) as rate
FROM newgrad
GROUP BY demographic_variable
ORDER BY rate desc
  )

-- Looking at Class size against Graduation Rate

SELECT 
  ROUND(AVG(class_avg),2), 
  ROUND(AVG(rate),2)
FROM
  (
    SELECT 
      ba.borough, 
      ba.b_avg as class_avg,
      ROUND((SUM(total_grads)/SUM(tot_st))*100,2) as rate
    FROM newgrad ng
    JOIN schooldirect as sd ON ng.dbn = sd.dbn
    JOIN zipcodes as zc ON sd.postcode = zc.zip
    JOIN classsize as cs ON zc.borough = cs.borough
    JOIN boroavg as ba ON cs.borough = ba.borough
    GROUP BY ba.borough, ba.b_avg
)

-- Looking at the average rate by demographic

SELECT 
  ROUND(AVG(rate),2) as grad_rate_demo
FROM
  (
    SELECT 
      demographic_variable, 
      ROUND((SUM(total_grads)/SUM(tot_st))*100,2) as rate
FROM newgrad
GROUP BY demographic_variable
ORDER BY rate desc
  )

-- Looking at the average rate by class size

SELECT 
  ROUND(AVG(class_avg),2) as class_avg_borough, 
  ROUND(AVG(rate),2)  as grad_rate_class
FROM
  (
    SELECT 
      ba.borough, 
      ba.b_avg as class_avg,
      ROUND((SUM(total_grads)/SUM(tot_st))*100,2) as rate
    FROM newgrad ng
    JOIN schooldirect as sd ON ng.dbn = sd.dbn
    JOIN zipcodes as zc ON sd.postcode = zc.zip
    JOIN classsize as cs ON zc.borough = cs.borough
    JOIN boroavg as ba ON cs.borough = ba.borough
    GROUP BY ba.borough, ba.b_avg
  )
