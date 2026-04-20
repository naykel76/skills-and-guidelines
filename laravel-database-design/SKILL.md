---
name: laravel-database-design
description: >-
  Use this skill whenever creating or updating database schemas, tables,
  migrations, models, or factories. Do not wait for an explicit request — if
  any of these are being created or modified, this skill applies.
---

## Standard column reference

Reference of standard column names, types, and naming conventions used across
the project. Use these patterns when creating new tables to maintain
consistency.

Keep logical order when creating new tables.

| Column Name  | Type            | Nullable | Default | Purpose                       |
| ------------ | --------------- | -------- | ------- | ----------------------------- |
| id           | bigint unsigned | No       | AUTO    | Primary key                   |
| code         | varchar(255)    | No       | UNIQUE  | Human-readable identifier     |
| name         | varchar(255)    | No       | -       | Simple label/display name     |
| title        | varchar(255)    | No       | -       | Formal display name           |
| slug         | varchar(255)    | No       | -       | URL-safe identifier           |
| headline     | varchar(255)    | Yes      | -       | Featured headline/tagline     |
| intro        | mediumText      | Yes      | -       | Short introductory text       |
| content      | longText        | Yes      | -       | Main body content             |
| image_name   | varchar(255)    | Yes      | -       | Stored image filename         |
| file_name    | varchar(255)    | Yes      | -       | Stored file name              |
| price        | bigint unsigned | Yes      | -       | Product price in cents        |
| stock        | bigint unsigned | No       | 0       | Quantity available            |
| quantity     | bigint unsigned | No       | 1       | Quantity for order details    |
| unit_price   | bigint          | Yes      | -       | Order item price (signed)     |
| amount       | bigint unsigned | Yes      | -       | Nullable amount               |
| extra_data   | json            | Yes      | -       | Flexible additional data      |
| published_at | timestamp       | Yes      | -       | When content went live        |
| released_at  | timestamp       | Yes      | -       | When content became available |

## Schema notes

- `created_at` and `updated_at` are standard columns and assumed on all tables
  unless stated.
- `deleted_at` is optional and used only where soft deletes are required.
- Use the Purpose column in the Reference Table only for internal context; for
  the Schema Document, the Notes column should only contain technical
  constraints (PK, FK, UNIQUE, etc.).

## Design document template

Use this template for all new table definitions.

```markdown +code
## Table: [TableName]

### Purpose

A short description of what this table stores or represents.

### Columns

| Column   | Type            | Nullable | Default   | Notes               |
| -------- | --------------- | -------- | --------- | ------------------- |
| id       | bigint unsigned | No       | AUTO      | PK                  |
| [column] | [type]          | [Yes/No] | [default] | [PK/FK/UNIQUE only] |
| ...      | ...             | ...      | ...       | ...                 |

### Constraints

List anything not obvious from the column definitions, like:

* Foreign keys (`column` references `other_table.id`)
* Soft deletes
* Composite keys or checks

### Relationships

Human-readable description of relationships with other tables:

* [Table] has many [Table]
* [Table] can belong to [Table]
* ...
```
