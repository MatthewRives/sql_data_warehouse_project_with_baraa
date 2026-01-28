# Create Database and Schemas

## Create and start SQL server
Install Microsoft SQL Server Express
Start SQL Server Express
Create SQLExpress server

If needed, in the Windows search bar, search "services" > right click > run as administrator. 
Find the SQLExpress > right click > start
![[img_sqlexpress_server_start.png]]

## Connect to SQL server
Install Microsoft SQL Server Management Studio 22 (SSMS)
Start SSMS and connect to the local SQL Server Express (SQLExpress)
![[img_start_server_with_ssms.png]]


## Create Database

Copy and execute the following code
Respect naming convention

```
/*
=============================================================
Create Database and Schemas
=============================================================
Script Purpose:
    This script creates a new database named 'Baraa_DataWarehouse' after checking if it already exists. 
    If the database exists, it is dropped and recreated. 
    Additionally, the script sets up three schemas within the database: 'bronze', 'silver', and 'gold'.
	
WARNING:
    Running this script will drop the entire 'Baraa_DataWarehouse' database if it exists. 
    All data in the database will be permanently deleted. 
    Proceed with caution and ensure you have proper backups before running this script.
*/

USE master;
GO

-- Drop and recreate the 'Baraa_DataWarehouse' database
IF EXISTS (SELECT 1 FROM sys.databases WHERE name = 'Baraa_DataWarehouse')
BEGIN
    ALTER DATABASE Baraa_DataWarehouse SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
    DROP DATABASE Baraa_DataWarehouse;
END;
GO

-- Create the 'Baraa_DataWarehouse' database
CREATE DATABASE Baraa_DataWarehouse;
GO

USE Baraa_DataWarehouse;
GO

-- Create Schemas
CREATE SCHEMA bronze;
GO

CREATE SCHEMA silver;
GO

CREATE SCHEMA gold;
GO
```

## Push to git
Update git with it in the script folder 