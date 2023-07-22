
# Hive

Check location from hdfs <br>

hadoop fs -ls /user/hive/warehouse<br>

Managed tables<br>

create table if not exists emp(empno int, ename string, sal float, comm float, dpno int) row format delimited fields terminated by ',’;

describe emp;<br>

load data local inpath '/home/imran12/Desktop/hadoopdata/emp.csv' into table emp;<br>

Select * from emp;<br>

External Tables<br>

create external table  emp_ext(empno int, ename string, sal float, comm float, dpno int) row format delimited fields terminated by ',’ location '/user/imran21/data’;

While giving path we have to give only directory path not file name<br>

Here, table will be in given hdfs path.<br>

create external table  emp_ext(empno int, ename string, sal float, comm float, dpno int) row format delimited fields terminated by ‘,’;

Table will be stored under /user/hive/warehouse/bda.db/ext_emp2/emp<br>

load data local inpath '/home/imran21/Desktop/hadoopdata/empdata' into table emp_ext;<br>

set hive.exec.dynamic.partition.mode;<br>
set hive.exec.dynamic.partition.mode=nonstrict;<br>

create external table emp_dept (empno int, ename string, sal float, comm float) partitioned by (dpno int) row format delimited fields terminated by ',’;

insert into table emp_dept partition(dpno) select * from emp;<br>

Check<br>
hadoop fs -ls /user/hive/warehouse/A.db<br>
