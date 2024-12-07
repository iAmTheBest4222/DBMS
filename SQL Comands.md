
# SQL CMD Comands



open
```md
psql -U postgres
```

all databases
```md
\l
```

```md
create database <nameOfDatabase> 
```

go into database
```md
\c <nameOfDatabase>
```

all tables
```md
\d
```

structure of the table
```md
\d <tableName>
```

open postgres database close previous database
```md
\c postgres
```




# SQL Querys


```md
create table <tableName> (<column1> <dataType> <constraint>)
```

```md
insert into <tableName> values (<data1>,<data2>,<data3>)

```





# SQL Theory


## dataTypes:

- char(n)   
(if have less no of char that n it adds black spaces)     
`'A'`

- varchar(n)   
(can have less that n characters also)   
`'Divyanshu'`

- date   
`'01-10-2004'`

- int  
`24`

- boolean   
`True`

## constraint:

- not null

- unique

- check   
`check(a<10)` 

- primary key

- forign key 

## keywords & more

- distinct    
```md
select distinct <coumnName> from <tableName>
```

- all    
```md
select all <coumnName> from <tableName>
```

- as    
```md
select <coumnName> from <tableName> as A
```

- or / and    
```md
select <coumnName> from <tableName> where <condition> or <condition>
```

- like (string)  
   1. `_`  single character not null
   2. `%`  any no. os string including null    

```md
select <coumnName> from <tableName> where <coumnName> like '___%' 
```  

- order by       
```md
select <coumnName> from <tableName> order by <columnName> desc
```

- union / intersect / except    
```md
select <coumnName> from <tableName> where <condition> union select <coumnName> from <tableName> where <condition> union
```

aggregate functions 
avg  min max count sum
select <columnName>,avg(<coumnName>)from <tableName>





# DDL

- create   
```md
create table <tableName> (<column1> <dataType> <constraint>)
```

- alter  
   1. to add new column      
```md
alter table <tableName> add <columnName> <dataType>
```
- alter   
   2. to drop column   
```md
alter table <tableName> drop column <columnName>
```

- drop   
(delet table/database completly)    
```md
drop table <tableName>
```

- truncate   
(records to delete not structure)   
```md
truncate <table tableName>
```

- rename   
```md
alter table <tableName> rename to <newTableName>
```

# nested query 
## in where clause
> Find cources offered in Fall 2009 and in Spring 2010 (intersect)
```md
select distinct cource_id 
from section
where semester='Fall' and year=	2009 and 
	cource_id in(select cource_id
		     from section
		     where semester ='Spring' and yearr=2010);
```
> Find cources offered in Fall 2009 but not in Spring 2010 (except example)
```md
select distinct cource_id 
from section
where semester='Fall' and year=	2009 and 
	cource_id not in(select cource_id
		     from section
		     where semester ='Spring' and yearr=2010);
``` 
### set membership
> Find total number of distinct students who have taken cource section taught by the instructor with id 10101
```md
select count(distinct ID)
from takes
where (cource_id,sec_id,semester,year) in (select cource_id,sec_id,semester,year
					   from teaches
					   where teaches.ID=10101)
```
### set comparison
> Find names of instructurs with salary greater than that of some (at least one) instructur in the biology department
```md
select distinct T.name
from instructur as T,instructur as S
where T.salary>S.salary and S.dept_name='Biology';
```
or 
using `some`
if atleast one is true hole pridicate is true
```md
select name
from instructur
where salary>some(select salary 
		  from instructur
		  where dept_name='Biology');
```
> find the name of all instructors whose salary is greater than the salary of all instructors in biology department
using `all`
if all tuples are true than pridicate is true
```md
select name
from instructur
where salary>all(select salary 
		  from instructur
		  where dept_name='Biology');
```
> Find all cources taught in both the Fall 2009 semester and in the Spring 2010 semester
using `exist`
if atleast one is true hole pridicate is true
```md
select cource_id
from section as S
where semester='Fall' and year=2009 and exist(select * 
		  				from section as T
						where semester='Spring' and year=2010
						and S.course_id=T.cource_i);
```
> Find all students who have taken all the cources offered in the bio;ogy department
```md
select distinct S.ID,S.name
from students as S
where not exist (select cources_id
 		  from cource
		  where dept_name='Biology')
		  except
		  (select T.cource_id
		    		 from takes as T 
         			 where S.ID=T.ID));
```
> Find all cources that were offered at most once in 2009
```md
select T.cource_id
from cource as T
where unique (select R.cources_id
 		  from cource as R
		  where T.cource_id=R.cource_id
		  and R.year=2009);
```
## in from clause
