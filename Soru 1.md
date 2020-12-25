# 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.

```SQL
select * from (select rank() over (partition by sport order by count(*) desc) as row_number,sport,country,count(*) as medal_count
    from `dsmbootcamp.han_kurt.summer_medals`
    where year >= 1980
    group by sport, country)
where row_number in (1,3,5)
limit 3
```
