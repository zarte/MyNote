##只更新相同条件中的前两条数据
```
order by id limit 2
假如满足条件的有4条记录，只更新前两条：
UPDATE `t_core_salecentercategory` SET channelType=0 WHERE salePriceId=5428 AND categoryType=0 ORDER BY salePriceId LIMIT 2
```