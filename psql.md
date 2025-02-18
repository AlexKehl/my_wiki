# Psql 

A command line interface for PostgreSQL.

## Drop a database

```bash
\c postgres
```

\c is needed because you can't drop a database that you are currently connected to.
It changes the connection to the postgres database.

After that you can drop the database.

```bash
DROP DATABASE mydatabase;
```

If run in docker then one can use the follwing:
```bash
docker exec -i <containerid> dropdb -U <user> <dbname>
```

And then create a new database:
```bash
docker exec -i <containerid> createdb -U <user> <dbname>
```
