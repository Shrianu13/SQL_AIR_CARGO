Air Cargo Analysis


Problem Statement Scenario:

Air Cargo is an aviation company that provides air transportation services for
passengers and freight. Air Cargo uses its aircraft to provide different services with
the help of partnerships or alliances with other airlines. The company wants to
prepare reports on regular passengers, busiest routes, ticket sales details, and
other scenarios to improve the ease of travel and booking for customers.
 
Project Objective:

You, as a DBA expert, need to focus on 
identifying the regular 
- customers to provide offers, analyze the 
- busiest route which helps to increase the number of aircraft required and 
-prepare an analysis to determine the ticket sales details. This will ensure that the company improves its operability and becomes more customer-centric and a favorable choice for air travel.

Dataset description:

Customer: Contains the information of customers
●	customer_id – ID of the customer
●	first_name – First name of the customer
●	last_name – Last name of the customer
●	date_of_birth – Date of birth of the customer
●	gender – Gender of the customer


passengers_on_flights: Contains information about the travel details

●	aircraft_id – ID of each aircraft in a brand
●	route_id – Route ID of from and to location
●	customer_id – ID of the customer
●	depart – Departure place from the airport
●	arrival – Arrival place in the airport
●	seat_num – Unique seat number for each passenger
●	class_id – ID of travel class
●	travel_date – Travel date of each passenger
●	flight_num – Specific flight number for each route


ticket_details: Contains information about the ticket details
●	p_date – Ticket purchase date 
●	customer_id – ID of the customer
●	aircraft_id – ID of each aircraft in a brand
●	class_id – ID of travel class
●	no_of_tickets – Number of tickets purchased
●	a_code – Code of each airport
●	price_per_ticket – Price of a ticket
●	brand – Aviation service provider for each aircraft

routes: Contains information about the route details

●	Route_id – Route ID of from and to location 
●	Flight_num – Specific fight number for each route
●	Origin_airport – Departure location
●	Destination_airport – Arrival location
●	Aircraft_id – ID of each aircraft in a brand
●	Distance_miles – Distance between departure and arrival location

Following operations should be performed:

1.	Create an ER diagram for the given airlines database.
Ans )   
2.	Write a query to create a route_details table using suitable data types for the fields, such as route_id, flight_num, origin_airport, destination_airport, aircraft_id, and distance_miles. Implement the check constraint for the flight number and unique constraint for the route_id fields. Also, make sure that the distance miles field is greater than 0. 
3.	Write a query to display all the passengers (customers) who have travelled in routes 01 to 25. Take data from the passengers_on_flights 
Table
Ans ) SELECT DISTINCT customer_id
FROM passengers_on_flights
WHERE route_id BETWEEN 01 AND 25;

4.	Write a query to identify the number of passengers and total revenue in business class from the ticket_details table
Ans )  SELECT 
    COUNT(customer_id) AS NumPassengers,
    SUM(Price_per_ticket) AS TotalRevenue
FROM 
    ticket_details
WHERE 
    Class_id = 'Bussiness';
5.	Write a query to display the full name of the customer by extracting the first name and last name from the customer table.
Ans )  SELECT 
concat(first_name,' ',Last_name) As Full_Name
From customer;
6.	Write a query to extract the customers who have registered and booked a ticket. Use data from the customer and ticket_details tables.
Ans )  SELECT C.* From
customer C
INNER JOIN
ticket_details td ON c.customer_id = td.customer_id;

7.	Write a query to identify the customer’s first name and last name based on their customer ID and brand (Emirates) from the ticket_details table.
Ans )  SELECT c.First_Name, c.Last_Name
FROM customer c
INNER JOIN ticket_details td ON c.Customer_ID = td.Customer_ID
WHERE td.Brand = 'Emirates';

8.	Write a query to identify the customers who have travelled by Economy Plus class using Group By and Having clause on the passengers_on_flights table. 
 Ans )  SELECT p.Customer_ID
FROM passengers_on_flights p
JOIN ticket_details td ON p.class_id = td.class_id
WHERE td.Class_id = 'Economy Plus'
GROUP BY p.Customer_ID
HAVING COUNT(*) > 0;
9.	Write a query to identify whether the revenue has crossed 10000 using the IF clause on the ticket_details table.
Ans )  SELECT 
    CASE 
        WHEN SUM(Price) > 10000 THEN 'Revenue crossed 10000'
        ELSE 'Revenue not crossed 10000'
    END AS RevenueStatus
FROM 
    ticket_details;

10.	Write a query to create and grant access to a new user to perform operations on a database.
Ans )  
11.	Write a query to find the maximum ticket price for each class using window functions on the ticket_details table. 
Ans )  SELECT DISTINCT
    Class_ID,
    MAX(Price_per_ticket) OVER (PARTITION BY Class_ID) AS MaxTicketPrice
FROM ticket_details;

12.	Write a query to extract the passengers whose route ID is 4 by improving the speed and performance of the passengers_on_flights table.
Ans )    SELECT p.*
FROM passengers_on_flights p
JOIN routes r ON p.flight_num = r.flight_num
WHERE r.route_id = 4;

13.	 For the route ID 4, write a query to view the execution plan of the passengers_on_flights table.
Ans )   EXPLAIN SELECT p.*
FROM passengers_on_flights p
JOIN routes r ON p.flight_num = r.flight_num
WHERE r.route_id = 4;

14.	Write a query to calculate the total price of all tickets booked by a customer across different aircraft IDs using rollup function. 
Ans )  SELECT 
    Customer_ID,
    Aircraft_ID,
    SUM(Price_per_ticket) AS TotalPrice
FROM 
    ticket_details
GROUP BY 
    Customer_ID,
    Aircraft_ID WITH ROLLUP;

15.	Write a query to create a view with only business class customers along with the brand of airlines. 
Ans )   CREATE VIEW BusinessClassCustomers AS
SELECT 
    td.Customer_ID,
    td.Brand,
    c.First_Name,
    c.Last_Name
FROM 
    ticket_details td
JOIN 
    customer c ON td.Customer_ID = c.Customer_ID
WHERE 
    td.Class_ID = 'Bussiness';

16.	Write a query to create a stored procedure to get the details of all passengers flying between a range of routes defined in run time. Also, return an error message if the table doesn't exist.
Ans )  
17.	Write a query to create a stored procedure that extracts all the details from the routes table where the travelled distance is more than 2000 miles.
Ans )   DELIMITER // 
CREATE PROCEDURE DISMORETHAN2k()
BEGIN
    SELECT *
    FROM routes
    WHERE Distance > 2000;
END //
18.	Write a query to create a stored procedure that groups the distance travelled by each flight into three categories. The categories are, short distance travel (SDT) for >=0 AND <= 2000 miles, intermediate distance travel (IDT) for >2000 AND <=6500, and long-distance travel (LDT) for >6500.
Ans )  DELIMITER //

CREATE PROCEDURE DisByCategory()
BEGIN
    SELECT 
        flight_num,
        distance_miles,
        CASE 
            WHEN Distance >= 0 AND Distance <= 2000 THEN 'SDT'
            WHEN Distance > 2000 AND Distance <= 6500 THEN 'IDT'
            WHEN Distance > 6500 THEN 'LDT'
        END AS DistanceCategory
    FROM 
        routes;
END //
19.	Write a query to extract ticket purchase date, customer ID, class ID and specify if the complimentary services are provided for the specific class using a stored function in stored procedure on the ticket_details table. 
Condition: 
●	If the class is Business and Economy Plus, then complimentary services are given as Yes, else it is No
Ans ) 
20.	Write a query to extract the first record of the customer whose last name ends with Scott using a cursor from the customer table.



