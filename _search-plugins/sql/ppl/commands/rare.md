---
layout: default
title: rare
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 16
has_children: false
---

## rare

Use the `rare` command to find the least common values of all fields in a field list.
A maximum of 10 results are returned for each distinct set of values of the group-by fields.

### Syntax

```sql
rare <field-list> [by-clause]
```

Field | Description | Required
:--- | :--- |:---
`field-list` | Specify a comma-delimited list of field names. | No
`by-clause` | Specify one or more fields to group the results by. | No

**Example 1: Find the least common values in a field**

To find the least common values of gender:

```sql
search source=accounts | rare gender;
```

| gender
:--- |
| F
| M

**Example 2: Find the least common values grouped by gender**

To find the least common age grouped by gender:

```sql
search source=accounts | rare age by gender;
```

| gender | age
:--- | :--- |
| F  | 28
| M  | 32
| M  | 33

### Limitations

The `rare` command is not rewritten to OpenSearch DSL, it is only executed on the coordination node.
