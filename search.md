Postgres can support rich search functionality (substring search, fuzzy search, semantic search) through pg_vector and pg_trgm extensions. This is good for small to medium scale. 

But, at scale

**1. Resource Contention (The "Throttling" Effect)**
Your main database is responsible for Writes (users listing books, updating profiles, processing payments).

The Conflict: Search is CPU and RAM intensive. If a user performs a heavy "Fuzzy Search" or a "Vector Similarity Search" on millions of rows, it can spike the CPU to 100%.

The Result: Your payment processing or checkout flow hangs because the CPU is too busy calculating "Biryani" distances or trigram scores. In a scaled system, you never want your "Discovery" (Search) to kill your "Transaction" (Buying).

**2. Lack of "Horizontal" Scaling**
Postgres is primarily a Vertical Scaling database. To handle more load, you usually have to buy a bigger, more expensive server (more RAM, more CPU).

Elasticsearch Advantage: It is "Distributed" by design. You can start with 3 small servers and grow to 30. It splits your 750M products into "Shards" across these servers.

The Postgres Wall: While you can use "Read Replicas" in Postgres, managing 50 replicas just for search becomes an operational nightmare compared to an Elasticsearch cluster.

**3. The "Expensive" Indexing**
Indexing 750M records with GIN (for trigrams) or HNSW (for vectors) creates massive index files.

Write Performance: Every time you insert a new book, Postgres has to update those heavy indexes. This slows down your INSERT and UPDATE queries significantly.

Memory Pressure: To keep search fast, Postgres needs to keep those indexes in the Buffer Cache (RAM). At scale, your indexes will eventually become larger than your RAM, forcing the database to read from the SSD, which is much slower.

**4. Limited Ranking Logic**
Search is about Relevance, not just matching.

SQL Limitations: In SQL, it is very hard to write a query that says: "Find books where the title matches 80%, the author matches 100%, the seller is within 5km, and the user has a 5-star rating."

The "Scoring" Engine: Elasticsearch uses algorithms like BM25 and "Function Score" queries natively. Doing this in Postgres requires complex math in your WHERE and ORDER BY clauses, which are notoriously slow and hard to optimize.

**5. No Native "Did You Mean?" or Stemming**
Postgres sees "Running" and "Run" as different words unless you use complex Full-Text Search (FTS) configurations.

The UX Gap: Elasticsearch handles Stemming (understanding word roots) and Synonyms (searching "Cell phone" finds "iPhone") out of the box.

Typo Tolerance: While pg_trgm helps with typos, it isn't as tunable as Elasticsearch’s "Fuzziness" parameters, which allow you to control exactly how many character "edits" a user can make.
