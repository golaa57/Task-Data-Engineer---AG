# Task Data Engineer - AG

This project simulates a data transformation scenario using Docker containers. It consists of a relational database container and a Python-based analytics container that runs Jupyter Notebook to perform data transformations on the 
MovieLens dataset.

## Docker

Most important thing is downloading a Docker application from https://www.docker.com/.

## Docker Setup and Network Configuration

In this project, a Docker Compose setup is used to orchestrate two containers that communicate with each other through a dedicated Docker network. The Docker Compose configuration defines and connects both the database service and the analytics (Jupyter Notebook) service.

### Services Overview

1. **Database Service (PostgreSQL)**:
   - The first service is a PostgreSQL container, which acts as the relational database. PostgreSQL was chosen over simpler databases like SQLite due to its robustness and ability to handle more complex queries efficiently. PostgreSQL also supports the data transformations and analytics required in this task, making it an ideal choice for working with structured data like the MovieLens dataset.
   - The database is exposed on port `5432`, making it accessible to other services (like the analytics container) and tools like pgAdmin if needed.

2. **Analytics Service (Jupyter Notebook)**:
   - The second service is a Jupyter Notebook environment, which is used to run Python scripts for data transformations. This container is based on a preconfigured image that includes Python and commonly used data science libraries. The `Dockerfile` adds additional dependencies required for this project.
   - This service connects to the PostgreSQL database through the internal Docker network and exposes port `8888` to the host system for easy access to the Jupyter interface from a web browser.

### Network Configuration

Docker Compose automatically creates a network for the services to communicate. Both the PostgreSQL and Jupyter containers are connected to this network, allowing the analytics container to query the database internally without needing to expose the database to the outside world.

### Volumes for Persistent Data

A **Docker volume** is used for the PostgreSQL service to persist data between container runs. This ensures that any data you store in the database is written to the host machine’s file system, so it remains even after the container is stopped or removed. By using a volume, we avoid losing the imported dataset and transformed data each time the container is rebuilt or restarted.

This is particularly important for database containers, as the data remains available and consistent even if the container itself is recreated.

Here is a summary of the key components:
- **Network**: Enables communication between containers.
- **Services**: Two services are set up — a PostgreSQL database and a Jupyter Notebook environment.
- **Volumes**: The PostgreSQL database uses a volume to ensure data persistence on the host machine.

## Launching (how to run the code)

To launch necessary is usage of the following command ```docker compose up -d```.

Open http://localhost:8888 in your browser. If password or token is required use command ```docker exec -it task_de-transformation-1 jupyter server list``` and copy the token.

Open task_DE.ipynb.

## Working with dataset

Datasets were downloaded with help of requests, zipfile, pandas, sqlalchemy libraries. 

For further analysis movies and ratings, datasets necessary for answering questions, were inserted into the PostgreSQL databases. 

## Python - answering questions

Questions were answered with use of pandas, sqlalchemy libraries. Pandas might seem unnecessary here but usage of pandas is helpful in formatting query results and operating on pandas.DataFrame provides a good base for future possible data transformations.

Things worth noticing are usage of double quotes when selecting a movieId/userId column and "text" in question 6 (...text(query)…). The first solution was applied due to the fact that in PostgreSQL, unquoted identifiers are automatically lowercased, so if the column was defined with mixed case, it must be referenced with double quotes. The second solution was applied to avoid compatibility issues.