## Описание

Этот нострадамус явно знает больше, чем вы, однако на любой вопрос он способен говорить только да или нет.

## Решение

Это классическая RSA LSB Oracle Attack (атака на оракул четности).

Суть уязвимости:
Оракул parity_oracle возвращает m % 2 (четность) для любого расшифрованного шифротекста. Это позволяет восстановить полное сообщение.

Алгоритм атаки:
Получить публичный ключ и зашифрованный флаг

Используя свойства RSA, постепенно восстанавливать биты флага

Математика атаки:
Если мы умножаем шифротекст на 2^e mod n, то при расшифровке получаем 2*m mod n. Оракул четности говорит нам младший бит (2*m mod n).

Пишем код:

```
import requests
from Crypto.Util.number import long_to_bytes

URL = "http://tasks.duckerz.ru:30014"

# 1. Получить публичный ключ и зашифрованный флаг
resp = requests.get(f"{URL}/public_key")
data = resp.json()

n = int(data["n"])
e = int(data["e"])
c = int(data["ciphertext"])

print(f"n = {n}")
print(f"e = {e}")
print(f"c = {c}")


# 2. Атака LSB Oracle
def oracle(ciphertext):
    resp = requests.get(f"{URL}/oracle", params={"ciphertext": str(ciphertext)})
    return resp.json()["even"]


lower = 0
upper = n

for i in range(n.bit_length()+50):
    c = (c * pow(2, e, n)) % n  # Умножаем на 2^e mod n

    if oracle(c):
        upper = (lower + upper) // 2
    else:
        lower = (lower + upper) // 2

    # Прогресс
    if i % 50 == 0:
        print(f"Progress: {i}/{n.bit_length()}")

# Восстанавливаем флаг
flag = long_to_bytes(upper)
print(f"Flag: {flag.decode()}")
```

В флаге меняем последний символ на }

DUCKERZ{0r4cl3_s41d_p4r1ty_0f_RS4_is_3v3n}
