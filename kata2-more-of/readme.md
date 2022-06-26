# DVD Rental - Part II
----

There may be other ways to achieve the same result.  Remember that SQL commands are not case sensitive (but data values and table and column names are).

All of these exercises use the `dvdrental` database.

Exercises often use multiple commands or aspects of SQL, but they are titled/grouped by their focus.

## Step1: Subqueries

What films are actors with ids 129 and 195 in together?

    select title from film where film_id = (select film_id from film_actor where actor_id = 129 or actor_id= 195 group by film_id having count(*) >1 limit 1) 
    or film_id = (select film_id from film_actor where actor_id = 129 or actor_id= 195 group by film_id having count(*) >1 limit 1 offset 1)



Challenge: How many actors are in more films than actor id 47?  Hint: this takes 2 subqueries (one nested in the other).  Work inside out: 1) how many films is actor 47 in; 2) which actors are in more films than this? 3) Count those actors.

    select count(*) from (select count(*) as result_query from film_actor group by actor_id having count(*)> (select count(*) from film_actor where actor_id= 47 group by actor_id)) alias;


## Step2: Inner Joins

Select `first_name`, `last_name`, `amount`, and `payment_date` by joining the customer and payment tables.
    
    select c.first_name, c.last_name, p.amount, p.payment_date from customer c inner join payment p on c.customer_id=p.customer_id ;


Select film\_id, category\_id, and name from joining the film\_category and category tables, only where the category\_id is less than 10.
    
    select p.film_id, p.category_id, c.name from film_category p inner join category c on p.category_id=c.category_id where p.category_id <10;


## Step3: Joining and Grouping: Customer Spending

Get a list of the names of customers who have spent more than $150, along with their total spending.
    
    select c.first_name, c.last_name, sum(p.amount) as total_spent from customer c inner join payment p on c.customer_id = p.customer_id group by c.first_name, c.last_name having sum(p.amount) > 150 

Who is the customer with the highest average payment amount?

    select c.first_name, c.last_name, sum(p.amount) as total_spent from customer c inner join payment p on c.customer_id = p.customer_id 
    group by c.first_name, c.last_name order by  sum(p.amount) desc limit 1


## Step4: Joining for Better Addresses

Create a list of addresses that includes the name of the city instead of an ID number and the name of the country as well.
    
    select a.address, a. district, c.city, a.postal_code, cont.country from address a inner join city c on a.city_id=c.city_id join country cont on c.country_id=cont.country_id


## Step5: Joining Customers, Payments, and Staff

Join the customer and payment tables together with an inner join; select customer id, name, amount, and date and order by customer id.  Then join the staff table to them as well to add the staff's name.
    
    select cus.customer_id,  cus.first_name||' '||cus.last_name as name, p.amount, p.payment_date, st.first_name||' '||st.last_name as staffname from customer cus 
    inner join payment p on cus.customer_id=p.customer_id  inner join staff st on p.staff_id=st.staff_id order by cus.customer_id asc


## Step6: Joining and Grouping: Films and Actors

Repeating an exercise from Part 1, but adding in information from additional tables:  Which film (_by title_) has the most actors?  

     select title from film where film_id = (SELECT film_id FROM film_actor group by film_id order by count(*) desc limit 1)

Which actor (_by name_) is in the most films?

     select ac.first_name||' '||ac.last_name as actor_name from actor ac where actor_id=  (SELECT actor_id  FROM film_actor group by actor_id order by count(*) desc limit 1)
     
Challenge: Which two actors have been in the most films together?  Hint: You can join a table to itself by including it twice with different aliases.  Hint 2: Try writing the query first to find the answer in terms of actor ids (not names); then for a super challenge (it takes a complicated query), rewrite it to get the actor names instead of the IDs.  Hint 3: make sure not to count pairs twice (a in the movie with b and b in the movie with a) and avoid counting cases of an actor being in a movie with themselves.






