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

As you can see, the relationship between tables occur through the `customer_id`column.
