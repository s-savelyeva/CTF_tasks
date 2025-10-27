## Описание

Я играл во дворе в шпионов. Задача была доставить письмо от Базы к базе безопасно, я решил закодировать сообщения, но похоже я перепутал порядок действий и теперь сообщение не вернуть, может ты найдёшь ошибку?

## Решение

Декодируем в base64:

```
import base64
import binascii

hex_data = "0d408a11167eaf7bf7af9dff6f8e77eb8ffbe3993e"
bytes_data = binascii.unhexlify(hex_data)
b64_encoded = base64.b64encode(bytes_data)
print(b64_encoded)
```

Получаем строку:
```
b'DUCKERZ+r3v3r53/b45364/745k+'
```
Заменяем / на _ и удаляем плюсы:

DUCKERZ{r3v3r53_b45364_745k}
