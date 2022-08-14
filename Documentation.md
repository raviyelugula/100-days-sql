# Day 1 - BTS - display the node type in order
<p>You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.</p>
<p>Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:</p>
<ol>
  <li>Root: If node is root node.</li>
  <li>Leaf: If node is leaf node.</li>
  <li>Inner: If node is neither root nor leaf node.</li>
</ol>

sample Input

|N   |P |
|:----|:----|
|1   |2|
|3   |2|
|6   |8|
|9   |8|
|2   |5|
|8   |5|
|5   |null|

Sample Output
|   |   |
|:----|:----|
|1 |Leaf|
|2 |Inner|
|3 |Leaf|
|5 |Root|
|6 |Leaf|
|8 |Inner|
|9 |Leaf |

```
select 
    t1.N,  
    case 
        when t1.P is NULL then 'Root'
        when t2.P is NULL then 'Leaf'
        when t2.P is not NULL and t1.P is not NULL then 'Inner' 
    end
from BST as t1 
left join 
(select distinct P from BST) as t2 
on t1.N = t2.P
order by t1.N;
```
# Day 2 - Occupation pivot
<p>Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, respectively.</p>
<p>Note: Print NULL when there are no more names corresponding to an occupation.</p>


Sample Input

|Name   |Occupation |
|:----|:----|
|Samantha   |Doctor|
|Julia   |Actor|
|Maria   |Actor|
|Meera   |Singer|
|Ashely   |Professor|
|Ketty   |Professor|
|Christeen   |Professor|
|Jane   |Actor|
|Jenny   |Doctor|
|Priya   |Singer|

Sample Output
|   |   |   |   |
|:----|:----|:----|:----|
|Jenny   |Ashley   |Meera   |Jane   |
|Samantha   |Christeen   |Priya   |Julia|
|NULL   |Ketty   |NULL   |Maria   |

```
set @r1=0, @r2=0, @r3=0, @r4=0;
select min(Doctor), min(Professor), min(Singer), min(Actor)
from(
  select case when Occupation='Doctor' then (@r1:=@r1+1)
            when Occupation='Professor' then (@r2:=@r2+1)
            when Occupation='Singer' then (@r3:=@r3+1)
            when Occupation='Actor' then (@r4:=@r4+1) end as RowNumber,
    case when Occupation='Doctor' then Name end as Doctor,
    case when Occupation='Professor' then Name end as Professor,
    case when Occupation='Singer' then Name end as Singer,
    case when Occupation='Actor' then Name end as Actor
  from OCCUPATIONS
  order by Name
    ) temp
group by RowNumber;
```

