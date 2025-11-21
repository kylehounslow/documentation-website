---
layout: default
title: where
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 14
has_children: false
---

## where

Use the `where` command with a bool expression to filter the search result. The `where` command only returns the result when the bool expression evaluates to true.

### Syntax

```sql
where <boolean-expression>
```

Field | Description | Required
:--- | :--- |:---
`bool-expression` | An expression that evaluates to a boolean value. | No

**Example: Filter result set with a condition**

To get all documents from the `accounts` index where `account_number` is 1 or gender is `F`:

```sql
search source=accounts | where account_number=1 or gender=\"F\" | fields account_number, gender;
```

| account_number | gender
:--- | :--- |
| 1  | M
| 13 | F
