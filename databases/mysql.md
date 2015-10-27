mysqldump -u root -p --all-databases > alldb.sql

Look up the documentation for mysqldump. You may want to use some of the options mentioned in comments:

mysqldump -u root -p --opt --all-databases > alldb.sql
mysqldump -u root -p --all-databases --skip-lock-tables > alldb.sql

Import:

mysql -u root -p < alldb.sql

