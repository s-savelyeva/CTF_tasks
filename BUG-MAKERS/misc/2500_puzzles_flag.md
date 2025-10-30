## Описание

Исходную картинку, где написан флаг, разрезали на 2500 равных частей. Чтобы прочитать флаг, нужно собрать все на место. Части пазлов пронумерованы слева направо и сверху вниз. Исходное изображение имеет размер 600х600 пикселей.

## Решение

Собираем картинку с помощью питона, получаем флаг:

```
from PIL import Image
import os

# Создаём новое изображение 600x600
result = Image.new('RGB', (600, 600))

# Проходим по всем фрагментам
for n in range(1, 2501):
    # Вычисляем позицию
    row = (n - 1) // 50
    col = (n - 1) % 50
    x = col * 12
    y = row * 12

    # Открываем фрагмент
    fragment_path = f'2500-puzzles-flag/{n}.png'
    if os.path.exists(fragment_path):
        frag = Image.open(fragment_path)
        # Вставляем в результат
        result.paste(frag, (x, y))
    else:
        print(f"Отсутствует фрагмент {n}")

# Сохраняем результат
result.save('restored_flag.png')
print("Сборка завершена, флаг на restored_flag.png")
```

BugCTF{h0w_m4ny_p13c3s}
