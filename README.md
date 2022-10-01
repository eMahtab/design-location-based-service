# Design Location based service

## Functional requirements
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
All the read traffic can directly go to any of the read replicas.


# Naive Approach : Table Scan - To find the nearby businesses/places within the radius of user (e.g. 500 meter or 1000 meter) : O(n) operation

```sql
SELECT business_id, latitude, longitude
FROM business
WHERE (latitude BETWEEN myLatitude - radius AND myLatitude + radius)
AND (longitude BETWEEN myLongitude - radius AND myLongitude + radius)         
```

# Lets make it faster : GeoHash to the rescue
A geohash is a convenient way of expressing a location (anywhere in the world) using a short alphanumeric string, **with greater precision obtained with longer strings.**

https://www.movable-type.co.uk/scripts/geohash.html

!["Geohash"](geohash.PNG?raw=true)


## Choosing geohashing precision for your application
Selecting the correct geohash length is important for the efficiency of the search.

## Too Precise : Too much Zoom In

If the geohash is too precise, meaning the cell is small, the search radius may include many more cells:
![Too Precise](too-precise.png?raw=true)

## Too Coarse : Too much Zoom Out

If the geohash is too coarse, meaning the cell is too large, the query may return too many results for a single cell for the search to be efficient. You may also find a large number of results in those cells are not within the search radius:
![Too Coarse](too-coarse.png?raw=true)

# References :
https://www.youtube.com/watch?v=M4lR_Va97cQ (Very good)

Implementing Geohashing at scale in Serverless applications https://aws.amazon.com/blogs/compute/implementing-geohashing-at-scale-in-serverless-web-applications/
