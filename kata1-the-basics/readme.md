# DVD Rental - Part I
----
## Intro

To resolve this kata you will work with dvdrental database.

The DVD rental database represents the business processes of a DVD rental store. The DVD rental database has many objects, including:

15 tables
1 trigger
7 views
8 functions
1 domain
13 sequences
DVD Rental ER Model

![](https://www.postgresqltutorial.com/wp-content/uploads/2018/03/dvd-rental-sample-database-diagram.png)

There are 15 tables in the DVD Rental database:

- actor – stores actors data including first name and last name.
- film – stores film data such as title, release year, length, rating, etc.
- film_actor – stores the relationships between films and actors.
- category – stores film’s categories data.
- film_category- stores the relationships between films and categories.
- store – contains the store data including manager staff and address.
- inventory – stores inventory data.
- rental – stores rental data.
- payment – stores customer’s payments.
- staff – stores staff data.
- customer – stores customer data.
- address – stores address data for staff and customers
- city – stores city names.
- country – stores country names.


The database file is in zipformat ( `dvdrental.zip`) so you need to extract it to  `dvdrental.tar` before loading the sample database into the PostgreSQL database server.

## PREWORK

To load the database follow the steps from this [web-page](https://www.postgresqltutorial.com/postgresql-getting-started/load-postgresql-sample-database/)

One the dvd rental database has been loaded you can start to run the next queries:

*tip*: There may be other ways to achieve the same result.  Remember that SQL commands are not case Waiting for pgAdmin 4 to start...sensitive (but data values are).

### Step1: Describe Commands

Get a list of the tables in the database.

    SELECT table_name FROM information_schema.tables WHERE table_schema='public' AND table_type='BASE TABLE';


### Step2: Select

Get a list of actors with the first name Julia.

    SELECT first_name||' '||last_name AS JuliaActors FROM actor WHERE first_name='Julia';



Get a list of actors with the first name Chris, Cameron, or Cuba.

    SELECT first_name||' '||last_name AS JuliaActors FROM actor WHERE first_name='Chris' OR first_name='Cameron' OR first_name='Cuba';



Select the row from customer for customer named Jamie Rice.

    SELECT *  FROM customer WHERE first_name='Jamie' AND last_name='Rice'; 


Select amount and payment_date from payment where the amount paid was less than $1.

    SELECT amount, payment_date  FROM payment WHERE amount<'1'; 


### Step3: Distinct

What are the different rental durations that the store allows?

    SELECT DISTINCT rental_duration FROM film;

### Step4: Order By

What are the IDs of the last 3 customers to return a rental?

    SELECT customer_id, return_date from rental where return_date is not null order by return_date desc limit 3  ;

    
### Step5: Counting

How many films are rated NC-17?  

    SELECT count(*) FROM film WHERE rating = 'NC-17';

How many are rated PG or PG-13?

    SELECT count(*) FROM film WHERE rating = 'PG' OR rating='PG-13';

Challenge: How many different customers have entries in the rental table?  [Hint](http://www.w3resource.com/sql/aggregate-functions/count-with-distinct.php)

    SELECT count(distinct customer_id ) FROM rental;


### Step6: Group By

Does the average replacement cost of a film differ by rating?

    


Challenge: Are there any customers with the same last name?

## Step7: Functions

What is the average rental rate of films?
    SELECT AVG(rental_rate)FROM film;


Can you round the result to 2 decimal places?
    SELECT ROUND(AVG(rental_rate),2)FROM film;

Challenge: What is the average time that people have rentals before returning?  Hint: the output you'll get may include a number of hours > 24.  You can use the function `justify_interval` on the result to get output that looks more like you might expect.

    

Challenge 2: Select the 10 actors who have the longest names (first and last name combined).

### Step8: Count, Group, and Order

Which film (id) has the most actors?  

    SELECT film_id, count(*) FROM film_actor group by film_id order by count desc limit 1 ;


Which actor (id) is in the most films?

    SELECT actor_id, count(*) FROM film_actor group by actor_id order by count desc limit 1 ;



    

