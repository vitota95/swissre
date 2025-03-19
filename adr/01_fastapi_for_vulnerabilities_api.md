# ADR-002: Use FastAPI for the Vulnerability API

## Status

Accepted

## Context

The application requires a framework to build a RESTful API for managing vulnerabilities. The framework should:

- Be lightweight and fast.
- Support asynchronous programming for handling concurrent requests efficiently.
- Provide built-in support for data validation and serialization.
- Be easy to use and maintain, with good documentation and community support.

Several alternatives were considered:

1. **Django REST Framework (DRF)**: A popular framework for building APIs in Python.
2. **Flask**: A lightweight framework for building web applications and APIs.
3. **FastAPI**: A modern framework designed for building APIs with Python 3.6+.

## Decision

- Use **FastAPI** as the framework for building the Vulnerability API.

## Consequences

### Positive Consequences

1. **Performance**:

   - FastAPI is built on **Starlette** and **Pydantic**, making it one of the fastest Python frameworks available.
   - Its asynchronous capabilities allow it to handle high-concurrency workloads efficiently.

2. **Ease of Use**:

   - FastAPI provides automatic generation of OpenAPI documentation, which can be accessed via `/docs`.
   - Its declarative syntax for defining routes and data models simplifies development.

3. **Data Validation**:

   - FastAPI uses **Pydantic** for data validation and serialization, ensuring that incoming and outgoing data adheres to the defined schema.

4. **Asynchronous Support**:

   - FastAPI's native support for `async`/`await` makes it ideal for handling I/O-bound operations, such as database queries and external API calls.

5. **Community and Ecosystem**:
   - FastAPI has a growing community and excellent documentation, making it easy to find resources and support.

### Negative Consequences

1. **Learning Curve**:

   - Developers unfamiliar with asynchronous programming in Python may face a learning curve.
   - FastAPI's reliance on type hints may require additional effort for developers not accustomed to using them.

2. **Ecosystem Maturity**:
   - While FastAPI is growing rapidly, it is relatively new compared to Django REST Framework and Flask, which have larger ecosystems and more third-party integrations.

---

### Alternatives Considered

#### **Django REST Framework (DRF)**

- **Pros**:
  - Mature and feature-rich.
  - Built-in support for authentication, permissions, and serialization.
- **Cons**:
  - Heavier and slower compared to FastAPI.
  - Not designed for asynchronous programming.

#### **Flask**

- **Pros**:
  - Lightweight and flexible.
  - Large ecosystem of extensions.
- **Cons**:
  - Requires additional libraries for features like data validation and OpenAPI documentation.
  - No native support for asynchronous programming.

---

## Final Decision

FastAPI was chosen because it provides the right balance of performance, simplicity, and modern features. Its asynchronous capabilities and built-in support for data validation make it an excellent choice for building a scalable and maintainable API.

---
