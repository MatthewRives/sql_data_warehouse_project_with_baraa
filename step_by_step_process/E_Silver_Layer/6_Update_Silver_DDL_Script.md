# Silver DDL Script change
Apply modifications to the Silver DDL script according to the cleaning done at the previous step. 
Derived columns : new columns based on calculations or transformations of existing ones. 
Data Enrichment : add new, relevant data to enhance the dataset for analysis
## Summary of Derived columns and Data Enrichment

| Table                | crm_cust_info                                                                                                               | crm_prd_info                                                                                                                               | crm_sales_details                                                                                                                                                     |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| New cols             | -                                                                                                                           | cat_id          NVARCHAR(50)<br>prd_key       NVARCHAR(50)                                                                                 | -                                                                                                                                                                     |
| Updated cols         | cst_id keep unique ID, remove duplicates<br>lastname TRIM<br>firstname TRIM<br>gdr mapping values<br>martial mapping values | prd_cost handling missing value<br>prd_start_dt CAST DATE<br>prd_end_dt  CAST DATE + end date calculation<br>prd_line TRIM + mapping value | sls_order_dt CAST DATE and null values<br>sls_ship_dt CAST DATE and null values<br>sls_due_dt CAST DATE and null values<br>sls_sales add rules<br>sls_price add rules |
| Removed cols         | -                                                                                                                           | -                                                                                                                                          | -                                                                                                                                                                     |
| Impact on DDL script | No                                                                                                                          | Yes                                                                                                                                        | Yes                                                                                                                                                                   |



| Table                | erp_loc_a101                        | erp_cust_az12                                                            | erp_px_cat_g1v2 |
| -------------------- | ----------------------------------- | ------------------------------------------------------------------------ | --------------- |
| New cols             | -                                   | -                                                                        | -               |
| Updated cols         | cid replace<br>cntry mapping values | cid keep substring<br>bdate removed extreme values<br>gen mapping values | -               |
| Removed cols         | -                                   | -                                                                        | -               |
| Impact on DDL script | No                                  | No                                                                       | No              |

## Script change
If new columns are added or if datatype is changed, the DDL scirpt must be modified


## Full code 

```
/*
===============================================================================
DDL Script: Create Silver Tables
===============================================================================
Script Purpose:
    This script creates tables in the 'silver' schema, dropping existing tables if they already exist.
    Run this script to re-define the DDL structure of 'bronze' Tables
===============================================================================
*/

IF OBJECT_ID('silver.crm_cust_info', 'U') IS NOT NULL
    DROP TABLE silver.crm_cust_info;
GO

CREATE TABLE silver.crm_cust_info (
    cst_id             INT,
    cst_key            NVARCHAR(50),
    cst_firstname      NVARCHAR(50),
    cst_lastname       NVARCHAR(50),
    cst_marital_status NVARCHAR(50),
    cst_gndr           NVARCHAR(50),
    cst_create_date    DATE,
    dwh_create_date    DATETIME2 DEFAULT GETDATE()
);
GO

IF OBJECT_ID('silver.crm_prd_info', 'U') IS NOT NULL
    DROP TABLE silver.crm_prd_info;
GO

CREATE TABLE silver.crm_prd_info (
    prd_id          INT,
    cat_id          NVARCHAR(50),
    prd_key         NVARCHAR(50),
    prd_nm          NVARCHAR(50),
    prd_cost        INT,
    prd_line        NVARCHAR(50),
    prd_start_dt    DATE,
    prd_end_dt      DATE,
    dwh_create_date DATETIME2 DEFAULT GETDATE()
);
GO

IF OBJECT_ID('silver.crm_sales_details', 'U') IS NOT NULL
    DROP TABLE silver.crm_sales_details;
GO

CREATE TABLE silver.crm_sales_details (
    sls_ord_num     NVARCHAR(50),
    sls_prd_key     NVARCHAR(50),
    sls_cust_id     INT,
    sls_order_dt    DATE,
    sls_ship_dt     DATE,
    sls_due_dt      DATE,
    sls_sales       INT,
    sls_quantity    INT,
    sls_price       INT,
    dwh_create_date DATETIME2 DEFAULT GETDATE()
);
GO

IF OBJECT_ID('silver.erp_loc_a101', 'U') IS NOT NULL
    DROP TABLE silver.erp_loc_a101;
GO

CREATE TABLE silver.erp_loc_a101 (
    cid             NVARCHAR(50),
    cntry           NVARCHAR(50),
    dwh_create_date DATETIME2 DEFAULT GETDATE()
);
GO

IF OBJECT_ID('silver.erp_cust_az12', 'U') IS NOT NULL
    DROP TABLE silver.erp_cust_az12;
GO

CREATE TABLE silver.erp_cust_az12 (
    cid             NVARCHAR(50),
    bdate           DATE,
    gen             NVARCHAR(50),
    dwh_create_date DATETIME2 DEFAULT GETDATE()
);
GO

IF OBJECT_ID('silver.erp_px_cat_g1v2', 'U') IS NOT NULL
    DROP TABLE silver.erp_px_cat_g1v2;
GO

CREATE TABLE silver.erp_px_cat_g1v2 (
    id              NVARCHAR(50),
    cat             NVARCHAR(50),
    subcat          NVARCHAR(50),
    maintenance     NVARCHAR(50),
    dwh_create_date DATETIME2 DEFAULT GETDATE()
);
GO
```

