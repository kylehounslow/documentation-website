---
layout: default
title: join
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 20
has_children: false
---

## join

This is an experimental feature and is not recommended for use in a production environment. For updates on the progress of the feature or if you want to leave feedback, join the discussion on the [OpenSearch forum](https://forum.opensearch.org/).    
{: .warning}

You can combine two datasets using the `join` command. The left side can be an index or results from piped commands, while the right side can be either an index or a subquery.

### Syntax

```sql
[join-type] join [left-alias] [right-alias] on <join-criteria> <right-dataset>
* joinCriteria: mandatory. It could be any comparison expression.
* right-dataset: mandatory. Right dataset could be either an index or a subquery with/without alias.
```

Field | Description | Type | Required | Default
:--- |:--- | :--- | :--- | :---
`join-type` | The type of join to perform. Valid values are `inner`, `left`, `right`, `full`, `cross`, `semi`, and `anti`.  | `String` | No   | `inner`
`left-alias` | The subquery alias to use with the left join side in order to avoid ambiguous naming. Fixed pattern: `left = <left-alias>`    | `String` | No   | N/A
`right-alias` | The subquery alias to use with the right join side in order to avoid ambiguous naming. Fixed pattern: `right = <right-alias>` | `String` | No   | N/A
`join-criteria` | Any comparison expression.  | `String` | Yes  | N/A
`right-dataset` | Either an index or a subquery with/without an alias.  | `String` | Yes  | N/A

The following examples use the `state_country` and `occupation` indexes.

`state_country`:

| Name  | Age | State   | Country
:--- | :--- | :--- | :---
| Jake  | 70  | California | USA
| Hello | 30  | New York   | USA
| John  | 25  | Ontario    | Canada
| Jane  | 20  | Quebec     | Canada
| Jim   | 27  | B.C.        | Canada
| Peter | 57  | B.C.       | Canada
| Rick  | 70  | B.C.        | Canada
| David | 40  | Washington | USA

`occupation`:

| Name  | Occupation  | Country | Salary
:--- | :--- | :--- | :---
| Jake  | Engineer    | England | 100000
| Hello | Artist      | USA     | 70000
| John  | Doctor      | Canada  | 120000
| David | Doctor      | USA     | 120000
| David | Unemployed  | Canada  | 0
| Jane  | Scientist   | Canada  | 90000

**Example 1: Join two indexes**

The following example performs an inner join between two indexes:

```sql
search source = state_country
| inner join left=a right=b ON a.name = b.name occupation
| stats avg(salary) by span(age, 10) as age_span, b.country
```

avg(salary) | age_span | b.country
:--- | :--- | :---
120000.0 | 40 | USA
105000.0 | 20 | Canada
0.0 | 40 | Canada
70000.0 | 30 | USA
100000.0 | 70 | England

**Example 2: Join with a subsearch**

The following example performs a left join with a subsearch:

```sql
search source = state_country as a
| where country = 'USA' OR country = 'England'
| left join on a.name = b.name [
    source = occupation
    | where salary > 0
    | fields name, country, salary
    | sort salary
    | head 3
  ] as b
| stats avg(salary) by span(age, 10) as age_span, b.country
```

avg(salary) | age_span | b.country
:--- | :--- | :---
null | 40 | null
70000.0 | 30 | USA
100000.0 | 70 | England

### Limitations

The `join` command works only when `plugins.calcite.enabled` is set to `true`.
