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

/* Q2: Let's invite the artists who have written the most rock music in our dataset. 
Write a query that returns the Artist name and total track count of the top 10 rock bands. */

Select Ar.Name, Count(T.track_id)as cnt from Track T
Join Genre G On T.genre_id=G.genre_id
Join ALbum A On A.Album_ID=T.Album_ID
Join Artist Ar ON Ar.Artist_ID=A.Artist_ID
where G.Name Like 'Rock'
group by Ar.Name
Order by cnt desc
Limit 10

/* Q3: Return all the track names that have a song length longer than the average song length. 
Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first. */

Select Name, milliseconds from public.track
where milliseconds>(Select AVG(milliseconds) from public.track)
order by milliseconds desc

/* Question Set 3 - Advance */

/* Q1: Find how much amount spent by each customer on artists? Write a query to return customer name, artist name and total spent */

With best_sell_artist AS (
Select ar.artist_id,ar.name, Sum(il.unit_price*il.quantity) as total from public.invoice_line il
Join track t ON il.track_id=t.track_id
Join album a ON a.album_id=t.album_id
Join public.artist ar ON ar.artist_id=a.artist_id
group by 1
order by 3 desc
limit 1
)

Select c.customer_id,c.first_name,c.last_name,ba.name, SUM(il.unit_price*il.quantity) as total from public.invoice i
join customer c on i.customer_id=c.customer_id
Join public.invoice_line il ON i.invoice_id=il.invoice_id
Join track t ON il.track_id=t.track_id
Join album a ON a.album_id=t.album_id
Join best_sell_artist ba ON ba.artist_id=a.artist_id
group by 1,2,3, 4

/* Q2: We want to find out the most popular music Genre for each country. We determine the most popular genre as the genre 
with the highest amount of purchases. Write a query that returns each country along with the top Genre. For countries where 
the maximum number of purchases is shared return all Genres. */

with populargenre As(
Select count(il.quantity) as count,c.country,g.genre_id,g.name,
Row_number()over (Partition by c.country order by count(il.quantity)desc )as row_no
from  public.invoice_line il
JOIN invoice i ON i.invoice_id = il.invoice_id
	JOIN customer c ON c.customer_id = i.customer_id
	JOIN track t ON t.track_id = il.track_id
	JOIN genre g ON g.genre_id = t.genre_id
	group by 2,3,4
	order by 2 ASC, 1 Desc)

	Select * from populargenre where row_no<=1

 /* Q3: Write a query that determines the customer that has spent the most on music for each country. 
Write a query that returns the country along with the top customer and how much they spent. 
For countries where the top amount spent is shared, provide all customers who spent this amount. */
with top_country_spend AS(
Select c.first_name,c.last_name,c.country,sum(i.total) as total,
Row_number() over(Partition by country order by sum(i.total) desc  ) as row
 from public.invoice i
Join public.customer c ON i.customer_id=c.customer_id
group by 1,2,3
order by 3 asc,4 desc)
Select * from top_country_spend where row <=1
<img width="693" height="554" alt="image" src="https://github.com/user-attachments/assets/9409c6f9-9395-469f-9bae-492d8a0c926e" />

