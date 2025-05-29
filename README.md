# DBMS Practice Project: E-commerce Simulation 💾

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub issues](https://img.shields.io/github/issues/M-Mikran-Sandhu/DBMS-Practice)](https://github.com/M-Mikran-Sandhu/DBMS-Practice/issues)
[![GitHub forks](https://img.shields.io/github/forks/M-Mikran-Sandhu/DBMS-Practice)](https://github.com/M-Mikran-Sandhu/DBMS-Practice/network)
[![GitHub stars](https://img.shields.io/github/stars/M-Mikran-Sandhu/DBMS-Practice)](https://github.com/M-Mikran-Sandhu/DBMS-Practice/stargazers)

<!-- Optional: Add a project logo or banner here -->

## Overview

Welcome to the DBMS Practice repository! This project serves as a practical environment for learning, experimenting, and honing skills related to Database Management Systems (DBMS) by simulating a basic **E-commerce Database**. It focuses on relational database concepts, SQL query practice, database design principles, and implementing core DBMS features within the context of managing customers, products, and orders. This repository provides a foundation for exploring data modeling, query optimization, and transaction management.

## Features

This repository is designed to support the implementation and practice of various DBMS features within an e-commerce context:

*   **🧑‍💼 Data Modeling:** Designing and implementing database schemas (Conceptual, Logical, Physical) for customers, products, orders, and line items.
*   **⚙️ SQL Querying:** Practicing basic to advanced SQL commands (SELECT, INSERT, UPDATE, DELETE, JOINs, Subqueries, Aggregations, Window Functions) relevant to e-commerce reporting (e.g., finding top customers, calculating sales totals).
*   **➕ Data Manipulation:** Adding, updating, and deleting records for customers, products, and orders.
*   **📊 Indexing & Optimization:** Understanding and applying indexing strategies (e.g., on foreign keys, frequently queried columns) to improve query performance for typical e-commerce lookups.
*   **🔒 Transaction Management:** Implementing transactions to ensure atomicity for operations like placing an order (updating stock, creating order record).
*   **🔄 Backup & Recovery:** Practicing basic database backup and recovery procedures (using tools like `pg_dump`).
*   **🔐 Basic Access Control:** Exploring user roles and permissions within PostgreSQL (if desired).
*   **📈 Reporting:** Generating sample reports, such as total sales per product or orders per customer, using SQL.

## Technical Details

*   **Programming Languages:** Python 🐍 (for scripting/application logic), SQL 📊 (for database interaction)
*   **Database System:** PostgreSQL 🐘 (Version 12+ recommended)
*   **Tools & Technologies:** Git 🐙 (for version control), SQLAlchemy (optional, for ORM interaction), DBeaver or pgAdmin (optional, for GUI database management)
*   **Operating System:** Cross-platform (Linux 🐧, Windows 💻, macOS 🍎)

## System Architecture (Optional)

This project primarily focuses on the database layer. It can be interacted with directly via SQL clients or through simple Python scripts. A potential extension could involve building a simple client-server application (e.g., using Flask/Django) that interacts with this database, creating a 3-Tier architecture 🏢🏢🏢.

## Getting Started

Follow these instructions to set up the project environment on your local machine for development and practice purposes.

### Prerequisites

Ensure you have the following software installed:

*   Python 3.8+ 🐍
*   PostgreSQL Server (Version 12 or higher recommended) 🐘
*   Git 🐙
*   `pip` (Python package installer)

### Installation

A step-by-step guide to get your development environment running:

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/M-Mikran-Sandhu/DBMS-Practice.git
    cd DBMS-Practice
    ```
2.  **Set up the PostgreSQL Database:**
    *   Connect to your PostgreSQL server (e.g., using `psql`).
    *   Create a new database for this project:
        ```sql
        CREATE DATABASE ecommerce_practice;
        ```
    *   Create a dedicated user (optional but recommended):
        ```sql
        CREATE USER practice_user WITH PASSWORD 'your_strong_password';
        GRANT ALL PRIVILEGES ON DATABASE ecommerce_practice TO practice_user;
        ```
    *   Connect to the new database (`\c ecommerce_practice`) and run the schema creation script (assuming you create a `schema.sql` file based on the diagram below):
        ```bash
        psql -U practice_user -d ecommerce_practice -a -f schema.sql 
        # You might need to provide the host and prompt for password depending on your setup
        ```
3.  **Configure Environment (if using Python scripts):**
    *   Create a virtual environment (recommended):
        ```bash
        python -m venv venv
        source venv/bin/activate  # On Windows use `venv\Scripts\activate`
        ```
    *   Create a `.env` file (copy from a potential `.env.example`) and add your database connection details:
        ```plaintext
        DB_NAME=ecommerce_practice
        DB_USER=practice_user
        DB_PASSWORD=your_strong_password
        DB_HOST=localhost
        DB_PORT=5432 
        ```
4.  **Install Python Dependencies (if any):**
    *   If you have a `requirements.txt` file (e.g., for SQLAlchemy or psycopg2):
        ```bash
        pip install -r requirements.txt
        ```
        *(You might need `pip install psycopg2-binary sqlalchemy python-dotenv` initially)*

5.  **Run Practice Scripts/Connect:**
    *   You can now connect to the `ecommerce_practice` database using a SQL client (like DBeaver, pgAdmin, or `psql`) with the `practice_user` credentials.
    *   If you create Python scripts (e.g., `populate_data.py`, `run_reports.py`), run them:
        ```bash
        python your_script_name.py
        ```

## Usage

Examples of how to interact with the database:

*   **Connecting via `psql`:**
    ```bash
    psql -U practice_user -d ecommerce_practice -h localhost
    ```
*   **Running SQL Queries (within `psql` or a GUI tool):**
    ```sql
    -- Find all customers
    SELECT * FROM customer;

    -- Find total quantity ordered for each product
    SELECT p.name, SUM(li.quantity) AS total_quantity_ordered
    FROM product p
    JOIN line_item li ON p.product_id = li.product_id
    GROUP BY p.name
    ORDER BY total_quantity_ordered DESC;
    ```
*   **Using Python Scripts (Example):**
    ```bash
    # Example: Populate database with sample data
    python scripts/populate_sample_data.py 

    # Example: Generate a sales report
    python scripts/generate_sales_report.py --month 2024-05
    ```
    *(These script names are examples; you would need to create them.)*

## Database Schema

The database schema defines the structure for managing customers, products, orders, and line items.

*   **Diagrams:** A visual representation (ERD) of the schema:
    ```mermaid
    erDiagram
        CUSTOMER ||--o{ ORDER : places
        ORDER ||--|{ LINE_ITEM : contains
        PRODUCT ||--o{ LINE_ITEM : ordered_in

        CUSTOMER {
            int customer_id PK "SERIAL"
            varchar name
            varchar email UNIQUE
            text address
            timestamp created_at
        }
        ORDER {
            int order_id PK "SERIAL"
            int customer_id FK
            timestamp order_date
            varchar status
        }
        LINE_ITEM {
            int order_id PK, FK
            int product_id PK, FK
            int quantity
            decimal price_per_unit "At time of order"
        }
        PRODUCT {
            int product_id PK "SERIAL"
            varchar name
            text description
            decimal current_price
            int stock_quantity
        }
    ```
    *(This Mermaid diagram represents the intended schema. Ensure your `schema.sql` file implements this structure, potentially adding constraints like NOT NULL, CHECK, etc.)*

*   **Text Description:**
    *   `CUSTOMER`: Stores customer information (unique ID, name, unique email, address, creation timestamp).
    *   `PRODUCT`: Stores product details (unique ID, name, description, current price, stock level).
    *   `ORDER`: Stores order header information (unique ID, customer reference, date, status like 'pending', 'shipped').
    *   `LINE_ITEM`: Represents the items within an order (links Order and Product, stores quantity and the price paid per unit at the time of the order).
    *   **Relationships:** Customers place Orders. Orders contain Line Items. Line Items refer to Products.

## Contributing

Contributions are welcome, especially for practice and learning purposes! If you find bugs, have suggestions, or want to add more practice examples (like complex queries or stored procedures), please follow these steps:

1.  **Fork the repository** 🍴 on GitHub.
2.  **Create a new branch** for your feature or bug fix: `git checkout -b feature/your-feature-name` or `bugfix/issue-description`. 🌿
3.  **Make your changes** and commit them with clear, descriptive messages: `git commit -m 'Add feature X'` ✍️
4.  **Push your branch** to your fork: `git push origin feature/your-feature-name` ⬆️
5.  **Submit a pull request** 📤 to the `main` branch of the original repository. Describe your changes clearly in the pull request. 🤝

Please ensure your code adheres to any existing style guidelines and includes relevant explanations or documentation for new practice examples.

## License 📜

This project is licensed under the [MIT License](LICENSE). (Consider adding a LICENSE file with the full MIT license text to the repository root).

## Acknowledgements 🙏

*   Inspired by common DBMS course exercises and online tutorials on relational database design.
*   Utilizes the powerful and open-source PostgreSQL database.
*   README structure inspired by examples found in `matiassingers/awesome-readme`.

## Contact 📧

*   **Name:** M-Mikran-Sandhu
*   **Email:** sandhumikran@gmail.com
*   **GitHub:** [M-Mikran-Sandhu](https://github.com/M-Mikran-Sandhu)

