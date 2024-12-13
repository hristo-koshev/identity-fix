# Our goal is to create a single Authentication Endpoint for all connected applications:

1. We have 3 applications, which serve different busness functionality, but they require a single pool of users accounts to be used in all applications.
2. Each application currently issue it's own JWT (JSON Web Tokens) on each users login and authenticate them with user information from it's Database users information.
3. In order this information to be synchronize we currently runing 3 chron scripts to seed all new data from each Database to each other.

# Propposed Solution for users accounts:

Instead we could all application Authentication to Identity app and it will be only place which will create and issue JWT tokens:
   1. JWT tokens should hold a roles field, which will hold roles which user holds and each application will keep there existing Authorization logic, according to those roles.
   2. JWT tokens includes an issuer signature and validity timeframe, which must be validated by all applications, that JWT token signature 
      is valid (this requires JWT token Pubik key is to be exposed to all application)
   3. The RWA and SpaceGalaxy would not have any new user data in there database and would redirct create user request to Identity Application
   4. We should keep chron script running for syncronising the additional information or we could use Redis Stream functionality, where each application could post 
      Events with newly or updated data and could be consumed by every application which need to be updated accordingly.         
