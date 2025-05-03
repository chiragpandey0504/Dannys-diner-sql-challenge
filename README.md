# Danny’s Diner – 8 Week SQL Challenge Case Study #1

**Dataset:** PostgreSQL schema `dannys_diner`  
**Tables:** `sales`, `menu`, `members`

---

## **Table of Contents**
1. [Overview](#overview)  
2. [Schema](#schema)  
3. [Questions & Solutions](#questions--solutions)  
4. [Repository Structure](#repository-structure)  
5. [How to Run](#how-to-run)  
6. [Results Highlights](#results-highlights)  
7. [Contact](#contact)

---

## **Overview**
This repo contains SQL answers for **Case Study #1: Danny’s Diner** from the 8 Week SQL Challenge. You’ll find 10 core questions plus 2 bonus queries that explore customer spending, visit patterns, item popularity, membership effects, and points calculations.

---

## **Schema**
- **`sales`**  
  - `customer_id`, `order_date`, `product_id`
- **`menu`**  
  - `product_id`, `product_name`, `price`
- **`members`**  
  - `customer_id`, `join_date`

---

## **Questions & Solutions**
| Question | Description                                                      | SQL File                                 |
|:--------:|:-----------------------------------------------------------------|:-----------------------------------------|
| **Q1**   | Total amount each customer spent                                 | `sql/Q1_total_spent.sql`                 |
| **Q2**   | Number of distinct days each customer visited                    | `sql/Q2_days_visited.sql`                |
| **Q3**   | First menu item purchased by each customer                       | `sql/Q3_first_item.sql`                  |
| **Q4**   | Most purchased item overall & counts by customer                 | `sql/Q4_most_purchased_overall.sql`      |
| **Q5**   | Most popular item per customer                                    | `sql/Q5_most_popular_per_customer.sql`   |
| **Q6**   | First item bought _after_ joining                                | `sql/Q6_first_after_join.sql`            |
| **Q7**   | Item bought just _before_ joining                                 | `sql/Q7_before_join.sql`                 |
| **Q8**   | Total items & amount spent _before_ joining                       | `sql/Q8_pre_join_spending.sql`           |
| **Q9**   | Points calculation (₹1 = 10 pts; sushi ×2 multiplier)              | `sql/Q9_points_multiplier.sql`           |
| **Q10**  | Points in first week of membership (Jan orders)                   | `sql/Q10_points_first_month.sql`         |
| **Bonus 1** | Tag orders as member (`Y`/`N`)                                 | `sql/bonus1_member_tag.sql`              |
| **Bonus 2** | Ranking _(only)_ once a customer is a member                   | `sql/bonus2_member_ranking.sql`          |

---

## **Repository Structure**
``` text
.
├── sql/
│   ├── Q1_total_spent.sql
│   ├── Q2_days_visited.sql
│   ├── Q3_first_item.sql
│   ├── Q4_most_purchased_overall.sql
│   ├── Q5_most_popular_per_customer.sql
│   ├── Q6_first_after_join.sql
│   ├── Q7_before_join.sql
│   ├── Q8_pre_join_spending.sql
│   ├── Q9_points_multiplier.sql
│   ├── Q10_points_first_month.sql
│   ├── bonus1_member_tag.sql
│   └── bonus2_member_ranking.sql
└── README.md
