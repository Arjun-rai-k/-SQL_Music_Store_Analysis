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

/* Q6: Who is the best customer? The customer who has spent the most money will be declared the best customer. 
Write a query that returns the person who has spent the most money.*/

Select c.first_name ||' '|| c.last_name as name,sum(i.total) as total from public.invoice as i
Join public.customer as c ON c.customer_id=i.customer_id
group by name
Order by total desc

/* Question Set 2 - Moderate */

/* Q1: Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
Return your list ordered alphabetically by email starting with A. */

Select distinct c.email,c.first_name,c.last_name,G.name from public.customer as c
Join public.invoice as i ON i.customer_id=c.customer_id
Join public.invoice_line as il ON il.invoice_id=i.invoice_id
Join public.track as t ON t.track_id=il.track_id
Join (Select Name, genre_id from public.genre 
where Name='Rock') as g ON g.genre_id=t.genre_id
order by c.email ASC

