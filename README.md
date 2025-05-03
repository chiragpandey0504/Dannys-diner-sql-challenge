# Danny's Diner – 8 Week SQL Challenge Case Study #1

---

## Overview
This README consolidates every question, SQL query, and result for **Case Study #1: Danny's Diner** from the [8 Week SQL Challenge](https://8weeksqlchallenge.com/case-study-1/). Run these queries against the `dannys_diner` PostgreSQL schema to reproduce the insights below.


---

## **Schema**
- **`sales`**  
  - `customer_id`, `order_date`, `product_id`
- **`menu`**  
  - `product_id`, `product_name`, `price`
- **`members`**  
  - `customer_id`, `join_date`

---

Q1. What is the total amount each customer spent at the restaurant?
Query:
SELECT
  sales.customer_id,
  SUM(menu.price) AS total_sales
FROM dannys_diner.sales
INNER JOIN dannys_diner.menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id ASC;
Answer:
  Customer A spent $76.
  Customer B spent $74.
  Customer C spent $36.

Q2. How many days has each customer visited the restaurant?
Query:
SELECT
  customer_id,
  COUNT(DISTINCT order_date) AS days_of_visit
FROM dannys_diner.sales
GROUP BY customer_id
ORDER BY customer_id;
Answer:
  Customer A: 4 days
  Customer B: 6 days
  Customer C: 2 days

Q3. What was each customer’s first order of the day?
Query:
SELECT
  customer_id,
  order_date,
  MIN(product_id) AS first_product_id
FROM dannys_diner.sales
GROUP BY customer_id, order_date
ORDER BY customer_id, order_date;
Answer:
  A: curry & sushi on first date
  B: curry
  C: ramen

Q4. Which item was purchased the most across all customers?
Query:
SELECT
  m.product_name,
  COUNT(*) AS times_purchased
FROM dannys_diner.sales s
JOIN dannys_diner.menu m
  ON s.product_id = m.product_id
GROUP BY m.product_name
ORDER BY times_purchased DESC
LIMIT 1;
Answer:
  ramen (8 times)

Q5. What is each customer’s favorite item?
Query:
SELECT
  customer_id,
  m.product_name,
  COUNT(*) AS count_per_item
FROM dannys_diner.sales s
JOIN dannys_diner.menu m
  ON s.product_id = m.product_id
GROUP BY customer_id, m.product_name
HAVING COUNT(*) = (
  SELECT MAX(ct)
  FROM (
    SELECT COUNT(*) AS ct
    FROM dannys_diner.sales
    WHERE customer_id = s.customer_id
    GROUP BY product_id
  ) sub
)
ORDER BY customer_id;
Answer:
  A & C: ramen
  B: tie among all items

Q6. What was the first item each customer purchased after joining the loyalty program?
Query:
SELECT DISTINCT ON (s.customer_id)
  s.customer_id,
  m.product_name,
  s.order_date
FROM dannys_diner.sales s
JOIN dannys_diner.members mem
  ON s.customer_id = mem.customer_id
JOIN dannys_diner.menu m
  ON s.product_id = m.product_id
WHERE s.order_date >= mem.join_date
ORDER BY s.customer_id, s.order_date ASC;
Answer:
  A: ramen
  B: sushi

Q7. What was the last item each customer purchased before joining?
Query:
SELECT DISTINCT ON (s.customer_id)
  s.customer_id,
  m.product_name,
  s.order_date
FROM dannys_diner.sales s
JOIN dannys_diner.members mem
  ON s.customer_id = mem.customer_id
JOIN dannys_diner.menu m
  ON s.product_id = m.product_id
WHERE s.order_date < mem.join_date
ORDER BY s.customer_id, s.order_date DESC;
Answer:
  A & B: sushi

Q8. How many items and how much did each customer purchase before membership?
Query:
SELECT
  s.customer_id,
  COUNT(*) AS item_count,
  SUM(m.price) AS total_spent
FROM dannys_diner.sales s
JOIN dannys_diner.menu m
  ON s.product_id = m.product_id
JOIN dannys_diner.members mem
  ON s.customer_id = mem.customer_id
WHERE s.order_date < mem.join_date
GROUP BY s.customer_id;
Answer:
  A: 2 items for $25
  B: 3 items for $40

Q9. How many loyalty points does each customer have?
(10 pts per $1 spent; double pts on sushi)
Query:
SELECT
  s.customer_id,
  SUM(
    CASE WHEN m.product_name = 'sushi'
         THEN m.price * 2
         ELSE m.price
    END
  ) * 10 AS points
FROM dannys_diner.sales s
JOIN dannys_diner.menu m
  ON s.product_id = m.product_id
GROUP BY s.customer_id;
Answer:
  A: 860 pts
  B: 940 pts
  C: 360 pts

Q10. How many bonus points did each customer earn in January?
(10 pts per $1; double on sushi)
Query:
SELECT
  s.customer_id,
  SUM(
    CASE WHEN m.product_name = 'sushi'
         THEN m.price * 2
         ELSE m.price
    END
  ) * 10 AS january_points
FROM dannys_diner.sales s
JOIN dannys_diner.menu m
  ON s.product_id = m.product_id
JOIN dannys_diner.members mem
  ON s.customer_id = mem.customer_id
WHERE EXTRACT(MONTH FROM s.order_date) = 1
  AND s.order_date >= mem.join_date
GROUP BY s.customer_id;
Answer:
  A: 1,020 pts
  B: 320 pts

Bonus Question 1: Tag sales as member (‘Y’) or non-member (‘N’).
Query:
SELECT
  s.customer_id,
  s.order_date,
  m.product_name,
  CASE WHEN s.order_date >= mem.join_date THEN 'Y' ELSE 'N' END AS member
FROM dannys_diner.sales s
JOIN dannys_diner.menu m
  ON s.product_id = m.product_id
LEFT JOIN dannys_diner.members mem
  ON s.customer_id = mem.customer_id
ORDER BY s.customer_id, s.order_date;

Bonus Question 2: Rank purchases after joining for each customer.
Query:
WITH joined_sales AS (
  SELECT
    s.customer_id,
    s.order_date,
    m.product_name
  FROM dannys_diner.sales s
  JOIN dannys_diner.menu m
    ON s.product_id = m.product_id
  JOIN dannys_diner.members mem
    ON s.customer_id = mem.customer_id
  WHERE s.order_date >= mem.join_date
)
SELECT
  customer_id,
  order_date,
  product_name,
  ROW_NUMBER() OVER (
    PARTITION BY customer_id ORDER BY order_date
  ) AS purchase_rank
FROM joined_sales;
