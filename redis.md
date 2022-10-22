#### Redis

```diff
+ 6379/tcp
```

There are different types of databases and one among them is Redis, which is an 'in-memory' database. 
In-memory databases are the ones that rely essentially on the primary memory for data storage (meaning that the database is managed in the RAM of the system); 
in contrast to databases that store data on the disk or SSDs. 
As the primary memory is significantly faster than the secondary memory, the data retrieval time in the case of 'in-memory' databases is very small, 
thus offering very efficient & minimal response times.

In-memory databases like Redis are typically used to cache data that is frequently requested for quick
retrieval. For example, if there is a website that returns some prices on the front page of the site. 
The website may be written to first check if the needed prices are in Redis, and if not, then check the traditional
database (like MySQL or MongoDB). When the value is loaded from the database, it is then stored in Redis
for some shorter period of time (seconds or minutes or hours), to handle any similar requests that arrive
during that timeframe. For a site with lots of traffic, this configuration allows for much faster retrieval for the
majority of requests, while still having stable long term storage in the main database.
