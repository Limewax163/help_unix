### Про подключение и по администрированию

<details>
  <summary><code>psql -U user -d database -h ip/host -p port/default-5432</code> - подключение к БД</summary>

> соответственно если БД находится в контейнере можно передать в него через docker exec не заходя в контейнер

- По командам уже внутри БД
  - `\l` - посмотреть список баз данных
  - `\c <db_name>` - выбрать БД
  - `\dt` - посмотреть таблицы в выбранной БД
    - возможно потребуется указать схему в формате `\dt <schema.*>`
  - `\dt+ <table_name>` - посмотреть подробную информацию по таблице
  - `SELECT * FROM <table_name>;` - посмотреть структуру таблицы и содержимое

</details>

___

### Про бекапы

`psql -U <user> -d <database> -f /path/to/backup.sql` - восстановление из бекапа(дампа)

`pg_dump -U <user> -d <database> > ./path/to/backup.sql` - дамп определенной базы данных

`pg_dump -U <user> -d <database> --exclude-table-data=public.table1 --exclude-table-data=public.table2 > backup.sql` - создается бекап БД `<database>` но для таблиц `<table1>` и `<table2>` не бекапится данные (только пустая таблица)

>[!NOTE]
>Если не указывать схему таблицы, то автоматически pgsq выберет схему `<public>` в примерах выше схема по умолчанию указана явно `--exclude-table-data=public.table1`
>
>Схема указывается в формате `<schema>.<table_name>`
