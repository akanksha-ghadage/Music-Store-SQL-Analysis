# Music-Store-SQL-Analysis
This project analyzes a Music Store database to uncover customer spending patterns, sales trends, top-selling artists and popular music genres using SQL joins, aggregations, subqueries, CTEs and recursive queries for better insights.

## Technology Used

**SQL:** Joins, Aggregations, Subqueries, CTEs, Recursive Queries

**Database:** PostgreSQL


## Dataset Information

The key tables used in this analysis include :

- **customer :** Customer details and purchase history    

- **invoices :** Sales invoices for music purchases

- **tracks :** Song details, duration, genres

- **genre :** Categorizes tracks into different music genres

- **artists :** Store artist details with artist_id and name


## Database Schema
![Database Schema](schema_diagram.png)

## Key SQL Queries and Insights

### 1. Most popular music genre for each country

```sql
WITH popular_genre AS
(
     SELECT COUNT(invoice_line.quantity) AS purchase, customer.country, genre.name, genre.genre_id,
	 ROW_NUMBER() OVER(PARTITION BY customer.country ORDER BY COUNT(invoice_line.quantity)DESC) AS RowNo
	 FROM invoice_line
	 JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id
	 JOIN customer ON customer.customer_id = invoice.customer_id
	 JOIN track ON track.track_id = invoice_line.track_id
	 JOIN genre ON genre.genre_id = track.genre_id
	 GROUP BY 2,3,4
	 ORDER BY 2 ASC, 1 DESC
)
select * from popular_genre WHERE RowNo <= 1

 ```

Insight : Finds the most popular music genre per country based on purchases.

### 2. Customer that has spent the most on music for each country
```sql 
WITH customer_with_country AS(
     SELECT customer.customer_id, first_name, last_name, billing_country, SUM(total) AS total_spending,
	 ROW_NUMBER() OVER(PARTITION BY billing_country ORDER BY SUM(total) DESC) AS RowNo
	 FROM invoice
	 JOIN customer ON customer.customer_id =invoice.customer_id
	 GROUP BY 1,2,3,4
	 ORDER BY 4 ASC, 5 DESC)
SELECT * FROM customer_with_country WHERE RowNo <= 1
```
Insight : Determines the top-spending customer in each country.

### 3. Artists who has written the most Rock music(Top 10 Rock bands)
```sql

select artist.artist_id, artist.name, COUNT(artist.artist_id) AS number_of_songs
from track
JOIN album ON album.album_id = track.album_id
JOIN artist ON artist.artist_id = album.artist_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name LIKE 'Rock'
group by artist.artist_id
order by number_of_songs desc
limit 10;
```
Insight : Identifies the top 10 rock artists based on the number of tracks.

## Full Query File

For all SQL queries, check the queries.sql file in this repository.

## Results and Insights
- The most popular music genre is Rock dominating sales across multiple locations.

- The top spending customer has spent USD 144.54 on the store.

- The USA has the highest number of invoices, while Prague generates the highest total invoice value.

- The most senior employee has been with the company since 2016.

## How to Use This Project 

1. Clone the repository:

```sql
git Clone
https://github.com/akanksha-ghadage/Music-Store-SQL-Analysis.git
cd music-store-sql-analysis
```
2. Open the SQL file in PostgreSQL and run the queries.

3. Analyze the insights from the results.

## Connect with me


**[Linkedin](https://www.linkedin.com/in/akanksha-ghadage?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_contact_details%3BdlFNzzQrTAiOhMUx8JAUmA%3D%3D)**    &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;  &emsp; &emsp; &emsp; &emsp; &emsp;  **[GitHub](https://github.com/akanksha-ghadage)**
