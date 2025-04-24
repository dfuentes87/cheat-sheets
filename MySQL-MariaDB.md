# Useful MySQL/MariaDB queries

## CLI

- Log in as `user` on another `host`, and use database `mydb` (password is prompted):

    ```bash
    mysql -h host -u user -p mydb
    ```

- Secure MySQL/MariaDB after a clean install:

    ```bash
    mysql_secure_installation
    ```

## Queries

In the overview below, CAPITALIZED words are part of the SQL syntax, lowercase words are names of tables, columns, etc.

| Task                            | Query                                        |
| :---                            | :---                                         |
| Show table properties           | `DESCRIBE tablename;`                        |
| List users                      | `SELECT user,host FROM mysql.user;`          |
| Show installed components       | `SELECT * FROM mysql.component;`             |

### Resetting the root password

1. Stop the mysql/mariadb service
2. Create a file owned by the `mysql` user with the query to change the password. The file should also be somewhere accesible by the mysql user.

    ```bash
        ALTER USER 'root'@'localhost' IDENTIFIED BY 'NEW_PASSWORD';
    ```

3. Sudo as the mysql user and manually start the mysql service

    ```bash
        mysqld --init-file=/path/to/file 
    ```

4. Let the service fully start then kill it: `pkill mysql`
5. Start the mysql/mariadb service as normal

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

- First, ensure that the `drupal` database exists
- `mysql -u root -p drupal < drupal_backup.sql`
