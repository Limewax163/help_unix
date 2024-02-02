`mysqldump -u <user> -h xxx.xxx.xxx.xxx -p --no-tablespaces <database> > backup.sql` - дамп базы данных MySQL

___
Для миграции дампа таблиц необходимо зайти в MySQL:

`mysql -p -u ` - флаг -p для запроса пароля, флаг -u для запроса пользователя

в MySQL передать команду:

`source /path/to/database_dump.sql;` - для миграции таблиц из дампа в базу
___
