# ADR-002: Use Uvicorn as the ASGI Server

## Status

Accepted

## Context

The application requires an ASGI (Asynchronous Server Gateway Interface) server to run the FastAPI application. The server should:

- Be lightweight and fast.
- Support asynchronous programming to handle concurrent requests efficiently.
- Be fully compatible with FastAPI and provide good performance in production environments.

Several Python-based ASGI servers were considered:

1. **Uvicorn**: A lightning-fast ASGI server built on `uvloop` and `httptools`.
2. **Hypercorn**: A flexible ASGI server supporting HTTP/2 and WebSockets.
3. **Gunicorn with Uvicorn Workers**: A production-grade WSGI/ASGI server that can use Uvicorn workers for ASGI support.

## Decision

- Use **Uvicorn** as the ASGI server for running the FastAPI application.

## Consequences

### Positive Consequences

1. **Performance**:

   - Uvicorn is one of the fastest ASGI servers available, thanks to its use of `uvloop` (a high-performance event loop) and `httptools` (a fast HTTP parser).

2. **Compatibility**:

   - Uvicorn is fully compatible with FastAPI and supports all its features, including WebSockets and background tasks.

3. **Ease of Use**:

   - Uvicorn is simple to configure and run, making it ideal for both development and production environments.

4. **Community and Ecosystem**:

   - Uvicorn has a strong community and is widely used in the Python ecosystem, ensuring good support and frequent updates.

5. **Lightweight**:
   - Uvicorn has minimal dependencies, making it lightweight and easy to deploy.

### Negative Consequences

1. **Limited Features**:

   - Compared to more feature-rich servers like Hypercorn, Uvicorn has fewer built-in features (e.g., no native HTTP/2 support without additional configuration).

2. **Production Configuration**:
   - Uvicorn requires additional configuration (e.g., using a process manager like Gunicorn) to handle high-load production environments effectively.

---

### Alternatives Considered

#### **Hypercorn**

- **Pros**:
  - Supports HTTP/2 and WebSockets out of the box.
  - Highly configurable and flexible.
- **Cons**:
  - Slightly slower than Uvicorn in benchmarks.
  - More complex to configure compared to Uvicorn.

#### **Gunicorn with Uvicorn Workers**

- **Pros**:
  - Production-grade server with robust process management.
  - Can use Uvicorn workers for ASGI support, combining the strengths of both tools.
- **Cons**:
  - Slightly more complex to set up compared to standalone Uvicorn.
  - Adds additional overhead due to the Gunicorn layer.

---

## Final Decision

Uvicorn was chosen because it provides the best balance of performance, simplicity, and compatibility with FastAPI. While it requires additional configuration for production environments, its speed and ease of use make it the ideal choice for this project.

---
