- `mysqldump -u <user> -h xxx.xxx.xxx.xxx -p --no-tablespaces <database> > backup.sql` - дамп базы данных MySQL
  - зайти в MySQL:`mysql -p -u ` - флаг -p для запроса пароля, флаг -u для запроса пользователя
  - выбрать целевую БД: `use <dbname>`
  - указать путь до дампа: `source /path/to/database_dump.sql;`
- `SELECT User, Host FROM mysql.user;` - посмотреть пользователей в СУБД
- `CREATE USER 'USER'@'%' IDENTIFIED BY 'PASSWORD';` - Создать пользователя (% - означает доступ к подключению будет с любого хоста)
- `ALTER USER 'USER'@'%' IDENTIFIED BY 'new_password';` - сменить пароль пользователю
- `GRANT ALL PRIVILEGES ON my_database.* TO 'my_user'@'%';` - Предоставление привилегий к базе данных для пользователя
- `USE DATABASE;` - Выбрать базу
- `SHOW TABLES;` - Показать таблицы
- `SHOW TABLE STATUS;`- Показать статус таблиц
- `CREATE DATABASE DATABASE;` - Создать БД
- `mysql -u root -p DATABASE < /path/to/backup.sql` - Восстановление таблиц из бекапа 

<details>
  <summary><b>Настройка ивентов и хранимых процедур</b></summary>

  Включение/Выключение планировщика
   ```
   SET GLOBAL event_scheduler = ON/OFF;
   ```

  Например необходимо создать процедуру удаления данных (строк) из определенной таблицы, а затем создать ивент на выполнение определенной хранимой процедуры

  1. Создание хранимой процедуры
     
     ```
     DELIMITER //

     CREATE PROCEDURE MY_PROCEDURE()
     BEGIN
         DECLARE rows_affected INT DEFAULT 1;
     
         -- Цикл удаления по 10,000 записей за раз
         WHILE rows_affected > 0 DO
             DELETE FROM MY_DATABASE.MY_TABLE
             WHERE MY_COLUMN < DATE_SUB(NOW(), INTERVAL 2 YEAR)
             LIMIT 10000;
      
             -- Проверим, сколько строк было затронуто
             SET rows_affected = ROW_COUNT();
         END WHILE;
     END //
 
     DELIMITER ;
     ```
  1.1 Пример запроса, который выводит список всех хранимых процедур в текущей базе данных
  ```
  SELECT ROUTINE_NAME, ROUTINE_SCHEMA, ROUTINE_TYPE, CREATED, LAST_ALTERED
  FROM information_schema.ROUTINES
  WHERE ROUTINE_TYPE = 'PROCEDURE' AND ROUTINE_SCHEMA = DATABASE();
  ```
  1.2 Удаление хранимой процедуры
  ```
  DROP PROCEDURE IF EXISTS MY_PROCEDURE;
  ```
  2. Создание ивента в котором будет вызываться хранимая процедура
  ```
  CREATE EVENT IF NOT EXISTS MY_EVENT
  ON SCHEDULE EVERY 1 DAY
  STARTS CURRENT_TIMESTAMP
  DO
  CALL MY_PROCEDURE();
  ```
  2.1 Пример запроса, который выводит список всех ивентов в текущей базе данных
  ```
  SHOW EVENTS FROM `MY_DATABASE`;
  ```
  2.2 Включение/Выключение ивента
  ```
  ALTER EVENT `MY_EVENT` ENABLE/DISABLE;
  ```
  2.3 Удаление ивента  
  ```
  DROP EVENT `MY_EVENT`;
  ```
     
</details>

<details>
  <summary><b>Запрос для проверки зависших dead-lock запросов</b></summary>

  ```
  SELECT 
    ml.OBJECT_NAME      AS lock_name,
    ml.LOCK_STATUS,
    t.PROCESSLIST_ID,
    t.PROCESSLIST_USER,
    t.PROCESSLIST_HOST,
    t.PROCESSLIST_INFO AS holding_query
  FROM performance_schema.metadata_locks ml
  JOIN performance_schema.threads t 
    ON ml.OWNER_THREAD_ID = t.THREAD_ID
  WHERE ml.LOCK_STATUS = 'GRANTED';
  ```

  ```
  SELECT
    holder.PROCESSLIST_ID       AS holder_session_id,
    holder.PROCESSLIST_USER     AS holder_user,
    holder.PROCESSLIST_HOST     AS holder_host,
    holder.PROCESSLIST_INFO     AS holder_query,
    waiter.PROCESSLIST_ID       AS waiting_session_id,
    waiter.PROCESSLIST_USER     AS waiting_user,
    waiter.PROCESSLIST_HOST     AS waiting_host,
    waiter.PROCESSLIST_INFO     AS waiting_query,
    ml_wait.OBJECT_TYPE         AS lock_type,
    ml_wait.OBJECT_NAME         AS lock_name,
    CONCAT('KILL ', holder.PROCESSLIST_ID, ';') AS kill_holder_command,
    CONCAT('KILL ', waiter.PROCESSLIST_ID, ';') AS kill_waiter_command
  FROM performance_schema.metadata_locks ml_wait
  JOIN performance_schema.threads waiter
    ON ml_wait.OWNER_THREAD_ID = waiter.THREAD_ID
  JOIN performance_schema.metadata_locks ml_hold
    ON ml_wait.OBJECT_NAME = ml_hold.OBJECT_NAME
  AND ml_wait.OBJECT_TYPE = ml_hold.OBJECT_TYPE
  AND ml_hold.LOCK_STATUS = 'GRANTED'
  JOIN performance_schema.threads holder
    ON ml_hold.OWNER_THREAD_ID = holder.THREAD_ID
    WHERE ml_wait.LOCK_STATUS = 'PENDING';
  ```
</details>
