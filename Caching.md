Point#1: What we know is caching is for speeding up load times. But in reality, it is protect to DB.
Point#2: In caching you choose acceptable wrongness.

Caching Topics:
1. Cache Hit / Cache Miss
2. TTL : How long cache lives in cache before they are removed automatically from cache.
3. Eviction Policy: Which data to remove from cache when cache is full. LRU, LFU
4. Cache Invalidation: When data is changed, how do you update or remove stale data from cache
5. Caching Techniques: Write-Through, Write-Around, Write-Back, Cache Aside
   a. Write-Through: Data is written to both cache and database at the same time. This ensures consistency but makes writes a bit slower.
   b. Write-Around: Writes skip the cache and go directly to the database. Caching happens when data is read.
   c. Write-Back: Writes go to the cache first, and the database is updated later in the background. This improves write speed but risks loss if the cache fails before flushing.
   d. Cache-Aside: Writes to DB and Cache is invalidated. Later cache happens after read.
7. Distributed Cache vs Local Cache
8. Hot Keys and Cache Stampede
