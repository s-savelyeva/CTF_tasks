## Описание

Что опять придумал этот безумец?

## Решение

Видим в коде username и password:

```
if ($username === 'admin' && $password === 'bl4ckbl4ckbl4ck') {
        $_SESSION['username'] = $username;
        $_SESSION['role'] = $params['role'] ?? 'user';
        header('Location: index.php');
        exit;
    }
```

В этом же коде видим, что параметр role можно передать через строку запроса:

```
$params = [];
parse_str($_SERVER['QUERY_STRING'], $params);
$_SESSION['role'] = $params['role'] ?? 'user';
```

Подаем запрос:

<img width="624" height="314" alt="image" src="https://github.com/user-attachments/assets/56341bf5-d0b3-434d-9820-627fdb6cd25e" />

В ответ получаем флаг и радуемся:

CODEBY{PHP_KN0WL3DG3_H3LPS}
