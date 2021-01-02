# Soru4
Bizim isteğimiz sample.content_category ve sample.content_category_20201222_00_59 tablolarını karşılaştırarak
insert edilen yeni kayıtları sample.content_category tablosunada eklemek,
update edilen kayıtlar var ise sample.content_category tablosunda da update etmek ve
silinmiş olan kayıtların sample.content_category tablosundaki karşılıklarının is_deleted alanını true olarak güncellemek ve silmeden saklamaktır.
Ve bunu yaparken tek bir create or replace table ya da merge statementı kullanarak yapmak istiyoruz.
Gereken sorguyu çalıştırdıktan sonra sample.content_category ve sample.content_category_target tablolarının içerikleri birebir aynı olmalıdır!

```SQL
merge `han_kurt.content_category` t
using `han_kurt.content_category_20201222_00_59` s
on t.id = s.id
when matched then
update set cdc_date = s.cdc_date, category = s.category 
when not matched by target then 
insert (cdc_date, is_deleted, id, category)
values (s.cdc_date,s.is_deleted,s.id,s.category)
when not matched by source then 
update set is_deleted = true;
  
with table_1
as (select farm_fingerprint(to_json_string(t1)) as hash1
from `dsmbootcamp.han_kurt.content_category_target` t1),
table_2 as (select farm_fingerprint(to_json_string(t2)) as hash2
from `dsmbootcamp.han_kurt.content_category` t2)
select * from table_1 full outer join table_2 on table_1.hash1=table_2.hash2 where hash1 is null or hash2 is null
```
