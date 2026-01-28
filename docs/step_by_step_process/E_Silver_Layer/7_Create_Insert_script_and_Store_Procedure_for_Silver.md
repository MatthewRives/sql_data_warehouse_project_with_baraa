# Create Insert Script And Stored Procedure
## Aggregate queries for script
Concatenate the different cleaning operations and put it inside an insert function. It will take the data from from bronze.crm_cust_info, transform and populate the silver.crm_cust_info table. 

``` 
INSERT INTO silver.[table] (
			(list of column names)
		)
		SELECT
			queries_for_each_column
```

Example: 

``` 
INSERT INTO silver.crm_cust_info (
			cst_id, 
			cst_key, 
			cst_firstname, 
			cst_lastname, 
			cst_marital_status, 
			cst_gndr,
			cst_create_date
		)
		SELECT
			cst_id,
			cst_key,
			TRIM(cst_firstname) AS cst_firstname,
			TRIM(cst_lastname) AS cst_lastname,
			CASE 
				WHEN UPPER(TRIM(cst_marital_status)) = 'S' THEN 'Single'
				WHEN UPPER(TRIM(cst_marital_status)) = 'M' THEN 'Married'
				ELSE 'n/a'
			END AS cst_marital_status,
			CASE 
				WHEN UPPER(TRIM(cst_gndr)) = 'F' THEN 'Female'
				WHEN UPPER(TRIM(cst_gndr)) = 'M' THEN 'Male'
				ELSE 'n/a'
			END AS cst_gndr,
			cst_create_date
		FROM (
			SELECT
				*,
				ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last
			FROM bronze.crm_cust_info
			WHERE cst_id IS NOT NULL
		) AS t
		WHERE flag_last = 1
```


## Check quality of silver

Re-run the quality check queries from the bronze layer to verify the quality of data in the silver layer


## Repeat for every table


## Add steps duration calculation


```
-- Loading silver.[table]
        SET @start_time = GETDATE();
		PRINT '>> Truncating Table: silver.[table]';
		TRUNCATE TABLE silver.[table];
		PRINT '>> Inserting Data Into: silver.[table]';
		INSERT INTO 
		
			(insert script for [table])
			
		SET @end_time = GETDATE();
        PRINT '>> Load Duration: ' + CAST(DATEDIFF(SECOND, @start_time, @end_time) AS NVARCHAR) + ' seconds';
        PRINT '>> -------------';
```


### Add try

```
    BEGIN TRY
    
		(insert script for all table)
		
	END TRY
	BEGIN CATCH
		PRINT '=========================================='
		PRINT 'ERROR OCCURED DURING LOADING BRONZE LAYER'
		PRINT 'Error Message' + ERROR_MESSAGE();
		PRINT 'Error Message' + CAST (ERROR_NUMBER() AS NVARCHAR);
		PRINT 'Error Message' + CAST (ERROR_STATE() AS NVARCHAR);
		PRINT '=========================================='
	END CATCH
```


## Add script duration calculation

```
    DECLARE @start_time DATETIME, @end_time DATETIME, @batch_start_time DATETIME, @batch_end_time DATETIME; 

	(begin try, 
		insert script for all table)
		
		SET @batch_end_time = GETDATE();
		PRINT '=========================================='
		PRINT 'Loading Silver Layer is Completed';
        PRINT '   - Total Load Duration: ' + CAST(DATEDIFF(SECOND, @batch_start_time, @batch_end_time) AS NVARCHAR) + ' seconds';
		PRINT '=========================================='
	
	(end try
	begin catch
		catch
	end catch)
```

## Add procedures


```
CREATE OR ALTER PROCEDURE silver.load_silver AS
BEGIN
	(begin insert duration, 
		begin try, 
			insert script for all table
		end insert duration calculation
		end try,
		begin catch
			catch block
		end catch)
END
```