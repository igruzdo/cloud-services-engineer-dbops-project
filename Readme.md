## Yandex Practicum: DBOps

### Добавление секретов для GitHub Actions

Необходимо добавить в секреты репозитория следующие переменные для правильной работы GitHub Actions и возможности
установить удалённое соединение:

```properties
DB_HOST=<IP_address>
DB_NAME=store
DB_PORT=5432
DB_USER=store_user
DB_PASSWORD=store_password
```

### Подключение к PostgreSQL для создания нового пользователя для миграций и прогона автотестов

```bash
psql "host=<IP_address> port=5432 dbname=store_default user=user password=password"
```

### Создание пользователя в PostgreSQL

Создание нового пользователя PostgreSQL с необходимыми правами для миграций и прогона автотестов

```sql
CREATE USER store_user WITH PASSWORD 'store_password';
```

### Создание базы данных для пользователя store_user

```sql
CREATE DATABASE store OWNER store_user;
```

### Выход из текущей сессиии

```sql
\q
```

### Подключение к базе данных store под пользователем store_user

```bash
psql "host=<IP_address> port=5432 dbname=store user=store_user password=store_password"
```

### SQL-запрос, показывающий какое количество сосисок было продано за каждый день предыдущей недели.

```sql
SELECT o.date_created, SUM(op.quantity)
FROM orders AS o
         JOIN order_product AS op ON o.id = op.order_id
WHERE o.status = 'shipped'
  AND o.date_created > now() - INTERVAL '7 DAY'
GROUP BY o.date_created;
```
