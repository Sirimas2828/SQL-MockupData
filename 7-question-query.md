Case No.1 
hat are the Top 25 schools (.edu domains)? (Answer - SQL & Result)

--Answer: 

Select email_domain AS 'E-mail Domain',
Count(email_domain) AS 'Total'
From users
Where email_domain like '%.edu'
Group BY 1
Order By 2 DESC
Limit 25;


Case No.2 
How many .edu learners are located in New York?  (Answer - SQL & Result)

--Answer: Total Learner = 50

Select email_domain AS 'E-mail Domain',
Count(email_domain) AS 'Total Learner'
From users
Where email_domain like '%.edu' AND city = 'New York';


Case No.3
The mobile_app column contains either mobile-user or NULL. How many of these Codecademy learners are using the mobile app? (Answer - SQL & Result)

--Answer: 330 learners who use mobile app or 16% use mobile app.

Select mobile_app AS 'Mobile_App_User',
count(user_id) As 'No_Of_Users',
100 * COUNT(mobile_app) / (SELECT COUNT(*) FROM users) AS '%'
From users
Where mobile_app IS Not null
AND mobile_app != '' ;

Case No.4
Query for the sign-up counts for each hour  (Answer - SQL & Result)

--Answer: 
SELECT
  strftime('%H', sign_up_at) AS 'Signup Time',
  COUNT(*) AS 'No. Of Signup users' 
FROM users 
GROUP BY 1 ;


Case no.5 
Do different schools (.edu domains) prefer different courses? (Answer - SQL & Result)

--Answer: No, as data shows the percentage of students who signed up for the course in each school to see the most popular, we can see some courses in some schools have no one signed up. 
SQL query below
----------------

with summary as

(
SELECT
users.email_domain as 'School',

COUNT(*) as NoStudents,
sum(case when progress.learn_cpp != '' then 1 else 0 end) as LearnCPPCount,
sum(case when progress.learn_sql != '' then 1 else 0 end) as LearnSQLCount,
sum(case when progress.learn_html != '' then 1 else 0 end) as LearnHTMLCount,
sum(case when progress.learn_javascript != '' then 1 else 0 end) as LearnJavaScriptCount,
sum(case when progress.learn_java != '' then 1 else 0 end) as LearnJavaCount

FROM users JOIN progress ON users.user_id = progress.user_id
GROUP BY users.email_domain
ORDER BY 2 DESC

)

select
School,
round(((1.0 * LearnCPPCount / NoStudents)*100),2) as PercentCpp,
round(((1.0 * LearnSQLCount / NoStudents)*100),2) as PercentSQL,
round(((1.0 * LearnHTMLCount / NoStudents)*100),2) as PercentHTML,
round(((1.0 * LearnJavaScriptCount / NoStudents)*100),2) as PercentJavascript,
round(((1.0 * LearnJavaCount / NoStudents)*100),2) as PercentHJava

from summary;


Case No. 6 
What courses are the New Yorkers students taking? (Answer - SQL & Result)

--Answer: All of them; CPP, SQL, HTML, JavaScripts and Java. 
SQL query below
-----------------------------------------------

SELECT

city,

sum(case when p.learn_cpp != '' then 1 else 0 end) as LearnCPPCount,
sum(case when p.learn_sql != '' then 1 else 0 end) as LearnSQLCount,
sum(case when p.learn_html != '' then 1 else 0 end) as LearnHTMLCount,
sum(case when p.learn_javascript != '' then 1 else 0 end) as LearnJavaScriptCount,
sum(case when p.learn_java != '' then 1 else 0 end) as LearnJavaCount

from users AS u
join progress AS p
on u.user_id = p.user_id

where city = 'New York'

group by 1;


Case No. 7
What courses are the Chicago students taking?

--Answer: All of them; CPP, SQL, HTML, JavaScripts and Java. 
SQL query below
------------------------------

SELECT

city,

sum(case when p.learn_cpp != '' then 1 else 0 end) as LearnCPPCount,
sum(case when p.learn_sql != '' then 1 else 0 end) as LearnSQLCount,
sum(case when p.learn_html != '' then 1 else 0 end) as LearnHTMLCount,
sum(case when p.learn_javascript != '' then 1 else 0 end) as LearnJavaScriptCount,
sum(case when p.learn_java != '' then 1 else 0 end) as LearnJavaCount

from users AS u
join progress AS p
on u.user_id = p.user_id

where city = 'Chicago'

group by 1;
