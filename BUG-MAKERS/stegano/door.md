## Описание

Бродя по улочкам Глубины, вы наткнулись на странную дверь. Шестое чувство подсказывает, что ключ в двери, но в замочной скважине его не видно. Попробуйте найти ключ, и откройте доступ к следующему заданию.

Решение этой задачи необходимо для доступа к следующей – «Двери».

## Решение

Открываем файл через zsteg и получаем текст с флагом:
```
text: "Start of message --- You think you found a entrance, huh? But i just got you inside of my three-walled bastion! Try to escape, if you can, hehehe... --- First flag is: --- xof{D00r_15_4_7r4P} --- password for the next part: --- LEET1BOOBS00 --- End of mess"

b1,bgr,lsb,xy       .. file: OpenPGP Public Key
b3,rgb,lsb,xy       .. text: "r;y.Oe&%>p"
b3,bgr,msb,xy       .. file: OpenPGP Public Key
```
Флаг: xof{D00r_15_4_7r4P}
