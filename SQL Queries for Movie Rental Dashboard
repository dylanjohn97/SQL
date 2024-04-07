# SQL Queries for Movie Rental Dashboard

## Table of Contents 

- [Project Overview](#project-overview)
- [Data Cleaning](#data-cleaning)
- [Data Analysis](#data-analysis)
  



### Project Overview

This project aims to transform data from a movie rental database into a format that can be easily consumed by the comapny dashboard.
The transformation process will ensure that the data is properly organized, cleaned, and aggregated, allowing the data to be effectively visualized and analysed it within the dashboard.

### Project Outcome

The final query should include all customer information and a column showing the average difference in days between return_date and rental_date, grouped by customer. 
Segment customers into three groups based on their return times: "GOOD" for those who return (on average) within a week after rental, "FAIR" for those who return within 30 days, and "BAD" for those who take more than 30 days. For the rentals, please only use data from 2005 onwards.


### Data Sources
The primary analysis for this data is the Saskila Database file, containing detailed information about the films, customers and rentals.
For this task, I will be using data from the 'customer' and 'rental' tables.


### Data Cleaning

#### Create a view to return the difference between return date and rental date for each movie rental

```sql
CREATE VIEW rental_return_times AS
SELECT rental_id,
customer_id,
rental_date,
DATEDIFF(IF(ISNULL(return_date), CURRENT_DATE, return_date),rental_date) AS rental_days
FROM rental
```

The rentals that have not been returned will have null values in the return_date column and would not be considered in the return table. 
Therefore, the difference between the rental_date and current_date was calculated to account for this.

![image](https://github.com/dylanjohn97/SQL_Prep_for_Dashboard/assets/165499782/ddf902d6-cd2c-4831-afec-31db6a9bc8cf)

#### Create a view to obtain the avearge movie return time of each customer

```sql
CREATE VIEW customer_avg_return_times AS
SELECT customer_id, AVG(rental_days) AS avg_rental_days
FROM rental_return_times
WHERE YEAR(rental_date) >= 2005
GROUP BY customer_id
```

![image](https://github.com/dylanjohn97/SQL_Prep_for_Dashboard/assets/165499782/58fcd090-4cc3-4204-914d-05f3f6375533)

#### Create a view from the query that feeds the customer dashboard

```sql
CREATE VIEW customer_dashboard AS
SELECT customer.*, 
CASE
WHEN customer_avg_return_times.avg_rental_days <= 7 THEN 'GOOD' 
WHEN customer_avg_return_times.avg_rental_days > 7 AND customer_avg_return_times.avg_rental_days <= 30 THEN 'FAIR'
ELSE 'BAD'
END AS customer_category
FROM customer
INNER JOIN customer_avg_return_times
ON customer_avg_return_times.customer_id = customer.customer_id
```

![image](https://github.com/dylanjohn97/SQL_Prep_for_Dashboard/assets/165499782/096ed5bd-7f0d-4125-b39f-c58aa2829f49)


### Data Analysis 

#### How many customers returns fall into the respective cateories of Good, Fair and Bad?

```sql
SELECT customer_category, COUNT(*) AS customer_returns
FROM customer_dashboard
GROUP BY customer_category
```

![image](https://github.com/dylanjohn97/SQL_Prep_for_Dashboard/assets/165499782/fda840aa-3a5a-4a92-80c2-a0b5416bf657)

##### Insight

There were not any customers who returned their DVD's between 7 and 30 days. 
This shows that if a customer does not return their dvd within a week, then they are unlikely to return it for a while.
It may be beneficial to send reminder emails to customers who do not make returns after 7 days to prompt them into returning the DVD.

#### Which store has a higher rate of 'GOOD' returns?

```SQL
SELECT store_id,customer_category, COUNT(*) AS customer_returns
FROM customer_dashboard
GROUP BY store_id, customer_category
```


![image](https://github.com/dylanjohn97/SQL_Prep_for_Dashboard/assets/165499782/25a4797a-dbaf-4e9c-a864-91a621b0e650)

##### Insight

Store 1 had a 'good' return rate of 74% whereas Store 2 had a 'good' return rate of 73%. This show's that both stores are performing the same. 
To improve on figures, company policy may need to be changed to encourage those with a 'bad' return rate to return their DVD's. 
This can be as simple as adding fines to late returners. 


#### Who are the top 10 customers who take the longest to return dvds? 

```SQL
SELECT first_name,last_name, email, avg_rental_days
FROM customer_avg_return_times
INNER JOIN customer_dashboard
ON customer_dashboard.customer_id = customer_avg_return_times.customer_id
ORDER BY avg_rental_days DESC
LIMIT 10
```

![image](https://github.com/dylanjohn97/SQL_Prep_for_Dashboard/assets/165499782/bed8b7a3-b788-45f9-97bc-ca510a76e84e)

##### Insight

These customers have not returned the DVD's in over a year. It can be assumed that these customers have either forgotted or have no intention of returning the DVDs
To reduce the chance of this happening in the future, new return staregies should be adopted. 
For example, if customers were able to return the DVDs via the post or postal lockers instead of coming back inot the store, it could reduce any inconvience and boost good return rates. 















