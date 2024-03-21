
# Aiswarya P M
## Data Analyst
### About Me

I am Aiswarya P M from Kerala. With a solid educational background in Mathematics, complemented by recent hands-on training and certification in Google Data Analytics, I am eager to leverage my skills and experience to contribute to your team and drive business improvement through actionable insights.

During my tenure as a Data Analyst Intern at Titan Tech Emirates Pvt Ltd, I gained practical experience in collecting, cleaning, and analyzing data from diverse sources. I collaborated closely with the data analytics team to conduct exploratory data analysis, develop visualizations, and generate insightful reports for stakeholders. My role also involved interpreting analysis results and providing recommendations to enhance business processes, demonstrating my ability to translate data into valuable insights.

I am experienced in SQL for data manipulation and analysis, and I am also familiar with tools such as Tableau, R, and Power BI, enabling me to effectively analyze and visualize data to drive informed decision-making. My educational background includes a Master's degree in Mathematics and a Bachelor's degree in Mathematics with Statistics, reflecting my commitment to quantitative analysis and data-driven decision-making.

---

### Table Of Contents
1. [Operation Analytics and Investigating Metric Spike](#operation-analytics-and-investigating-metric-spike) - MySQL

2. [Marketing AD Campaign Analysis](#marketing-ad-campaign-analysis) - Power BI

3. [Movie Analysis](#movie-analysis) - Excel, Power BI 

---
### Operation Analytics and Investigating Metric Spike.

This project focuses on analyzing the data which is provided by company. My task is to derive insights. So that these insights can be used by ops team, suppoert team, marketing team etc to predict the overall growth or decline of a company.We have 2 cases, Job data and users, evnets, email events tables.

In case study 1 the insights are found on following question:
1. Number of jobs reviewed: Amount of jobs reviewed over time.
2. Throughput : Calculate 7 day rolling average of throughput? For throughput, do you prefer metric or 7-day rolling and why?
3. Percentage share of each language : Share of each language for different contents.

In case study 2 the insights are found based on following questions :
1. User Engagement : To measure the activeness of a user. measuring if the user finds quality product/services.
2. User Growth : Amount of users growing over time for a product.
3. Weekly Retention : Users getting retained weekly after signing up for  aproduct.
4. Weekly Engagement : To measure the activeness of a user. Measuring if the user finds quality in a service weekkly.
5. Email Engagement : User engaging with email service.

This project is developed using SQL. First , created a database using the file and loaded the data into SQL Workbench. Then analysed the data.

Case Study 1 :

To check the duplicate:

``` duplicates
SELECT actor_id, COUNT(*) as Duplicates
FROM operational_analysis.job_data
group by actor_id
having count(*)>1 ;
```
<img width="116" alt="1" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/99cf374d-c196-40db-b2d4-655cc1cd047b">


1. Number of jobs reviewed: Amount of jobs reviewed over time :

``` jobs reviewed
SELECT ds as Dates, Round((count(job_id)/sum(time_spent))*3600) as Jobs_Reviewed_per_hour
FROM operational_analysis.job_data
group by ds;
```
<img width="180" alt="2" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/28852d99-9869-4a33-a255-a4693869bdf2">


Max no. of jobs were reviewed on 28-11-2020 and minimum jobs were reviewed on 27-11-2020.

2. Throughput : Calculate 7 day rolling average of throughput? For throughput, do you prefer metric or 7-day rolling and why?

``` average
select round(count(event)/sum(time_spent),2) as Weekly_Throughput
FROM operational_analysis.job_data;
```

<img width="106" alt="3" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/a7a68afc-ad89-4486-9467-3f720331e985">

``` Rolling Avg
select ds as Date, round(count(event)/sum(time_spent),2) as Daily_Throughput,
avg(count(event)/sum(time_spent)) over (order by ds desc ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS Rolling_Avg
FROM operational_analysis.job_data
group by ds
order by ds;
```

<img width="203" alt="4" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/3895a0ef-2079-4e08-a6c8-868f66f2a476">


Daily Throughput and rolling average is highest on 28-11-2020. Rolling metrics helps us to show if the metrics is going up or down as the values changes on daily, weekly, monthly or yearly basis.

3. Percentage share of each language : Share of each language for different contents.

```
Select count(*) as Total_Count
from operational_analysis.job_data;
```

<img width="77" alt="5" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/34417d9e-524a-40bc-83fb-6f5b00a0902f">

```
select languages,round((count(languages)/9) * 100,2) as Percent
from operational_analysis.job_data
group by languages
order by Percent desc;
```

<img width="112" alt="6" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/cd118e29-f62a-4986-a071-c8b49d7b259f">

Persian is the most used language here.


   


---
### Marketing AD Campaign Analysis
This is a project that I worked on as part of internship. Here we have data of various advertisement campaign the company did for the clients. As a part of digital marketing the data includes the cities, device used, clicks, impression, click through rate (CTR), average cost per click (CPC), conversions, cost per conversion, conversion rate and Absolute top impression. I was assigned to analyse the data and create a dashboard. Here I used Power Bi for analysis.

<img width="578" alt="campaign dash" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/3b27eb83-53ce-41bb-bbdf-ec51c62bb474">

[Link to the Interactive report of this analysis.](https://app.powerbi.com/links/sVtLZxGUp1?ctid=48982c93-be9a-4edc-b6c3-23c9093d7403&pbi_source=linkShare)

**What I found out from the analysis :**
1. Digital marketing reaches out mostly via mobile phones. Marketing through social medias can reach out to more customers.
2. TT Air Ambulance UAE Ad created highest impressions and Kuwait Ad had the least Impressions.
3. Kuwait Ad and Baharin Ad impression rate should be elevated to reach more customers. It had least to no clicks in the year 2023.
4. This table shows the percent of clicks each ad got from impression and the prcent of clicks which were converted. An ad is successfull when the more clicks get converted. Kuwait ad got more clicks but low conversion whereas Qatar Ad got 0.94% clicks and 8.50 % was converted. Only the Qatar ad had the ability to convert those clicks. Here Qatar ad was succesfull that others.
   
    | AD | Click Through Rate (%) | Conversion Rate (%) |
   |---|---|---|
   | Baharin AD | 1.10 | 2.80 |
   | Kuwait AD | 10.84 | 2.07 |
   | Qatar AD | 0.94 | 8.50 |
   | Saudi AD | 2.55 | 3.30 |
   | UAE AD | 0.97 | 4 |

5. The TT Air Ambulance AD Saudi, UAE, Baharin and Qatar AD had a spike in the conversion trend line in the month of June and July 2023. We have to check whether there is any specific reason for such occurence or not.

Through this project I got to learn about the marketing field which is entirely new for me. The terms impressions, click through rate and cost per click was new for me. Used charts and measures for analysis. Also created an ineractive dashboard and presented my findings to the team.

Thank You.

   
   
---
### Movie Analysis
The dataset provided is related to IMDB Movies. A potential problem to investigate could be: "What factors influence the success of a movie on IMDB?" Here, success can be defined by high IMDB ratings. The impact of this problem is significant for movie producers, directors, and investors who want to understand what makes a movie successful to make informed decisions in their future projects. The data source is google.

We have data containing movie name, directors, actors, budget, gross, imdb score etc. I analysed the data in Microsoft Excel and visualised in Power BI. Before analysing I cleaned the data by removing the duplicates.

First I looked into the Genre list and found out which is the Genre in which most of the movies are made.Comedy, Action and Drama are the top 3 genres in the industry. Now we can check the IMDB scores on specific genres.

![image](https://github.com/AiswaryaPM98/Portfolio/assets/149407441/54eb71bc-4549-482a-8601-8e8e1a6618d3)


|<img src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/5add5f86-c335-4903-ba8e-e6de973b671e" width="300" height="200"/>|<img src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/da7b4e38-5327-4ccb-b755-f21f5463a5ff" width="300" height="200"/>|
|----|----|
|<img src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/b8f86415-71b1-4400-880c-f82637a04a70" width="300" height="200"/>|<img src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/9e2d9b3d-0261-4d38-b429-0617b6fb7e06" width="300" height="200"/>|


Eventhough most of the movies are made in Action, Comedy and Drama genre, the highest imdb scores are for the genre History, Documentary and Biography. Mostly the films in this genre are not commercial filims. The highest ImDB scores indicate the quality of the filim. 

Now lets look into the influence of directors. Here many directors have an average IMDB Scores above 8.5. When we dive deep into the relation between budget, profit and IMDB Scores its clear that budget have no relation with IMDB score. They are not correlated. We can see scenarios where high budget films have low profit and IMDB Scores. It is evident that films with IMDB Score above 8.5 have mighest profit. Also films in the genre Musical, family, animation, horror have more profit than other genre.

<img src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/9877d166-70a4-411c-9f23-11b30a4d039c" width="500" height="200">
<img src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/f743b589-9e56-4ef7-b314-b375c06c4ebd" width="400" height="200">

We can check the budget and profit relation with respect to each genre using an interactive report. [Report](https://app.powerbi.com/links/nywHjCcYKB?ctid=48982c93-be9a-4edc-b6c3-23c9093d7403&pbi_source=linkShare&bookmarkGuid=7e0475a0-11d5-48a6-872f-cfab11f6d092)

**Conclusion :**

•	Mostly commercial films are made in the genre comedy, action and drama. Most profited genre is adventure. 

•	Highest IMDB scores are for the genre documentary, thriller, sci-fi, family, western and the number of movies released in those genre is very few compared to others.

•	Top Directors with average imdb score above 8.5 have released only few films.

•	Budget and Earnings of a film is not at all connected to each other.

Quality of the film matters more than anything no matter how much the budget is. Those directors make only few films. Films from other genre apart from common genre like drama comedy, action scores high in IMDB. For commercial profit comedy, adventure and action would be best.

In this project I learnt advanced function of excel to analyse this data and used power bi to visualise thefindings more effectively.

Thank You.

---
















