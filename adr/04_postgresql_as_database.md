# ADR-004: Use PostgreSQL as the Primary Database

## Status

Accepted

## Context

The application requires a reliable and scalable database to store vulnerability data persistently. The database should:

- Support complex queries and filtering (e.g., by title, criticality range).
- Be robust and capable of handling concurrent read and write operations.
- Provide strong consistency and transactional guarantees.
- Be widely supported and easy to integrate with the chosen technologies (FastAPI and .NET).

Several Python-compatible databases were considered:

1. **PostgreSQL**: A powerful open-source relational database.
2. **MySQL**: Another popular open-source relational database.
3. **SQLite**: A lightweight, file-based relational database.
4. **MongoDB**: A NoSQL database designed for document storage.

## Decision

- Use **PostgreSQL** as the primary database for storing vulnerability data.

## Consequences

### Positive Consequences

1. **Advanced Querying**:

   - PostgreSQL supports advanced SQL features, including filtering, sorting, and full-text search, which are essential for querying vulnerabilities.

2. **Reliability**:

   - PostgreSQL is known for its robustness and strong ACID (Atomicity, Consistency, Isolation, Durability) compliance, ensuring data integrity.

3. **Scalability**:

   - PostgreSQL can handle large datasets and high-concurrency workloads, making it suitable for future growth.

4. **Community and Ecosystem**:

   - PostgreSQL has a large community and extensive documentation, making it easy to find resources and support.

5. **Integration**:
   - PostgreSQL integrates seamlessly with Python (via `psycopg2`), ensuring compatibility with the chosen technologies.

### Negative Consequences

1. **Setup and Maintenance**:

   - PostgreSQL requires more setup and maintenance compared to lightweight databases like SQLite.

2. **Resource Usage**:
   - PostgreSQL consumes more resources (CPU, memory) than simpler databases like SQLite, which may be overkill for very small-scale applications.

---

### Alternatives Considered

#### **MySQL**

- **Pros**:
  - Similar to PostgreSQL in terms of features and performance.
  - Slightly easier to set up for beginners.
- **Cons**:
  - Lacks some advanced features of PostgreSQL, such as richer JSON support and window functions.
  - Historically weaker community support for Python compared to PostgreSQL.

#### **SQLite**

- **Pros**:
  - Lightweight and easy to set up.
  - No need for a separate database server.
- **Cons**:
  - Not suitable for high-concurrency or large-scale applications.
  - Limited support for advanced SQL features.

#### **MongoDB**

- **Pros**:
  - Flexible schema design, ideal for unstructured or semi-structured data.
  - High performance for certain use cases (e.g., document storage).
- **Cons**:
  - Lacks the strong consistency and transactional guarantees of PostgreSQL.
  - Not ideal for relational data like vulnerabilities, which have structured relationships.

---

## Final Decision

PostgreSQL was chosen because it provides the best balance of performance, reliability, and advanced features. Its strong support for relational data and compatibility with Python make it the ideal choice for this project.

---
