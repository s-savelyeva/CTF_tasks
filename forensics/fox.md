## Описание

В корпоративной сети был развернут экспериментальный WebSocket-чат внутри браузера Firefox. По внутренним данным, злоумышленники могли передавать через этот чат скрытые команды к серверам C2

## Решение

Открываем session.pcap через Wireguard. Заходим в Wireshark -> Preferences -> Protocols -> TLS.

В поле (Pre)-Master-Secret log filename указываем путь к sslkeys.log

Сразу же появляются новые строки и тыкаем на строку:

12	0.007467	::1	::1	WebSocket	150	WebSocket Text [FIN] [MASKED]

Получаем флаг:

CODEBY{f1ref0x_s3cr3t_dump}
