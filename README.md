# WealthCast

## Getting Started

### Prerequisites

- [Git](https://git-scm.com/downloads)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) or Docker Engine with Docker Compose

You do not need to install Java, Maven, Node.js, or PostgreSQL locally.

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/Lipiu/WealthCast.git
   cd WealthCast/backend
   ```

2. Create your local environment file:

   ```bash
   cp .env.example .env
   ```

   The defaults work out of the box. If you change a credential, keep the
   `POSTGRES_*` and `DATABASE_*` values in sync.

3. Build and start everything (database, backend, frontend):

   ```bash
   docker compose up --build
   ```
4. To stop the project from running you can use `CTRL + C`
5. To start it back up you can use `docker-compose up`

### Troubleshooting

- **`password authentication failed`:** the database volume was created with
  different credentials. Reset it with `docker compose down -v`, then start again.