#MySQL 
 
## Databases export

```sql
mysqldump -u root -p --all-databases > alldb.sql
```
Look up the documentation for [mysqldump](http://dev.mysql.com/doc/refman/5.5/en/mysqldump.html). You may want to use some of the options mentioned in comments:

```sql
mysqldump -u root -p --opt --all-databases > alldb.sql
mysqldump -u root -p --all-databases --skip-lock-tables > alldb.sql
```

## Databases Import:

```sql
mysql -u root -p < alldb.sql
```
