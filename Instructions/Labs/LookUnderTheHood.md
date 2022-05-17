```sql
--  A look under the hood

-- Table Distribution views

-- Returns the database object identification number of a table 
SELECT OBJECT_ID(N'[dbo].[Category]', N'U') AS 'Object ID';  
GO

-- Return Table properties. Holds distribution information for columns.
SELECT object_name (OBJECT_ID), * from sys.pdw_table_distribution_properties; 


-- Holds information about the distributions. 
SELECT * from sys.pdw_distributions; 
-- Return 
-- distribution_id:	id associated with the distribution.
-- pdw_node_id:  the node this distribution is on
-- name	String identifier associated with the distribution.
-- position	Position of the distribution within a node respective to other distributions on that node.	1 to the number of distributions per node.

-- Map the ‘logical’ table record from sys.tables with the actual internal table names, due to the 60 distribution.   
SELECT * from sys.pdw_table_mappings;

-- Map which compute node and distribution corresponds to each internal table
SELECT name, object_id, * from sys.pdw_nodes_tables;

-- This view has the partition information matching those internal tables
SELECT partition_id, pdw_node_id, distribution_id from sys.pdw_nodes_partitions;

-- For crucial information like the amount of rows, the space used for a particular table, index or partition.
 SELECT TOP (60) partition_id, object_id, index_id, row_count, used_page_count, pdw_node_id, distribution_id from sys.dm_pdw_nodes_db_partition_stats;

-- Holds distribution information for columns. 0 = Not a distribution column. 1 = Azure Synapse Analytics is using this column to distribute the parent table.
 SELECT * from sys.pdw_column_distribution_properties;


--   Find distribution of a table 
SELECT t.name, td.distribution_policy_desc 
FROM sys.tables t INNER JOIN 
sys.pdw_table_distribution_properties td ON t.object_id=td.object_id
WHERE t.name='SaleSmall';

-- Which column is used for hashing 
SELECT t.name, cols.name, st.name [type_name]
FROM sys.tables t INNER JOIN
sys.columns cols ON t.object_id=cols.object_id 
INNER JOIN sys.pdw_column_distribution_properties colp ON cols.object_id=colp.object_id AND cols.column_id=colp.column_id
INNER JOIN sys.types st ON cols.system_type_id=st.system_type_id
WHERE t.name='SaleSmall' AND colp.distribution_ordinal=1;

-- Space taken by table  
SELECT t.name,nps.[row_count],nps.[used_page_count]*8.0/1024 as usedSpaceMB, nt.distribution_id
 FROM
   sys.tables t
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1 /* HEAP = 0, CLUSTERED or CLUSTERED_COLUMNSTORE =1 */
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
WHERE t.name='SaleSmall'
ORDER BY usedSpaceMB DESC;

-- Query Labels for tracking individual queries
SELECT AVG(ProfitAmount) 
FROM wwi.SaleSmall
OPTION (LABEL = 'AverageProfit');

-- Track query with label 
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'AverageProfit';


-- How many queries are currently running and how many queued?
SELECT active_requests, queued_requests FROM sys.dm_pdw_sys_info;

-- System Status Views
SELECT client_id, encrypt_option, auth_scheme 
FROM sys.dm_pdw_exec_connections
WHERE encrypt_option='TRUE';

-- Query Status 
SELECT [status], command, resource_class FROM
sys.dm_pdw_exec_requests r
WHERE r.[label]='AverageProfit';

-- Last 5 runs 
SELECT TOP 5 [status], command, total_elapsed_time FROM
sys.dm_pdw_exec_requests r
WHERE r.[label]='AverageProfit'
ORDER BY total_elapsed_time DESC;

-- Data Movement Services Views

-- This view shows the status of the DMS service on each compute node in the system
SELECT * from sys.dm_pdw_dms_cores;

-- This view shows each step that a DMS worked has taken to read a file from an external location like Blob Storage. No result for now. 
SELECT * from sys.dm_pdw_dms_external_work;

-- This view shows each DMS worker thread and information on the work they are doing or have done. This includes the node and distribution that the worker is working on, CPU, time and other measurements
SELECT * from sys.dm_pdw_dms_workers;

```
