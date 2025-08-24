# -SQL_Music_Store_Analysis

/*Q1) Who is the Senior Most Employee based on the job title? */

Select * from Employee
order by levels desc
limit 1

/* Q2: Which countries have the most Invoices? */

Select billing_country,count(invoice_id) from public.invoice
group by 1
order by count(invoice_id) desc
limit 1

/* Q3: What are top 3 values of total invoice? */

Select invoice_id,total from public.invoice
order by total desc
limit 3

/* Q4: Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. 
Write a query that returns one city that has the highest sum of invoice totals. 
Return both the city name & sum of all invoice totals */

/* Q5: Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. 
Write a query that returns one city that has the highest sum of invoice totals. 
Return both the city name & sum of all invoice totals */

Select billing_city, Sum(total) as total from public.invoice
group by billing_city
order by total desc
