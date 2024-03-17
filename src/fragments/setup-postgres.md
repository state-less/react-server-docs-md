- Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) if you haven't done that yet.

Pull the postgres image and run it

```bash
docker pull postgres
docker volume create react-server
docker run --name react-server -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_USER=postgres -p 5433:5432 -v react-server:/var/lib/postgresql/data -d postgres
```

That's it, your database should be running and is ready for use.

![Docker screenshot](https://raw.githubusercontent.com/state-less/react-server-docs-md/master/images/docker.png)

## Init Table

In order for react-server to connect to your database, you need a states table.
Connect to the postgres instance from a docker terminal.

```bash
docker exec -it container_id /bin/bash
```

This gives you a terminal on the container which you can use to connect to postgres:

```bash
psql -U postgres
```

Copy and paste the following SQL code into the postgres shell and hit enter.

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

That's all you need to do to prepare the database. Your React Server instance should now connect to the database.
