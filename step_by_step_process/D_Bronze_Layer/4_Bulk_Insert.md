Transfer a large amount of data from files to a DB

![[img_database_insert_vs_bulk.png]]

```
BULK INSERT bronze.crm_cust_info
FROM 'H:\TRAINING\DATA_ANALYST\SQL_DATA_WAREHOUSE_FROM_SCRATCH_WITH_BARAA\sql_data_warehouse_project_with_baraa\datasets\source_crm\cust_info.csv'
WITH (
	FIRSTROW = 2,
	FIELDTERMINATOR = ',',
	TABLOCK
	);
```
	
FIRSTROW = 2 : skip the header row, useless since specified in the table creation
FIELDTERMINATOR : csv file with a comma separator
TABLOCK : Improve performances. SQL Server applies a shared lock to the entire table. The whole table is locked. Other users cannot update data because they will be blocked.

## Basic checks
### Separators
Must do a check in case there is a problem with separators, a common problem with csv files
```
SELECT * 
FROM bronze.crm_cust_info
```

### Count
Check the number of rows imported and the ones in the file. Remember to count -1 because of the skipped header row. 
```
SELECT COUNT(*)
FROM bronze.crm_cust_info
```


## Truncate
Quickly delete all rows from a table, resetting it to an empty state. 
Each time we run the query, it refresh the source file in the bronze layer table.
Any change in the source file will be loaded in the table.

```
TRUNCATE TABLE bronze.crm_cust_info
BULK INSERT bronze.crm_cust_info
FROM 'H:\TRAINING\DATA_ANALYST\SQL_DATA_WAREHOUSE_FROM_SCRATCH_WITH_BARAA\sql_data_warehouse_project_with_baraa\datasets\source_crm\cust_info.csv'
WITH (
	FIRSTROW = 2,
	FIELDTERMINATOR = ',',
	TABLOCK
	);
```


## Repeat for all files
