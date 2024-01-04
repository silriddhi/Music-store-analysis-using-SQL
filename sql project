--senior most employee based on age
select first_name,age(current_date,birthdate)as age from employee
order by age desc
limit 1

--  senior most employee based on job title
whselect * from employee
order by levels desc
limit 1

--countries with most invoices
select billing_country,count(*) as count_of_invoice from invoice
group by billing_country
order by count_of_invoice desc

-- top3 values of total invoice
select total from invoice
order by total desc
limit 3

--Write a query that returns one city that has the highest sum of invoice totals. 
--Return both the city name & sum of all invoicetotals
select billing_city,sum(total)as invoice_total from invoice
group by billing_city
order by invoice_total desc
limit 1

--Write a query that returns the person who has spent the most money
select first_name,last_name,sum(total)as money_spent from customer c
join invoice i on
c.customer_id=i.customer_id
group by first_name,last_name
order by money_spent desc
limit 1

--Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
--Return your list ordered alphabetically by email starting with A
select distinct first_name,last_name,email from customer c
join invoice i on c.customer_id=i.customer_id
join invoice_Line il on i.invoice_id=il.invoice_id
join track t on
il.track_id=t.track_id
join genre g on
t.genre_id=g.genre_id
where g.name like 'Rock'
order by email
-- OR 2nd method is better bcz multiple joins increases the time for processing
select distinct first_name,last_name,email from customer c
join invoice i on c.customer_id=i.customer_id
join invoice_Line il on i.invoice_id=il.invoice_id
where track_id in ( select track_id from track t 
                  join genre g on
                  t.genre_id=g.genre_id
                  where g.name like 'Rock')
                  order by email
                  
--Let's invite the artists who have written the most rock music in our dataset.
--Write a query that returns the Artist name and total track count of the top 10 rock bands
select ar.name,count(*)as no_of_songs from track t
join album a on t.album_id=a.album_id
join artist ar on a.artist_id=ar.artist_id
join genre g on t.genre_id=g.genre_id
where g.name like 'Rock'
group by ar.name
order by no_of_songs desc

--Return all the track names that have a song length longer than the average song length.
--Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first
select name,milliseconds as song_length from track
where milliseconds > (select avg(milliseconds) from track)
order by song_length desc

--Find how much amount spent by each customer on artists? Write a query to return customer name, artist name and total spent
select concat(c.first_name,c.last_name) as full_name,ar.name,sum(il.unit_price*il.quantity) from customer c
join invoice i on c.customer_id=i.customer_id
join invoice_Line il on i.invoice_id=il.invoice_id
join track t on il.track_id=t.track_id
join album a on t.album_id=a.album_id
join artist ar on a.artist_id=ar.artist_id
group by c.first_name,c.last_name,ar.name
order by 3 desc

--Write a query that returns each country along with the top Genre. For countries where the maximum
--number of purchases is shared return all Genres

with cte as (select billing_country as country,g.name,count(*) as purchases,row_number() over(partition by billing_country order by count(*) desc) as rnk
from invoice i
join invoice_line il on i.invoice_id=il.invoice_id
join track t on il.track_id=t.track_id
join genre g on t.genre_id=g.genre_id
group by billing_country,g.name  )
select * from cte where rnk=1

--Write a query that determines the customer that has spent the most on music for each
--country. Write a query that returns the country along with the top customer and how
--much they spent. For countries where the top amount spent is shared, provide all
--customers who spent this amount

with cte as (select c.first_name,c.country,sum(total) as purchases,dense_rank() over(partition by c.country order by sum(total) desc) as rnk
from invoice i
join customer c on i.customer_id=c.customer_id
group by c.first_name,c.country  )
select * from cte where rnk=1

