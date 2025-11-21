---
layout: default
title: ad
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 18
has_children: false
---

## ad

The `ad` command applies the Random Cut Forest (RCF) algorithm in the [ML Commons plugin]({{site.url}}{{site.baseurl}}/ml-commons-plugin/index/) on the search result returned by a PPL command. Based on the input, the plugin uses two types of RCF algorithms: fixed in time RCF for processing time-series data and batch RCF for processing non-time-series data.

### Syntax: Fixed In Time RCF For Time-series Data Command

```sql
ad <shingle_size> <time_decay> <time_field>
```

Field | Description | Required
:--- | :--- |:---
`shingle_size` | A consecutive sequence of the most recent records. The default value is 8. | No
`time_decay` | Specifies how much of the recent past to consider when computing an anomaly score. The default value is 0.001. | No
`time_field` | Specifies the time filed for RCF to use as time-series data. Must be either a long value, such as the timestamp in miliseconds, or a string value in "yyyy-MM-dd HH:mm:ss".| Yes

### Syntax: Batch RCF for Non-time-series Data Command

```sql
ad <shingle_size> <time_decay>
```

Field | Description | Required
:--- | :--- |:---
`shingle_size` | A consecutive sequence of the most recent records. The default value is 8. | No
`time_decay` | Specifies how much of the recent past to consider when computing an anomaly score. The default value is 0.001. | No

**Example 1: Detecting events in New York City from taxi ridership data with time-series data**

The example trains a RCF model and use the model to detect anomalies in the time-series ridership data.

PPL query:

```sql
os> source=nyc_taxi | fields value, timestamp | AD time_field='timestamp' | where value=10844.0
```

value | timestamp | score | anomaly_grade
:--- | :--- |:--- | :---
10844.0 | 1404172800000 | 0.0 | 0.0    

**Example 2: Detecting events in New York City from taxi ridership data with non-time-series data**

PPL query:

```sql
os> source=nyc_taxi | fields value | AD | where value=10844.0
```

value | score | anomalous
:--- | :--- |:--- 
| 10844.0 | 0.0 | false  
