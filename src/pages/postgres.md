## Spin up a Postgres database

```bash
docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```
## Create a states table

```sql
CREATE TABLE States (
    id SERIAL,
    key varchar(255) PRIMARY KEY,
    scope varchar(255),
    value json
);
```

Add a unique constraing

```
ALTER TABLE states ADD CONSTRAINT states_scope_key_unique UNIQUE (scope, key);
``` 

## Connect to the database

```
postgres://postgres:mysecretpassword@localhost:5433/postgres
```