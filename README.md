# Business Analytics with SQL

To create this project, I used two tables to simulate e-commerce transactions.
The first table is Customers table.

## Customers table

| Column                | Description                                                                                                               |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `customer_id`         | Unique identifier for each customer.                                                                                      |
| `customer_name`       | Customer’s full name.                                                                                                     |
| `email`               | Customer’s email address.                                                                                                 |
| `city`                | City where the customer lives.                                                                                            |
| `state`               | Brazilian state abbreviation where the customer lives, such as `SP`, `RJ`, or `BA`.                                       |
| `signup_date`         | Date when the customer created an account.                                                                                |
| `segment`             | Customer segment, such as `New`, `Returning`, `VIP`, or `At Risk`.                                                        |
| `acquisition_channel` | Marketing or acquisition channel that brought the customer, such as `Organic`, `Paid Ads`, `Referral`, or `Social Media`. |

The description of each column you can see in the table below.

## Customers table dictionary

| Column                | Description                                                                                                               |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `customer_id`         | Unique identifier for each customer.                                                                                      |
| `customer_name`       | Customer’s full name.                                                                                                     |
| `email`               | Customer’s email address.                                                                                                 |
| `city`                | City where the customer lives.                                                                                            |
| `state`               | Brazilian state abbreviation where the customer lives, such as `SP`, `RJ`, or `BA`.                                       |
| `signup_date`         | Date when the customer created an account.                                                                                |
| `segment`             | Customer segment, such as `New`, `Returning`, `VIP`, or `At Risk`.                                                        |
| `acquisition_channel` | Marketing or acquisition channel that brought the customer, such as `Organic`, `Paid Ads`, `Referral`, or `Social Media`. |

The second table is Orders table.

## Orders table

| Column             |          Type | Key                                   |
| ------------------ | ------------: | ------------------------------------- |
| `order_id`         |       INTEGER | Primary key                           |
| `customer_id`      |       INTEGER | Foreign key → `customers.customer_id` |
| `order_date`       |          DATE |                                       |
| `product_category` |          TEXT |                                       |
| `order_status`     |          TEXT |                                       |
| `quantity`         |       INTEGER |                                       |
| `unit_price`       | DECIMAL(10,2) |                                       |
| `discount_amount`  | DECIMAL(10,2) |                                       |
| `shipping_fee`     | DECIMAL(10,2) |                                       |
| `payment_method`   |          TEXT |                                       |

## Orders table dictionary

| Column             | Description                                                                                    |
| ------------------ | ---------------------------------------------------------------------------------------------- |
| `order_id`         | Unique identifier for each order.                                                              |
| `customer_id`      | Identifier of the customer who placed the order. This column connects `orders` to `customers`. |
| `order_date`       | Date when the order was placed.                                                                |
| `product_category` | Category of the product purchased, such as `Electronics`, `Beauty`, `Home`, or `Sports`.       |
| `order_status`     | Current status of the order, such as `completed`, `cancelled`, `pending`, or `returned`.       |
| `quantity`         | Number of units purchased in that order.                                                       |
| `unit_price`       | Price of one unit of the product.                                                              |
| `discount_amount`  | Total discount applied to the order.                                                           |
| `shipping_fee`     | Shipping amount charged to the customer.                                                       |
| `payment_method`   | Payment method used by the customer, such as `credit_card`, `pix`, `boleto`, or `debit_card`.  |

> As you can see, the relationship between tables occurs through the `customer_id`column.

I will answer business questions and I will show how to translate them into SQL scripts.
For revenue questions, I'm going to use this definition:
`revenue = quantity * unit_price - discount_amount`

The questions are ordered by difficulty level from lowest to highest.

**_Question 01: How many unique customers placed at least one completed order?_**

Let's break the question in small peaces in order to understand what the questions are asking for us:

> - _How many_ -> aggregation using `COUNT()`
> - _unique_ -> `DISTINCT()`
> - _unique customers_ -> `DISTINCT(customer_id)`
> - _How many unique customers_ -> `COUNT(DISTINCT(customer_id))`
> - _placed at least one completed order_ -> it means that we need to filter our query using `WHERE order_status = 'completed'`.

So, putting everything together, we have:

```sql
SELECT
    COUNT(DISTINCIT(customer_id)) AS number_distinct_customer
FROM orders
WHERE order_status = 'completed';
```

**_Question 02: For each product category, calculate the average completed-order revenue per customer_.**
Use this definition:
`category revenue per customer = total completed revenue in that category / number of unique customers who completed orders in that category`

> - _For each product category_ -> we need to aggregate by product_category
> - _calculate the average completed-order revenue per customer_ -> it means that we need calculate the sum of completed-order and divide by the number of unique customer `COUNT(DISTINCT(customer_id))`.
> - _completed-order revenue_ -> we need to filter using `WHERE order_status = 'completed'`.

```sql
SELECT
    o.product_category,
    ROUND(SUM(o.quantity * o.unit_price - o.discount_amount), 2) AS total_category_revenue,
    COUNT(DISTINCT o.customer_id) AS unique_customers_completed,
    ROUND(
        1.0 * SUM(o.quantity * o.unit_price - o.discount_amount)
        / COUNT(DISTINCT o.customer_id),
        2
    ) AS category_revenue_per_customer
FROM orders o
WHERE o.order_status = 'completed'
GROUP BY o.product_category
ORDER BY category_revenue_per_customer DESC;
```

**_Question 03: Which customers placed completed orders in more than one product category?_.**

> - _Which customers placed_ -> `c.customer_id`
> - _placed completed orders_ -> we need to filter using `WHERE order_status = 'completed'`
> - _placed ... more than one product category_ -> it means that we need to count unique product categories `COUNT(DISTINCT(o.order_status))` and filter the result.
> - _To filter a result of aggregation we use `HAVING`_

```sql
SELECT
    c.customer_id,
    c.customer_name,
    COUNT(DISTINCT (o.product_category)) AS number_categories
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.order_status = 'completed'
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(DISTINCT o.product_category) > 1
ORDER BY number_categories DESC;
```
