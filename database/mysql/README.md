- `mysqldump -u <user> -h xxx.xxx.xxx.xxx -p --no-tablespaces <database> > backup.sql` - дамп базы данных MySQL
  - зайти в MySQL:`mysql -p -u ` - флаг -p для запроса пароля, флаг -u для запроса пользователя
  - выбрать целевую БД: `use <dbname>`
  - указать путь до дампа: `source /path/to/database_dump.sql;`
- `SELECT User, Host FROM mysql.user;` - посмотреть пользователей в СУБД
- `CREATE USER 'USER'@'%' IDENTIFIED BY 'PASSWORD';` - Создать пользователя (% - означает доступ к подключению будет с любого хоста)
- `GRANT ALL PRIVILEGES ON my_database.* TO 'my_user'@'%';` - Предоставление привилегий к базе данных для пользователя
- `USE DATABASE;` - Выбрать базу
- `SHOW TABLES;` - Показать таблицы
- `SHOW TABLE STATUS;`- Показать статус таблиц
- `CREATE DATABASE DATABASE;` - Создать БД
- `mysql -u root -p DATABASE < /path/to/backup.sql` - Восстановление таблиц из бекапа 

<details>
  <summary><b>Настройка ивентов и хранимых процедур</b></summary>

  Например необходимо создать процедуру удаления данных (строк) из определенной таблицы, а затем создать ивент на выполнение определенной хранимой процедуры

  1. Создание хранимой процедуры
     ```
     DELIMITER //

     CREATE PROCEDURE DeleteOrder_rormTables_older2year()
     BEGIN
         DECLARE rows_affected INT DEFAULT 1;
     
         -- Цикл удаления по 10,000 записей за раз
         WHILE rows_affected > 0 DO
             DELETE FROM test.test
             WHERE MY_COLUMN < DATE_SUB(NOW(), INTERVAL 2 YEAR)
             LIMIT 10000;
      
             -- Проверим, сколько строк было затронуто
             SET rows_affected = ROW_COUNT();
         END WHILE;
     END //

     DELIMITER ;
     ```
  2. Создание ивента в котором будет вызываться хранимая процедура
     ```
     CREATE EVENT IF NOT EXISTS DeleteOrder_rormTables_older2year
     ON SCHEDULE EVERY 1 DAY
     STARTS CURRENT_TIMESTAMP
     DO
     CALL DeleteOrder_rormTables_older2year();
     ```
</details>
