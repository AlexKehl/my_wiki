# Postgres

conf location: `/etc/postgresql/<version>/main/postgresql.conf`

## Make dump

Optionally you can install `postgresql-client` to use `pg_dump` command.
Only required if postgres runs in docker container and not on the machine itself.

```bash
sudo apt install postgresql-client
```

```bash
pg_dump -h host -p port -U username -d database_name -f output_file.sql
```

OR if with the connection string

```bash
pg_dump --dbname="postgres://..." -f output_file.sql
```

## Restore dump

```bash
docker exec -i <container_id> psql -U <username> -d <database_name> < dump_file.sql
```

## Query stats

```sql
SELECT
  *
FROM
  pg_stat_activity
```
