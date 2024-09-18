# Music Store Sales Analysis: Insights and Trends
The Music Store Analysis project aims to evaluate various aspects of sales and customer data from a music store to gain insights into employee hierarchy, invoice distribution, customer spending, and music genre preferences. The project involves querying and analyzing data to inform strategic decisions such as promotional events and targeted marketing.
## Features:

**1.Employee Hierarchy Analysis**:
- Identify the senior-most employee based on job title.
- 
**2.Invoice Analysis**:
- Determine which countries have the highest number of invoices.
- Find the top 3 highest total invoices.
- Identify the city with the highest sum of invoice totals to target for promotional events.
- 
**3.Customer Spending Analysis**:
- Determine the best customer based on total spending.
- 
**4.Music Preferences**:
- Identify Rock music listeners and retrieve their contact details.
- Determine the top 10 rock bands based on the number of Rock tracks they have produced.

## SQL Queries Used:

**1.Senior Most Employee**:

SELECT * FROM employee ORDER BY levels DESC LIMIT 1;

**2.Countries with Most Invoices**:

SELECT COUNT(*) AS c, billing_country 
FROM invoice 
GROUP BY billing_country 
ORDER BY c DESC;

**3.Top 3 Invoice Totals**:

SELECT total 
FROM invoice 
ORDER BY total DESC 
LIMIT 3;

**4.City with Highest Invoice Total**:

SELECT SUM(total) AS invoice_total, billing_city 
FROM invoice 
GROUP BY billing_city 
ORDER BY invoice_total DESC 
LIMIT 1;

**5.Best Customer**:

SELECT customer.customer_id, customer.first_name, customer.last_name, SUM(invoice.total) AS total 
FROM customer 
JOIN invoice ON customer.customer_id = invoice.customer_id 
GROUP BY customer.customer_id 
ORDER BY total DESC 
LIMIT 1;

**6.Rock Music Listeners**:

SELECT DISTINCT email, first_name, last_name 
FROM customer 
JOIN invoice ON customer.customer_id = invoice.customer_id 
JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id 
WHERE track_id IN (
    SELECT track_id 
    FROM track 
    JOIN genre ON track.genre_id = genre.genre_id 
    WHERE genre.name = 'Rock'
) 
ORDER BY email;

**7.Top 10 Rock Bands by Track Count**:

SELECT artist.artist_id, artist.name, COUNT(track.track_id) AS number_of_songs 
FROM track 
JOIN album ON track.album_id = album.album_id 
JOIN artist ON album.artist_id = artist.artist_id 
JOIN genre ON track.genre_id = genre.genre_id 
WHERE genre.name = 'Rock' 
GROUP BY artist.artist_id 
ORDER BY number_of_songs DESC 
LIMIT 10;
