- Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) if you haven't done that yet.

Pull the postgres image and run it

```bash
docker pull postgres
docker volume create react-server
docker run --name react-server -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_USER=postgres -p 5433:5432 -v react-server:/var/lib/postgresql/data -d postgres
```

That's it, your database should be running and is ready for use.

## Init Table

In order for react-server to connect to your database, you need a special table.

You need to connect to the postgres instance using a docker terminal.

```bash
docker exec -it container_id /bin/bash
```

This gives you a terminal on the container which you can use to connect to postgres:

```bash
psql -U postgres
```

Copy and paste the following SQL code into the terminal and hit enter.

```sql
CREATE TABLE states
(
    id SERIAL,
    key character varying(255) NOT NULL,
    scope character varying(255),
    value json,
    CONSTRAINT states_scope_key_unique UNIQUE (scope, key)
);
```

Once you created the table, your server should be able to connect to the database.
