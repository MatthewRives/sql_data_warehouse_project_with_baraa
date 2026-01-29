# Building views planning

3 tables to create:

- Customer
- Product
- Sales

## Joining strategy

Avoid the inner join because the other source might not have all the customers/product.

![img_gold_join_strategy.png](../../../_img/img_gold_join_strategy.png)

## Primary key & surrogate keys

A Dim table must have a primary key.
In this case, it's customer_id.
If it doesn't exists yet, we must generate a new primary key in the datawarehouse : surrogate keys

Surrogate Key : system-generated unique identifier assigned to each record in a table.
Only used to connect our data model. No use in the business.
Several way to do it :

- DDL-based generation
- Query-based using window function (row number)

## Building fact table

We use the dimension's surrogate keys instead of IDs to easily connect facts with dimensions.

We do it with Data lookup : join the dimensions in order to get the surrogate keys.

## View

As a reminder of the Data Architecture, the gold data are created with SQL views from silver tables.
