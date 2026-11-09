# WealthCast Application Specifications

## 1. Project Overview

WealthCast is a personal financial management backend built with Spring Boot. It aggregates multi-bank data to forecast a user's 30-day to 90-day liquid wealth trajectory by analyzing recurring cash flows.

## 2. Technology Stack

* **Backend Framework:** Spring Boot / React (Spring Web, Spring Security, Spring Data JPA)

* **Database:** PostgreSQL

* **External Integration:** Open Banking API (e.g., Plaid, Tink) for read-only financial data.

* **Infrastructure & Deployment:** **Docker** (containerization of the Spring Boot application and database), **Docker Compose** (local environment orchestration)

## 3. Functional Requirements

* **Account Aggregation:** The system shall authenticate and sync balances and transaction logs from multiple external bank accounts.

* **Transaction Categorization:** The system shall identify and tag recurring inflows (salary, interest) and outflows (subscriptions, loans) from historical data.

* **Predictive Engine:** The backend shall expose an endpoint that calculates a daily projected balance array for the next 30-90 days using categorized recurring transactions and rolling averages for variable spending.

* **Scenario Modeling:** The API shall accept hypothetical transactions (add/remove expense) and instantly return a recalculated future projection.

## 4. Non-Functional Requirements

* **Security:** The API must secure endpoints using OAuth 2.0 / JWT. Financial access tokens must be encrypted at rest using AES-256.

* **Performance:** The predictive calculation endpoint must respond in under 500ms to ensure a seamless graphing experience on the frontend dashboard.

* **Data Integrity:** The system must strictly enforce read-only access to banking APIs; no money movement capabilities will be built.

* **Architecture:** The code must follow a clear layered architecture (Controllers, Services, Repositories) for maintainability and testability.