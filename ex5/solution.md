# Solutions of exercise 5

## 5.1

### code:
```sql
select sum(l1.l_extendedprice) from 
lineitem l1, (select l_orderkey, avg(l_extendedprice)  as a from lineitem  GROUP BY l_orderkey) d
where l1.l_orderkey = d.l_orderkey
and l1.l_extendedprice > d.a
```

## 5.2

### code:
```sql
select o1. o_orderkey from 
orders o1, (select distinct o_shippriority, o_orderstatus, avg(o_totalprice) as a from orders GROUP BY o_shippriority, o_orderstatus) d
where 
o1.o_shippriority = d.o_shippriority
and o1.o_orderstatus = d.o_orderstatus
and o1.o_totalprice < d.a
```
