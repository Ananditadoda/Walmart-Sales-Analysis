USE walmart;

SELECT * FROM sales;
-- I have imported the data from excel sheet and now let's do feature engineering 

select timee from sales;

SELECT
  CASE
    WHEN CONVERT(TIME, timee) BETWEEN '00:00:00' AND '12:00:00' THEN 'Morning'
    WHEN CONVERT(TIME, timee) BETWEEN '12:01:00' AND '16:00:00' THEN 'Afternoon'
    ELSE 'Evening'
  END AS time_of_day
FROM
  sales;

alter table sales add time_of_day1 VARCHAR (20);


  SELECT time_of_day1 FROM sales;

  update sales 
  set time_of_day1 = 
  CASE
    WHEN CONVERT(TIME, timee) BETWEEN '00:00:00' AND '12:00:00' THEN 'Morning'
    WHEN CONVERT(TIME, timee) BETWEEN '12:01:00' AND '16:00:00' THEN 'Afternoon'
    ELSE 'Evening'
  END

SELECT time_of_day1 FROM sales;

select * from sales;

ALTER TABLE sales
DROP COLUMN time_of_day;

select * from sales;

-- day name 

SELECT 
  date, 
  FORMAT(date, 'dddd') AS day_of_week
FROM 
  sales;

  ALTER TABLE sales ADD day_name varchar(10);

  update sales 
  set day_name = FORMAT(date, 'dddd');

  select * from sales;

  -- Month name
SELECT 
  date, 
  FORMAT(date, 'MMMM') AS month_name
FROM 
  sales;

  alter table sales add month_name varchar(10);

  update sales set month_name =  FORMAT(date, 'MMMM');

  select * from sales;

  -- Exploratory data analysis --------------------------------

  --no. of cities in the data 

  select distinct city from sales;

--in which city is each branch

select distinct branch from sales;

select distinct city, branch from sales;

--no of unique product lines 

select distinct product_line from sales;
select count(distinct product_line) from sales;
select distinct city, branch, product_line from sales;

--payment method that is most common 

select payment, count(payment) as cnt from sales
group by payment
order by cnt desc;

--most selling product line 

select product_line, count(product_line) as cnt from sales
group by product_line
order by cnt desc;

-- total revenue by months
select month_name as month,
sum(total) as total_revenue
from sales
group by month_name
order by total_revenue desc;

--highest cogs 

select month_name as month, 
sum(cogs) as cogs from sales
group by month_name
order by cogs desc;

--product line with largest revenue 

select product_line, sum(total) as total_revenue
from sales
group by product_line 
order by total_revenue desc;

--city with largest revenue 

select branch, city, sum(total) as total_revenue from sales
group by city, branch 
order by total_revenue desc;

--productline having highest VAT 

select product_line as PL,
avg(tax_pct) as avg_VAT from sales
group by product_line
order by avg_VAT desc;

-- which branch did better on avg 

select branch, sum(quantity) as qty from sales
group by branch 
having sum(quantity) > (select avg(quantity) from sales);

-- most common product line by gender 

select gender, product_line, count(gender) as total_cnt from sales 
group by gender, product_line 
order by total_cnt;

--avg rating of each product line

select round(avg(rating),2) as avg_rating,
product_line from sales
group by product_line 
order by avg_rating desc;

