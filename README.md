# SQL Conventions

(translation in progress)

* Use PostgreSQL (fallback on SQLite)
* pas d'abbréviations des mots sauf pour des expressions bien connues et longue (e.g. "i18n")
* No reserved keywords (for example, `user` in PGSQL)
* Table and view names should be singular, e.g. `team` not `teams` ([why](https://launchbylunch.com/posts/2014/Feb/16/sql-naming-conventions/#singular-relations))
* Fields, views and tables should be in **camelCase**, e.g. `createdAt`
* Use snake case for foreign keys, e.g. `team_id`
* Use UUID for primary and foreign keys
* Each table should have a `createdAt` and a  [`deletedAt`](http://stackoverflow.com/questions/8289100/create-unique-constraint-with-null-columns/8289253#8289253) field, a `updatedAt` should be added too if the table fields are mutable.
* utiliser une lib de data-mapping (anorm/slick) mais pas d'ORM
* utiliser BNCF (au dessus de la 3NF) (cf normal form)
* always set column to NOT NULL by default, use NULL only when necessary
* never use ON DELETE CASCADE, set `deletedAt` to `NOW()`
* leverage using, so instead of:

```
select <fields> from
  table_1
  inner join table_2
    using (table_1_id)
```

use:

```
select <fields> from
  table_1
  inner join table_2
    on table_1.table_1_id =
       table_2.table_1_id
```

* utiliser les enum PG qui sont des types
* use the right PostgreSQL types:

```
inet (IP address)
timestamp with time zone
point (2D point)
tstzrange (time range)
interval (duration)
```

* utiliser les tableaux si besoin (permet de gérer la notion "d'ordre" facilement)
* constraint should be inside your database as much as possible:

```sql
create table reservation(
    reservation_id uuid primary key,
    dates tstzrange not null,
    exclude using gist (dates with &&)
);
```

* standard names for indexes in PostgreSQL are: `{tablename}_{columnname(s)}_{suffix}` (e.g. `item_a_b_pkey`) where the suffix is one of the following:
  * Primary Key constraint: `pkey`
  * Foreign key: `fkey`
  * Unique constraint: `key`
  * Check constraint: `check`
  * Exclusion constraint: `excl`
  * Any other kind of index: `idx`

([source](http://stackoverflow.com/questions/4107915/postgresql-default-constraint-names/4108266#4108266))
