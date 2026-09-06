# BasicApp2

BasicApp2 is a Docker Compose-based application consisting of a frontend, backend, and MySQL database. The frontend and backend are maintained as Git submodules.

## Prerequisites

Before getting started, make sure you have:

-   [Git](https://git-scm.com/) installed
-   [Docker](https://docs.docker.com/get-docker/) installed
-   [Visual Studio Code](https://code.visualstudio.com/) with the **Dev Containers** extension (optional, for development)

## Getting Started

### 1\. Clone the submodules


After you cloned the repository, initialize the submodules, by running:

```bash
git submodule update --init
```

### 2\. Configure database secrets

The application requires two database passwords to be provided as Docker secrets.

Create a `secrets` directory in the project root:

```bash
mkdir -p secrets
```

The directory must contain:

```text
secrets/
├── db_root_password.txt
└── db_user_password.txt
```

Add the appropriate passwords to each file:

```bash
echo "your-root-password" > secrets/db_root_password.txt
echo "your-user-password" > secrets/db_user_password.txt
```

## Running the Application

The project provides **two Docker Compose configurations**, depending on how you want to run the application.

### Development

Development uses `compose.yaml` and is intended for local development.

The frontend and backend source directories are mounted into their respective containers, allowing changes to the source code to be picked up without rebuilding the images. This configuration is therefore suitable for **hot reload and active development**.

Start the development environment with:

```bash
docker compose up
```

Or run it in the background:

```bash
docker compose up -d
```

The development frontend is available on port `5173`.

### Production

Production uses `compose.prod.yaml` and is intended for running a deployed version of the application.

Unlike the development configuration, the production setup uses **pre-built Docker images** for the frontend and backend. This avoids building the application locally and requires less disk space.

Start the production environment with:

```bash
docker compose -f compose.prod.yaml up
```

The production frontend is available on port `8080`.

The production images are selected using the `APP_VERSION` environment variable:

```text
bernobin/basicapp2-frontend:${APP_VERSION}
bernobin/basicapp2-backend:${APP_VERSION}
```

Make sure `APP_VERSION` is set to the desired application version before starting the production environment.

### Production Deployment Requirements

The `db/init.sql` file must be present in the production deployment environment, relative to `compose.prod.yaml`.

Required file:

db/init.sql

The production Compose configuration mounts this file into the MySQL container for database initialization.

Note that MySQL only runs the initialization script when the database is initialized for the first time. Changes to `db/init.sql` will not be applied automatically if an existing database volume is reused.

## Development with Dev Containers

The individual components of the project are maintained as Git submodules and can be opened separately for development.

If you are using **Visual Studio Code** with the **Dev Containers** extension installed:

1.  Open the desired submodule in VS Code.
2.  VS Code will detect the development container configuration.
3.  You will be prompted to **Reopen in Container**.
4.  Accept the prompt to start the development environment inside the container.

This provides a consistent development environment with the dependencies and tools required by the individual component.

## Stopping the Application

To stop the development environment:

```bash
docker compose down
```

To stop the production environment:

```bash
docker compose -f compose.prod.yaml down
```

## Troubleshooting

<!-- Add troubleshooting information here. -->
