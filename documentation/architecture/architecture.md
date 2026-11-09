# System Architecture

*WealthCast: A three-tier application running on Docker Compose: React, Spring Boot and PostgreSQL*

> **Architecture at a glance.** The application follows a three-tier architecture: a React frontend, a Spring Boot REST API, and a PostgreSQL database. The backend is built as a single monolithic application. Docker Compose runs these services as separate containers and connects them through a shared network.

## 1. Overview

Users access the application through a web browser. The browser loads the React application, which communicates with the Spring Boot backend over HTTP. The backend contains the application's business logic and is responsible for reading and writing data in PostgreSQL.

## 2. Architectural Style: Monolithic Backend

WealthCast uses a monolithic architecture for its backend. All business logic lives in a single Spring Boot application that is built, deployed and scaled as one unit, rather than being split into independent microservices.

### 2.1 Why it fits WealthCast

- **Simpler development and testing:** One project to run, debug and test end to end.
- **Straightforward deployment:** One backend image to build and release, with no service discovery or inter-service messaging to manage.
- **Strong data consistency:** Business operations that touch several tables can use a single database transaction.
- **Low operational overhead:** The whole system is three containers on one Compose network.

### 2.2 Trade-offs

- **Whole-application scaling:** The backend scales as a single unit, not per feature.
- **Growing codebase:** The code can become harder to maintain over time, so a clear layered or modular package structure matters.
- **Coupled releases:** Any backend change requires redeploying the entire backend.
- **Uniform technology:** All backend modules share the same language and framework.

## 3. Architecture Components

### 3.1 Web Browser

The browser is the user's entry point to the application. It loads the frontend and displays the interface through which users interact with the system.

In the architecture diagram, the browser is outside the Docker Compose network. It accesses the React development server through a port published by Docker.

### 3.2 React Frontend: Vite

The frontend is built with React and served by Vite on port **5173**.

**Main responsibilities**

- Render the application's user interface.
- Handle user interactions and client-side state.
- Send HTTP requests to the backend REST API.
- Display data and responses returned by the backend.

The frontend communicates with Spring Boot through HTTP requests to the REST API. Request and response data are exchanged in JSON format.

### 3.3 Spring Boot Backend: REST API

The backend is built with Spring Boot and exposes the application's REST API on port **8080**. It is the monolithic core of the system, as described in section 2.

**Main responsibilities**

- Receive and validate requests from the frontend.
- Execute the application's business logic.
- Coordinate access to persistent data.
- Return responses to the frontend

When database access is required, the backend connects to PostgreSQL using JDBC. This connection takes place over the Docker Compose network.

### 3.4 PostgreSQL Database

PostgreSQL is the relational database used to store the application's persistent data. It listens on port **5432** and makes stored data available to the backend.

The Spring Boot backend communicates directly with the database. The React frontend does not connect to PostgreSQL.

### 3.5 Docker Compose Network

Docker Compose runs the React, Spring Boot, and PostgreSQL services in separate containers and connects them through a shared network.

The containers can communicate using their Compose service names and container ports. For example, the backend can reach PostgreSQL.

## 4. Request and Data Flow

The typical request lifecycle follows these steps:

1. **User access:** A user opens the application in a web browser.
2. **Frontend delivery:** The browser loads the React application from the Vite server on port 5173.
3. **API request:** React sends an HTTP request to the Spring Boot REST API on port 8080.
4. **Backend processing:** Spring Boot receives the request and executes the required business logic.
5. **Database access:** If data is needed, Spring Boot communicates with PostgreSQL through JDBC on port 5432.
6. **Database response:** PostgreSQL executes the operation and returns the result to the backend.
7. **API response:** Spring Boot sends an HTTP response to the frontend.
8. **UI update:** React processes the response and updates the interface.

## 5. Technology Summary

| Technology | Responsibility | Port / Interface |
|---|---|---|
| React + Vite | Frontend application | 5173 |
| Spring Boot (monolith) | Backend REST API and business logic | 8080 |
| PostgreSQL | Relational data storage | 5432 |
| Docker Compose | Container orchestration and networking | Internal Compose network |
| External APIs | To be decided... | - |

## 6. Key Architectural Considerations

- **Separation of concerns:** The frontend, backend, and database each have a distinct responsibility.
- **Monolithic backend:** All business logic is deployed as a single Spring Boot application, which keeps development and operations simple at the cost of per-feature scaling.
- **Controlled data access:** The frontend communicates with the backend through the REST API and never accesses the database directly.
- **Centralized business logic:** The Spring Boot backend handles application logic and database operations.
- **Containerized environment:** No need for contributors to have Node, PostgreSQL, JDK configured on their own computers. Only Docker and Docker Compose are required.
