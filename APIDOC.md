## APIDOC.md Requirements

<!-- This API provides endpoints for a sneaker shopping application, allowing users to sign up, log in, view items, confirm and purchase items, and view their order history. The API uses SQLite for data storage and Express.js for handling HTTP requests. -->

## Endpoints

### 1. User Authentication

#### 1.1. Register User

- **Endpoint:** `POST /signup`
- **Description:** Registers a new user.
- **Request Parameters:**
  - `username` (string) - The username of the new user.
  - `password` (string) - The password of the new user.
- **Return Type:**
  Text/Plain
- **Side-Effects:**
  Stores the new user's information in the database.
- **Example Request:**
  ```json
  {
  "username": "newuser",
  "password": "password123"
  }
  ```
- **Response:**
  "message": "Successfully signed up."
- **Client Error:**
  Status: 400 Bad Request
  Body:
  "Missing required fields."
  or
  "Username already exists."
- **Server Error:**
    Status: 500 Internal Server Error
    Body:
    "Encountered an error on the server."
#### 1.2. Login User

**Endpoint:** `POST /login`

- **Description:** Authenticates a user and provides a session token.
- **Request Parameters:**
- `username` (string) - The username of the user.
- `password` (string) - The password of the user.
- **Return Type:**
  JSON
- **Side-Effects:**
  Updates the user's session token in the database.
- **Example Request:**
  ```json
  {
  "username": "existinguser",
  "password": "password123"
  }
  ```
- **Response:**

  ```json
  {
  "token": "sessionToken123",
  "message": "Successfully logged in!"
  }
  ```
- **Client Error:**
  Status: 400 Bad Request
  Body:
  {
  "message": "Missing required fields."
  }
  or
  {
  "message": "User does not exist."
  }
  or
  {
  "message": "Invalid password."
  }
- **Server Error:**
  Status: 500 Internal Server Error
  Body:
  {
  "message": "Encountered an error on the server."
  }

### 2. Product Management

#### 2.1. Get All Products

- **Endpoint:** `GET /items/sneakers`
- **Description:** Retrieves a list of all products.
- **Request Parameters:** None
- **Return Type:**
  JSON
- **Example Request:**
  GET /items/sneakers
- **Response:**

  ```json
  {
    "id": "<item_id>",
    "name": "<item_name>",
    "image": "<image_path>",
    "price": <item_price>,
    "availability": <item_availability>,
    "gender": "<item_gender>"
  },
  ...
  ]
  ```
- **Server Error:**
  Status: 500 Internal Server Error
  Body:
  "Encountered an error on the server."


#### 2.2. Get Product Details

- **Endpoint:** `GET /items/sneakers/{id}`
- **Description:** Retrieves a specific sneaker item based on its ID.
- **Request Parameters:**
  id (integer) - The ID of the sneaker item.
- **Return Type:**
  JSON
- **Example Request:**
  GET /items/sneakers/1
- **Response:**

  ```json
    {
    "id": "1",
    "name": "Acceleration 2",
    "image": "img\sneakers\acceleration-2.png",
    "price": 250,
    "availability": 11,
    "gender": "women"
  }
  ```
- **Client Error:**
  Status: 400 Bad Request
  Body:
  {
  "message": "Item not found"
  }
- **Server Error:**
  Status: 500 Internal Server Error
  Body:
  {
  "message": "Server error"
  }

### 3. Transaction Management

#### 3.1. Process Purchase

- **Endpoint:** `POST /transaction`
- **Description:** Processes the purchase of confirmed items.
- **Request Parameters:**
  orderItems (array): An array of objects containing
  itemId (number)
  quantity (number) for each item.
- **Return Type:**
  JSON
- **Side-Effects:**
  Updates the item availability and stores the order details in the database.
- **Example Request:**
  ```json
  [
  {
    "itemId": 1,
    "quantity": 2
  },
  {
    "itemId": 2,
    "quantity": 1
  }
  ]
  ```
- **Response:**

  ```json
  {
  "message": "Purchase successful!",
  "confirmationCode": "<confirmation_code>"
  }
  ```
- **Client Error:**
  {
    "message": "Item not found."
  }
  or
  {
  "message": "Item not in stock"
  }
- **Server Error:**
  Status: 500 Internal Server Error
  Body:
  "Encountered an error on the server."

### 3.2 Get Order History
- **Endpoint:** `GET /orderHistory`
- **Description:** Retrieves the order history of the authenticated user.
- **Request Parameters:**
  none
- **Return Type:**
  JSON
- **Example Request:**
  GET /orderHistory
- **Response:**
  ```json
  [
  {
    "orderId": 1,
    "userId": 1,
    "itemId": 1,
    "quantity": 2,
    "confirmation": "confirmationCode123"
  },
  {
    "orderId": 2,
    "userId": 1,
    "itemId": 2,
    "quantity": 1,
    "confirmation": "confirmationCode123"
  }
  ]
  ```
- **Server Error:**
  Status: 500 Internal Server Error
  Body:
  "Encountered an error on the server."
### 4. User Management

### 4.1. Check If User Is Logged In
- **Endpoint:** `GET /isLoggedIn`
- **Description:** Checks if the user is currently logged in based on the session token.
- **Request Parameters:**
  none
- **Return Type: Text/Plain:**
  JSON
- **Example Request:**
  GET /isLoggedIn
- **Response:**
  ```json
  {
  "isLoggedIn": true,
  "username": "existinguser",
  "id": 1
  }
  or
  {
  "isLoggedIn": false
  }
  ```
- **Client Error:**
  Status: 400 Bad Request
  Body:
  "Unauthorized."
- **Server Error:**
  Status: 500 Internal Server Error
  Body:
  "Encountered an error on the server."