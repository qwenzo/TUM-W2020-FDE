
## 1


### 1

with examination (MatrNr , CourseNr ,PersNr , Grade ) as (
select * from pruefen
union
values (29120 ,0 ,0 ,3.0) , (29555 ,0 ,0 ,2.0) ,
(29555 ,0 ,0 ,1.3) , (29555 ,0 ,0 ,1.0)
),
grades (Name ,MatrNr , Semester , Grade ) as (
select s.name , s.matrnr , semester , avg( Grade )
from studenten s, examination p
where s. matrnr = p. matrnr
group by s.name , s.matrnr , semester
)
select matrnr, semester, avg, rank() over (partition by semester order by avg) from (select s2.matrnr, s2.semester, s1.avg as avg from studenten s2, (select s.matrnr, avg(grade) from studenten s, examination p where s.matrnr = p.matrnr group by s.matrnr) s1 where s2.matrnr = s1.matrnr);


### 2

with examination (MatrNr , CourseNr ,PersNr , Grade ) as (
select * from pruefen
union
values (29120 ,0 ,0 ,3.0) , (29555 ,0 ,0 ,2.0) ,
(29555 ,0 ,0 ,1.3) , (29555 ,0 ,0 ,1.0)
),
grades (Name ,MatrNr , Semester , Grade ) as (
select s.name , s.matrnr , semester , avg( Grade )
from studenten s, examination p
where s. matrnr = p. matrnr
group by s.name , s.matrnr , semester
)
select matrnr, semester, avg_grd, rank, -avg_grd+avg(avg_grd) over (partition by semester) from (select matrnr, semester, avg_grd, rank() over (partition by semester order by avg_grd) from (select s2.matrnr, s2.semester, s1.avg as avg_grd from studenten s2, (select s.matrnr, avg(grade) from studenten s, examination p where s.matrnr = p.matrnr group by s.matrnr) s1 where s2.matrnr = s1.matrnr));


## 2

### 1

with Professors (persnr , name , paygrade , room , salary , taxclass ) as
(
values (2125 , ' Sokrates ' , ' C4 ' ,226 ,85000 ,1) ,
(2126 , ' Russel ' , ' C4 ' ,232 ,80000 ,3) ,
(2127 , ' Kopernikus ' , ' C3 ' ,310 ,65000 ,5) ,
(2128 , ' Aristoteles ' , ' C4 ' ,250 ,85000 ,1) ,
(2133 , ' Popper ' , ' C3 ' ,52 ,68000 ,1) ,
(2134 , ' Augustinus ' , ' C3 ' ,309 ,55000 ,5) ,
(2136 , ' Curie ' , ' C4 ' ,36 ,95000 ,3) ,
(2137 , ' Kant ' , ' C4 ' ,7 ,98000 ,1)
)
select *, rank() over(order by salary desc) from Professors;

### 2

with Professors (persnr , name , paygrade , room , salary , taxclass ) as
(
values (2125 , ' Sokrates ' , ' C4 ' ,226 ,85000 ,1) ,
(2126 , ' Russel ' , ' C4 ' ,232 ,80000 ,3) ,
(2127 , ' Kopernikus ' , ' C3 ' ,310 ,65000 ,5) ,
(2128 , ' Aristoteles ' , ' C4 ' ,250 ,85000 ,1) ,
(2133 , ' Popper ' , ' C3 ' ,52 ,68000 ,1) ,
(2134 , ' Augustinus ' , ' C3 ' ,309 ,55000 ,5) ,
(2136 , ' Curie ' , ' C4 ' ,36 ,95000 ,3) ,
(2137 , ' Kant ' , ' C4 ' ,7 ,98000 ,1)
)
select *, rank() over(partition by paygrade order by salary desc) from Professors;

### 3

asc so we the smallest will be in the first row and the biggest in the last row.
with Professors (persnr , name , paygrade , room , salary , taxclass ) as
(
values (2125 , ' Sokrates ' , ' C4 ' ,226 ,85000 ,1) ,
(2126 , ' Russel ' , ' C4 ' ,232 ,80000 ,3) ,
(2127 , ' Kopernikus ' , ' C3 ' ,310 ,65000 ,5) ,
(2128 , ' Aristoteles ' , ' C4 ' ,250 ,85000 ,1) ,
(2133 , ' Popper ' , ' C3 ' ,52 ,68000 ,1) ,
(2134 , ' Augustinus ' , ' C3 ' ,309 ,55000 ,5) ,
(2136 , ' Curie ' , ' C4 ' ,36 ,95000 ,3) ,
(2137 , ' Kant ' , ' C4 ' ,7 ,98000 ,1)
)
select *, sum(salary) over(partition by paygrade order by salary rows) from Professors;

### 4

with Professors (persnr , name , paygrade , room , salary , taxclass ) as
(
values (2125 , ' Sokrates ' , ' C4 ' ,226 ,85000 ,1) ,
(2126 , ' Russel ' , ' C4 ' ,232 ,80000 ,3) ,
(2127 , ' Kopernikus ' , ' C3 ' ,310 ,65000 ,5) ,
(2128 , ' Aristoteles ' , ' C4 ' ,250 ,85000 ,1) ,
(2133 , ' Popper ' , ' C3 ' ,52 ,68000 ,1) ,
(2134 , ' Augustinus ' , ' C3 ' ,309 ,55000 ,5) ,
(2136 , ' Curie ' , ' C4 ' ,36 ,95000 ,3) ,
(2137 , ' Kant ' , ' C4 ' ,7 ,98000 ,1)
)
select *, avg(salary) over(partition by paygrade order by salary rows between 2 preceding and 2 following) from Professors;

### 5

with Professors (persnr , name , paygrade , room , salary , taxclass ) as
(
values (2125 , ' Sokrates ' , ' C4 ' ,226 ,85000 ,1) ,
(2126 , ' Russel ' , ' C4 ' ,232 ,80000 ,3) ,
(2127 , ' Kopernikus ' , ' C3 ' ,310 ,65000 ,5) ,
(2128 , ' Aristoteles ' , ' C4 ' ,250 ,85000 ,1) ,
(2133 , ' Popper ' , ' C3 ' ,52 ,68000 ,1) ,
(2134 , ' Augustinus ' , ' C3 ' ,309 ,55000 ,5) ,
(2136 , ' Curie ' , ' C4 ' ,36 ,95000 ,3) ,
(2137 , ' Kant ' , ' C4 ' ,7 ,98000 ,1)
)
select *, avg(salary) over(partition by paygrade order by salary range between 5000 preceding and 5000 following) from Professors;

### 6
with Professors (persnr , name , paygrade , room , salary , taxclass ) as
(
values (2125 , ' Sokrates ' , ' C4 ' ,226 ,85000 ,1) ,
(2126 , ' Russel ' , ' C4 ' ,232 ,80000 ,3) ,
(2127 , ' Kopernikus ' , ' C3 ' ,310 ,65000 ,5) ,
(2128 , ' Aristoteles ' , ' C4 ' ,250 ,85000 ,1) ,
(2133 , ' Popper ' , ' C3 ' ,52 ,68000 ,1) ,
(2134 , ' Augustinus ' , ' C3 ' ,309 ,55000 ,5) ,
(2136 , ' Curie ' , ' C4 ' ,36 ,95000 ,3) ,
(2137 , ' Kant ' , ' C4 ' ,7 ,98000 ,1)
)
select *, lag(name) over (order by salary), lead(name) over (order by salary)
from professors;


### 7

SELECT name, paygrade, salary FROM Professors p1 WHERE (SELECT count(*) FROM Professors p2 WHERE p2.salary > p1.salary)<3

from (SELECT *, rank() over (order by salary desc) rank
from professors) r
where rank <= 3;
> wrong, why?
> because the window function is done before the order by and the where.
select *, rank() over (order by salary desc) as r from Professors where rank>=3;


## 3

### 1
select count(*) + 1 from (select *, leg-lag(leg, 1) over (order by trail_id) as diff from completed) where not diff = 1;

### 2
