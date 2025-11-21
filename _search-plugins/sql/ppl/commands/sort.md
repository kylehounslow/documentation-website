---
layout: default
title: sort
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 12
has_children: false
---

## sort

Use the `sort` command to sort search results by a specified field.

### Syntax

```sql
sort [count] <[+|-] sort-field>...
```

Field | Description | Required | Default
:--- | :--- |:---
`count` | The maximum number results to return from the sorted result. If count=0, all results are returned. | No | 1000
`[+|-]` | Use plus [+] to sort by ascending order and minus [-] to sort by descending order. | No | Ascending order
`sort-field` | Specify the field that you want to sort by. | Yes | -

**Example 1: Sort by one field**

To sort all documents by the `age` field in ascending order:

```sql
search source=accounts | sort age | fields account_number, age;
```

| account_number | age |
:--- | :--- |
| 13 | 28
| 1  | 32
| 18 | 33
| 6  | 36

**Example 2: Sort by one field and return all results**

To sort all documents by the `age` field in ascending order and specify count as 0 to get back all results:

```sql
search source=accounts | sort 0 age | fields account_number, age;
```

| account_number | age |
:--- | :--- |
| 13 | 28
| 1  | 32
| 18 | 33
| 6  | 36

**Example 3: Sort by one field in descending order**

To sort all documents by the `age` field in descending order:

```sql
search source=accounts | sort - age | fields account_number, age;
```

| account_number | age |
:--- | :--- |
| 6 | 36
| 18  | 33
| 1 | 32
| 13  | 28

**Example 4: Specify the number of sorted documents to return**

To sort all documents by the `age` field in ascending order and specify count as 2 to get back two results:

```sql
search source=accounts | sort 2 age | fields account_number, age;
```

| account_number | age |
:--- | :--- |
| 13 | 28
| 1  | 32

**Example 5: Sort by multiple fields**

To sort all documents by the `gender` field in ascending order and `age` field in descending order:

```sql
search source=accounts | sort + gender, - age | fields account_number, gender, age;
```

| account_number | gender | age |
:--- | :--- | :--- |
| 13 | F | 28
| 6  | M | 36
| 18 | M | 33
| 1  | M | 32
