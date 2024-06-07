# Useful MySQL/MariaDB queries

## CLI


- Log in as `user` on another `host`, and use database `mydb` (password is prompted):

    ```
    mysql -h host -u user -p mydb
    ```

- Setting the root password (after clean install):

    ```
    mysqladmin password "my new password"
    ```

## Queries

First log in as root and use the `mysql` database: `mysql -uroot -p mysql` (password is prompted). Don't forget that every query must be terminated with `;`

In the overview below, CAPITALIZED words are part of the SQL syntax, lowercase words are names of tables, columns, etc.

| Task                            | Query                                        |
| :---                            | :---                                         |
| List databases                  | `SHOW DATABASES;`                            |
| Change active database          | `USE dbname;`                                |
| Change to the "system" database | `USE mysql;`                                 |
| Show tables in active database  | `SHOW TABLES;`                               |
| Show table properties           | `DESCRIBE tablename;`                        |
| List all users                  | `SELECT user,host,password FROM mysql.user;` |
| List databases                  | `SELECT host,db,user FROM mysql.db;`         |
| Quit                            | `exit` or `Ctrl-D`                           |


### Resetting the root password

TODO

## Create a new database and user

Create a database and a user with all privileges for that database (warning: user/db are first removed if they exist):

```bash
CREATE DATABASE ${db_name};
GRANT ALL ON ${db_name}.* TO '${db_user}'@'%' IDENTIFIED BY PASSWORD('${db_password}');
FLUSH PRIVILEGES;
```

## Backup/Restore

In this example, the database is `drupal`.

Backup: `mysqldump -u root -p drupal > drupal_backup.sql`


Restore:

  - First, ensure that the `drupal` database exists (see above)
  - `mysql -u root -p drupal < drupal_backup.sql`
