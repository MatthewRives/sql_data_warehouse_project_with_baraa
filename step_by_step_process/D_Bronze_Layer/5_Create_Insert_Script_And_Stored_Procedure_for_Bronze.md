# Create Insert Script And Stored Procedure

## Principles
We have a script to load all the data from source to bronze layer. 
Everyday, we will use this script to refresh data, in case new data are added.
Save frequently used SQL code in stored procedures in database.
Respect naming convention of procedures. 

## How it works
Procedures are a mix of sub-query and python functions. 
```
CREATE OR ALTER PROCEDURE + name_of_the_procedure AS
BEGIN
	(initial query loading the data)
END
```

## Code example

```
CREATE OR ALTER PROCEDURE bronze.load_bronze AS
BEGIN
	TRUNCATE TABLE bronze.crm_cust_info
	BULK INSERT bronze.crm_cust_info
	FROM 'H:\TRAINING\DATA_ANALYST\SQL_DATA_WAREHOUSE_FROM_SCRATCH_WITH_BARAA\sql_data_warehouse_project_with_baraa\datasets\source_crm\cust_info.csv'
	WITH (
		FIRSTROW = 2,
		FIELDTERMINATOR = ',',
		TABLOCK
		);
		
	TRUNCATE TABLE bronze.crm_prd_info
	BULK INSERT bronze.crm_prd_info
	FROM (etc. etc. etc.)
	
END
```


## Programmability
After executing the query, refresh.
In programmability > stored procedures
Create a new query and execute the procedure with the following code.

```
EXEC bronze.load_bronze
```

It will do all the procedure and load the data as defined. 


## Prints 

Add prints to improve the readability of what is happening in the query.
Add separators to distinguish which source files we are in


## Error handling

### Try & Catch
SQL runs the TRY block, and if it fails, it runs the CATCH block to handle the error.
And we put it inside the procedure. 

```
CREATE OR ALTER PROCEDURE bronze.load_bronze AS
BEGIN
	BEGIN TRY
		(initial query loading the data)
	END TRY
	BEGIN CATCH
		(error message)
	END CATCH
END
```


### ETL duration
Tracking ETL Duration helps to identify bottlenecks, optimize performance, monitor trends, detect issues. 
Must setup a starting and endtime between a code block. Then we calculate and print the difference between the two with DATEDIFF. 

```
SET @start_time = GETDATE();
(a step)
SET @end_time = GETDATE();
PRINT '>> Load Duration: ' + CAST(DATEDIFF(second, @start_time, @end_time) AS NVARCHAR) + ' seconds';
```

### Full insert duration
Do something similar with the full block to know how long it takes to load all the files data in the tables.

```
CREATE OR ALTER PROCEDURE bronze.load_bronze AS
BEGIN
	DECLARE @start_time DATETIME, @end_time DATETIME, @batch_start_time DATETIME, @batch_end_time DATETIME; 
	
	BEGIN TRY
	
		(initial query loading the data)
		
		SET @batch_end_time = GETDATE();
		PRINT '=========================================='
		PRINT 'Loading Bronze Layer is Completed';
        PRINT '   - Total Load Duration: ' + CAST(DATEDIFF(SECOND, @batch_start_time, @batch_end_time) AS NVARCHAR) + ' seconds';
		PRINT '=========================================='
	
	END TRY
	BEGIN CATCH
		(error message)
	END CATCH
END
```

