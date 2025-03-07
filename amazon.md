# Design Amazon (E-Commerece Website)

## Functional Requirements :

1. USer should be able to list down all the products (Inventory)
2. There should be a recommendation system on the user profile
3. Place Orders 
4. View the status of the orders.

## Assumptions:

1. User profile creation is provided.
2.Product Onboarding is provided.
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


### APIs :

- getRecommendations(UserId)  : return list of 10-15 product recommendations based on users past activity
- Search(SearchString, UserId) : GET method and it returns the list of products based on availability on the userId location
- AddToCart(ProductId,UserId,Qty,Amount) : return boolean
- PlaceOrder(ProductId, UserId, AddressId, PaymentStatus) : returns boolean
- CheckOrderStatus(OrderId) : return boolean

