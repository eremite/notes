# postgres notes

## Dump

```bash
pg_dump --clean --no-owner --username=db_user db_name | gzip -c > ~/db.sql.gz
```

## Kill all connections

To fix "PGError: ERROR:  database is being accessed by other users"

http://stackoverflow.com/a/12924735/167369

```bash
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE pid <> pg_backend_pid() AND datname='name_of_database';
```

## Console Commands

* `\l` List databases
* `\c` Connect to a database
* `\dt` List tables
* `\q` Quit
