---
layout: default
title: kmeans
parent: Commands
grand_parent: PPL
great_grand_parent: SQL and PPL
nav_order: 19
has_children: false
---

## kmeans

The kmeans command applies the ML Commons plugin's kmeans algorithm to the provided PPL command's search results.

### Syntax

```sql
kmeans <cluster-number>
```

For `cluster-number`, enter the number of clusters you want to group your data points into.

**Example: Group Iris data**

The example shows how to classify three Iris species (Iris setosa, Iris virginica and Iris versicolor) based on the combination of four features measured from each sample: the length and the width of the sepals and petals.

PPL query:

```sql
os> source=iris_data | fields sepal_length_in_cm, sepal_width_in_cm, petal_length_in_cm, petal_width_in_cm | kmeans centroids=3
```

sepal_length_in_cm | sepal_width_in_cm | petal_length_in_cm | petal_width_in_cm | ClusterID
:--- | :--- |:--- | :--- | :--- 
| 5.1 | 3.5 | 1.4 | 0.2 | 1   
| 5.6 | 3.0 | 4.1 | 1.3 | 0
| 6.7 | 2.5 | 5.8 | 1.8 | 2
