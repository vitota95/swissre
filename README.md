# Vulnerability Management System

## Overview

This project is a **Vulnerability Management System** designed to manage and analyze vulnerabilities. It consists of two main components:

1. **Vulnerability API**: A Python-based FastAPI application that provides RESTful endpoints for managing vulnerabilities.
2. **Vulnerability CLI**: A .NET CLI application for ingesting vulnerability data from JSON files and posting it to the API.

---

## Getting Started

### Prerequisites

- Install **Docker** and **Docker Compose** on your machine.
- Ensure you have Python 3.13 and .NET 9.0 SDK installed if you want to run the components locally.

---

### Running the Application with Docker Compose

1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/vulnerability-management.git
   cd vulnerability-management
   ```
2. Start all services using Docker Compose:

   ```bash
   docker-compose up --build
   ```

3. Access the services:
   - **Vulnerability API**: `http://localhost:8000/docs` (Swagger UI for API documentation).
   - **pgAdmin**: `http://localhost:5050` (Login with `admin@admin.com` and password `admin`).
4. Run the CLI:
   It runs once when initiating the compose file. It uses the files in [testData](VulnerabilityCli\src\VulnerabilityCli\test-data). Use the following commands for executing it standalone.

   ```bash
   cd VulnerabilityCli/src
   docker build -t vulnerabilitycli .
   docker run --rm --network swissre_default -v YOUR_ABSOLUTE_PATH/VulnerabilityCli/src/VulnerabilityCli/test-data:/app/data vulnerabilitycli --file /app/data/ten_elements.json --url http://vulnerabilityapi:8000
   ```

   Or to execute directly in the compose file:

   ```bash
   docker compose run vulnerabilitycli
   ```

---

### Running the Python Tests

1. Navigate to the `VulnerabilityApi` directory:

   ```bash
   cd VulnerabilityApi
   ```

2. Install dependencies in a virtual environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. Run the tests using `pytest`:
   ```bash
   pytest
   ```

---

### Running the .NET Tests

1. Navigate to the `VulnerabilityCli/tests` directory:

   ```bash
   cd VulnerabilityCli/tests
   ```

2. Run the tests using the .NET CLI:
   ```bash
   dotnet test
   ```

## API Endpoints

The **Vulnerability API** provides the following endpoints:

1. **POST /vulnerability**:
   - Create a new vulnerability.
2. **GET /vulnerability/{cve}**:
   - Retrieve a vulnerability by its CVE.
3. **GET /vulnerability**:
   - Retrieve vulnerabilities with optional filters (e.g., title, criticality range).
4. **DELETE /vulnerability/{cve}**:
   - Delete a vulnerability by its CVE.

---

## Sample CLI Usage

To run the CLI and post data to the API:

```bash
docker-compose run vulnerabilitycli --file /app/data/ten_elements.json --url http://vulnerabilityapi:8000
```

---

## Architecture

The system follows a **microservices architecture** with the following components:

- **Vulnerability API**:
  - Built with **FastAPI**.
  - Uses **PostgreSQL** as the primary database.
  - Uses **Redis** for caching and search capabilities.
- **Vulnerability CLI**:
  - Built with **.NET 9.0**.
  - Reads JSON files, validates the data, and posts it to the API.
- **pgAdmin**:
  - A web-based interface for managing the PostgreSQL database.

The services communicate within a **Docker Compose network**, allowing seamless integration between the API, CLI, PostgreSQL, and Redis.

---

## Redis Index

- **Indexing**: I used Redis full text functionality to create an index on fields like `title` and `criticality`. This allows for fast, full-text search and numeric range queries.
- **Search Queries**:
  - **Full-Text Search**: Users can search vulnerabilities by title using partial matches (e.g., searching for "buffer" will return vulnerabilities with titles containing "buffer overflow").
  - **Numeric Filters**: Queries can filter vulnerabilities by criticality using numeric ranges (e.g., `criticality:[2 5]`).
  - **Combined Queries**: Users can combine full-text search and numeric filters to perform complex queries (e.g., search for vulnerabilities with "overflow" in the title and criticality between 3 and 7).
- **Performance**: The redis full text module is optimized for high performance, making it suitable for real-time search operations.

---

---

## Architecture Decisions

This project uses **Architecture Decision Records (ADRs)** to document key architectural decisions. Below are the ADRs for this project:

1. [ADR-01: Use FastAPI for the Vulnerability API](adr/01_fastapi_for_vulnerabilities_api.md)
2. [ADR-02: Use Uvicorn as the ASGI Server](adr/02_uvicorn_as_asgi_server.md)
3. [ADR-03: Use Redis for Caching](adr/03_redis_for_cache.md)
4. [ADR-04: Use PostgreSQL as database](adr/04_postgresql_as_database.md)

These ADRs provide context, decisions, and consequences for the architectural choices made in this project.

---

## Future Work

### Expiring Cache in Redis

- Add expiration to cached entries in Redis to ensure stale data is automatically removed and memory usage is optimized. Or otherwise implement a background job to periodically synchronize Redis with the latest data from the database.

### Steps to Get Production Ready

- Introduce secrets management to securely handle sensitive information like database credentials and API keys.
- Set up logging and monitoring to track application performance and detect issues in real-time.
- Configure load balancing and scaling to handle increased traffic and ensure high availability.
- Use container orchestration tools to manage and deploy containers efficiently in production environments.

### Choosing a Cloud Provider or VPS

- Decide on a cloud provider or VPS for hosting the application, considering factors like cost, scalability, and ease of use. Decide on a container orchetrastor if relevant.

---

## Possible Improvements to consider

### Add an Endpoint for Creating Multiple Vulnerabilities at Once

- **Why**: Currently, vulnerabilities are created one at a time, which can be inefficient when processing large datasets. Adding an endpoint to create multiple vulnerabilities in a single request would reduce the number of API calls, improve performance, and simplify the client-side logic.
- **How**: Implement a `POST /vulnerabilities` endpoint that accepts a list of vulnerabilities in the request body. The server would validate and insert all vulnerabilities in a single database transaction to ensure consistency.
- **Benefits**:
  - Reduces the overhead of multiple HTTP requests.
  - Improves performance by batching database inserts.
  - Simplifies the client-side implementation for bulk uploads.

---
