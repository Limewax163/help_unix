### Про бекапы

`pg_dump -U <user> -W -h <ip/host> -p <port/default-5432> <dbname> > ./path/to/dump.dump` - дамп определенной базы данных POSTGRES

`psql -U <user> -d <database> -h <ip/host> -p <port/default-5432> -f /path/to/backup.sql` - восстановление из бекапа(дампа)

`pg_dump -U <user> -d <database> --table=table1 --table=table2 --data-only > backup.sql` - создается бекап БД `<your_database>` но для таблиц `<table1>` и `<table2>` не бекапится данные (только пустая таблица)

`pg_dump -U your_username -d your_database --ignore-table=public.table1 --ignore-table=public.table2 > backup.sql` - создается бекап БД `<your_database>` исключая таблицы `<table1>` и `<table2>`

>[!NOTE]
>Если не указывать, в конструкции выше, схему таблицы, то автоматически pgsq выберет схему `<public>` в примере выше схема по умолчанию указана явно `--ignore-table=public.table`
>
>Схема указывается в формате `<schema>.<table_name>`

___

### Про подключение

<details>
  <summary><code>psql -U user -d database -h ip/host -p port/default-5432</code> - подключение к БД</summary>

> соответственно если БД находится в контейнере можно передать в него через docker exec не заходя в контейнер

- По командам уже внутри БД
  - `\l` - посмотреть список баз данных
  - `\c <db_name>` - выбрать БД
  - `\dt` - посмотреть таблицы в выбранной БД
  - `\dt+ <table_name>` - посмотреть подробную информацию по таблице
  - `SELECT * FROM <table_name> LIMIT 10;` - посмотреть структуру таблицы и содержимое

</details>
