# E-Commerce API with Caching and Notifications

This project is a Django-based REST API for a simple e-commerce system. It allows users to register, log in, browse products, manage their profiles, place orders, and receive real-time notifications. The API also leverages caching for performance optimization and includes pagination and filtering for product listings.

## Features

1. **User Authentication (JWT-based)**:
   - User registration, login, and profile management.
   - JWT token-based authentication using `djangorestframework-simplejwt`.

2. **Product and Category Management**:
   - Admins can manage products and categories (CRUD operations).
   - Product stock management (stock decreases when a product is ordered).

3. **Order Management**:
   - Users can add products to their cart and place orders.
   - Order status updates (pending, shipped, delivered) with real-time notifications using Django Channels.

4. **Caching**:
   - Redis caching to optimize product and category queries.

5. **Pagination & Filtering**:
   - Pagination for product listings (10 products per page).
   - Filtering by category, price range, and stock availability.

## Table of Contents

- [Project Overview](#project-overview)
- [Technologies Used](#technologies-used)
- [Setup Instructions](#setup-instructions)
- [Endpoints](#endpoints)
  - [User Authentication](#user-authentication)
  - [Product Management](#product-management)
  - [Order Management](#order-management)
- [Testing with Postman](#testing-with-postman)
- [Redis Caching](#redis-caching)
- [Real-Time Notifications](#real-time-notifications)

## Project Overview

The project demonstrates how to build a scalable and efficient e-commerce backend system using Django, Django REST Framework, and Redis. It supports user authentication, product management, order placement, and real-time notifications. The API uses Redis for caching and channels for WebSocket-based notifications.

## Technologies Used

- **Django**: Backend framework.
- **Django REST Framework**: For creating RESTful APIs.
- **Django Channels**: For WebSocket support and real-time notifications.
- **Redis**: In-memory data store used for caching.
- **PostgreSQL**: Relational database.
- **JWT (JSON Web Tokens)**: For secure token-based authentication.

## Setup Instructions

### Prerequisites

- Python 3.7+
- PostgreSQL
- Redis
- Virtual environment (recommended)

### Step 1: Clone the Repository

```bash
git clone https://github.com/aansh06/Advanced-E-commerce-API.git
cd Advanced-E-commerce-API
```

### Step 2: Create and Activate Virtual Environment

```bash
python -m venv env
source env/bin/activate  # On Windows use `env\Scripts\activate`
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Set Up PostgreSQL and Redis

- Create a PostgreSQL database and update your `.env` file with the database credentials.
- Make sure Redis is running locally (default port `6379`).

### Step 5: Configure Environment Variables

Create a `.env` file in the project root directory and add the following environment variables:

```bash
DB_NAME=your_db_name
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_HOST=localhost
DB_PORT=5432
REDIS_HOST=localhost
REDIS_PORT=6379
```

### Step 6: Apply Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

### Step 7: Create a Superuser

```bash
python manage.py createsuperuser
```

### Step 8: Run the Development Server

```bash
python manage.py runserver
```

The API will be accessible at `http://127.0.0.1:8000/`.

## Endpoints

### User Authentication

- **Register**: `POST /register/`
  - Payload: `{ "email": "user@example.com", "password": "password123" }`
- **Login**: `POST /login/`
  - Payload: `{ "email": "user@example.com", "password": "password123" }`
- **Profile Management**: `GET /profile/`, `PUT /profile/`
  - View or update profile information.
- **Order History**: `GET /orders/`

### Product Management

- **Create Category** (Admin): `POST /categories/`
- **Create Product** (Admin): `POST /products/`
- **Product List**: `GET /products/`
  - Supports pagination and filtering by category, price range, and stock availability.
  - Example: `GET /products/?category=electronics&price_min=100&price_max=500`

### Order Management

- **Add to Cart**: `POST /cart/`
  - Add products to the cart.
- **Place Order**: `POST /orders/`
  - Place an order from the cart.
- **Order Status**: Real-time updates via WebSocket on status changes (pending, shipped, delivered).

## Testing with Postman

### 1. Register a User

- **Endpoint**: `POST /register/`
- **Payload**:
  ```json
  {
    "email": "user@example.com",
    "password": "password123"
  }
  ```

### 2. Login and Obtain JWT Token

- **Endpoint**: `POST /login/`
- **Payload**:
  ```json
  {
    "email": "user@example.com",
    "password": "password123"
  }
  ```
- Use the JWT token in subsequent requests to access protected endpoints.

### 3. Create Categories and Products (Admin)

- **Endpoint for Categories**: `POST /categories/`
- **Endpoint for Products**: `POST /products/`

### 4. Place an Order

- **Endpoint**: `POST /orders/`
- Add products to the cart, then place an order.

## Redis Caching

The API uses Redis for caching product and category data to enhance performance. Redis is configured to cache product listings and categories with a timeout of 1 hour. When a product or category is updated, the cache is invalidated.

- **Cached Queries**: Product and category listings.
- **Cache Expiration**: 1 hour.

## Real-Time Notifications

Real-time notifications are implemented using Django Channels. When an order's status changes (e.g., from pending to shipped), the user is notified in real time.

- **WebSocket Endpoint**: `/ws/orders/`
- **Order Status**: Users receive live updates when their order status is updated.
