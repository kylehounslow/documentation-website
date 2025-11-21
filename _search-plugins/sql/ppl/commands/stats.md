---
layout: default
title: stats
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 13
has_children: false
---

## stats

Use the `stats` command to aggregate from search results.

The following table lists the aggregation functions and also indicates how each one handles null or missing values:

Function | NULL | MISSING
:--- | :--- |:---
`COUNT` | Not counted | Not counted
`SUM` | Ignore | Ignore
`AVG` | Ignore | Ignore
`MAX` | Ignore | Ignore
`MIN` | Ignore | Ignore


### Syntax

```
stats <aggregation>... [by-clause]...
```

Field | Description | Required | Default
:--- | :--- |:---
`aggregation` | Specify a statistical aggregation function. The argument of this function must be a field. | Yes | 1000
`by-clause` | Specify one or more fields to group the results by. If not specified, the `stats` command returns only one row, which is the aggregation over the entire result set. | No | -

**Example 1: Calculate the average value of a field**

To calculate the average `age` of all documents:

```sql
search source=accounts | stats avg(age);
```

| avg(age)
:--- |
| 32.25

**Example 2: Calculate the average value of a field by group**

To calculate the average age grouped by gender:

```sql
search source=accounts | stats avg(age) by gender;
```

| gender | avg(age)
:--- | :--- |
| F  | 28.0
| M  | 33.666666666666664

**Example 3: Calculate the average and sum of a field by group**

To calculate the average and sum of age grouped by gender:

```sql
search source=accounts | stats avg(age), sum(age) by gender;
```

| gender | avg(age) | sum(age)
:--- | :--- |
| F  | 28   | 28
| M  | 33.666666666666664 | 101

**Example 4: Calculate the maximum value of a field**

To calculate the maximum age:

```sql
search source=accounts | stats max(age);
```

| max(age)
:--- |
| 36

**Example 5: Calculate the maximum and minimum value of a field by group**

To calculate the maximum and minimum age values grouped by gender:

```sql
search source=accounts | stats max(age), min(age) by gender;
```

| gender | min(age) | max(age)
:--- | :--- | :--- |
| F  | 28 | 28
| M  | 32 | 36
