## Описание

Душа машины сильнее стали. Конкретно у этой где-то есть флаг...

## Решение

Декомпилируем apk файл: http://www.javadecompilers.com/apk

Видим, что есть строка, которая закодирована AES шифром:

```
Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
private final String correctEncryptedString = "d5RIYRoC/3bQyfM8TSOlo463lgAMeSkAi/s8CEn1pMw=";
private final String iv = "89438fabcd849298";
private final String secretKey = "23ffabcde582ffacb49323fcbaddefac";
```

Декодируем и получаем флаг: https://www.devglan.com/online-tools/aes-encryption-decryption

CODEBY{ANdr0iD_t@sk_stR@nge}
