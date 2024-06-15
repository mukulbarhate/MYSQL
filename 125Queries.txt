1. List all the columns of the Salespeople table.

desc salespeople;


2. List all customers with a rating of 100.

select * from customers where rating=100;


3. Find all records in the Customer table with NULL values in the city column.

select * from customers where city=null;


4. Find the largest order taken by each salesperson on each date.

select sname,amt,odate from(select sname,odate,amt,dense_rank() over(partition by sname,odate order by amt desc) rn from salespeople s 
join orders o on s.snum=o.snum) s where rn=1;


5. Arrange the Orders table by descending customer number.

select * from orders order by cnum desc;


6. Find which salespeople currently have orders in the Orders table.

select distinct(sname) from salespeople s join orders o on s.snum=o.snum;


7. List names of all customers matched with the salespeople serving them.

select cname,sname from customers c join salespeople s on c.snum=s.snum;


8. Find the names and numbers of all salespeople who had more than one customer.

select sname,s.snum,count(sname) from customers c join salespeople s on c.snum=s.snum group by sname,snum having count(sname)>1;


9. Count the orders of each of the salespeople and output the results in descending order.

select sname,count(o.snum) count_of_orders from salespeople s join orders o on s.snum=o.snum group by sname order by count(o.snum) desc;


10. List the Customer table if and only if one or more of the customers in the Customer table are located in San Jose.

select * from customers where 'san jose' in(select city from customers);
				OR
select * from customers where exists(select 1 from customers where city='san jose');


11. Match salespeople to customers according to what city they lived in.

select sname,cname,s.city from salespeople s join customers c on s.city=c.city and s.snum=c.snum;


12. Find the largest order taken by each salesperson.

select sname,amt from(select sname,amt,dense_rank() over(partition by sname order by amt desc) rn from salespeople s
join orders o on s.snum=o.snum) q where rn=1 ;


13. Find customers in San Jose who have a rating above 200.

select cname,city,rating from customers where city='san jose' and rating>200;


14. List the names and commissions of all salespeople in London.

select sname,comm from salespeople where city='london';


15. List all the orders of salesperson Motika from the Orders table.

select * from orders where snum =(select snum from salespeople where sname='motika');


16. Find all customers with orders on October 3.

select * from customers where cnum in (select cnum from orders where odate like '%03-10%');


17. Give the sums of the amounts from the Orders table, grouped by date, eliminating all those
dates where the SUM was not at least 2000.00 above the MAX amount.

select sum(amt),odate from orders group by odate having sum(amt)<max(amt)+2000;


18. Select all orders that had amounts that were greater than at least one of the orders from October 6.

select * from orders where amt > any(select amt from orders where odate like'%06-10%') ;


19. Write a query that uses the EXISTS operator to extract all salespeople who have customers with a rating of 300.

select * from salespeople s where exists(select 1 from customers c where s.snum=c.snum and rating=300);


20. Find all pairs of customers having the same rating.

select c.cname,c1.cname,c.rating from customers c join customers c1 on c.rating=c1.rating where c.cname>c1.cname;


21. Find all customers whose CNUM is 1000 above the SNUM of Serres.

select * from customers where cnum>=((select snum from salespeople where sname='serres')+1000);


22. Give the salespeople’s commissions as percentages instead of decimal numbers.

select concat(comm*100,'%') percentage from salespeople;


23. Find the largest order taken by each salesperson on each date, eliminating those MAX orders which are less than $3000.00 in value.

select sname,amt,odate from (select sname,amt,odate,dense_rank()over(partition by sname,odate order by amt desc) rn from salespeople s 
join orders o on s.snum=o.snum) q where rn=1 and amt>3000;


24. List the largest orders for October 3, for each salesperson.

select sname,amt,odate from (select sname,amt,odate,dense_rank()over(partition by sname order by amt desc) rn from salespeople s 
join orders o on s.snum=o.snum where odate like '%03-10%') q where rn=1;


25. Find all customers located in cities where Serres (SNUM 1002) has customers.

select * from customers where city in (select city from customers where snum in(select snum from salespeople where sname='serres'));


26. Select all customers with a rating above 200.00.

select * from customers where rating>200;


27. Count the number of salespeople currently listing orders in the Orders table.

select count(sname) no_of_salespeople from salespeople where snum in(select snum from orders);


28. Write a query that produces all customers serviced by salespeople with a commission above 12%. 
Output the customer’s name and the salesperson’s rate of commission.

select cname,comm from customers c join salespeople s on c.snum=s.snum where comm>0.12;


29. Find salespeople who have multiple customers.

select * from salespeople where snum in(select snum from customers group by snum having count(snum)>1);
						OR
select sname,count(sname) count_of_customers from salespeople s join customers c on s.snum=c.snum group by sname having count(sname)>1;


30. Find salespeople with customers located in their city.

select s.sname,c.cname,s.city from salespeople s join customers c on s.city=c.city and s.snum=c.snum;


31. Find all salespeople whose name starts with ‘P’ and the fourth character is ‘l’.

select * from salespeople where sname like 'p__l%';


32. Write a query that uses a subquery to obtain all orders for the customer named Cisneros.
Assume you do not know his customer number.

select * from orders where cnum in(select cnum from customers where cname='cisneros');


33. Find the largest orders for Serres and Rifkin.

select * from(select *,dense_rank()over(partition by snum order by amt desc) rn from orders where snum in(select snum from 
salespeople where sname in('serres','rifkin'))) q where rn=1;


34. Extract the Salespeople table in the following order : SNUM, SNAME, COMMISSION, CITY.

select snum,sname,comm,city from salespeople;


35. Select all customers whose names fall in between ‘A’ and ‘G’ alphabetical range.

select * from customers where cname between 'A' and 'G' order by cname;


36. Select all the possible combinations of customers that you can assign.

select c.cname,c1.cname from customers c join customers c1 on c.cname<c1.cname;


37. Select all orders that are greater than the average for October 4.

select * from orders where amt>(select avg(amt) from orders where odate like '%04-10%');


38. Write a select command using a corelated subquery that selects the names and numbers of all customers with 
ratings equal to the maximum for their city.

select cnum,cname,city from customers c where rating in (select max(rating) from customers c1 where c.city=c1.city);


39. Write a query that totals the orders for each day and places the results in descending order.

select odate, sum(amt) from orders group by odate order by sum(amt) desc;


40. Write a select command that produces the rating followed by the name of each customer in San Jose.

select cname,rating from customers where city='san jose';


41. Find all orders with amounts smaller than any amount for a customer in San Jose.

select * from orders where amt < any(select amt from orders where cnum in(select cnum from customers where city='san jose'));


42. Find all orders with above average amounts for their customers.

select * from orders o where amt>(select avg(amt) from orders o1 where o.cnum=o1.cnum);


43. Write a query that selects the highest rating in each city.

select max(rating),city from customers group by city order by max(rating);


44. Write a query that calculates the amount of the salesperson’s commission on each order by a customer with a rating above 100.00.

select onum,s.snum,c.cnum,amt*comm from salespeople s join customers c on s.snum=c.snum join orders o on c.cnum=o.cnum where rating>100;


45. Count the customers with ratings above San Jose’s average.

select count(cname) count from customers where rating>(select avg(rating) from customers where city='san jose');


46. Write a query that produces all pairs of salespeople with themselves as well as duplicate rows with the order reversed.

select s.sname,s1.sname from salespeople s join salespeople s1;


47. Find all salespeople that are located in either Barcelona or London.

select * from salespeople where city='barcelona' or city='london';


48. Find all salespeople with only one customer.

select * from salespeople where snum in(select snum from customers group by snum having count(cnum)=1);


49. Write a query that joins the Customer table to itself to find all pairs of customers served by a single salesperson.

select c.cname,c1.cname from customers c join customers c1 on c.snum=c1.snum where c.cname<c1.cname;


50. Write a query that will give you all orders for more than $1000.00

select * from orders where amt>1000;


51. Write a query that lists each order number followed by the name of the customer who made that order.

select onum,cname from orders o join customers c on o.cnum=c.cnum;


52. Write 2 queries that select all salespeople (by name and number) who have customers in their cities who they do not service, 
one using a join and one a corelated subquery. Which solution is more elegant?

select s.snum,s.sname,s.city from salespeople s join customers c on s.city=c.city and s.snum<>c.snum group by s.snum,s.sname,s.city;
							OR
select snum,sname from salespeople s where city in(select city from customers c where s.snum<>c.snum);


53. Write a query that selects all customers whose ratings are equal to or greater than ANY (in the SQL sense) of Serres’?

select * from customers where rating >= any(select rating from customers where snum =(select snum from salespeople where sname='serres'));


54. Write 2 queries that will produce all orders taken on October 3 or October 4.

select * from orders where odate like '%03-10%';
select * from orders where odate like '%04-10%';
select * from orders where odate in('1996-10-03','1996-10-04');

55. Write a query that produces all pairs of orders by a given customer. Name that customer and eliminate duplicates.

select c.cname,o.onum,o1.onum from orders o join orders o1 on o.cnum=o1.cnum and o.onum<o1.onum join customers c on o.cnum=c.cnum;


56. Find only those customers whose ratings are higher than every customer in Rome.

select * from customers where rating > (select max(rating) from customers where city='rome');


57. Write a query on the Customers table whose output will exclude all customers with a rating <= 100.00, unless they are located in Rome.

select * from customers where rating >100 or city='rome';


58. Find all rows from the Customers table for which the salesperson number is 1001.

select * from customers where snum=1001;


59. Find the total amount in Orders for each salesperson for whom this total is greater than the amount of the largest order in the table.

select sname,sum(amt) from salespeople s join orders o on s.snum=o.snum group by sname having sum(amt)>(select max(amt) from orders);


60. Write a query that selects all orders save those with zeroes or NULLs in the amount field.

select * from orders where amt<>0 or amt is null;


61. Produce all combinations of salespeople and customer names such that the former precedes the latter alphabetically, 
and the latter has a rating of less than 200.

select sname,cname,rating from salespeople s join customers c on s.snum=c.snum where rating<200 and sname<cname;


62. List all Salespeople’s names and the Commission they have earned.

select sname,sum(amt*comm) from salespeople s join orders o on s.snum=o.snum group by sname;



63. Write a query that produces the names and cities of all customers with the same rating as Hoffman. Write the query using 
Hoffman’s CNUM rather than his rating, so that it would still be usable if his rating changed

select cname,city from customers where rating=(select rating from customers where cnum=2001);


64. Find all salespeople for whom there are customers that follow them in alphabetical order. 

select * from salespeople s join customers c where s.snum=c.snum and s.sname<c.cname;


65. Write a query that produces the names and ratings of all customers of all who have above average orders.

select cname,rating from customers c join orders o on c.cnum=o.cnum group by cname,rating having avg(amt)>(select avg(amt) from orders);


66. Find the SUM of all purchases from the Orders table. 

select sum(amt) from orders;


67. Write a SELECT command that produces the order number, amount and date for all rows in  the order table. 

select onum,amt,odate from orders;


68. Count the number of nonNULL rating fields in the Customers table (including repeats). 

Select count(rating) from customers;


69. Write a query that gives the names of both the salesperson and the customer for each order  after the order number.

select distinct onum,sname,cname from salespeople s join customers c on s.snum=c.snum join orders o on c.cnum=o.cnum;


70. List the commissions of all salespeople servicing customers in London. 

select distinct sname,comm from salespeople s join customers c on s.snum=c.snum where city ='london';


71. Write a query using ANY or ALL that will find all salespeople who have no customers located in their city.

select * from salespeople s where city<>all(select city from customers c where s.snum=c.snum);


72. Write a query using the EXISTS operator that selects all salespeople with customers located in their cities who are not assigned to them.

select * from salespeople s where exists(select 1 from customers c where s.city=c.city and s.snum<>c.snum);


73. Write a query that selects all customers serviced by Peel or Motika. (Hint : The SNUM field relates the two tables to one another.) 

select * from customers where snum in (select snum from salespeople where sname in ('peel','motika'));


74. Count the number of salespeople registering orders for each day. (If a salesperson has more than one order on a given day, 
he or she should be counted only once.) 

elect odate,count(distinct sname) from orders o join salespeople s on o.snum=s.snum group by odate;


75. Find all orders attributed to salespeople in London.

select * from orders where snum in(select snum from salespeople where city='london');


76. Find all orders by customers not located in the same cities as their salespeople. 

select * from orders o where cnum in (select cnum from customers c join salespeople s on s.snum=c.snum and s.city<>c.city);


77. Find all salespeople who have customers with more than one current order.

select distinct s.* from salespeople s join orders o on s.snum=o.snum where o.cnum in (select cnum from orders group by cnum having count(onum)>1);


78. Write a query that extracts from the Customers table every customer assigned to a salesperson who currently has at least 
one other customer (besides the customer being selected) with orders in the Orders table. (?????????????)

SELECT DISTINCT C.*
FROM Customers C
JOIN Orders O1 ON C.CNUM = O1.CNUM
JOIN Salespeople S ON C.SNUM = S.SNUM
WHERE S.SNUM IN (
    SELECT SNUM
    FROM Customers
    WHERE CNUM <> C.CNUM AND CNUM IN (SELECT CNUM FROM Orders)
);


79. Write a query that selects all customers whose names begin with ‘C’.

select * from customers where cname like 'c%';


80. Write a query on the Customers table that will find the highest rating in each city. Put the output in this form : 
for the city (city) the highest rating is : (rating).

select city,max(rating) from customers group by city;


81. Write a query that will produce the SNUM values of all salespeople with orders currently in the 
Orders table (without any repeats). 

select distinct s.snum from salespeople s join orders o on s.snum=o.snum;


82. Write a query that lists customers in descending order of rating. Output the rating field first, followed by the customer’s names and numbers. 

select rating,cname,cnum from customers order by rating desc;


83. Find the average commission for salespeople in London. 

select avg(comm) from salespeople where city='london';


84. Find all orders credited to the same salesperson who services Hoffman (CNUM 2001). 

select * from orders where snum =(select snum from customers where cnum=2001);


85. Find all salespeople whose commission is in between 0.10 and 0.12 (both inclusive).

select * from salespeople where comm between 0.10 and 0.12;


86. Write a query that will give you the names and cities of all salespeople in London with a commission above 0.10. 

select sname,city from salespeople where city='london' and comm>0.10;


87. What will be the output from the following query? 
 SELECT * FROM ORDERS 
 where (amt < 1000 OR NOT (odate = 10/03/1996 AND cnum > 2003)); 


88. Write a query that selects each customer’s smallest order.

select c.cnum,c.cname,min(amt) from customers c join orders o on c.cnum=o.cnum group by cnum,cname;


89. Write a query that selects the first customer in alphabetical order whose name begins with G. 

select * from customers where cname like 'G%' limit 1;


90. Write a query that counts the number of different nonNULL city values in the Customers table. 

select distinct * from customers where city is not null;


91. Find the average amount from the Orders table. 

select avg(amt) from orders;


92. What would be the output from the following query? 
SELECT * FROM ORDERS 
WHERE NOT (odate = 10/03/96 OR snum > 1006) AND amt >= 
1500;


93. Find all customers who are not located in San Jose and whose rating is above 200. 

select * from customers where city<>'san jose' and rating>200;


94. Give a simpler way to write this query : 
SELECT snum, sname city, comm FROM salespeople 
WHERE (comm >  0.12 OR comm < 0.14); 

SELECT snum, sname city, comm FROM salespeople where comm=0.13;


95. Evaluate the following query : 
SELECT * FROM orders 
WHERE NOT ((odate = 10/03/96 AND snum > 1002) OR amt > 2000.00);


96. Which salespersons attend to customers not in the city they have been assigned to? 

select sname,cname from salespeople s join customers c on s.snum=c.snum and s.city<>c.city;


97. Which salespeople get commission greater than 0.11 are serving customers rated less than 250?

select sname,comm,rating from salespeople s join customers c on s.snum=c.snum where comm>0.11 and rating<250;


98. Which salespeople have been assigned to the same city but get different commission percentages? 

select s1.sname,s2.sname,s1.city from salespeople s1 join salespeople s2 where s1.city=s2.city and s1.comm<>s2.comm and s1.sname<s2.sname;


99. Which salesperson has earned the most by way of commission? 

select sname,max(comm*amt) from salespeople s join orders o on s.snum=o.snum group by sname order by max(comm*amt) desc limit 1; 


100.Does the customer who has placed the maximum number of orders have the maximum rating?

with A as (select cname from(select cname,count(o.cnum),dense_rank()over(order by count(o.cnum) desc) rn from customers c 
join orders o on c.cnum=o.cnum group by cname) s where rn=1),
B as (select cname from(select cname,rating,dense_rank()over(order by rating desc) rn1 from customers) s1 where rn1=1)

Select concat(A.cname, ' has highest rating and also has maximum number of orders') Result from A,B
Where A.cname=B.cname;


101.Has the customer who has spent the largest amount of money been given the highest rating?

with A as (select cname from (select cname,amt,dense_rank()over(order by amt desc) rn from customers c join orders o on c.cnum=o.cnum) s where rn=1),
B as (select cname from customers where rating =(select max(rating) from customers))

select concat(A.cname, ' has spent the largest amount of money and has been given the highest rating') Results from A,B
where A.cname=B.cname;


102.List all customers in descending order of customer rating. 

select * from customers order by rating desc;


103.On which days has Hoffman placed orders? 

select odate from orders where cnum = (select cnum from customers where cname='hoffman');


104.Do all salespeople have different commissions? 

select s1.sname from salespeople s1 join salespeople s2 where s1.comm=s2.comm and s1.sname<s2.sname; #Yes


105.Which salespeople have no orders between 10/03/1996 and 10/05/1996? 

select s.sname from salespeople s where not exists(select 1 from orders o where o.snum=s.snum and odate between '1996-03-10' AND '1996-05-10');
								OR
select * from salespeople where snum not in (select snum from orders where odate between '1996-03-10' AND '1996-05-10');


106.How many salespersons have succeeded in getting orders? 

select sname,count(o.snum) from salespeople s join orders o on s.snum=o.snum group by sname;


107.How many customers have placed orders? 

select count(distinct cnum) from orders;


108.On which date has each salesperson booked an order of maximum value?

select * from(select sname,odate,amt,dense_rank()over(partition by sname order by amt desc) rn from salespeople s join orders o on s.snum=o.snum) s where rn=1;


109.Who is the most successful salesperson? 

select sname,sum(amt) from salespeople s join orders o on s.snum=o.snum group by sname order by sum(amt) desc limit 1;


110.Who is the worst customer with respect to the company? 

select sname from salespeople where snum not in (select snum from orders);


111.Are all customers not having placed orders greater than 200 totally been serviced by salespersons Peel or Serres? 

select * from customers where rating>200 and cnum not in (select cnum from orders) and snum in (select snum from salespeople where sname='peel' or sname='serres');


112.Which customers have the same rating?

select c1.cname,c2.cname from customers c1 join customers c2 where c1.rating=c2.rating and c1.cname<c2.cname; 


113.Find all orders greater than the average for October 4th. 

select * from orders where odate='1996-04-10' and amt>(select avg(amt) from orders);
					OR
SELECT *
FROM orders
WHERE odate = '1996-04-10'
AND amt > (SELECT AVG(amt) FROM orders WHERE odate = '1996-04-10');


114.Which customers have above average orders?

select * from customers where cnum in (select cnum from orders where amt>(select avg(amt) from orders));


115.List all customers with ratings above San Jose’s average. 

select * from customers where rating > (select min(rating) from customers where city='san jose');


116.Select the total amount in orders for each salesperson for whom the total is greater than the amount of the largest order in the table. 

select sname,sum(amt) from salespeople s join orders o on s.snum=o.snum group by sname having sum(amt) > (select max(amt) from orders);


117.Give names and numbers of all salespersons who have more than one customer.

select sname,s.snum from salespeople s join customers c on s.snum=c.snum group by sname,s.snum having count(distinct cnum)>1;


118.Select all salespersons by name and number who have customers in their city whom they don’t service.

select distinct sname,s.snum from salespeople s join customers c on s.city=c.city and s.snum<>c.snum;


119.Which customers’ rating should be lowered? 

Select cname,rating,sum(amt),count(onum) ,rank() over(order by sum(amt)  ) rn 
From customers c left join orders o 
On c.cnum=o.cnum
group by cname,rating;


120.Is there a case for assigning a salesperson to Berlin?

select * from salespeople where city='berlin';


121.Is there any evidence linking the performance of a salesperson to the commission that he or she is being paid?

??????????????


122.Does the total amount in orders by customer in Rome and London 
exceed 
the commission 
paid to salespersons in London and New York by more than 5 times?

with A as (select sum(amt) c from customers c join orders o on c.cnum=o.cnum where city in ('rome','london')),
 B as (select sum(amt*comm) d from salespeople s join orders o on s.snum=o.snum where city in('London','New York'))

select case when c>d*5 then 'yes' else 'No' end Result from A,B;


123.Which is the date, order number, amt and city for each salesperson (by name) for the maximum order he has obtained?

select * from (select sname,odate,onum,amt,city,dense_rank()over(partition by sname order by amt desc) rn from salespeople s join orders o on s.snum=o.snum) s where rn=1;


124.Which salesperson(s) should be fired?

select sname from salespeople where snum not in (select snum from orders);


125.What is the total income for the company?

select sum(amt) from orders;




















































