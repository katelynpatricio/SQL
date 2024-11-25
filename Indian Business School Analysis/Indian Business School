-- This data from an Indian business school provides insights into the relationship between various factors.

/* 1. What is the average total exam score across the five exams/grades? What is the correlation between
the degree percentage and the MBA percentage? */

SELECT 
	 ROUND(AVG((ssc_p + hsc_p + degree_p + etest_p + mba_p)/5),2) as avg_score
	,ROUND(CAST(CORR(degree_p, mba_p) as numeric),2) as corr_degree
FROM placement

/* 2. Does having work experience give you a bigger advantage for the employability exam or in the MBA
program grades?*/

SELECT 
	 workex
	,ROUND(AVG(etest_p),2) as avg_emp_score
	,ROUND(AVG(mba_p),2) as avg_mba
FROM placement
GROUP BY workex

/* 3. What is the expect change in wages for a 1 point change in the employability test (e_test)? Is it
significant?*/
--regression slope

SELECT 
	 ROUND(CAST(REGR_SLOPE(salary, etest_p) as numeric),2) as salary_per_test
FROM placement

/* 4. Is there a gender wage gap in this data? Could it be MBA program grades driving it? 
Specialty choice?*/ 

SELECT 
	 gender
	,specialization
	,ROUND(AVG(salary),2) as avg_salary
	,ROUND(AVG(mba_p),2) as avg_mba
FROM placement
GROUP BY gender, specialization

-- 5. What was the average MBA grade for those who were placed vs. those who were not?

SELECT pl.avg_mba as placed_score, npl.avg_mba as not_placed_score
	FROM
	(
	SELECT status, ROUND(AVG(mba_p),2) as avg_mba
	FROM placement
	WHERE status = 'Placed'
	GROUP BY status
	) as pl,
	(
	SELECT status, ROUND(AVG(mba_p),2) as avg_mba
	FROM placement
	WHERE status = 'Not Placed'
	GROUP BY status
	) as npl
	
-- 6. How much is work experience “worth” in terms of salary. Is this difference significant?

SELECT 
     we.avg_salary_we
    ,wo.avg_salary_wo
    ,we.avg_salary_we - wo.avg_salary_wo as variance
FROM 
    (
	 SELECT ROUND(AVG(salary), 2) as avg_salary_we 
     FROM placement 
     WHERE workex = 'Yes'
	 ) as we,
    (
	 SELECT ROUND(AVG(salary), 2) as avg_salary_wo 
     FROM placement 
     WHERE workex = 'No'
	 ) as wo
