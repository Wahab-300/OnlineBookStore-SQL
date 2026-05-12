# 📚 Online Bookstore Database

A relational database project built with **PostgreSQL** that simulates a real-world online bookstore system. It manages books, customers, and orders using structured, normalized data imported from CSV files.

---

## 🗄️ Database Overview

**Database Name:** `online_bookstore`  
**DBMS:** PostgreSQL 16  
**Total Records:** 500 books · 500 customers · 500 orders

---

## 🧱 Schema

### `books`
| Column | Type | Description |
|---|---|---|
| `book_id` | SERIAL (PK) | Unique book identifier |
| `title` | VARCHAR(100) | Title of the book |
| `author` | VARCHAR(100) | Author's name |
| `genre` | VARCHAR(50) | Genre (Fiction, Fantasy, Mystery, etc.) |
| `published_year` | INTEGER | Year of publication |
| `price` | NUMERIC(10,2) | Price in USD |
| `stock` | INTEGER | Available stock quantity |

### `customers`
| Column | Type | Description |
|---|---|---|
| `customer_id` | SERIAL (PK) | Unique customer identifier |
| `name` | VARCHAR(100) | Full name |
| `email` | VARCHAR(100) | Email address |
| `phone` | VARCHAR(15) | Phone number |
| `city` | VARCHAR(50) | City of residence |
| `country` | VARCHAR(150) | Country of residence |

### `orders`
| Column | Type | Description |
|---|---|---|
| `order_id` | SERIAL (PK) | Unique order identifier |
| `customer_id` | INTEGER (FK) | References `customers.customer_id` |
| `book_id` | INTEGER (FK) | References `books.book_id` |
| `order_date` | DATE | Date the order was placed |
| `quantity` | INTEGER | Number of copies ordered |
| `total_amount` | NUMERIC(10,2) | Total price for the order |

---

## 🔗 Entity Relationships

```
customers (1) ──── (many) orders (many) ──── (1) books
```

Each order belongs to one customer and one book. A customer can place many orders, and a book can appear in many orders.

---

## 📂 Project Structure

```
online-bookstore/
├── onlinebookstore.sql   # Full database dump (schema + data)
├── books.csv             # Source data for books table
├── customers.csv         # Source data for customers table
├── orders.csv            # Source data for orders table
└── README.md
```

---

## 🚀 Setup Instructions

### Prerequisites
- PostgreSQL 14+ installed
- `psql` command-line tool or pgAdmin

### Restore the Database

**Using terminal:**
```bash
# Create the database
psql -U postgres -c "CREATE DATABASE online_bookstore;"

# Import the dump
psql -U postgres -d online_bookstore < onlinebookstore.sql
```

**Using pgAdmin:**
1. Right-click on **Databases** → **Create** → **Database** → name it `online_bookstore`
2. Right-click the new database → **Restore**
3. Select `onlinebookstore.sql` and click **Restore**

---

## 🛠️ Sample Queries

```sql
-- Top 5 best-selling books by total revenue
SELECT b.title, SUM(o.total_amount) AS revenue
FROM orders o JOIN books b ON o.book_id = b.book_id
GROUP BY b.title
ORDER BY revenue DESC
LIMIT 5;

-- Customers who placed the most orders
SELECT c.name, COUNT(o.order_id) AS total_orders
FROM customers c JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.name
ORDER BY total_orders DESC
LIMIT 10;

-- Books low in stock (less than 5 copies)
SELECT title, author, stock
FROM books
WHERE stock < 5
ORDER BY stock;
```

---

## 📊 Data Highlights

- **Genres covered:** Fiction, Fantasy, Mystery, Romance, Science Fiction, Biography, Non-Fiction
- **Publication range:** 1900 – 2023
- **Price range:** $5.07 – $49.96
- **Order date range:** December 2022 – December 2024
- **Customers from:** 150+ countries worldwide

---

## 👤 Author

**Abdul Wahab**  
Built as a SQL practice project to explore relational database design, data import via CSV, and query writing in PostgreSQL.
