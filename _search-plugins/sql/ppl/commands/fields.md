---
layout: default
title: fields
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 9
has_children: false
---

## fields

Use the `fields` command to keep or remove fields from a search result.

### Syntax

```sql
fields [+|-] <field-list>
```

Field | Description | Required | Default
:--- | :--- |:---|:---
`index` | Plus (+) keeps only fields specified in the field list. Minus (-) removes all fields specified in the field list. | No | +
`field list` | Specify a comma-delimited list of fields. | Yes | No default

**Example 1: Select specified fields from result**

To get `account_number`, `firstname`, and `lastname` fields from a search result:

```sql
search source=accounts | fields account_number, firstname, lastname;
```

| account_number | firstname  | lastname
:--- | :--- |
| 1   | Amber       | Duke
| 6   | Hattie      | Bond
| 13  | Nanette     | Bates
| 18  | Dale        | Adams

**Example 2: Remove specified fields from a search result**

To remove the `account_number` field from the search results:

```sql
search source=accounts | fields account_number, firstname, lastname | fields - account_number;
```

| firstname | lastname
:--- | :--- |
| Amber   | Duke
| Hattie  | Bond
| Nanette | Bates
| Dale    | Adams
