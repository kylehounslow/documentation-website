---
layout: default
title: lookup
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 21
has_children: false
---

## lookup

This is an experimental feature and is not recommended for use in a production environment. For updates on the progress of the feature or if you want to leave feedback, join the discussion on the [OpenSearch forum](https://forum.opensearch.org/).    
{: .warning}

The `lookup` command enriches your search data by adding or replacing data from a lookup index (dimension table). You can extend index fields with values from a dimension table or append/replace values when a lookup condition is matched. As an alternative to the `join` command, the `lookup` command is more suitable for enriching the source data with a static dataset.

### Syntax

```sql
lookup <lookup-index> (<lookup-mapping-field> [as <source-mapping-field>])... [(replace | append) (<input-field> [AS <output-field>])...]
```

Field | Description | Required | Default
:--- | :--- | :--- | :---
`lookup-index` | The name of lookup index (dimension table). | Yes | N/A
`lookup-mapping-field`| A mapping key in the `lookup-index`, analogous to a `join` key from the right table. You can specify multiple `lookup-mapping-field` values with commas. | Yes | N/A
`source-mapping-field`| A mapping key from the source (left side), analogous to a `join` key from the left side. | No | `lookup-mapping-field`
`replace` \| `append` | The output strategies. When specifying `replace`, matched values in the `lookup-index` field overwrite the values in the results. If you specify `append`, matched values in the `lookup-index` field only append to the missing values in the results. | No | `replace`
`input-field` | A field in `lookup-index` where matched values are applied to the result output. You can specify multiple `input-field` values with commas. If you don't specify any `input-field`, all fields except `lookup-mapping-field` from `lookup-index` are matched values that are applied to the result output. | No | N/A
`output-field` | A field of output. You can specify zero or multiple `output-field` values. If you specify `output-field` with an existing field name in the source query, its values will be replaced or appended by the matched values from `input-field`. If the field specified in `output-field` is a new field, an extended new field will be applied to the results. | No | `input-field`

The following examples use the `workers` and `work_information` indexes.

`workers`:

| ID | Name | Occupation | Country | Salary
:--- | :--- | :--- | :--- | :---
| 1000 | Jake | Engineer | England | 100000
| 1001 | Hello | Artist | USA | 70000
| 1002 | John | Doctor | Canada | 120000
| 1003 | David | Doctor | N/A | 120000
| 1004 | David | N/A | Canada | 0
| 1005 | Jane | Scientist | Canada | 90000

`work_information`:

| UID | Name  | Department | Occupation
:--- | :--- | :--- | :---
| 1000 | Jake  | IT | Engineer |
| 1002 | John  | DATA | Scientist |
| 1003 | David | HR | Doctor |
| 1005 | Jane  | DATA | Engineer |
| 1006 | Tom   | SALES | Artist |

**Example 1: Look up workers and return the corresponding department**

The following example looks up workers and returns the corresponding department:

```sql
source = workers | lookup work_information uid as id append department
```

| id | name | occupation | country | salary | department
:--- | :--- | :--- | :--- | :--- | :---
1000 | Jake | Engineer | England | 100000 | IT
1001 | Hello | Artist | USA | 70000 | Null
1002 | John | Doctor | Canada | 120000 | DATA
1003 | David | Doctor | Null | 120000 | HR
1004 | David | Null | Canada | 0 | Null
1005 | Jane | Scientist | Canada | 90000 | DATA


**Example 2: Look up workers and replace their occupation and department**

The following example looks up workers and replaces their occupation and department using their `work_information`:

```sql
source = workers | lookup work_information uid as id, name
```

id | name | occupation | country | salary | department
:--- | :--- |:-----------| :--- | :--- | :---
1000 | Jake | Engineer   | England | 100000 | IT
1001 | Hello | null       | USA | 70000 | null
1002 | John | Scientist  | Canada | 120000 | DATA
1003 | David | Doctor     | null | 120000 | HR
1004 | David | null       | Canada | 0 | null
1005 | Jane | Engineer  | Canada | 90000 | DATA

**Example 3: Look up workers and create a new occupation field**

The following example looks up workers and appends their occupation from `work_information` as a new field:

```sql
source = workers | lookup work_information name replace occupation as new_occupation
```

id | name | occupation | country | salary | new_occupation
:--- | :--- |:-----------| :--- | :--- | :---
1000 | Jake | Engineer | England | 100000 | Engineer
1001 | Hello | Artist | USA | 70000 | null
1002 | John | Doctor | Canada | 120000 | Scientist
1003 | David | Doctor | null | 120000 | Doctor
1004 | David | null | Canada | 0 | Doctor
1005 | Jane | Scientist | Canada | 90000 | Engineer

### Limitations

The `lookup` command works only when `plugins.calcite.enabled` is set to `true`.
