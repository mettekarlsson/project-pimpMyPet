# PimpMyPet

A web application for pet owners to browse pet-related products. Built as a group project during the Fullstack Development programme at Teknikhögskolan in Lund, with a MySQL database.

> This repository is a fork of the team's original repository: [https://github.com/InnaKuljanac/project-pimpMyPet](https://github.com/InnaKuljanac/project-pimpMyPet)

## Features

- Browse the product catalogue and view individual product details, including price, description and stock status
- Search products by name or description
- Browse products by category
- Admin view for checking stock levels and viewing orders
- Admin tools to add, edit and delete products
- Backend API built with Express and connected to MySQL
- Backend checkout endpoint that creates an order, records its items and updates product stock

**Current limitations:** The customer-facing cart and checkout flow are not implemented in the frontend. There is no login/authentication functionality, and the cart icon is currently only a visual element. The project code does not implement pet-related services, although service-related tables are included in the database schema.

## Tech stack

- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Node.js, Express 5
- **Database:** MySQL (via `mysql2`)
- **Tools:** dotenv, cors, nodemon, Git, GitHub

## Getting started

### Prerequisites

- [Node.js](https://nodejs.org/)
- A running MySQL server
- MySQL Workbench (optional, for importing the database schema)

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/mettekarlsson/project-pimpMyPet.git
   cd project-pimpMyPet
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Configure the database connection.

   `server.js` currently connects to a local MySQL server using hard-coded connection settings (`host`, `user`, `password` and `database`). The database name expected by the application is `pimpmypet`. Update the connection settings in `server.js` to match your local MySQL setup before starting the server.

   Although `dotenv` is installed and `.env` is loaded, the current `server.js` does **not** read database credentials from environment variables. Do not commit real database credentials to the repository; for a safer setup, move these settings to environment variables and read them with `process.env`.

4. Create the database and tables.

   The repository includes a `schema.sql` file containing the database structure for `pimpmypet`. It defines the tables, columns, primary keys, indexes and foreign-key relationships.

   You can import the schema using MySQL Workbench:

   - Open MySQL Workbench and connect to your local MySQL server.
   - Go to **File → Open SQL Script** and select `schema.sql`.
   - Execute the script.

   Alternatively, run the following command from the project directory:

   ```bash
   mysql -u YOUR_USERNAME -p < schema.sql
   ```

   The schema creates the `pimpmypet` database if it does not already exist.

   **Note:** `schema.sql` contains the database structure only, not the original data. You will need to add product, category and customer records before testing the application. Checkout also requires an existing customer record because orders reference a `customer_id`.

5. Start the server:

   ```bash
   npm run dev
   ```

   This runs `nodemon server.js`, which restarts the server when files change.

6. Open the frontend in your browser at `http://localhost:8080`.

## Database structure

The `schema.sql` file defines the following 10 tables:

| Table | Purpose |
| ----- | ------- |
| `products` | Stores product information, prices, stock and descriptions |
| `categories` | Stores product categories |
| `categories_products` | Links products to categories |
| `customers` | Stores customer information |
| `orders` | Stores customer orders |
| `orders_products` | Links products to orders, including quantity and purchase price |
| `services` | Stores service information |
| `services_details` | Stores additional service details |
| `categories_services` | Links services to categories |
| `services_orders` | Links services to orders |

The database uses primary keys, indexes and foreign-key constraints to define relationships between related tables.

**Note:** The service-related tables are part of the database schema but are not currently used by the application's frontend or backend functionality.

## API overview

### Public endpoints

| Method | Endpoint | Purpose |
| ------ | -------- | ------- |
| `GET` | `/products` | Get all products |
| `GET` | `/products?search=<term>` | Search product names and descriptions |
| `GET` | `/products/:id` | Get one product |
| `GET` | `/categories` | Get all categories |
| `GET` | `/category/:id` | Get products in a category |
| `GET` | `/orders/customer/:id` | Get orders for a customer |
| `GET` | `/orders/:id` | Get the products and purchase details for an order |
| `POST` | `/orders/checkout` | Create an order, add its order lines and decrement stock |

### Admin endpoints

| Method | Endpoint | Purpose |
| ------ | -------- | ------- |
| `GET` | `/admin/products` | Get product stock information |
| `GET` | `/admin/orders` | Get all orders |
| `POST` | `/admin/add-product` | Add a product |
| `PATCH` | `/admin/edit-product/:id` | Update product fields |
| `DELETE` | `/admin/products/:id` | Delete a product |

**Note:** The admin endpoints are not protected by authentication or authorization in the current implementation.

## Project structure

```text
├── index.html          # Main storefront page
├── app.js              # Frontend logic for browsing and searching products
├── style.css           # Storefront styles
├── index-admin.html    # Admin page
├── admin.js            # Admin page logic
├── adminStyle.css      # Admin page styles
├── server.js           # Express API and MySQL access
├── schema.sql          # MySQL database schema
├── assets/             # Images and other static files
├── favicon_io/         # Favicons
└── package.json
```

## My contribution

