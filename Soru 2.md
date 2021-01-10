# 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

```SQL
select * from (select sport, athlete,
    first_value(year) over(partition by sport, athlete order by sport, athlete) as first,
    lead(year, 1) over(partition by sport, athlete order by athlete, year) as second,
    lead(year, 2) over(partition by sport, athlete order by athlete, year) as third
    from `dsmbootcamp.han_kurt.summer_medals`
    where year >= 1980
    order by athlete)
where second - first = 4 and third - second = 4
```
