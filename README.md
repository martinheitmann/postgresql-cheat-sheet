# PostgreSQL Development Environment Cheat Sheet

This is a simple cheat sheet containing commonly used commands for setting up a local PostgreSQL server for development.

The following commands should not be considered guidance for setting up a production environment, and should only be referenced for local development in environments without strict security requirements.

## Basic commands

Help: `\h`

List databases: `\l`

Switch databases: `\c <database>`

List tables (requires connection): `\dt`

Quit psql: `\q`

## Create a new Docker container instance of a PostgreSQL server

`docker run --name my-postgres -e POSTGRES_PASSWORD=mypassword -d postgres`

Alternatively, specify a different port to bind to the host:

`docker run -p 7936:5432 --name my-postgres -e POSTGRES_PASSWORD=mypassword -d postgres`

## Sign into PostgreSQL using the command line

When signing in for the first time, use the psql root user and database:

`psql -d postgres -U postgres`

If you're not prompted with a password, append `-W` to the list of arguments:

`psql -d <database> -U <user> -W`

Alternatively specify the host:

`psql -h <host> -d <database> -U <user> -W`

## Create a new database

`CREATE DATABASE mydatabase;`

An owner can be specified when creating a new database:

`CREATE DATABASE mydatabase OWNER myuser;`

Database ownership can also be altered post-creation:

`ALTER DATABASE mydatabase OWNER TO myuser;`

Alternatively specify a tablespace:

`CREATE DATABASE mydatabase OWNER myuser TABLESPACE myuserspace;`

### Creating a new schema within a database

First, select the database with `\c <database>` before creating the schema:

`CREATE SCHEMA myschema;`

## Create a new user with a password

`CREATE USER myuser WITH PASSWORD 'secretpassword';`

A user can also be created with database creation rights:

`CREATE USER myuser WITH PASSWORD 'secretpassword' CREATEDB;`

## Granting and creating roles

Grant a user all rights to a database:

`GRANT ALL PRIVILEGES ON mydatabase TO myuser;`

Grant membership for a specific role to user:

`GRANT admins TO myuser;`

Create a new role with a set of permissions:

`CREATE ROLE myrole WITH CREATEDB CREATEROLE;`

Alter an existing role:

`ALTER ROLE myrole WITH NOLOGIN;`

## Dump and Restore

Create a database dump from a an instance running in a Docker container:

`docker exec -t <your-container-id> pg_dumpall -c -U postgres > your-dump.file.sql`

Example, using the current date as the dump file name:

`` docker exec -t addfa87604bb pg_dumpall -c -U postgres > pg_db_dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql` ``
