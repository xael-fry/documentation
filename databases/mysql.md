# MySQL 
 
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

## FAQ

###  How to fix Access denied for user 'root'@'localhost'
Follow the steps below.

Start the MySQL server instance or daemon with the --skip-grant-tables option (security setting).
```sql
$ mysqld --skip-grant-tables
```
Execute these statements.

```sql
mysql -u root mysql
mysql> UPDATE user SET Password=PASSWORD('my_password') where USER='root';
mysql> FLUSH PRIVILEGES;
```

Finally, restart the instance/daemon without the --skip-grant-tables option.



