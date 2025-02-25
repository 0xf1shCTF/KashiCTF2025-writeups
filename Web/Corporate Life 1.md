Corporate Life was is a challenge displaying a dashboard that making queries.

By looking at burpsuite we can see that it is calling /api/list to get its list. This is a get request and parameters do not seem to work. Trying any other type of request returns that only get requests are allowed.

Looking at the `build manifest` you see :

``` js
        sortedPages: ["/", "/_app", "/_error", "/v2-testing"]
```

v2-testing queries a different endpoint called api/list-v2. This allows you to filter with a post request. However the name of the area such as 'filter' does not match with the field it is querying for 'department'. Trying a basic sql injection of "\' OR 1=1 --" solves the challenge when placed with filter. This takes out the part of the query that only fetches requests where status is pending and gets you the flag.

    Shitty job, I hate working here, I will leak all important information like KashiCTF{s4m3_old_c0rp0_l1f3_6mJMFQxU}
