---
layout: default
title: rename
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 11
has_children: false
---

## rename

Use the `rename` command to rename one or more fields in the search result.

### Syntax

```sql
rename <source-field> AS <target-field>["," <source-field> AS <target-field>]...
```

Field | Description | Required
:--- | :--- |:---
`source-field` | The name of the field that you want to rename. | Yes
`target-field` | The name you want to rename to. | Yes

**Example 1: Rename one field**

Rename the `account_number` field as `an`:

```sql
search source=accounts | rename account_number as an | fields an;
```

| an
:--- |
| 1
| 6
| 13
| 18

**Example 2: Rename multiple fields**

Rename the `account_number` field as `an` and `employer` as `emp`:

```sql
search source=accounts | rename account_number as an, employer as emp | fields an, emp;
```

| an   | emp
:--- | :--- |
| 1    | Pyrami
| 6    | Netagy
| 13   | Quility
| 18   | null

### Limitations

The `rename` command is not rewritten to OpenSearch DSL, it is only executed on the coordination node.
