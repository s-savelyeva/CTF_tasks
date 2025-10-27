## Описание

Ты никогда не сможешь взломать этот код! Тут все по новейшим технологиям.

## Решение

Функция block_xor берет SHA256 хеш (32 байта) и:

Разбивает на 4 блока по 8 байт

Для каждого блока вычисляет XOR всех 8 байт

Возвращает 4 байта (по одному на блок)

Функция insecure_compare сравнивает только эти 4 XOR-байта, а не полный 32-байтный хеш!

Пишем код:

```
import requests

URL = "http://tasks.duckerz.ru:30017"

# 1. Получить правильные XOR-байты для команды
resp = requests.post(f"{URL}/oracle", json={"msg": "give_me_flag_plz"})
target_xor = resp.json()["block_xor"]  # 4 байта в hex

# 2. Создать поддельный MAC (32 байта)
# Каждый блок из 8 байтов должен иметь XOR = target_xor[i]
fake_mac = bytearray(32)

# Заполняем каждый блок так, чтобы XOR был правильным
target_bytes = bytes.fromhex(target_xor)
for i in range(4):
    block_start = i * 8
    # Первые 7 байтов блока - нули, последний = target_bytes[i]
    fake_mac[block_start + 7] = target_bytes[i]

# 3. Отправляем верификацию
resp = requests.post(f"{URL}/verify", json={
    "msg": "give_me_flag_plz",
    "mac": fake_mac.hex()
})

print(resp.text)
```

DUCKERZ{Crypt0_1S_V3ry_D1FF1cuLt_F0R_Y0uuuuu??}
