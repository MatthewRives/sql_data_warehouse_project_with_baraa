For the silver tables creation. 
	Why Sir Baraa did the creation of the silver tables BEFORE doing the bronze tables check ? 
	Execpt while doing checks with data from different tables, the Silver tables are never called before the Silver insert.

We have : 
1. Create Silver DDL Script by copying Bronze DDL Script and replace bronze. by silver. 
2. Execute Silver DDL script to create Silver tables
3. Check the bronze data
4. Create queries to clean the Bronze data during Silver's insert, potentially creating new cols
5. Modify the Silver DDL script to accommodate the new cols
6. Execute the newest Silver DDL script to have updated Silver tables
7. Create the Silver insert
8. Execute Silver insert

Instead, we could do :
1. Check the bronze data
2. Create queries to clean the Bronze data during Silver's insert, potentially creating new cols
3. Create Silver DDL Script by copying Bronze DDL Script and replace bronze. by silver AND by modifying it with the new cols we planned to have during the cleaning 
4. Execute Silver DDL script to create Silver tables
5. Create the Silver insert
6. Execute Silver insert




--- 
What to do when :
- sales table has some customer IDs that do not exists in the customer table? 