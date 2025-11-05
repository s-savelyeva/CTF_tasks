## Описание

Мой друг написал научную статью на тему экспансии Чужих, и, для доказательства авторских прав, спрятал флаг в ней.
Формат флага BugCTF{fl4g_7ex7}

## Решение

Смотрим через exiftool всю информацию о файле, обращаем внимание на строку закодированную в base64 в Zip Comments :

```
apple@MacBook-Pro-apple Downloads % echo "QnVnQ" | base64 -d
Bug% 
```

Собираем все строки и декодируем сразу же:

```
apple@MacBook-Pro-apple Downloads % exiftool -a -u -g1 AlienExpansion.docx | grep "Zip File Comment" | awk '{print $NF}' | tr -d '\n' | base64 -d && echo

BugCTF{!Thi$_fl@g_1s_inj3cted_in_7he_4rch1ve_l!ke_an_a1ien!}
```
