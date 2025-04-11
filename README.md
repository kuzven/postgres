# PostgreSQL Docker Setup

This project provides a pre-configured PostgreSQL instance using Docker. The setup is designed for use in development environments, supporting multiple projects and ensuring isolated databases and users.

## Features
- Uses the official PostgreSQL 14 image.
- Centralized database service to support multiple projects.
- Easy environment configuration using `.env`.

## Requirements
- [Docker](https://docs.docker.com/get-docker/) (latest version)
- [Docker Compose](https://docs.docker.com/compose/install/) (latest version)

## Setup Instructions

### 1. Clone this repository

```bash
git clone https://github.com/yourusername/postgres.git
cd postgres
```

### 2. Create a .env file
Copy the example .env.example to .env:

```bash
cp .env.example .env
```

Then update the values in .env to suit your environment, for example:

```
POSTGRES_USER=admin # PostgreSQL administrator username
POSTGRES_PASSWORD=password # PostgreSQL administrator password
```

### 3. Start the PostgreSQL container
Run the following command to start the PostgreSQL container:

```bash
docker-compose -f docker-compose-postgres.yml up -d
```

### 4. Connect to PostgreSQL
You can connect to the running PostgreSQL instance using:

```bash
docker exec -it postgres psql -U <POSTGRES_USER>
```

Replace <POSTGRES_USER> with the admin username defined in your .env.

### 5. Create a new database and user for a project
To create a new database and user for a project:
Connect to PostgreSQL:

```bash
docker exec -it postgres psql -U <POSTGRES_USER>
```

Run the following SQL commands:

```bash
CREATE DATABASE project_db;
CREATE USER project_user WITH PASSWORD 'project_pass';
GRANT ALL PRIVILEGES ON DATABASE project_db TO project_user;
```

### 6. Configure projects to use this PostgreSQL instance
In the .env file of each project, use the following database connection details:

```
DB_HOST=postgres
DB_PORT=5432
DB_USER=project_user
DB_PASSWORD=project_pass
DB_NAME=project_db
```

Ensure that the project's services are connected to the same Docker network projects-network.

### 7. Managing PostgreSQL

#### Stop the container
To stop the PostgreSQL container:

```bash
docker-compose -f docker-compose-postgres.yml down
```

#### Backup and restore
Backup  (No need to stop the container):

```bash
docker exec postgres pg_dumpall -U <POSTGRES_USER> > backup.sql
```

Restore (Optional: stop the container to avoid conflicts):

```bash
cat backup.sql | docker exec -i postgres psql -U <POSTGRES_USER>
```

⚠️ It is recommended that you stop the container so that no new entries occur during recovery.

Additional Notes
Ensure that all connected projects are on the projects-network.