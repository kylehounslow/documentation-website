---
layout: default
title: eval
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 8
has_children: false
---

## eval

The `eval` command evaluates an expression and appends its result to the search result.

### Syntax

```sql
eval <field>=<expression> ["," <field>=<expression> ]...
```

Field | Description | Required
:--- | :--- |:---
`field` | If a field name does not exist, a new field is added. If the field name already exists, it's overwritten. | Yes
`expression` | Specify any supported expression. | Yes

**Example 1: Create a new field**

To create a new `doubleAge` field for each document. `doubleAge` is the result of `age` multiplied by 2:

```sql
search source=accounts | eval doubleAge = age * 2 | fields age, doubleAge;
```

| age | doubleAge
:--- | :--- |
32    | 64
36    | 72
28    | 56
33    | 66

*Example 2*: Overwrite the existing field

To overwrite the `age` field with `age` plus 1:

```sql
search source=accounts | eval age = age + 1 | fields age;
```

| age
:--- |
| 33
| 37
| 29
| 34

**Example 3: Create a new field with a field defined with the `eval` command**

To create a new field `ddAge`. `ddAge` is the result of `doubleAge` multiplied by 2, where `doubleAge` is defined in the `eval` command:

```sql
search source=accounts | eval doubleAge = age * 2, ddAge = doubleAge * 2 | fields age, doubleAge, ddAge;
```

| age | doubleAge | ddAge
:--- | :--- |
| 32    | 64   | 128
| 36    | 72   | 144
| 28    | 56   | 112
| 33    | 66   | 132


### Limitation

The ``eval`` command is not rewritten to OpenSearch DSL, it is only executed on the coordination node.
