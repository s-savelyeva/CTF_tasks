## Описание

Наши разработчики создали надежную систему авторизации и персональное хранилище для каждого пользователя. Они уверены в безопасности приложения и готовы поспорить, что никто не сможет получить доступ к административной панели. Сможете ли вы доказать обратное?

## Решение

Тут мы сталкиваемся с такой вещью как прототипное загрязнение:

```
 merge(target, source) {
        for (let key in source) {
            if (key === '__proto__') {
                Object.assign(this.prototype, source[key]);
                continue;
            }
```

То есть при подаче в запросе ключа proto он будет присвоен напрямую объекту

Регистрируемся, затем отправляем отпрос на логин, получаем ответ с токеном:
<img width="454" height="262" alt="image" src="https://github.com/user-attachments/assets/3946f05f-5d1d-4ddc-8d18-f7e98a71a694" />

```
{
    "success": true,
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwidXNlcm5hbWUiOiJ1c2VyIiwiaWF0IjoxNzYwNjQ0MTgyfQ.w4fRO47Bu8sp2pRjESDO_V2165F_DqxD7wzWKx9Vp9E"
}
```

