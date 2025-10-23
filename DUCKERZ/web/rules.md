## Описание

Вы попали в самый запутанный юридический квест! Ваша цель — подтвердить, что вы прочитали статью 272 УК РФ и осознаёте всю серьёзность её последствий.

## Решение

Получаем код страницы через GET запрос, в нем видим файл analytic.js, достаем уже его через GET запрос:

http://tasks.duckerz.ru:30054/analytics.js

Получаем JS код, декодируем его через дипсик :)
```
Основная логика находится в конце:
alert('GV@HFQYxwkbw\\avwwlm\\tbp\\gfejmjwfoz\\mlw\\lmf\\le\\wkf\\wfqnp~'['split']``['map'](_0x1717c4 => String['fromCharCode'](_0x1717c4['charCodeAt'](0x0) ^ 0x3))['join']``);
Это XOR шифрование с ключом 3.
```

Скрипт на питоне:
```
def decode_message():
    # Зашифрованное сообщение из кода
    encrypted = 'GV@HFQYxwkbw\\avwwlm\\tbp\\gfejmjwfoz\\mlw\\lmf\\le\\wkf\\wfqnp~'

    # XOR декодирование с ключом 3
    decoded = ''.join(chr(ord(c) ^ 3) for c in encrypted)

    return decoded


# Декодируем
result = decode_message()
print(f"Декодированное сообщение: {result}")
```

Флаг:

DUCKERZ{that_button_was_definitely_not_one_of_the_terms}
