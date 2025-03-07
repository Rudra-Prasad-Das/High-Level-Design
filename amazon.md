# Design Amazon (E-Commerce Website)

## Functional Requirements :

1. USer should be able to list down all the products (Inventory)
2. There should be a recommendation system on the user profile
3. Place Orders 
4. View the status of the orders.

## Assumptions:

1. User profile creation is provided.
2. Product Onboarding is provided.
3. Payment gateway is provided.

## Non Functional Requirements :

1. Low latency ( For the user-facing components)
2. High Consistency ( for the payments and the orders placed)

## Capacity Estimations : 

Active Users  = 300 M users / month making 10 searches a day.
              = 300*10 searches / day 
              = 3B searches / (30days * 24h * 60min * 60s)
              = 1000 searches / s

Total Products = 10M
               = 1 product requires 10MB space
               = 100 TB space

Database Design : 

User (SQL Data base)

- UserID (Primary Key)
- Password(String) Encrypted
- First Name(String)
- Last Name(String)
- Email (String)
- Last Loggedin (DTTM)
- Profile Created (DTTM)


Address :

- AddressID (Primary Key)
- UserID (Foreign Key)
- Address (String)
- State (String)
- Country (String)
- Pin (aplhanumeric)
- Address Type (Integer)


 Product : 

The product data is not very structured. 
Database choice : No-SQL Database / Document DB  -> MongoDB or DynamoDB

Order :

While SQL databases typically require two separate tables—one for storing order details and another for storing the individual items within each order—using a NoSQL database offers several advantages, especially when dealing with large, variable numbers of items in each order.

With a NoSQL database, such as MongoDB, you can store all the items within an order as an array inside the same JSON document. This eliminates the need for multiple tables and complex joins, making data retrieval simpler and more efficient.

### APIs :

- getRecommendations(UserId)  : return list of 10-15 product recommendations based on users past activity
- Search(SearchString, UserId) : GET method and it returns the list of products based on availability on the userId location
- AddToCart(ProductId,UserId,Qty,Amount) : return boolean
- PlaceOrder(ProductId, UserId, AddressId, PaymentStatus) : returns boolean
- CheckOrderStatus(OrderId) : return boolean

# Simplified Version:
Let’s break down the high-level system design and its components:

- Search: When a user searches for a product, the search service looks up the product in the product database. We store this search in a search history database. Then, we use batch jobs with machine learning algorithms to recommend products based on the user's search history. These recommendations are stored in a recommendation database for fast access.

- Add to Cart: Before placing an order, users must add items to their cart. This is handled by the add-to-cart service. Users can add products either from search results or recommended products. The service stores these items in a cart database.

- Place Order: After adding items to the cart, users can place an order. The place order service removes the items from the cart database and adds them to the order database.

- Order Status: Users can check the status of their orders. The check order status service retrieves this from the order database and shows it to the user.

- Product Reviews: Users can read and write product reviews. The product review service fetches reviews from the product review database and displays them to the user. This service can also be triggered through the search or recommendation service.

## Scaling the System: To handle millions of users, we use:

- Load Balancer: To evenly distribute traffic across multiple servers and ensure system stability.
- Multiple Service Instances: We run multiple instances of each service to prevent downtime.
- Elasticsearch: For fast product search.
- Master-Slave Database Design: To manage a read-heavy product database by separating read and write operations.
- Database Redundancy: Multiple copies of databases to prevent data loss.
- Data Archiving: We archive old search history and orders to maintain performance, with a retention policy for deleting old data after a few years.
- Inventory Check: Before placing an order, we check inventory availability.
- Caching: We store recommendations, reviews for popular products, and recent order status in a cache for fast access.

- Message Queue: We use a message queue between recommendation and add-to-cart services to decouple services and reduce dependencies.

Now we have a fully scaled system that can handle millions of users efficiently.

### Key Points:
- Search Function:

Users search for products via the search service, and search history is stored in a database.
Machine learning generates product recommendations, which are stored for faster retrieval.
Cart and Order Management:

- Add-to-cart service lets users add products from search or recommendations.
When an order is placed, the items are moved from the cart database to the order database.
Order Status and Product Reviews:

Users can check their order status and read/write product reviews from respective services.
System Scalability:

- Load balancers and multiple service instances are used to evenly distribute traffic.
- Elasticsearch speeds up search operations.
- Master-slave database design separates read and write operations for better performance.
- Redundant databases protect against data loss.
- Data Management:

- Archiving old search history and orders to maintain performance.
- Inventory checks are done before placing orders.
- Caching and Message Queue:

- Caching stores recommendations, popular product reviews, and recent order statuses.
- A message queue decouples recommendation and cart services.
#### Summary:
The system design includes various services for searching, adding to the cart, placing orders, and reviewing products, all built for scalability and performance. It uses load balancing, multiple database copies, caching, and message queues to ensure that the system handles millions of users smoothly. Additionally, machine learning helps in recommending products, and older data is archived to maintain system efficiency.
