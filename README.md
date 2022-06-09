# Introduction

Create a backend for an Ecommerce website using microservices architecture and support for filtering data etc. 

Tech Stack required:
- NodeJS + TS
- OpenAPI Doc
- Testing using Jest
- TypeORM + Postgres
- Microservices Architecture

Bonus points for using Redis, ElasticSearch, Indexing in DB, adding support for filtering Data in GET reqs.

Explanation:
1) Users Service:
      i) This service stores the User information, no data related to authentication should be stored. (userType or authorization related data however can be stored)
      ii) Create/Read/Update/Delete User(s): Since we're building using Microservices architecture, Make sure Create/Read/Delete requests support bulk requests (look at the bottom for more info)
      iii) Create User model as per your choice but make sure it at least has the following: Name, UserId, phone number, address 
2) Orders Service:
      i) Create/Read/Update Order(s): Bulk request for Create/Read/Update.
      ii) Order Model must have the following + any additional field you would like to add: orderId, userId (ref), status, createdAt, updatedAt.
3) Auth Service:
      i) This service will sign and create accessTokens and refreshTokens.
      ii) Create and sign accessTokens using the 'JWT' library
      iii) For RefreshTokens create a separate Table (can use Redis here) for RefreshTokens which has the following: tokenId (UUID), userId (ref) and expiry (Date), Make tokenId as UUID and use this as the RefreshToken.
      iv) Create a cron job that checks for expired refreshTokens every 30 mins, a refreshToken is valid if it exists in the RefreshToken Table otherwise its invalid.
      v) Create a UserAuth table that stores user login data with its password salted and hashed/encrypted (use bcrypt) keep the model as: userId, username, password

Additional Explanation:

Bulk Requests: Bulk reqs take in one or more parameters (array of userIds for example) and work on an array of data and return the updated data. For example, Create Users would take in one or more "User" objects in its requestBody and would bulk create the Users after request validation. Run the bulk in a tracsaction so that if any one of the creation fails the whole creation is rolledback and an error is returned to the user.
