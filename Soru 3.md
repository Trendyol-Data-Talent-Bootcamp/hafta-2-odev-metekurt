# "sample.pageview": tablosunda 1 gün içerisinde trendyol.com a gelen tüm ziyaretlerin logu var.

```SQL
select date ,sum(active_users) over (order by date rows between 4 preceding and current row) as active_users 
from(select timestamp_trunc(view_ts,minute) date,
count(distinct deviceid) active_users,
from `dsmbootcamp.han_kurt.pageview`
group by 1
order by date)
```
