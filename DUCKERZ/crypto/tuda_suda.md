## Описание

Туда - сюда, туда — сюда.… и ведь ничего не меняется

## Решение

Каждый символ флага XORится с индексом, декодируем это:

```
def decrypt_message(encrypted_message):
    """Дешифрует сообщение, зашифрованное XOR с индексом"""
    decrypted = ''

    for i, char in enumerate(encrypted_message):
        # XOR обратно с индексом
        decrypted_char = chr(ord(char) ^ i)
        decrypted += decrypted_char

    return decrypted


# Зашифрованное сообщение (без "GLHF" в конце)
encrypted = "DTAHAW\\|p9xT=8Qa &Mw%sP&{Lv,C~/o\x13S_"

print("Зашифрованное сообщение:", repr(encrypted))
decrypted = decrypt_message(encrypted)
print("Расшифрованное сообщение:", decrypted)
```

DUCKERZ{x0r_15_n07_d1fF1cUl7_c1p3r}
