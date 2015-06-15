# mysql notes

## Create a database

```sql
CREATE DATABASE dbname
GRANT ALL PRIVILEGES ON dbname.* TO 'root'@'localhost' IDENTIFIED BY '';
```

## Export to csv

```bash
mysql -uexampleuser -pletmein exampledb -B -e "select * from \`person\`;" | sed 's/\t/","/g;s/^/"/;s/$/"/;s/\n//g' > filename.csv
```

## To drop all the tables in a database

```bash
#from http://knaddison.com/technology/mysql-drop-all-tables-database-using-single-command-line-command I changed gawk to awk and now it appears to crash the terminal, though it still works :)
db=dbname
mysql -u root -p $db -e "show tables" | grep -v Tables_in | grep -v "+" | awk '{print "drop table " $1 ";"}' | mysql -u root -p $db
```

## Dump database

```bash
mysqldump -u${DB_USERNAME} -h${DB_HOST} -p${DB_PASSWORD} ${DB_NAME} --single-transaction --quick | gzip > "$HOME/db.sql.gz"
```

## Load database

```bash
gunzip -f db.sql.gz
mysql -uroot $db < db.sql
```

## Load data infile

```sql
LOAD DATA LOCAL INFILE 'infile'
INTO TABLE test_table
(field1, filed2, field3);
#FIELDS TERMINATED BY '\t' ENCLOSED BY '' ESCAPED BY '\\'
#LINES TERMINATED BY '\n' STARTING BY ''
```

## View console results in less

```bash
# http://stackoverflow.com/questions/924729/mysql-select-many-fields-how-best-to-display-in-terminal#6422698
pager less -SFX
```

## Show results vertically

Use `\G` instead of `;`:

```sql
SELECT * FROM tables\G
```

## Show count of rows for each table

http://stackoverflow.com/questions/286039

```sql
SELECT table_name, TABLE_ROWS FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'database_name';
```

## List all users

http://stackoverflow.com/questions/1135245

```sql
select User,Host from mysql.user;
```
