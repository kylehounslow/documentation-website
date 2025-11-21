---
layout: default
title: top
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 17
has_children: false
---

## top {#top-command}

Use the `top` command to find the most common values of all fields in the field list.

### Syntax

```sql
top [N] <field-list> [by-clause]
```

Field | Description | Default
:--- | :--- |:---
`N` | Specify the number of results to return. | 10
`field-list` | Specify a comma-delimited list of field names. | -
`by-clause` | Specify one or more fields to group the results by. | -

**Example 1: Find the most common values in a field**

To find the most common genders:

```sql
search source=accounts | top gender;
```

| gender
:--- |
| M
| F

**Example 2: Find the most common value in a field**

To find the most common gender:

```sql
search source=accounts | top 1 gender;
```

| gender
:--- |
| M

**Example 3: Find the most common values grouped by gender**

To find the most common age grouped by gender:

```sql
search source=accounts | top 1 age by gender;
```

| gender | age
:--- | :--- |
| F  | 28
| M  | 32

### Limitations

The `top` command is not rewritten to OpenSearch DSL, it is only executed on the coordination node.
