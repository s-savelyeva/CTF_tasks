## Описание

Мой друг сисадмин забыл пароль от своей админской учётки.. стоит ли мне ему говорить, что я уже как месяц её взломал?

## Решение

Первый байт флага 'D' = 68

Зашифрованный первый байт = 0x1c = 28

key = ord('D') - 28 = 68 - 28 = 40

Пишем код для расшифровки:

```
flag = b'\x1c-\x1b#\x1d*2S(\x0c\n\nO\x08J<7\x1bJ\x0c\x1bC=\x1c7/\t\x0f@7\x1b\x0c\x1d\r\x0cJU'

# Первый байт флага 'D' = 68
# Зашифрованный первый байт = 0x1c = 28
# key = ord('D') - 28 = 68 - 28 = 40

key = 40

flag_output = []
for i in range(0, len(flag)):
    flag_output.append(chr(flag[i] + key))

str_flag = ''.join(flag_output)
print('Flag:', str_flag)
```

DUCKERZ{P422w0rd_Cr4CkeD_W17h_C4E54r}
