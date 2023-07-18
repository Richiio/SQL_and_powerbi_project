# SQL_and_powerbi_project: Data Science Job Salaries

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*56eZk4zS0wj_PQQ77vNbLw.jpeg"/>
For the longest time now, I’ve wanted to work on this project and even though I’ve done quite a lot in python, I haven’t used other tools which limits what I can show as my strengths. Most times I use SQL/Power BI or Excel, it’s either for a case study or contract job, and most times I start one for my project, but I never finish.

One of my goals this month is to rebuild my resume and portfolio so I can go into proper job hunt mode next month and so far so well. Whenever I try to start any project, I look around me to see what’s interesting enough to look into. I also try to source for uncommon/ weird datasets. You’ll see as I post more about my projects. This particular dataset has been in my documents since May? or April. I’ve gotten it updated many times but the moment I saw it, I know I’d do something with it.

It was inspired literally by my journey. Even with contract jobs, I’ve always had issues defining the worth of the job I’m doing. In the few jobs I’ve applied to, the scariest question for me is the salary range. So for a while, I just wanted to know the average pay.

I stumbled across this <a href="https://salaries.ai-jobs.net/download/">website</a> where there are lots of data jobs and they keep records of jobs posted on their site since 2020. It honestly felt like some of my ‘salary’ questions will be answered and at least I’ll have a clearer view of the market. I queried the dataset using PostgreSQL and designed a report using Power BI.

I do find writing tutorials on power bi weird because it’s more of an action thing so when I find time again, I’ll probably record a time-lapse video and post about it. Today, I’ll be taking you through my SQL code and some tips in designing the report.

I’m used to PostgreSQL because let’s be honest, it has the simplest syntax. If need be, I can use others but mostly it is postgreSQL. You can use PostgreSQL in your cmd or pgadmin. I use pgadmin.

The data consist of the following columns: Year, job_title, salary, currency for salary, salary in USD, remote or not column(values are 100 for fully remote, 50 for partly remote, 0 for onsite jobs), employment type(whether full time, part-time, contract or freelance), company location, employee location and experience level(whether entry-level, mid-level, executive or senior level).

Since it’s a CSV file and I’m more comfortable with editing in excel, I did my data cleaning in excel. I dropped all salary columns except for the one in USD so all salaries are in dollars. I also renamed the abbreviations (e.g PT is part-time, EL is Entry Level). I changed the values for remote where 100 , 50, and 0 are now fully remote, partly remote, and Onsite respectively. I also renamed the column to mobility. I dropped the employee’s location too (will say why in the end). We have no duplicates and no missing data. 818 records in total (after cleaning)

year, job_title, salary(in USD),mobility, employment_type, company_location and experience_level.

The first thing to do is to create a table. Usually, pgadmin has a way of simplifying this but we will be writing codes.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*EuTOUDH1ONgL3sS2W1yeNw.png" />

CREATE TABLE
We already have the clean data in excel (as a CSV file) but to use it in pgadmin, we have to create a table with similar columns and then import the CSV file. When that is done, we are in.

Now to the querying part. I tried to use query statements that I know you’ll need every day. This includes select, where, like, CTE(instead of subqueries), window functions, and joins(particularly the left). We’ve tutorials on joins and window functions in case you want to catch up. This project is honestly for beginners because it is about something we can all relate to and we are incorporating those scary statements.

As usual, the first step is knowing what you are answering. I jotted down the questions I’m interested in and they are below.

How many job titles are we looking at? What are they?
Top 10 highest paid salaries vs the average pay of the job
Top 5 most popular jobs and their average pay
What is the average pay for each country?
Do countries pay a lot more than their country average? which ones?
What is the average pay of entry-level jobs?
Which countries pay their entry-level above the general average pay?
Is there a change in average entry-level pay yearly?
How many fully remote jobs do we have?
Do countries pay fully remote entry-level jobs well?
Lets dig in .

How many job titles are we looking at? What are they?
The first one is the distinct number of jobs present. That is quite easy. There are 52 different jobs. This is nice to hear because most people (myself included) are hung up on common job titles like data analyst, data scientist, machine learning engineer, etc. Still, there are different 52 different job titles in this dataset.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*q82LG_Y6FSiJxbRElAuscw.png" />

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*15fGmNDpPcNJJouPeR5toQ.png" />

The titles of the jobs are: “Principal Data Scientist”, “BI Data Analyst”, “Machine Learning, Infrastructure Engineer”, “Computer Vision Software Engineer”, “Cloud Data Engineer”, “Big Data Engineer”, “Head of Data”, “Marketing Data Analyst”, “Data Analytics Manager”, “Data Science Consultant”, “Lead Machine Learning Engineer”, “Research Scientist”, “Applied Machine Learning Scientist”, “ETL Developer”, “Director of Data Engineering”, “Financial Data Analyst”, “Data Specialist”, “Machine Learning Manager”, “Data Architect”, “AI Scientist”, “Lead Data Analyst”, “Data Analytics Engineer”, “Staff Data Scientist”, “Big Data Architect”, “Data Engineer”, “ML Engineer”, “Head of Data Science”, “Machine Learning Research Engineer”, “Product Data Analyst”, “Data Analytics Lead”, “Data Engineering Manager”, “Machine Learning Developer”, “Principal Data Analyst”, “Data Analyst”, “Analytics Engineer”, “NLP Engineer”, “Lead Data Scientist”, “Director of Data Science”, “Data Scientist”, “Data Science Engineer”, “Data Science Manager”, “Principal Data Engineer”, “Head of Machine Learning”, “Computer Vision Engineer”, “Business Data Analyst”, “Cloud Data Architect”, “Machine Learning Engineer”, “Lead Data Engineer”, “3D Computer Vision Researcher”, “Applied Data Scientist”, “Machine Learning Scientist”, “Finance Data Analyst”

I had to copy them out in case anyone needs them. You can add similar job titles to your LinkedIn for easier reach.

2. Top 10 most paid salaries vs the average pay for the job

The most paid job is Machine Learning Manager with 900,000 dollars per annum pay. The average pay for the job is 508,552 dollars. It is expected. Machine learning job generally pays high. I used the window function in the code to produce the average pay for each job.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*zZxztQflJwqwpQYHceSHtw.png" />

3. Top 5 most popular jobs and their average pay

Data Analysts, Data Engineers, Data Scientists, and ML engineers are the most common fields of data jobs. They’ll be the most jobs any of us are going into so I’m a little curious about their average pay. Surprisingly, Data Engineer jobs are the highest. I thought Data Analysts would have the highest number of jobs but the pay is just as I expected.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*H6bBIP1bejxegOLyAegQGA.png" />

4. What is the average pay for each country?

The country column in the dataset consists of country codes instead of the full names of each country. What I did was look for a dataset with country names and their codes then I imported the data into pgadmin (using the same method I talked about). That way I can easily join both tables while querying, so the names can be known. The country with the most paid salary is Aland Islands with an average of 900k dollars monthly per annum. The US came 5th with an average of 146,263.45 dollars annually. There is obviously an imbalance in the dataset as the data consist of jobs only from that website and there were lots of US jobs which makes the US own more accurate than the Aland islands own(I think there is only one job from this country in fact). Out of curiosity, I checked for Nigeria jobs and it’s 30,000 USD yearly. Ranking last in Vietnam and Iraq with 4000 USD annually. Again, this isn’t really accurate but it does let you know the offers in the market.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*2G968knGa6ttu5z2vpdMgQ.png" />
5. Do countries pay a lot more than their country’s average? which ones?

Here, I checked the average of each country and compare it with salaries. What the query return is a table where the pay is greater than the average pay of the country and the number of jobs where the pay is greater in that particular country. There are about 351 jobs where the pay is greater than the country's avg pay, which is very close to half of the jobs. The USA has 229 jobs them. Again, the USA has a large percentage of jobs in the dataset so it is expected. In the query I used CTE(common table expressions) which serves as creating another table from a preexisting table which you can use all through this particular query. The table is not explicitly created and cannot be reused outside this query unless it is recreated. It is possible to do this same thing using subquery but I always pick CTEs over subqueries because they are reusable within the query and also seem less complicated.

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*LEpd8MjLWYzF_p-QkhOQRg.png" />
6. What is the average pay of entry-level jobs?

This seems self-explanatory so I’ll skip through it

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*zzwYPf1ikWrXooA4XClMqw.png" />
7. Which countries pay their entry-level above the general average pay for entry-level jobs?

Australia ranks highest with pay of 117,772 USD annually and there are just 6 countries in total.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*zE6-klxuyWFKTN5j1Su-5Q.png" />
8. Is there a change in average entry-level pay yearly?

There has been an increase which is a good thing because the year hasn’t even ended yet and 2022 is the highest. Growth in the demand for data jobs might have led to an increase in pay as there is more recognition now than ever.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*NflEe15mbyR-kVt1rDZniQ.png" />
9. How many fully remote jobs do we have?

There are 489 fully remote jobs in total which are above the 50% mark of the total number of jobs.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*BMOl3vIPFUEK1atsO2Z6ug.png" />
10. Do countries pay fully remote entry-level jobs well?

Yes. They do but only 4 pass the average pay of entry-level jobs.

<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*EwgEfctDOU0adak5sbiSrg.png" />
We have reached the end of this query and all I can say is this is definitely interesting. According to this dataset, the pay is good. The pay seems to be worth the stress and most importantly, there are fully remote jobs for entry levels. What I would be more interested in are countries that offer sponsorships but I don’t have data on that. Something I did but failed to show was check how many jobs have the employee’s location different from the company’s location. I think these are the actual fully remote roles and they were very few.

We’ve been here for a while now so I’ll go through the report design another day. Hope you learned something at least. Have a nice week ❤
