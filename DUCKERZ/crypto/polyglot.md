## Описание

Задали тут выучить стих на нескольких языках, только я такие раньше не встречал.

## Решение

В файле видим комбинацию hex + Unicode escape sequences + base64, декодируем (https://www.dcode.fr/cipher-identifier):

Декодируем hex:

```
def decode_hex_string(hex_string):
    """Декодирует hex строку в текст"""
    # Убираем пробелы и преобразуем в байты
    hex_clean = hex_string.replace(' ', '').replace('\n', '')
    bytes_data = bytes.fromhex(hex_clean)
    # Декодируем в текст
    return bytes_data.decode('utf-8')

# Ваши данные
hex_data = """42 65 6e 65 61 74 68 20 74 68 65 20 73 6b 79 20 73 6f 20 66 61 73 74 20 61 6e 64 20 62 6c 75 65 2c 0a 54 68 65 20 67 65 6e 74 6c 65 20 77 69 6e 64 73 20 69 6e 20 77 68 69 73 70 65 72 73 20 66 6c 65 77 2c 0a 54 68 65 20 73 74 61 72 73 20 61 62 6f 76 65 2c 20 61 20 73 68 69 6e 69 6e 67 20 6c 69 67 68 74 2c 0a 47 75 69 64 65 20 74 68 65 20 73 6f 75 6c 20 74 68 72 6f 75 67 68 20 64 61 72 6b 65 73 74 20 6e 69 67 68 74 2e 0a 44 55 43 4b 45 52 5a 7b 34 77 33 73 30 6d 65"""

decoded_text = decode_hex_string(hex_data)
print("Декодированный текст:")
print(decoded_text)

Декодированный текст:
Beneath the sky so fast and blue,
The gentle winds in whispers flew,
The stars above, a shining light,
Guide the soul through darkest night.
DUCKERZ{4w3s0me
```

Unicode: _c0mput3r

base64: _3nc0d1ng!}

DUCKERZ{4w3s0me_c0mput3r_3nc0d1ng!}
