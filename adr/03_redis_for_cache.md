# ADR-001: Use Redis for Caching

## Status

Accepted

## Context

The application requires a fast and efficient caching layer to handle frequent queries for vulnerability data. The dataset is relatively small (approximately 200,000 vulnerabilities over the last 10 years), making it manageable in memory. Additionally, the system needs full-text search capabilities and numeric range filtering for querying vulnerabilities.

Several alternatives were considered:

1. **Memcached**: A lightweight caching solution.
2. **Elasticsearch**: A powerful search engine with full-text search capabilities.
3. **PostgreSQL with Full-Text Search**: Using the primary database for search and caching.

## Decision

- Use **Redis** as the caching layer for the application.
- Leverage the **RediSearch** module for full-text search and numeric range queries.

## Consequences

### Positive Consequences

1. **Performance**:

   - Redis is an in-memory data store, making it extremely fast for both reads and writes.
   - It can handle millions of operations per second, ensuring real-time performance for queries.

2. **Advanced Features**:

   - The RediSearch module provides full-text search capabilities and supports complex queries, such as combining text search with numeric filters.

3. **Scalability**:

   - Redis can be scaled horizontally using clustering, making it suitable for handling larger datasets and higher traffic in the future.

4. **Memory Efficiency**:

   - The dataset size (200,000 vulnerabilities found in the last 10 years) is manageable in RAM, even on modestly sized servers.

5. **Ease of Use**:
   - Redis is simple to set up and integrates well with various programming languages and frameworks.

### Negative Consequences

1. **Additional Infrastructure**:

   - Redis requires additional infrastructure to manage alongside PostgreSQL.

2. **Consistency**:

   - Cached data in Redis may become stale if not synchronized with the database. This requires implementing mechanisms like TTL (Time-To-Live) or periodic synchronization jobs.

3. **Limited Analytics**:
   - Redis is not designed for complex analytics or aggregations, which may require additional tools like PostgreSQL or Elasticsearch.

---

### Alternatives Considered

#### **Memcached**

- **Pros**:
  - Simple and lightweight.
  - High performance for basic key-value caching.
- **Cons**:
  - Limited data structure support (only key-value pairs).
  - No built-in persistence or advanced querying capabilities.
  - Lacks modules like RediSearch for full-text search.

#### **Elasticsearch**

- **Pros**:
  - Excellent for full-text search and analytics.
  - Supports distributed architecture and handles large-scale data.
- **Cons**:
  - Higher memory and CPU usage compared to Redis.
  - More complex to set up and maintain.
  - Slower for real-time caching compared to Redis.

#### **PostgreSQL with Full-Text Search**

- **Pros**:
  - Built-in full-text search capabilities.
  - No need for an additional caching layer if PostgreSQL is already in use.
- **Cons**:
  - Slower than Redis for real-time queries.
  - Higher latency for frequent reads and writes compared to an in-memory store.

---

## Final Decision

Redis was chosen because it strikes the right balance between performance, simplicity, and functionality. Its in-memory architecture ensures real-time performance, and the RediSearch module provides the necessary search capabilities. While it introduces additional infrastructure, its benefits outweigh the drawbacks for this use case.

---
