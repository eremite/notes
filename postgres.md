# postgres notes

## Dump

```bash
pg_dump --clean --no-owner --username=db_user db_name | gzip -c > ~/db.sql.gz
```

## Kill all connections

To fix "PGError: ERROR:  database "name_of_database" is being accessed by other users"

https://gist.github.com/mattscilipoti/4455341/#comment-878446

```bash
psql -c "SELECT pid, pg_terminate_backend(pid) as terminated FROM pg_stat_activity WHERE pid <> pg_backend_pid();" -d 'name_of_database'
```

## Console Commands

* `\l` List databases
* `\c` Connect to a database
* `\dt` List tables
* `\q` Quit
