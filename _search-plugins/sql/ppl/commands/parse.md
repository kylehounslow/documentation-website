---
layout: default
title: parse
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 10
has_children: false
---

## parse

Use the `parse` command to parse a text field using regular expression and append the result to the search result. 

### Syntax

```sql
parse <field> <regular-expression>
```

Field | Description | Required
:--- | :--- |:---
field | A text field. | Yes
regular-expression | The regular expression used to extract new fields from the given test field. If a new field name exists, it will replace the original field. | Yes

The regular expression is used to match the whole text field of each document with Java regex engine. Each named capture group in the expression will become a new ``STRING`` field.

**Example 1: Create new field**

The example shows how to create new field `host` for each document. `host` will be the hostname after `@` in `email` field. Parsing a null field will return an empty string.

```sql
os> source=accounts | parse email '.+@(?<host>.+)' | fields email, host ;
fetched rows / total rows = 4/4
```

| email | host  
:--- | :--- |
| amberduke@pyrami.com  | pyrami.com 
| hattiebond@netagy.com | netagy.com 
| null                  | null          
| daleadams@boink.com   | boink.com  

*Example 2*: Override the existing field

The example shows how to override the existing address field with street number removed.

```sql
os> source=accounts | parse address '\d+ (?<address>.+)' | fields address ;
fetched rows / total rows = 4/4
```

| address
:--- |
| Holmes Lane      
| Bristol Street   
| Madison Street   
| Hutchinson Court

**Example 3: Filter and sort be casted parsed field**

The example shows how to sort street numbers that are higher than 500 in address field.

```sql
os> source=accounts | parse address '(?<streetNumber>\d+) (?<street>.+)' | where cast(streetNumber as int) > 500 | sort num(streetNumber) | fields streetNumber, street ;
fetched rows / total rows = 3/3
```

| streetNumber | street  
:--- | :--- |
| 671 | Bristol Street 
| 789 | Madison Street 
| 880 | Holmes Lane  

### Limitations

A few limitations exist when using the parse command:

- Fields defined by parse cannot be parsed again. For example, `source=accounts | parse address '\d+ (?<street>.+)' | parse street '\w+ (?<road>\w+)' ;` will fail to return any expressions.
- Fields defined by parse cannot be overridden with other commands. For example, when entering `source=accounts | parse address '\d+ (?<street>.+)' | eval street='1' | where street='1' ;` `where` will not match any documents since `street` cannot be overridden.
- The text field used by parse cannot be overridden. For example, when entering `source=accounts | parse address '\d+ (?<street>.+)' | eval address='1' ;` `street` will not be parse since address is overridden. 
- Fields defined by parse cannot be filtered/sorted after using them in the `stats` command. For example, `source=accounts | parse email '.+@(?<host>.+)' | stats avg(age) by host | where host=pyrami.com ;` `where` will not parse the domain listed.
