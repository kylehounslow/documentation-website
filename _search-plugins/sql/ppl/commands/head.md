---
layout: default
title: head
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 15
has_children: false
---

## head

Use the `head` command to return the first N number of results in a specified search order.

### Syntax

```sql
head [N]
```

Field | Description | Required | Default
:--- | :--- |:---
`N` | Specify the number of results to return. | No | 10

**Example 1: Get the first 10 results**

To get the first 10 results:

```sql
search source=accounts | fields firstname, age | head;
```

| firstname | age
:--- | :--- |
| Amber  | 32
| Hattie | 36
| Nanette | 28

**Example 2: Get the first N results**

To get the first two results:

```sql
search source=accounts | fields firstname, age | head 2;
```

| firstname | age
:--- | :--- |
| Amber  | 32
| Hattie | 36

### Limitations

The `head` command is not rewritten to OpenSearch DSL, it is only executed on the coordination node.
