## Описание

«Я по частицам собираю твой портрет»
Подсказок и так слишком много, флаг где-то в файле.

## Решение

Смотрим через strings dabro_bro.png

Видим в начале и конце файла чанки с данными, собираем по крупицам флаг:

```
pT49: First part of your flag is 'y0u_h4ve_

pT50: c0l1ected'

pT76: Second part is '_4ll_7he_p4rt5'

pT99: ts of the flag into 'bUgCtF{}'
```

bUgCtF{y0u_h4ve_c0l1ected_4ll_7he_p4rt5}
