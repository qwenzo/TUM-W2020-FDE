# Solutions of exercies 5

## 5.1

### code:
```sql
select sum(l1.l_extendedprice) from 
lineitem l1, (select l_orderkey, avg(l_extendedprice)  as a from lineitem  GROUP BY l_orderkey) d
where l1.l_orderkey = d.l_orderkey
and l1.l_extendedprice > d.a
```
