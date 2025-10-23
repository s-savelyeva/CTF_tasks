## Описание

На этот раз придется доказать свое упорство, набрав всего 1000000 очков в самой знаменитой игре.

## Решение

Ну конечно же мы не будем играть до посинения во Flappy Bird (правда же?...)

Переходим по ссылке http://tasks.duckerz.ru:30061/game.js (видим название этого скрипта в инструментах разработчика) и скармливаем скрипт дипсику:

```
В функции getFlag() есть XOR-шифрование:

function getFlag() {
    const _0x354951 = _0x4bd33d,
        _0x480cb6 = _0x354951(0x10c),  // Ключ для XOR
        _0x496538 = _0x354951(0xfc),   // Зашифрованный флаг
        _0x225a3e = xorEncryptDecrypt(_0x496538, _0x480cb6);
    alert(_0x225a3e);
}

Находим значения по смещениям:

0x10c = 268 в десятичной

0xfc = 252 в десятичной

Смотрим что находится по этим индексам в массиве _0xa65f:

_0xa65f[268] - это ключ для XOR

_0xa65f[252] - это зашифрованный флаг

Из массива _0xa65f:

Индекс 252: '\x00\x00\x00\x00\x00\x00\x00$\x06{3l06\x0bk\x17\x0a\x00$!7\x27'`

Индекс 268: 'DUCKERZ_THE_BEST!'

```

Ладно, заткнись и дай мне мой флаг:

```
def xor_encrypt_decrypt(data, key):
    result = ""
    for i in range(len(data)):
        result += chr(ord(data[i]) ^ ord(key[i % len(key)]))
    return result

# Данные из JavaScript кода
encrypted_flag = '\x00\x00\x00\x00\x00\x00\x00$\x06{3l06`\x0bk\x17\x0a\x00$!7\x27'
key = 'DUCKERZ_THE_BEST!'

# Декодируем флаг
flag = xor_encrypt_decrypt(encrypted_flag, key)
print(f"Декодированный флаг: {flag}")
```

Декодированный флаг: DUCKERZ{R3v3rs3_JS_Code}

