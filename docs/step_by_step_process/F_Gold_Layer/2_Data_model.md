# Data model

![img_data_model_schema_choice.png](../../../_img/img_data_model_schema_choice.png)

## 3 stages for data models

![img_data_model_schema_choice.png](../../../_img/img_data_model_schema_choice.png)

## Schema

![img_data_model_star_vs_snowflake_schema.png](../../../_img/img_data_model_star_vs_snowflake_schema.png)
### Star Schema
A fact table surrounded by DIM tables
Simple & Easy
Big dimensions, slow query
### Snowflake schema
A star schema where DIM tables have DIM tables
Break redundancies
Hard to setup
Optimized for data analysis

For this project, Star Schema

## Dimension vs Fact
Dimension : 
- descriptive information that give context to the data
- Who
- What 
- Where

Example : 
- product details (name, size, color...)

Fact : 
- quantitative information that represents events 
- How much
- How many

Example : 
- sales quantity
- purchase date  
- prices