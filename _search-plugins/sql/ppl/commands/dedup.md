---
layout: default
title: dedup
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 7
has_children: false
---

## dedup

The `dedup` (data deduplication) command removes duplicate documents defined by a field from the search result.

### Syntax

```sql
dedup [int] <field-list> [keepempty=<bool>] [consecutive=<bool>]
```

Field | Description | Type | Required | Default
:--- | :--- |:--- |:--- |:---
`int` |  Retain the specified number of duplicate events for each combination. The number must be greater than 0. If you do not specify a number, only the first occurring event is kept and all other duplicates are removed from the results. | `string` | No | 1
`keepempty` | If true, keep the document if any field in the field list has a null value or a field missing. | `nested list of objects` | No | False
`consecutive` | If true, remove only consecutive events with duplicate combinations of values. | `Boolean` | No | False
`field-list` | Specify a comma-delimited field list. At least one field is required. | `String` or comma-separated list of strings | Yes | -

**Example 1: Dedup by one field**

To remove duplicate documents with the same gender:

```sql
search source=accounts | dedup gender | fields account_number, gender;
```

| account_number | gender
:--- | :--- |
1 | M
13 | F


**Example 2: Keep two duplicate documents**

To keep two duplicate documents with the same gender:

```sql
search source=accounts | dedup 2 gender | fields account_number, gender;
```

| account_number | gender
:--- | :--- |
1 | M
6 | M
13 | F

**Example 3: Keep or ignore an empty field by default**

To keep two duplicate documents with a `null` field value:

```sql
search source=accounts | dedup email keepempty=true | fields account_number, email;
```

| account_number | email
:--- | :--- |
1 | amberduke@pyrami.com
6 | hattiebond@netagy.com
13 | null
18 | daleadams@boink.com

To remove duplicate documents with the `null` field value:

```sql
search source=accounts | dedup email | fields account_number, email;
```

| account_number | email
:--- | :--- |
1 | amberduke@pyrami.com
6 | hattiebond@netagy.com
18 | daleadams@boink.com

**Example 4: Dedup of consecutive documents**

To remove duplicates of consecutive documents:

```sql
search source=accounts | dedup gender consecutive=true | fields account_number, gender;
```

| account_number | gender
:--- | :--- |
1 | M
13 | F
18 | M

### Limitations

The `dedup` command is not rewritten to OpenSearch DSL, it is only executed on the coordination node.
