# data-analyse-pizza-order

~Basic:
~Retrieve the total number of orders placed.
~Calculate the total revenue generated from pizza sales.
~Identify the highest-priced pizza.
~Identify the most common pizza size ordered.
~List the top 5 most ordered pizza types along with their quantities.

1)Retrieve the total number of orders placed..
#SELECT count(order_id) FROM orders
2)Calculate the total revenue generated from pizza sales.

#select sum(order_details.quantity*pizzas.price) from pizzas join order_details on pizzas.pizza_id=order_details.pizza_
3)Identify the highest-priced pizza.

SELECT max(price) FROM product.pizzas join order_details on 
pizzas.pizza_id=order_details.pizza_id;

4)Identify the most common pizza size ordered.

SELECT count(pizza_type_id) as pr,size FROM product.pizzas join order_details on 
pizzas.pizza_id=order_details.pizza_id
group by size
order by pr desc

5)List the top 5 most ordered pizza types along with their quantities.

SELECT name, SUM(order_details.quantity) AS total_quantity
FROM pizzas
JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY name
ORDER BY total_quantity DESC limit 5;


Intermediate:

Join the necessary tables to find the total quantity of each pizza category ordered.
Determine the distribution of orders by hour of the day.
Join relevant tables to find the category-wise distribution of pizzas.
Group the orders by date and calculate the average number of pizzas ordered per day.
~Determine the top 3 most ordered pizza types based on revenue.

1)Join the necessary tables to find the total quantity of each pizza category ordered.
select category, sum(quantity) as quant from order_details join pizzas on
order_details.pizza_id= pizzas.pizza_id join pizza_types on
pizza_types.pizza_type_id=pizzas.pizza_type_id
group by category

2)Determine the distribution of orders by hour of the day.
#select year(time), count(order_id) from orders
#group by year(time)
#order by year(time) desc


3)Join relevant tables to find the category-wise distribution of pizzas.
select category, count(name)  from pizza_types 
group by category

4)Group the orders by date and calculate the average number of pizzas ordered per day.


#SELECT AVG(sum_quantity) AS avg_quantity_per_day
#FROM (
    #SELECT orders.date, SUM(order_details.quantity) AS sum_quantity
   #fROM orders
    #JOIN order_details ON orders.order_id = order_details.order_id
    #GROUP BY orders.date
#) AS daily_totals;


5)Determine the top 3 most ordered pizza types based on revenue.
select name, sum(price*quantity) as say from pizza_types join pizzas on
pizza_types.pizza_type_id=pizzas.pizza_type_id join order_details on
order_details.pizza_id=pizzas.pizza_id
group by name 
order by say desc limit 3

Advance:

1)Calculate the percentage contribution of each pizza type to total revenue.


select category, round(sum(price*quantity)/ (select round(sum(price*quantity),2) as total from  order_details join pizzas on
order_details.pizza_id=pizzas.pizza_id)*100,2) as say from pizza_types join pizzas on
pizza_types.pizza_type_id=pizzas.pizza_type_id join order_details on
order_details.pizza_id=pizzas.pizza_id
group by category 
order by say desc limit 3
