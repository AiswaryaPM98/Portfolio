
# Aiswarya P M

### About Me

I am Aiswarya P M from Kerala. With a solid educational background in Mathematics, an Operations professional with hands-on experience in process management, data handling, and cross-functional coordination. Currently working in an operations role and actively seeking an opportunity in data analysis to apply analytical skills, problem-solving abilities, and a strong interest in data-driven decision-making.

---

### Table Of Contents
1. [Zepto Inventory Analysis](#Zepto-Inventory-Analysis) - PostgreSQL

2. [Operation Analytics and Investigating Metric Spike](#operation-analytics-and-investigating-metric-spike) - MySQL

3. [Marketing AD Campaign Analysis](#marketing-ad-campaign-analysis) - Power BI

4. [Movie Analysis](#movie-analysis) - Excel, Power BI 

---

### Zepto Inventory Analysis.

This project involved performing a complete inventory analysis for Zepto, a quick-commerce grocery platform, using SQL. The goal was to study product availability, identify inconsistencies, calculate revenue, analyze discounts, and understand product performance across categories.

A structured dataset was created containing product information such as SKU ID, category, MRP, discount percentage, stock levels, and weight. The data was explored for duplicates, null values, and incorrect price formats. After cleaning and correcting the data, several business questions were answered using SQL.

To Create a new table named Zepto  with columns as sku id(stock keeping unit), Name, Category, MRP, Discount Percentage, Avaliable Quantity, Discounted Selling Price, Weight in Gms, Out of Stock and Quantity.

```
CREATE TABLE Zepto (
sku_id SERIAL PRIMARY KEY,
category VARCHAR(50),
Name VARCHAR(100) not null,
mrp NUMERIC(8,2),
discountPercentage NUMERIC(5,2),
availableQuantity INTEGER,
discountedSellingPrice NUMERIC(8,2),
weightInGms INTEGER,
outofStock BOOLEAN,
quantity INTEGER
);
```
Data Exploration:

```
SELECT COUNT (*) FROM zepto;
```
<img width="277" height="146" alt="image" src="https://github.com/user-attachments/assets/de32fed3-0449-4dfd-91b9-0652f0721b25" />

Here we have total 3731 rows. Here is the sample data from the table -

```
SELECT * FROM zepto
LIMIT 10
```
<img width="781" height="218" alt="image" src="https://github.com/user-attachments/assets/25d622a5-ef95-49d3-a80a-629fee2cde36" />

Different categories of products mentioned in the table -
```
SELECT DISTINCT category FROM zepto
ORDER BY category;
```
<img width="146" height="294" alt="image" src="https://github.com/user-attachments/assets/1a911c6c-0aed-482a-97e1-34278330a815" />

Lets find out product names which are repeated multiple times-
```
SELECT NAME, COUNT(sku_id) AS "Number of SKUs"
FROM zepto
GROUP BY name
HAVING COUNT(sku_id)>1
ORDER BY COUNT(sku_id) DESC;
```
<img width="519" height="391" alt="image" src="https://github.com/user-attachments/assets/4bd3570b-d073-4871-837b-0f8fc1ad650b" />

Some products are entered multiple times, which may occur when the same product is listed with different quantities or features.

Now, lets clean the data, First of all lokking for any null values-

```
SELECT * FROM zepto
WHERE name IS NULL
OR
category IS NULL
OR
mrp IS NULL
OR
discountpercentage IS NULL
OR
availablequantity IS NULL
OR
discountedsellingprice IS NULL
OR
weightingms IS NULL
OR
outofstock IS NULL
OR
quantity IS NULL;
```
<img width="776" height="136" alt="image" src="https://github.com/user-attachments/assets/52810209-f72d-4d27-9033-66716b77cc03" />

There is no null values. Now lets chcek if there is any product with mrp or discounted selling price listed as zero which should never happen.

```
SELECT * FROM zepto
WHERE mrp = 0
OR discountedSellingPrice = 0;
```
We have no products with an MRP or discounted selling price of 0. If there is any such entry, we can either delete it or update it with the correct data.

As we check the data, we can see that the MRP and discounted selling price are listed with incorrect decimals. We need to change them to the correct INR amounts.

<img width="800" height="200" alt="image" src="https://github.com/user-attachments/assets/46337144-8ce7-4de3-a495-ae6e5aeff940" />

```
UPDATE zepto
SET mrp = mrp/100,
discountedsellingprice = discountedsellingprice/100;
```
<img width="272" height="320" alt="image" src="https://github.com/user-attachments/assets/4f98d087-a69b-4170-b970-709123f4c706" />

We have corected and cleaned the data. Now lets try to answer some business questions.

Now lets try to answer some business questions.

#### Q1: The top 10 best value products based on the discount percentage.

```
SELECT DISTINCT name, mrp, discountpercentage from zepto
ORDER BY discountpercentage DESC 
limit 10;
```
<img width="470" height="265" alt="image" src="https://github.com/user-attachments/assets/00ceeb78-40a8-47ca-8e78-55f9e6322649" />

This will help us identify which products are heavily promoted. To find the least promoted products, we can use the SQL query below.

```
SELECT DISTINCT name, mrp, discountpercentage from zepto
WHERE discountpercentage=0
ORDER BY mrp DESC;
```
<img width="547" height="392" alt="image" src="https://github.com/user-attachments/assets/3f2049c4-c67a-4592-a00c-6026ce43b61b" />

This will give us the products with no discounts applied. These 535 products may be the best-selling essential items, which is why no discount was offered.

#### Q2: Which are the products with high mrp which are out of stock?

If there are any such products, it would result in a loss of revenue if we don’t restock them.

Lets find the total out of products -

```
SELECT DISTINCT name, mrp FROM zepto
WHERE outofstock= true
ORDER BY mrp DESC;
```
<img width="401" height="373" alt="image" src="https://github.com/user-attachments/assets/9ca3f205-6923-4be8-a406-4c5d1e433148" />

Lets find the number of products with mrp>300 which are outofstock -

```
SELECT DISTINCT name, mrp FROM zepto
WHERE outofstock= true
AND mrp > 300
ORDER BY mrp DESC;
```
<img width="362" height="175" alt="image" src="https://github.com/user-attachments/assets/46e90d24-090d-4874-ba35-efa21a486e6e" />

We have a total of 216 products that are out of stock, and among them, 4 products have an MRP greater than ₹300.

#### Q3: Calculate each category Revenue.

To calulate the category revenue we using the SUM formula 
```
SELECT category,
SUM(discountedsellingprice*availablequantity) as Total_Revenue
FROM zepto
group by category
ORDER BY Total_Revenue DESC;
```
<img width="227" height="349" alt="image" src="https://github.com/user-attachments/assets/ca04f3df-1975-41c7-bcad-b0e4a2428683" />

There are 14 product categories, and cooking essentials and munchies generate the highest revenue. Fruits and vegetables have the lowest revenue.

Let's find the products with an MRP greater than ₹300 in the cooking essentials category.

```
SELECT DISTINCT name, mrp, discountpercentage FROM zepto
WHERE category = 'Cooking Essentials' 
AND mrp >300
ORDER BY mrp DESC;
```

<img width="425" height="389" alt="image" src="https://github.com/user-attachments/assets/e8ac42c5-ab8c-4cb7-82bd-c7e7bba891a6" />

We have 55 products with an MRP greater than ₹300, which make up a significant portion of the revenue in the cooking essentials category.

#### Q4: Find all product where mrp is greater than 500 and discount is less than 10%.

```
SELECT DISTINCT name, mrp, discountpercentage FROM zepto
WHERE mrp > 500
AND discountpercentage < 10
ORDER BY mrp DESC ;
```
<img width="539" height="374" alt="image" src="https://github.com/user-attachments/assets/b2fee0a1-41c7-47d0-b7d4-93e7cb603b4e" />

There are 39 products that are not premium but are well-known and will sell even without discounts or with minimal discounts.

#### Q5: Identify the average discount percentage offered by each category.

```
SELECT category,
ROUND(AVG(discountpercentage),2) AS Avg_Discount
FROM zepto
GROUP BY category
ORDER BY  Avg_Discount DESC;
```
<img width="221" height="341" alt="image" src="https://github.com/user-attachments/assets/2c50f273-39a9-433d-b825-2ba7b64643ff" />

Fruits and vegetables have the highest average discount, yet they generate the least revenue. The team should investigate why this category has collected such low revenue even after offering discounts.

#### Q6: Find price per gram for product above 100g and sort by best value.

```
SELECT DISTINCT name, weightingms, discountedsellingprice,
ROUND((discountedsellingprice/weightingms),2) AS Price_Per_Gram
FROM zepto
WHERE weightingms > 100
ORDER BY Price_Per_Gram;
```
<img width="661" height="396" alt="image" src="https://github.com/user-attachments/assets/d8e95048-cfe1-4e8c-a399-06d4f80bb9d2" />

There are 1,239 products that weigh more than 100 grams. The price per gram will help customers identify the best-value products and also support internal pricing and strategy decisions.

#### Q7: Calculate total inventory weight per category

```
SELECT category,
SUM(weightingms*availablequantity/1000) AS Total_Weight_in_KG
FROM zepto
GROUP BY category
ORDER BY Total_Weight_in_KG DESC;
```
<img width="242" height="365" alt="image" src="https://github.com/user-attachments/assets/ca2563e4-021c-475a-b360-27865d3200d8" />

This helps in warehouse planning and in identifying bulky categories.

#### Key Learnings from the project:-

1. Data Cleaning & Validation
Understood how to detect duplicates, missing values, and impossible values like MRP = 0.
Corrected price formatting errors by adjusting decimal values.
Identified repeated product entries and learned why they occur.

2. Importance of Stock Accuracy
Analysed out-of-stock products and highlighted revenue risks when high-value items are unavailable.
Understood the impact of accurate inventory tracking on customer experience and sales.

3. Insights from Discount Analysis
Identified products with the highest discounts to understand promotional strategies.
Found products with zero discounts, indicating essential or high-demand items.
Observed that fruits and vegetables have the highest average discount but lowest revenue—indicating inefficiency in marketing or strategy.

4. Revenue Calculation & Category Performance
Calculated total revenue by category using selling price and available quantity.
Identified strong revenue-generating categories (cooking essentials, munchies).
Detected underperforming categories to guide business decisions.

5. Price–Value Understanding
Computed price per gram to understand value efficiency, helping in pricing strategy.
Useful for customers, pricing teams, and competitive analysis.

6. Inventory Planning & Operations Insights
Calculated total inventory weight per category for warehouse planning.
Identified bulky categories that require more storage and handling effort.

The analysis revealed patterns in stock availability, top-selling categories, heavily promoted products, least-promoted products, and out-of-stock high-value items. Category-level revenue was calculated, showing that cooking essentials and munchies generate the highest revenue, while fruits and vegetables perform poorly despite having the highest discounts. Metrics such as price per gram and total inventory weight were also derived to support pricing and warehouse planning decisions.


---
### Operation Analytics and Investigating Metric Spike.

This project focuses on analyzing the data which is provided by company. My task is to derive insights. So that these insights can be used by ops team, suppoert team, marketing team etc to predict the overall growth or decline of a company.We have 2 cases, Job data and users, evnets, email events tables.

In case study 1 the insights are found on following question:
1. Number of jobs reviewed: Amount of jobs reviewed over time.
2. Throughput : Calculate 7 day rolling average of throughput? For throughput, do you prefer metric or 7-day rolling and why?
3. Percentage share of each language : Share of each language for different contents.

In case study 2 the insights are found based on following questions :
1. User Engagement : To measure the activeness of a user.
2. User Growth : Amount of users growing over time for a product.
3. Weekly Retention : Users getting retained weekly after signing up for  aproduct.
4. Weekly Engagement : To measure the activeness of a user. Measuring if the user finds quality in a service weekkly.
5. Email Engagement : User engaging with email service.

This project is developed using SQL. First , created a database using the file and loaded the data into SQL Workbench. Then analysed the data.

**Case Study 1 :**

To check the duplicate:


``` duplicates
SELECT actor_id, COUNT(*) as Duplicates
FROM operational_analysis.job_data
group by actor_id
having count(*)>1 ;
```

<img width="116" alt="1" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/99cf374d-c196-40db-b2d4-655cc1cd047b">

**Number of jobs reviewed: Amount of jobs reviewed over time :**

```
SELECT ds as Dates, Round((count(job_id)/sum(time_spent))*3600) as Jobs_Reviewed_per_hour
FROM operational_analysis.job_data
group by ds;
```

<img width="180" alt="2" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/28852d99-9869-4a33-a255-a4693869bdf2">


Max no. of jobs were reviewed on 28-11-2020 and minimum jobs were reviewed on 27-11-2020.

**Throughput : Calculate 7 day rolling average of throughput? For throughput, do you prefer metric or 7-day rolling and why?**

```
select round(count(event)/sum(time_spent),2) as Weekly_Throughput
FROM operational_analysis.job_data;
```

<img width="106" alt="3" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/a7a68afc-ad89-4486-9467-3f720331e985">

``` 
select ds as Date, round(count(event)/sum(time_spent),2) as Daily_Throughput,
avg(count(event)/sum(time_spent)) over (order by ds desc ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS Rolling_Avg
FROM operational_analysis.job_data
group by ds
order by ds;
```

<img width="203" alt="4" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/3895a0ef-2079-4e08-a6c8-868f66f2a476">

Daily Throughput and rolling average is highest on 28-11-2020. Rolling metrics helps us to show if the metrics is going up or down as the values changes on daily, weekly, monthly or yearly basis.

**Percentage share of each language : Share of each language for different contents.**

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

**Case Study 2 :**

**User Engagement : To measure the activeness of a user.**

```
select extract(week from occured_at) as Week_Number ,
count(distinct user_id) as Weekly_Active_Users
from operational_analysis.events
where event_type = 'engagement'
group by Week_Number;
```


 <img width="176" alt="7" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/3e11308e-118a-46b5-af18-921806146457">

Users were mostly engaged in the weeks : 

<img width="175" alt="8" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/93554dc1-5ea5-4883-a731-1a04fc4979bb">

**User Growth : Amount of users growing over time for a product.**

```
select extract(month from Created_Date) as Months, count(Activated_date) as Users
from(
select 
convert (created_at, DATE) as Created_Date,
convert (activated_at, DATE) as Activated_Date
from operational_analysis.users_1) as date_change
group by 1
order by 1;
```

Inner query changed the datetime to date only format.

<img width="363" alt="9" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/13fb586c-8224-4810-9916-ee234af34b49"> <img width="89" alt="10" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/ab9cec18-4c96-4773-a448-0f1c1a44e684">

The user growth increases by month and by june, july, august the growth is at peak.After August growth drops.

**Weekly Retention : Users getting retained weekly after signing up for  aproduct.**

```
select Start_Week as Week,
sum(case when Week_Number = 0 then 1 else 0 end) as Week_0,
sum(case when Week_Number = 1 then 1 else 0 end) as Week_1,
sum(case when Week_Number = 2 then 1 else 0 end) as Week_2,
sum(case when Week_Number = 3 then 1 else 0 end) as Week_3,
sum(case when Week_Number = 4 then 1 else 0 end) as Week_4,
sum(case when Week_Number = 5 then 1 else 0 end) as Week_5,
sum(case when Week_Number = 6 then 1 else 0 end) as Week_6,
sum(case when Week_Number = 7 then 1 else 0 end) as Week_7,
sum(case when Week_Number = 8 then 1 else 0 end) as Week_8,
sum(case when Week_Number = 9 then 1 else 0 end) as Week_9,
sum(case when Week_Number = 10 then 1 else 0 end) as Week_10,
sum(case when Week_Number = 11 then 1 else 0 end) as Week_11,
sum(case when Week_Number = 12 then 1 else 0 end) as Week_12,
sum(case when Week_Number = 13 then 1 else 0 end) as Week_13,
sum(case when Week_Number = 14 then 1 else 0 end) as Week_14,
sum(case when Week_Number = 15 then 1 else 0 end) as Week_15,
sum(case when Week_Number = 16 then 1 else 0 end) as Week_16,
sum(case when Week_Number = 17 then 1 else 0 end) as Week_17,
sum(case when Week_Number = 18 then 1 else 0 end) as Week_18,
sum(case when Week_Number = 19 then 1 else 0 end) as Week_19,
sum(case when Week_Number = 20 then 1 else 0 end) as Week_20,
sum(case when Week_Number = 21 then 1 else 0 end) as Week_21,
sum(case when Week_Number = 22 then 1 else 0 end) as Week_22,
sum(case when Week_Number = 23 then 1 else 0 end) as Week_23,
sum(case when Week_Number = 24 then 1 else 0 end) as Week_24,
sum(case when Week_Number = 25 then 1 else 0 end) as Week_25,
sum(case when Week_Number = 26 then 1 else 0 end) as Week_26,
sum(case when Week_Number = 27 then 1 else 0 end) as Week_27,
sum(case when Week_Number = 28 then 1 else 0 end) as Week_28,
sum(case when Week_Number = 29 then 1 else 0 end) as Week_29
from (
select m.user_id, m.Login_Week, n.Start_Week, m.Login_Week as Week_Number
from (
(select user_id, extract( week from(convert(occured_at, date))) as Login_Week
from operational_analysis.events
group by 1, 2) as m ,
(select user_id, min(extract(week from(convert(occured_at, date)))) as Start_Week
from operational_analysis.events
group by 1) as n )
where m.user_id = n.user_id ) as Retention
group by Start_Week
order by Start_Week;
```

<img width="753" alt="11" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/23f1144f-b398-4dfe-a147-d3baba26e1fb">

**Weekly Engagement : To measure the activeness of a user. Measuring if the user finds quality in a service weekkly.**

```
select device, count(device) as count
from operational_analysis.events
group by device
order by count desc;
```

<img width="152" alt="12" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/73f723d6-fd94-4ced-89ff-aa963b45bd9e">

```
select extract(week from(convert(occured_at, date))) as Week_Number,
count(distinct case when device in('macbook pro') then user_id else null end) as Macbook_Pro,
count(distinct case when device in('lenove thinkpad') then user_id else null end) as Lenove_Thinkpad,
count(distinct case when device in('macbook air') then user_id else null end) as Macbook_Air,
count(distinct case when device in('iphone 5') then user_id else null end) as Iphone_5,
count(distinct case when device in('dell inspiron notebook') then user_id else null end) as Dell_Inspiron_Notebook,
count(distinct case when device in('samsung galaxy s4') then user_id else null end) as Samsung_Galaxy_S4,
count(distinct case when device in('nexus 5') then user_id else null end) as Nexus_5,
count(distinct case when device in('iphone 5s') then user_id else null end) as Iphone_5s,
count(distinct case when device in('dell inspiron desktop') then user_id else null end) as Dell_Inspiron_Desktop,
count(distinct case when device in('iphone 4s') then user_id else null end) as Iphone_4s,
count(distinct case when device in('asus chromebook') then user_id else null end) as Asus_Chromebook,
count(distinct case when device in('ipad air') then user_id else null end) as Ipad_Air,
count(distinct case when device in('acer aspire notebook') then user_id else null end) as Acer_Aspire_Notebook,
count(distinct case when device in('hp pavillion desktop') then user_id else null end) as HP_Pavillion_Desktop,
count(distinct case when device in('nexus 7') then user_id else null end) as Nexus_7,
count(distinct case when device in('nokia lumia 635') then user_id else null end) as Nokia_Lumia_635,
count(distinct case when device in('ipad mini') then user_id else null end) as Ipad_Mini,
count(distinct case when device in('acer aspire desktop') then user_id else null end) as Acer_Aspire_Desktop,
count(distinct case when device in('mnexus 10') then user_id else null end) as Nexus_10,
count(distinct case when device in('mac mini') then user_id else null end) as Mac_Mini,
count(distinct case when device in('htc one') then user_id else null end) as HTC_One,
count(distinct case when device in('kindle fire') then user_id else null end) as Kindle_Fire,
count(distinct case when device in('windows surface') then user_id else null end) as Windows_Surface,
count(distinct case when device in('samsung galaxy note') then user_id else null end) as Samsung_Galaxy_Note,
count(distinct case when device in('amazon fire phone') then user_id else null end) as Amazon_Fire_Phone,
count(distinct case when device in('samsung galaxy tablet') then user_id else null end) as Samsung_Galaxy_Tablet
from operational_analysis.events
where event_type = 'engagement'
group by 1
order by 1;
```

<img width="749" alt="13" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/234214eb-608d-46b9-bd9c-bf155673aaf3">

<img width="707" alt="14" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/09086d80-a1b1-4795-9db8-8daf4bca6a39">

<img width="413" alt="15" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/2c937814-008d-406d-8467-4c3e0acbf701">

**Email Engagement : User engaging with email service.**

```
select Week,
round((Weekly_Digest/Total*100),2) as Weekly_Digest_Rate,
round((Email_Open/Total*100),2) as Email_Open_Rate,
round((Email_Clickthrough/Total*100),2) as Email_Clickthrough_Rate,
round((Reengagement_Emails/Total*100),2) as Reengagement_Emails_Rate
from (
select extract(week from(convert(occured_at, date))) as Week,
count(case when action = 'sent_weekly_digest' then user_id else null end) as Weekly_Digest,
count(case when action = 'email_open' then user_id else null end) as Email_Open,
count(case when action = 'email_clickthrough' then user_id else null end) as Email_Clickthrough,
count(case when action = 'sent_reengagement_email' then user_id else null end) as Reengagement_Emails,
count(user_id) as Total
from operational_analysis.email_events
group by 1) as Email_Engagement_Metrics
group by 1
order by 1;
```

<img width="435" alt="16" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/8f0aec67-f6c4-412c-be94-a8c86a1eb3ec">

<img width="429" alt="17" src="https://github.com/AiswaryaPM98/Portfolio/assets/149407441/b11d9872-2330-483b-87a2-13644189a461">


This project helped me to understand the importance of operation analytics. I am able to understand how companies use the metric spike to leverage insights to make data driven decisions.


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

**My Findings:**

•	Mostly commercial films are made in the genre comedy, action and drama. Most profited genre is adventure. 

•	Highest IMDB scores are for the genre documentary, thriller, sci-fi, family, western and the number of movies released in those genre is very few compared to others.

•	Top Directors with average imdb score above 8.5 have released only few films.

•	Budget and Earnings of a film is not at all connected to each other.

Quality of the film matters more than anything no matter how much the budget is. Those directors make only few films. Films from other genre apart from common genre like drama comedy, action scores high in IMDB. For commercial profit comedy, adventure and action would be best.

In this project I learnt advanced function of excel to analyse this data and used power bi to visualise thefindings more effectively.

Thank You.


---
















