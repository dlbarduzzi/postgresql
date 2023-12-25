# postgresql

This repository contains information about how to run a [PostgreSQL database](https://www.postgresql.org/) in your local machine with [docker-compose](https://docs.docker.com/compose/).

## Running docker-compose

First, export the required environment variables. You can generate a random password with `openssl rand -base64 32`.

```sh
export POSTGRES_DB=
export POSTGRES_USER=
export POSTGRES_PASSWORD=
```

Then, start the container with docker-compose.

```sh
# Add `-d` to the end of the command to run the container in the background.
docker-compose up
```

## Connect to the running postgres database

```sh
docker exec -it postgres psql --host=localhost --dbname=${POSTGRES_DB} --username=${POSTGRES_USER}
```

## PostgresSQL commands

```sql
-- check current user
SELECT current_user;

-- create a new role/user
CREATE ROLE user WITH LOGIN PASSWORD 'password';

-- list databases
\l

-- connect to a given database
\c my_db

-- list database tables
\dt

-- view database table description
\d my_table

-- list database users
\du

-- print full list of available commands 
\?
```

## Other

### Running postgres with docker command

```sh
docker run -d \
    --name postgres \
    --restart always \
    --volume postgres_data:/var/lib/postgresql/data \
    -p 5432:5432 \
    -e POSTGRES_DB=${POSTGRES_DB} \
    -e POSTGRES_USER=${POSTGRES_USER} \
    -e POSTGRES_PASSWORD=${POSTGRES_PASSWORD} \
    postgres:15.5
```

### Setup a local connection string to the database

```sh
vim $HOME/.profile

# if SSL is enabled, remove `?sslmode=disable`
export POSTGRES_CONN='postgres://user:pass@localhost:5432/db?sslmode=disable'

source $HOME/.profile
```

### Connect to the running postgres database with the env variable connection string

```sh
docker exec -it postgres psql $POSTGRES_CONN
```

## License

MIT License
