# Design Location based service

## Function requirements
1. User should be able to search for the nearby places
2. User should be able to see the details of an individual place
3. User should be able to add a new bussiness in the system

## Non functional requirements
1. The system should have low latency
2. The system should have high availability

## Some numbers :
1. System will have around 100 million daily active users
2. System will approximately have 200 million bussiness/places registered
3. Each user will make approximately 5 search requests in a day


!["Proximity Service"](proximity-service.PNG?raw=true)

Since the system is read heavy and the addition of a new place/business in not very frequent.
This makes a great case for using the read replicas, this will help in offloading the load from primary db.
All the read traffic can directlt go to any of the read replicas.


# References :
https://www.youtube.com/watch?v=M4lR_Va97cQ (Very good)
