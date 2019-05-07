## Design consideration 

- single machine, single threaded scenario
- single machine, multi threaded scenario (race conditions)
- distributed scenario (use distributed cache like Redis)
- client side (throttles network calls)

## Thoughts
- how to deal with delay from reading/setting the values in distributed system? Add additional time

## Algorithms

- leaky bucket
- token bucket
- fixed window counter
- sliding window logs
- sliding window counters

## Fixed Window Counter

- create buckets for a time window (if it's 5 seconds, then the time window will be 00:00, 00:05, 00:10)
- each bucket holds a count
- space complexity: O(1) - storing the current window count
- time complexity: O(1) - get and simple atomic increment operation
- can be incorrect, take the 1 hr time window, 00:00, 01:00, 5 req/hr is not honored if we take the time from 00:30 to 01:30

## Sliding Window Logs

- store every timestamps for the events that happen
- when making a new request, purge all events that time older than the time window. This can be done lazily or in the background
- the new timestamp is appended to the user's queue
- check the length as the count
- space complexity: O(max requests seen in time window) - stores all timestamps of the requests in a time window
- time complexity: O(max requests seen in a window time) - deleting a subset of timestamps
- high memory footprint
- high time complexity for removing older timestamps
- works perfectly

## Sliding Window Counter

- hybrid of both fixed window counters and sliding window logs
- break time window into smaller buckets
- each bucket stores the request count
- space complexity: O(number of buckets)
- time complexity: O(1) fetch the recent bucket, increment and check against the total sum of buckets

Implementation of the various rate limiting algorithms here:

- https://github.com/alextanhongpin/go-learn/blob/master/ratelimit.md

- https://medium.com/@saisandeepmopuri/system-design-rate-limiter-and-data-modelling-9304b0d18250