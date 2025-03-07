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

