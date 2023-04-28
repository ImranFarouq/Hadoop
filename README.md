# Pig


### Copy files to read<br>

hadoop fs -copyFromLocal emp.csv pig/emp.csv <br>

### Load Data<br>

 A = load '/user/root/pig/emp.csv' using PigStorage(',') as (eid:int,ename:chararray,epos:chararray,esal:int,ecom:int,edpno:int);
 
Dump A;<br>

A2 = load '/user/root/pig/emp.csv’;<br>
describe A2;<br>




### Aggregate (by row)<br>

B = filter A by edpno==20;<br>
B2 = filter A by edpno==20 and epos=='MANAGER’,<br>
C = limit B 3;<br>
D = order C by esal desc;<br>


### Store Data<br>

store D into '/pig/pigout1’ using PigStorage(',’);<br>


### Transform (by column)<br>

Select existing column<br>

E = foreach A generate eid;<br>


### Create new column<br>

F = foreach A generate *, ecom*2 as Bonus,esal*5 as Incentive;<br>


### Transform columns<br>

G = foreach A generate SUBSTRING(ename,0,4);<br>

### Advanced codes<br>

H = foreach A generate $0,$1;<br>
I = group A by edpno;<br>
J = foreach I generate group as edpno, COUNT($1) as count;<br>
K = foreach A generate MAX(A.esal) as maxsal,MIN(A.esal) as minsal, SUM(A.esal) as sumsal, COUNT($1) as count;<br>
L = group A by (edpno, epos)<br>

SPLIT A into B if edpno==10, C if edpno==20, D if epos=='MANAGER';<br>
