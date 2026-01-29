# Exploring and understanding data
Explore data manually to understand what means each columns and the potential relationship between each.
in sql server management, DB > Tables > right click on the desired table > select top 1000 rows

## crm cust info

![img_data_explo_bronze_crm_cust_info.png](../../../_img/img_data_explo_bronze_crm_cust_info.png)



## crm prd info

![img_data_explo_bronze_crm_prd_info.png](../../../_img/img_data_explo_bronze_crm_prd_info.png)
## crm sales details

![img_data_explo_bronze_crm_sales_details.png](../../../_img/img_data_explo_bronze_crm_sales_details.png)

sls_cust_id : FK crm cust info
sls_prd_key : FK crm prd info
quantity and price : event table -> transactional

## erp cust az12
![img_data_explo_bronze_erp_cust_az12.png](../../../_img/img_data_explo_bronze_erp_cust_az12.png)
cid seems to be connected to the cst_key from crm cust info. 

## erp loc a101

![img_data_explo_bronze_erp_loc_a101.png](../../../_img/img_data_explo_bronze_erp_loc_a101.png)
## erp px cat g1v2 
![img_data_explo_bronze_erp_px_cat_g1v2.png](../../../_img/img_data_explo_bronze_erp_px_cat_g1v2.png)

partial match between id from erp px cat g1v2 and prd_key in crm prd info 

![img_data_explo_bronze_erp_px_cat_g1v2_with_crm_prd_info.png](../../../_img/img_data_explo_bronze_erp_px_cat_g1v2_with_crm_prd_info.png)
