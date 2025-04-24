# PostgreSQL

## Terms and Concepts

| Concept           | PostgreSQL                                            | MySQL                                                     |
| :---              | :---                                                  | :---                                                      |
| Database          | Top-level container; can't cross-query across DBs     | What you `USE dbname;` - a namespace                      |
| Schema            | Namespace within a single DB (tables, types, etc.)    | Not separately managed                                    |
| Cross-DB          | Need FDW or dblink to query another PG DB             | Can `SELECT ... FROM otherdb.table;`                      |
| Default Schema    | public                                                | No schema concept; objects reside in the database itself  |

## Queries

In the overview below, CAPITALIZED words are part of the SQL syntax, lowercase words are names of tables, columns, etc.

| Task                                  | Query                                             |
| :---                                  | :---                                              |
| List databases                        | `\l`                                              |
| Use a database                        | `\c myappdb`                                      |
| List tables in current database       | `\dt`                                             |
| Describe a table, view, or index      | `\d wordpress`                                    |
| Describe but with additional metadata | `\d+ wordpress`                                   |
| List all schemas                      | `\dn`                                             |
| List all roles                        | `\du`                                             |
| Create user                           | `CREATE USER username WITH PASSWORD 'password';`  |
| Grant the role/user access            | `GRANT pg_read_all_data TO my_database;`          |

## Full Access

| Name                                      | Query                        |
| :---                                      | :---                         |
| INSERT, UPDATE, DELETE on all Databases   | `pg_write_all_data`          |
| Read Only on All Databases                | `pg_read_all_data`           |

## Read-Only User Steps

1. Create the role

    `CREATE ROLE readonly_user LOGIN PASSWORD 'strongpassword';`

2. Allow connecting to the target database

    `GRANT CONNECT ON DATABASE mydb TO readonly_user;`

3. Allow usage of the public schema (or your schema name)

    `GRANT USAGE ON SCHEMA public TO readonly_user;`

4. Grant SELECT on all existing tables in public

    `GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly_user;`

5. Future tables in public inherit SELECT

    `ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO readonly_user;`

## Config Files

Database configuration = /var/lib/pgsql/data/postgresql.conf

Host Based Access configuration = /var/lib/pgsql/data/pg_hba.conf
