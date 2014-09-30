title: How to use variables in MSSQL
alias: /blog/2014/08/how-to-use-variables-in-mssql/index.html
date: 2014-08-18 20:41:31 +0200
categories:
- Database
tags:
- mssql
- database
---

To use variables in MSSQL queries simply declare and set them before using:

	--
	-- Declare and set variables
	--
	DECLARE @migrationDate VARCHAR(100), @groupName VARCHAR(100)
	SET @migrationDate = '2014-08-15'
	SET @groupName = 'SOME-GROUP-NAME'

	--
	-- Execute query using variables
	--
	SELECT DISTINCT [username]
      ,[groupname]
      ,[migrationdate]
	FROM [MYDB].[dbo].[vw_my_special_view]
	WHERE [groupname] LIKE @groupName
	AND [migrationdate] LIKE @migrationDate
